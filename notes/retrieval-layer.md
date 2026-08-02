# Building a retrieval layer I could defend

Notes on the semantic search and retrieval-augmented generation layer in [Inner Order OS](https://inner-order-os.vercel.app), a production LLM application I designed, built, deployed, and operate on my own.

The feature is small to describe: after you save a record, the app shows you things you have written before that are close to it, and the follow-up question the model asks can point at the repeat instead of treating every entry as isolated. Most of the work was in decisions that are invisible from the front end, so this is a note about those.

The source repository is private. If you want to read it, ask me and I will add you.

## Choosing an embedding provider, and why the three obvious answers failed

Three options look correct and are not.

**Groq has no embeddings endpoint.** The application already runs its generation on Groq, so reusing it was the default assumption. Its catalogue is language, speech, Prompt Guard, and Compound only. The `embedding.py` types inside the installed SDK are vestigial and do not imply a live endpoint.

**The platform's built-in model is English-only.** Supabase Edge Functions ship `gte-small`, which is free and one line to call. It truncates at 512 tokens and does not handle Chinese. The product's default language is Chinese and its source material is classical Chinese text, so this was unusable rather than merely imperfect.

**OpenAI does not serve this region.** `text-embedding-3-small` is the reflexive answer. OpenAI does not serve Hong Kong, Macau, or mainland China, and their documentation warns that access from an unsupported region risks account suspension. That rules it out at both ends of this project's geography.

The choice was Jina `jina-embeddings-v3`: multilingual, 8192-token context, Matryoshka dimensions so the vector width is a decision rather than a constant. `BAAI/bge-m3` through a domestic provider is the drop-in alternative at the same 1024 dimensions, which matters because it means the column type does not change if the provider does.

## Decisions that took longer than the code

**No fallback mock, deliberately.** Elsewhere in this application, a missing API key falls back to a plausible mock so local development keeps working. The embedding client does the opposite: no key means no embeddings, in every environment. A random vector produces confident nonsense neighbours, and a feature that quietly lies is worse than a feature that is absent. The client never throws either. It returns null on every failure and the save path continues, because losing someone's writing to a failed embedding call would be an absurd trade.

**Asymmetric encoding.** Stored records are embedded as `retrieval.passage`, search text as `retrieval.query`. jina-v3 applies a different LoRA adapter per task, and the two are not interchangeable even though the API accepts either.

**A stale vector is worse than a missing one.** When a record is edited, the vector is recomputed. If recomputation fails, the existing vector is set to null rather than left in place. Missing merely drops the record out of results. Stale files it under the wrong neighbourhood, where it will surface for the wrong queries and quietly degrade every result around it.

**Two thresholds, not one.** The browsable list of neighbours uses a similarity floor of 0.35. The context injected into the model's prompt uses 0.5. A loose match in a list costs the reader nothing, since they can see it is unrelated and move on. A loose match inside a prompt actively degrades the question that comes out. In testing, a record at 0.207 similarity is excluded and one at 0.953 is injected.

**Hard caps on injected context: two records, 120 characters each.** Every character of context is input that the anti-summary prompt has to hold against, and the small model this application uses for question generation degenerates on long open-ended input. More context here is risk without benefit.

**When there are no neighbours, the prompt is byte-identical to what it was before this feature existed.** There is a regression test asserting exactly that, because the failure mode worth fearing is degrading the experience for people who have written nothing similar yet.

## The authorization decision, which is the one that matters

Both SQL matching functions are declared `SECURITY INVOKER`. They run as the calling user, so row-level security governs retrieval the same way it governs every other read.

`SECURITY DEFINER` would run them as the owner and bypass RLS entirely. That is the most common authorization hole in vector search implementations, and it is easy to reach by accident because the function works either way during development, when you are the only user.

For the same reason there is deliberately no `user_id` filter inside the function bodies. RLS is the single gate. A second filter in application code would imply the first one is not trusted, and the day someone adds a code path that forgets the second filter, you want the database to still say no. Verified by test: user B requesting user A's record id gets zero rows.

One related decision in the type layer. The `embedding` column is absent from the row type on purpose, and lives only on the update type plus an explicit column list. If it were on the row type, every `select("*")` would ship several kilobytes of floats per record, including into server-rendered page payloads, and nobody would notice until the page got slow.

## Operations

Backfilling embeddings for existing records is an admin API route, not a local script. The API key and the target database both live in the deployment environment, so no production credentials need to sit on a laptop, and the route shares the live code path so it cannot drift away from it. It processes 25 rows per call, sequentially, and only touches rows where the embedding is null, so it is safe to run repeatedly.

Rejected model output is logged in production with a flag recording whether retrieved context was present. That flag is the point: longer context reopens the degeneration problem this prompt was hardened against twice, so rejection rates have to be compared across the two cases rather than in aggregate.

## What this does not show

Production currently holds 15 active records, and all 15 belong to one account. Six other accounts have written nothing. The alpha has not started producing data.

Two consequences worth stating rather than dressing up. The HNSW index buys nothing at this row count. It exists so that it grows with the data instead of needing a migration later, and there is no benchmark behind it. And chunking is genuinely unnecessary rather than skipped: the longest record is 279 characters, so one record is one chunk. Adding a chunking layer would make the pipeline look more like a textbook RAG diagram and do nothing.

The model's output quality with retrieved context in the prompt has not been observed on real traffic yet. The wiring is verified end to end, up to and including the model call. Whether the questions get better is an open question, and the rejection logs are how I intend to answer it.
