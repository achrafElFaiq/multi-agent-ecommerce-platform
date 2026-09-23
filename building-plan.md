# ACOS — Building Plan

## Phase 0 — Foundation (Week 1)

Get the skeleton running before any AI work.

- **Monorepo structure** — FastAPI backend, Next.js frontend, shared configs
- **PostgreSQL** with a realistic e-commerce schema (products, orders, customers, inventory)
- **Seed data** — find a public dataset (Brazilian E-Commerce from Kaggle is solid, ~100K orders with reviews, sellers, geolocation) or generate synthetic data that feels real
- **Docker Compose** for local dev — Postgres, Redis, Qdrant, the API, the frontend
- Skip Kafka, Neo4j, K8s, and Terraform for now. Add them only when a real need emerges.

## Phase 1 — First Agent Loop (Weeks 2-3)

One end-to-end flow that proves the architecture.

- **Executive Agent** (LangGraph) — the orchestrator, only framework needed
- **Sales Intelligence Agent** — detects anomalies in sales data (simple statistical detection first, not ML)
- **MCP server for PostgreSQL** — agents query sales data through it
- **Memory layer** — Redis for short-term conversation state, Qdrant for long-term context
- **Minimal dashboard** — ask a question, see the agent chain work, get an answer

Demo target: *"Why did product X sales drop?"* → agents investigate → structured diagnosis appears.

## Phase 2 — Add Depth (Weeks 4-6)

Expand along two axes simultaneously.

### Person A — More agents

- Pricing Agent (rule-based first, RL later)
- Supply Chain Agent (stock analysis, reorder alerts)
- Inter-agent communication through the Executive Agent

### Person B — RAG + Data layer

- Hybrid RAG pipeline (BM25 + embeddings + reranking)
- Ingest product catalogs, customer reviews, support tickets into Qdrant
- GraphRAG with Neo4j (add it here, not before — now there's a real use case)

## Phase 3 — ML Layer (Weeks 7-9)

Only after agents work reliably.

- **Demand forecasting** — Prophet or LightGBM (pick one)
- **Churn prediction** — XGBoost
- **MLflow** for tracking experiments and model registry
- Agents call trained models as tools

Skip RL pricing, TFT, Two Tower, and Apriori unless there is time. One well-trained forecasting model beats five half-baked ones.

## Phase 4 — Polish (Week 10+)

- Dashboard with real-time agent activity visualization
- LangSmith/LangFuse tracing (easy to add, high demo impact)
- Airflow for scheduled retraining
- Architecture write-up for portfolio

## Key Principles

1. **One agentic framework.** LangGraph. Drop CrewAI and AutoGen.
2. **Earn each technology.** Don't add Kafka until there's a real streaming need. Don't add Neo4j until building GraphRAG. Don't add K8s until deploying somewhere.
3. **Real data or nothing.** Agents that analyze synthetic noise aren't impressive. Spend time on good seed data.
4. **Demo-driven development.** At the end of every phase, there should be a working interaction to show — not just code.
5. **Split by capability, not by layer.** "I do agents, you do frontend" creates integration bottlenecks. Better: "I do Sales + Pricing agents end-to-end, you do RAG + Supply Chain end-to-end."
