# Zijun Wang

Hi, I'm Zijun. I build applied AI products, with most of my recent work focused on agents, retrieval, evaluation, and the safeguards around tool use.

I came to AI by a slightly indirect route. I studied geographic information science and remote sensing, then spent four years working across data analysis, product delivery, and government digitalisation projects in China. Later, at the University at Buffalo, I used NLP and agent based modelling to study how sustainability narratives spread through social networks.

Most of my work starts with a question that is still too vague to code. I like talking it through, deciding what evidence would count, and only then choosing the model or stack. I also try to be plain about limits. A local MVP is a local MVP, and a metric on synthetic data is useful but is not the same as user adoption.

I am looking for applied AI and AI agent roles in Singapore, and I am happy to relocate and work locally. I work in English, Mandarin, and Cantonese.

## Current project

### [Social Memory Copilot](https://github.com/zijunVV/social-memory-copilot)

I built this because a useful personal assistant should remember more than a contact's name. It should know where the last conversation stopped, what was promised, and how that person prefers to communicate. The project is a local portfolio MVP built with Google ADK, FastAPI, PostgreSQL, and pgvector.

The retrieval path combines BM25 and vector search, followed by RRF and a lightweight reranker. On a small synthetic evaluation set, Recall@5 rose from 0.682 with either route alone to 0.889 after fusion and reranking. Memory writes and reminders stay pending until the user confirms them. The repository currently has 354 passing tests across unit and integration suites.

This is a working local MVP, not a deployed product with real users. The work has taught me a lot about session state, memory boundaries, retrieval quality, user isolation, prompt injection handling, and safe side effects.

## Other work

- **[Stock Analyzer](https://github.com/zijunVV/stock-analyzer)** ([live](https://stockanalyzer-ai.vercel.app)): Equity analysis across the US, Hong Kong, and China A-share markets. It includes live market data, an S&P 500 screener, company analysis, and automatic model fallback when a provider is unavailable.
- **Inner Order OS** ([live](https://inner-order-os.vercel.app)): A reflection product built on Next.js and PostgreSQL. Its retrieval layer brings relevant past entries back into the conversation, while output checks keep the model within the product's intended role. I wrote about the design choices in [Building a retrieval layer I could defend](./notes/retrieval-layer.md).
- **Bridge the Gap** ([live](https://bridge-the-gap-ai.vercel.app)): A bilingual knowledge product for cross-cultural food questions. Its ranking method combines weighted endorsements, Wilson score confidence, and freshness decay.
- **[AirportTwin AI](https://github.com/zijunVV/airport-twin-ai)**: A GIS to 3D prototype for roads around Hong Kong International Airport. It cleans OpenStreetMap data and turns it into scene-ready geometry for future mobility simulation.
- **[KCAS](https://github.com/zijunVV/kcas-core)**: Four open-source frameworks for research, capital allocation, and project decisions. This is documentation and method design rather than a running application.

## Earlier work

Before these projects, I worked as a product manager and data analyst on more than six government digitalisation projects. I defined metrics, wrote SQL and Python analysis workflows, translated stakeholder needs into product requirements, and coordinated delivery across engineering, data, and design teams.

I also worked in remote sensing, where I used satellite imagery and machine learning for public-sector planning and environmental assessment. At the University at Buffalo, I later joined an NSF-funded research project on how communities share information and help one another during extreme winter storms.

## What I am studying now

I am learning how economic value moves through the AI industry, particularly across chips, cloud infrastructure, models, and applications in the US and China. Scheduled agents help me collect weekly material and consensus data, but I keep the reading and interpretation manual. I started this part of the work in early 2026, so I still describe it as a learning track rather than expertise.

More projects and background are available at [zijunvv.github.io](https://zijunvv.github.io).
