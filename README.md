# Zijun Wang

I build production AI systems, model data that does not arrive clean, and read spatial evidence about what the technology is doing on the ground.

Four strands, and they keep turning out to be the same habit applied to different material: take a question nobody has phrased precisely yet, find the data that can answer it, build the thing that produces the answer, and stay accountable for the number at the end.

- **AI engineering.** Production LLM applications I design, build, deploy, and operate on my own: retrieval over vector search, output guardrails, per-task model assignment, and the debugging that starts once real traffic arrives.
- **Data and modelling.** NLP, statistics, and machine learning, from topic modelling and sentiment classification to agent-based simulation of how behaviour spreads through a network.
- **Geospatial.** Two degrees in geographic information science, and satellite-based estimation models for public-sector planning. The least fashionable and most distinctive thing I do.
- **Product and delivery.** Four years converting client problems into requirements, workflows, dashboards, and rollout plans, then presenting the results to the people who had to act on them.

I follow both the US and China AI markets, and work in English, Mandarin, and Cantonese.

## Selected work

**AI systems in production**

- **Inner Order OS** ([live](https://inner-order-os.vercel.app)): An AI-assisted reflection platform on Next.js, PostgreSQL, and Groq. Built and deployed solo: multilingual embeddings with pgvector retrieval feeding context back into generation, three layers of output guardrails with rejections logged so model drift stays visible, row-level security, and a library-level guard that keeps user content out of the analytics table.
- **Bridge the Gap** ([live](https://bridge-the-gap-ai.vercel.app)): A cross-cultural Q&A platform with a content ranking algorithm of my own design: tiered quality buckets, weighted endorsements, Wilson lower-bound confidence scoring, and freshness decay.

**Analytics and markets**

- **[Stock Analyzer](https://github.com/zijunVV/stock-analyzer)** ([live](https://stockanalyzer-ai.vercel.app)): Equity analytics across US, Hong Kong, and China A-share markets. Live data ingestion, four-dimension analysis, an AI-ranked S&P 500 screener, and a curated [AI value-chain map](https://stockanalyzer-ai.vercel.app/value-chain). FastAPI, React, and Groq, with automatic model fallback when a provider hits capacity.

**Geospatial**

- **[AirportTwin AI](https://github.com/zijunVV/airport-twin-ai)**: A GIS-to-3D digital twin MVP for Hong Kong International Airport mobility simulation. Cleans public OpenStreetMap data and generates scene-ready geometry in a USD-ready hierarchy.

**Research code**

- **[OpinionDynamics_ESG](https://github.com/zijunVV/OpinionDynamics_ESG)**: My master's thesis pipeline. LDA topic modelling over 78,116 ESG-related reviews filtered from the Yelp Open Dataset with the topic count selected by coherence score, sentiment scored with a pretrained multilingual BERT model, then a Mesa agent-based model on a Barabási-Albert scale-free network to test when a narrative spreads through a community and when it stalls.

**Frameworks and method**

- **[KCAS](https://github.com/zijunVV/kcas-core)**: Four open-source frameworks for evaluating, researching, allocating, and building knowledge capital. Documentation and framework design, not a running system.
- **[agent-spec-template](https://github.com/zijunVV/agent-spec-template)**: A two-layer specification template for AI coding agents. Fourteen patterns, each one traced back to a real failure.

## What I am learning

Where the economic value of AI ends up, layer by layer, from semiconductors and cloud through models, agents, and applications. I come at that question from an unusual direction, which is what these systems cost and what they can genuinely do once you have built with them.

What runs today is the collection layer: scheduled agents assemble weekly industry digests and pull consensus data, with interpretation kept manual on purpose. The industry and financial half of this is a track I started in early 2026 and am still learning, so I would rather label it that way than overstate it.

More at [zijunvv.github.io](https://zijunvv.github.io).
