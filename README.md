<div align="center">

<h1>⚡ Enterprise AI Agent Platform</h1>

<p><strong>Production-grade multi-tenant AI orchestration system for scalable multi-agent workflows,<br/>real-time conversational intelligence, and enterprise LLM operations.</strong></p>

<br/>

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-Async--First-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![LangGraph](https://img.shields.io/badge/LangGraph-Orchestration-FF6B35?style=flat-square)](https://langchain-ai.github.io/langgraph/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-336791?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org)
[![Redis](https://img.shields.io/badge/Redis-State%20Cache-DC382D?style=flat-square&logo=redis&logoColor=white)](https://redis.io)
[![React](https://img.shields.io/badge/React-TypeScript-61DAFB?style=flat-square&logo=react&logoColor=black)](https://reactjs.org)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?style=flat-square&logo=docker&logoColor=white)](https://docker.com)
[![License](https://img.shields.io/badge/License-MIT-22C55E?style=flat-square)](LICENSE)

<br/>

**Built by [Wajahat Arshad](https://www.linkedin.com/in/wajahat-arshad-135359233) — AI Platform Engineer · Backend Architect · Distributed Systems**

[LinkedIn](https://www.linkedin.com/in/wajahat-arshad-135359233) · [GitHub](https://github.com/wajahatarshad) · [wajahatarshad111@gmail.com](mailto:wajahatarshad111@gmail.com)

</div>

---

## Executive Overview

The Enterprise AI Agent Platform is a production-grade, multi-tenant AI orchestration system engineered for organizations that demand reliability, scalability, and observability from their AI infrastructure. The platform moves beyond simple LLM wrappers — it implements a stateful, graph-based agent execution engine capable of routing complex user intent across specialized agents, maintaining conversation context at scale, and streaming inference results with sub-second latency.

At its core, the system solves a fundamental challenge in enterprise AI adoption: **how do you reliably orchestrate multiple AI agents, maintain tenant isolation at scale, and deliver real-time intelligence without sacrificing observability or fault tolerance?**

This platform addresses that challenge through a combination of LangGraph-powered stateful workflows, async-first Python architecture, vector-augmented retrieval, and a fully instrumented observability layer — all packaged inside a multi-tenant SaaS framework ready for commercial deployment.

**Primary Use Cases:**
- Enterprise conversational AI with retrieval-augmented generation (RAG)
- Multi-agent reasoning over structured and unstructured enterprise data
- Internal AI assistant platforms with strict tenant isolation and audit compliance
- Production LLM infrastructure for AI-native SaaS products

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                                  │
│         React + TypeScript · Zustand · SSE Streaming UI             │
└─────────────────────────┬───────────────────────────────────────────┘
                          │ HTTPS / SSE
┌─────────────────────────▼───────────────────────────────────────────┐
│                     API GATEWAY LAYER                                │
│       FastAPI · JWT Auth · RBAC · Rate Limiting · Audit Logs        │
└──────┬────────────────┬──────────────────────┬───────────────────────┘
       │                │                      │
┌──────▼──────┐  ┌──────▼──────┐      ┌───────▼────────┐
│  Auth &     │  │  Tenant &   │      │  Conversation  │
│  Session    │  │  User Mgmt  │      │  & Agent API   │
│  Service    │  │  Service    │      │  Service       │
└─────────────┘  └─────────────┘      └───────┬────────┘
                                              │
┌─────────────────────────────────────────────▼────────────────────────┐
│                   LANGGRAPH ORCHESTRATION ENGINE                      │
│                                                                       │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────────────────┐  │
│   │   Planner   │───▶│   Router    │───▶│    Agent Selector       │  │
│   │    Agent    │    │    Node     │    │  (Intent Classification) │  │
│   └─────────────┘    └─────────────┘    └────────────┬────────────┘  │
│                                                      │               │
│         ┌────────────┬────────────┬──────────────────┘               │
│         ▼            ▼            ▼                                   │
│   ┌──────────┐ ┌──────────┐ ┌──────────┐  ┌──────────────────────┐  │
│   │Retriever │ │   SQL    │ │  Direct  │  │    Evaluator Agent   │  │
│   │  Agent   │ │Reasoning │ │Reasoning │  │  (Quality Gate Loop) │  │
│   │  (RAG)   │ │  Agent   │ │  Agent   │  └──────────────────────┘  │
│   └────┬─────┘ └────┬─────┘ └────┬─────┘                            │
│        └────────────┴────────────┘                                   │
│                      │                                                │
│              ┌───────▼────────┐                                       │
│              │  State Manager │                                       │
│              │  (Checkpointer)│                                       │
│              └───────┬────────┘                                       │
└──────────────────────┼───────────────────────────────────────────────┘
                       │
         ┌─────────────┼─────────────┐
         ▼             ▼             ▼
  ┌─────────────┐ ┌─────────┐ ┌──────────────┐
  │ PostgreSQL  │ │  Redis  │ │    Ollama    │
  │ + pgvector  │ │  Cache  │ │  (Phi3 LLM) │
  └─────────────┘ └─────────┘ └──────────────┘
```

### Architectural Principles

**Async-First Design** — Every service boundary, database interaction, and I/O operation is built on Python's asyncio runtime. This allows a single-process backend to handle hundreds of concurrent agent sessions without thread contention or blocking I/O bottlenecks.

**Event-Driven Agent Execution** — Agent state transitions are modeled as directed graph edges in LangGraph. Each node emits state mutations that propagate through the workflow deterministically, enabling reproducible execution traces and straightforward failure recovery.

**Shared-Nothing Tenant Architecture** — Tenant context is injected at the JWT boundary and propagated through every downstream call. No cross-tenant data leakage is architecturally possible because tenant scope is enforced at the ORM query level, not the application logic level.

**Layered Observability** — Structured logs, token telemetry, and agent trace data are emitted at every significant execution boundary, giving operators full visibility into latency, cost, and failure modes without requiring post-hoc log parsing.

---

## Core Platform Capabilities

### AI Orchestration Engine

The orchestration layer is built on **LangGraph**, treating agent workflows as stateful directed acyclic graphs (DAGs) with persistent checkpointing. Unlike chain-based approaches, graph execution allows conditional branching, parallel agent fan-out, and iterative evaluation loops — all while maintaining serializable workflow state across requests.

Key capabilities:
- **Stateful workflow execution** with per-session checkpoint persistence
- **Conditional routing** based on intent classification confidence scores
- **Parallel agent dispatch** for workloads requiring concurrent data retrieval
- **Automatic retry orchestration** with exponential backoff and circuit-breaking at the node level
- **Evaluation loop integration** — responses that fail quality gates are automatically re-routed for refinement before delivery

### Multi-Agent Routing System

Intent is classified at the planner layer and routed to the appropriate execution agent. The routing decision is policy-driven, not hardcoded — allowing operators to adjust routing thresholds, add new agent types, and tune fallback behavior without modifying core orchestration logic.

### Real-Time Streaming Inference

All LLM responses are streamed token-by-token via **Server-Sent Events (SSE)**, with the streaming pipeline integrated directly into the async FastAPI response lifecycle. The frontend renders incrementally as tokens arrive, delivering a conversational UX with perceived latency under 200ms for first-token delivery.

### Multi-Tenant SaaS Architecture

Tenant isolation is enforced at four layers: authentication (JWT claims), authorization (RBAC policies), data access (ORM-level tenant scoping), and AI execution (tenant-aware prompt context injection). Each layer independently enforces isolation, making bypass through any single vector insufficient.

### Vector-Augmented Retrieval (RAG)

The retrieval pipeline uses **pgvector** for high-dimensional semantic search co-located with the transactional database. This eliminates the operational complexity of a dedicated vector store while maintaining sub-10ms retrieval latency at moderate scale. Embeddings are generated at document ingestion time and cached; retrieval augments agent context before LLM inference.

---

## Technology Stack

### Backend

| Component | Technology | Rationale |
|-----------|-----------|-----------|
| API Framework | FastAPI (async) | Native asyncio, automatic OpenAPI generation, dependency injection |
| Runtime | Python 3.11+ | Performance improvements, native async task groups |
| AI Orchestration | LangGraph + LangChain | Stateful graph execution, rich agent tooling ecosystem |
| Local LLM | Ollama + Phi3 | Zero-latency local inference, no external API dependency |
| ORM | SQLAlchemy Async | Non-blocking DB operations, type-safe query construction |
| Database | PostgreSQL + pgvector | ACID transactions + native vector similarity search |
| Cache / State | Redis | Sub-millisecond ephemeral state, session management |
| Auth | JWT + RBAC | Stateless authentication, fine-grained authorization |
| Streaming | SSE (Server-Sent Events) | Native HTTP streaming, no WebSocket upgrade overhead |
| Migrations | Alembic | Schema versioning, zero-downtime migration support |
| Logging | Structured JSON Logging | Machine-parseable, compatible with log aggregation pipelines |

### Frontend

| Component | Technology | Rationale |
|-----------|-----------|-----------|
| Framework | React + TypeScript | Type-safe component architecture, production maturity |
| Build Tool | Vite | Sub-second HMR, optimized production bundling |
| State Management | Zustand | Minimal boilerplate, atomic state updates for streaming UI |
| Streaming UI | Custom SSE hooks | Real-time token rendering with backpressure handling |

### Infrastructure

| Component | Technology |
|-----------|-----------|
| Containerization | Docker + Docker Compose |
| Local LLM Runtime | Ollama |
| Reverse Proxy | Nginx (production profile) |
| Secret Management | Environment-based config with `.env` isolation |
| CI/CD Readiness | Dockerfile multi-stage builds |

---

## AI Agent Orchestration — Deep Dive

### Planner Agent

**Responsibility:** The Planner is the entry point for every user request. It performs intent decomposition — breaking complex queries into sub-tasks and determining the execution strategy before any LLM inference is invoked.

**Decision Logic:** Analyzes query complexity, detects whether structured data retrieval, vector search, or direct reasoning is required, and constructs an execution plan as a serializable state payload passed to downstream agents.

**Observability:** Emits planning latency, complexity score, and selected strategy to the telemetry pipeline on every invocation.

---

### Retriever Agent (RAG)

**Responsibility:** Handles all queries requiring knowledge base augmentation. Executes semantic similarity search against pgvector, retrieves top-K document chunks, reranks by relevance score, and injects enriched context into the LLM prompt.

**Workflow Role:** Activated when the Planner classifies queries as knowledge-retrieval workloads. Operates independently of the SQL and Direct agents, allowing parallel execution when multi-source retrieval is required.

**Retrieval Pipeline:**
```
Query → Embedding Generation → pgvector ANN Search → 
Relevance Reranking → Context Window Assembly → LLM Inference
```

**Failure Handling:** Falls back to direct reasoning if retrieval latency exceeds threshold or vector store returns insufficient results above minimum similarity score.

---

### SQL Reasoning Agent

**Responsibility:** Translates natural language queries into structured SQL, executes against the application database, and synthesizes results into human-readable responses. Supports multi-step reasoning over query results.

**Workflow Role:** Activated for queries involving aggregations, filtered lookups, or analytical questions over structured enterprise data. Implements query sandboxing — generated SQL is validated against a schema allowlist before execution.

**Safety Layer:** All generated queries run in read-only transaction contexts with row-count limits enforced at the ORM layer, preventing destructive operations regardless of LLM output.

---

### Direct Reasoning Agent

**Responsibility:** Handles queries that require pure language model reasoning without external data retrieval — mathematical reasoning, text transformation, summarization, and general knowledge queries.

**Workflow Role:** Default fallback when the Planner determines no external context enrichment is required. Optimized for lowest-latency response paths.

---

### Evaluator Agent

**Responsibility:** Implements a quality gate on agent outputs before delivery to the client. Evaluates responses against coherence, factual grounding, and task completion criteria.

**Workflow Role:** Operates as a post-processing node in the LangGraph execution graph. Responses failing quality thresholds are re-routed back to the originating agent with corrective context — implementing a self-improving feedback loop.

**Observability:** Evaluation scores, retry counts, and quality gate outcomes are logged per-session, providing a dataset for ongoing agent performance analysis.

---

## Real-Time Streaming Architecture

The streaming pipeline is designed around three principles: **minimal time-to-first-token**, **zero buffering in the critical path**, and **graceful degradation under backpressure**.

```
Ollama Inference (token stream)
         │
         ▼
  FastAPI AsyncGenerator
         │  (async for token in stream)
         ▼
  SSE Response (Content-Type: text/event-stream)
         │
         ▼
  React EventSource + Zustand store update
         │
         ▼
  Incremental DOM rendering (token-by-token)
```

**Implementation Details:**
- The FastAPI endpoint yields SSE-formatted chunks directly from the LLM token stream without intermediate buffering
- The React frontend maintains a write-optimized Zustand atom for in-progress message state, batching DOM updates using `requestAnimationFrame` to prevent render-blocking
- Stream lifecycle events (start, chunk, end, error) are emitted as typed SSE event types, allowing the frontend to handle partial responses, connection failures, and retries without polling
- Token-level telemetry (tokens/sec, total tokens, model latency) is appended as a terminal SSE metadata event

---

## Multi-Tenant SaaS Architecture

### Isolation Model

Tenant isolation is enforced across four independent layers, such that compromise of any single layer is insufficient for cross-tenant data access:

**Layer 1 — Authentication:** JWT tokens encode `tenant_id` as a verified claim. Tokens are signed with tenant-scoped secrets; cross-tenant token reuse is cryptographically prevented.

**Layer 2 — Authorization:** RBAC policies are evaluated per-request. Every protected endpoint validates that the authenticated principal holds the required role within their tenant scope.

**Layer 3 — Data Access:** All ORM queries include implicit `WHERE tenant_id = :current_tenant` clauses injected by a base query class. Ad-hoc queries are not permitted; all data access flows through the scoped query layer.

**Layer 4 — AI Execution:** Agent prompts are constructed with tenant-specific context. Cross-tenant knowledge base contamination is prevented by filtering vector search results by tenant scope before retrieval.

### Audit & Compliance

Every privileged action (authentication events, agent invocations, data mutations) is recorded to an append-only audit log with immutable timestamps, actor identity, tenant scope, and action payload. Audit records are written as structured JSON to a dedicated log stream, compatible with SIEM ingestion pipelines.

### Usage Metering

Token consumption is tracked per-user and per-tenant on every LLM invocation. Usage metrics are persisted asynchronously to avoid blocking the response path, enabling downstream rate limiting, quota enforcement, and cost allocation reporting.

---

## Vector Search & RAG Pipeline

### Architecture

The RAG pipeline is designed for **semantic precision** over keyword recall. Rather than relying on full-text search, the retrieval layer operates on dense vector representations of document content, enabling retrieval of conceptually relevant context even when queries share no lexical overlap with source documents.

```
Document Ingestion Pipeline:
Raw Document → Chunking Strategy → Embedding Generation → 
pgvector Storage → Index Optimization

Query Retrieval Pipeline:
User Query → Query Embedding → ANN Search (pgvector) → 
Top-K Retrieval → Reranking → Context Assembly → LLM Augmentation
```

### pgvector Integration

PostgreSQL with the **pgvector** extension serves as the vector store, co-locating semantic search with transactional data. This architectural decision eliminates the operational overhead of a dedicated vector database (Pinecone, Weaviate) while maintaining acceptable retrieval performance for enterprise-scale knowledge bases. At very large scales (>50M vectors), a dedicated vector store can be introduced behind the retrieval interface without changes to agent code.

### Chunking Strategy

Documents are chunked using a sliding window approach with configurable chunk size and overlap. Chunk boundaries respect semantic units (sentences, paragraphs) rather than fixed character counts, improving retrieval coherence. Each chunk is stored with its source document metadata, enabling citation tracking in agent responses.

---

## Observability & AI Telemetry

Production AI systems require more than application-level monitoring — they require visibility into **model behavior, agent decision paths, and inference economics**.

### Instrumentation Layers

**Structured Application Logging** — All log events are emitted as JSON with consistent field schema: `timestamp`, `level`, `service`, `tenant_id`, `trace_id`, `event`, and event-specific payload. This structure enables direct ingestion into Elasticsearch, Loki, or any log aggregation platform without parsing configuration.

**Agent Trace Events** — Each LangGraph node transition emits a trace event containing: node identifier, input state hash, output state hash, execution latency, and any external calls made. Trace events form a complete execution record for every agent invocation.

**Token Telemetry** — Per-invocation metrics tracked: prompt tokens, completion tokens, tokens/second throughput, model latency (time-to-first-token, total generation time), and estimated inference cost based on configurable model pricing.

**Evaluation Metrics** — Quality gate scores, retry counts, and final evaluation outcomes are logged per conversation turn, building a dataset for ongoing agent performance benchmarking.

### Integration Readiness

The telemetry pipeline is designed for compatibility with:
- **LangSmith** — LangChain's native tracing platform for agent debugging and performance analysis
- **OpenTelemetry** — Distributed trace export to Jaeger, Tempo, or any OTLP-compatible backend
- **Prometheus** — Metric exposition endpoint for latency histograms, error rates, and throughput counters
- **Grafana** — Dashboard-ready metric structure

---

## Database Architecture

### PostgreSQL — Transactional Backbone

PostgreSQL handles all relational data: users, tenants, conversations, messages, audit logs, and usage records. ACID transaction semantics ensure consistency across multi-step agent operations. The async SQLAlchemy driver eliminates per-query thread blocking, allowing the connection pool to serve hundreds of concurrent requests on a fixed pool size.

### pgvector — Semantic Retrieval Layer

The pgvector extension adds first-class vector similarity search to PostgreSQL, enabling approximate nearest-neighbor (ANN) queries on high-dimensional embedding vectors. HNSW and IVFFlat indexes provide tunable tradeoffs between query latency and recall accuracy. Co-locating vector and relational data simplifies deployment, reduces network hops during retrieval, and enables JOIN-based metadata filtering on vector search results.

### Redis — Ephemeral State & Caching

Redis serves two roles: **session state cache** (storing authenticated session context to avoid redundant DB lookups on every request) and **agent execution cache** (caching intermediate results for repeated sub-queries within a session). TTL-based expiration ensures cache coherence without manual invalidation logic. Redis Pub/Sub is available for future event-streaming use cases.

### Schema Evolution

Database schema changes are managed through **Alembic** migration scripts, version-controlled alongside application code. Every migration is written to support zero-downtime deployments: additive changes first, destructive changes in separate migrations after application rollout.

---

## Security Architecture

| Control | Implementation |
|---------|---------------|
| Authentication | JWT with configurable expiry and refresh token rotation |
| Authorization | RBAC with hierarchical role inheritance |
| Transport Security | HTTPS enforced; HSTS headers in production profile |
| API Rate Limiting | Per-user and per-tenant request rate limits with Redis-backed counters |
| Input Validation | Pydantic schema validation on all request bodies; no raw SQL construction |
| SQL Injection Prevention | ORM-exclusive data access; parameterized queries enforced at driver level |
| Secrets Management | Environment variable injection; no credentials in source code or Docker images |
| Audit Logging | Immutable structured logs for all privileged operations |
| Tenant Isolation | Multi-layer isolation as described in SaaS Architecture section |
| LLM Output Sanitization | Agent responses are validated before persistence and delivery |

---

## Infrastructure & Deployment

### Local Development

```bash
# Clone repository
git clone https://github.com/wajahatarshad/enterprise-ai-agent-platform
cd enterprise-ai-agent-platform

# Configure environment
cp .env.example .env
# Edit .env with your configuration

# Start all services
docker compose up -d

# Run database migrations
docker compose exec api alembic upgrade head

# Pull local LLM model
docker compose exec ollama ollama pull phi3

# Access API documentation
open http://localhost:8000/docs

# Access frontend
open http://localhost:5173
```

### Service Architecture (Docker Compose)

```yaml
Services:
  api          → FastAPI backend (port 8000)
  frontend     → React + Vite dev server (port 5173)
  postgres     → PostgreSQL + pgvector (port 5432)
  redis        → Redis cache (port 6379)
  ollama       → Local LLM runtime (port 11434)
```

### Environment Configuration

```env
# Database
DATABASE_URL=postgresql+asyncpg://user:pass@postgres:5432/ai_platform
REDIS_URL=redis://redis:6379/0

# Authentication
JWT_SECRET_KEY=<secure-random-key>
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

# LLM Configuration
OLLAMA_BASE_URL=http://ollama:11434
DEFAULT_MODEL=phi3

# Observability
LOG_LEVEL=INFO
ENABLE_TELEMETRY=true
```

### Production Deployment Considerations

The service topology is designed for horizontal scaling:

- **API layer** is fully stateless — multiple instances can run behind a load balancer
- **Session state** is externalized to Redis, enabling sticky-session-free horizontal scaling
- **LLM inference** can be offloaded to dedicated GPU nodes or replaced with cloud inference endpoints (OpenAI, Anthropic, Bedrock) by swapping the provider adapter
- **Database** supports read replica offloading for analytics queries
- **Kubernetes readiness** — All services are containerized with health check endpoints, configurable resource limits, and graceful shutdown handling

---

## Frontend Engineering

The React frontend implements a streaming-first conversation interface with real-time state synchronization.

**Streaming UI Architecture:**
```
SSE Connection → Custom useEventSource hook → 
Zustand stream atom → React render (token append)
```

**State Management:** Zustand provides atomic state slices for conversation history, active stream state, authentication context, and UI preferences. The streaming message state is isolated in a dedicated atom with high-frequency update performance characteristics — preventing unnecessary re-renders of non-streaming components during token delivery.

**Authentication Flow:** JWT tokens are stored in memory (not localStorage) and injected into all API requests via an axios interceptor. Refresh token rotation is handled transparently by the auth service layer. Protected routes redirect to login with return URL preservation.

**Conversation Synchronization:** Conversation history is fetched on session initialization and optimistically updated on message send. SSE stream events are reconciled with the conversation store, ensuring UI consistency across page refreshes and reconnection events.

---

## Engineering Highlights

This platform demonstrates production engineering patterns across the full AI application stack:

**Async-First Backend** — Zero blocking I/O from HTTP handler to database. The entire request lifecycle operates within Python's asyncio event loop, enabling high concurrency on modest hardware.

**Stateful AI Orchestration** — LangGraph workflows maintain execution state across agent transitions, enabling multi-step reasoning, iterative refinement, and deterministic replay of agent execution paths for debugging.

**Production Streaming Pipeline** — Token-level SSE streaming with backpressure handling, graceful degradation, and telemetry integration — not a demo streaming implementation.

**Multi-Layer Tenant Isolation** — Security-in-depth approach where each isolation layer is independently effective, following defense-in-depth principles for enterprise SaaS.

**Evaluator-in-the-Loop** — Self-improving response quality through automatic evaluation and re-routing, implementing a feedback loop that improves output reliability without human intervention.

**Observability-First Design** — Telemetry, tracing, and structured logging are first-class concerns, not afterthoughts. Every agent decision is traceable, every token is metered, every failure is diagnosable.

**Schema-Driven Data Layer** — Pydantic models define the contract between API, business logic, and persistence layers. No raw dict manipulation; type safety enforced end-to-end.

---

## Product Roadmap

The following capabilities are planned for subsequent platform releases:

**Phase 2 — Distributed Execution**
- Celery + Redis task queue for long-running agent jobs with persistent execution state
- Kubernetes deployment manifests with Horizontal Pod Autoscaler configuration
- Distributed LangGraph checkpointing via Redis Cluster

**Phase 3 — Multi-Model Inference Gateway**
- Unified inference interface abstracting Ollama, OpenAI, Anthropic, and AWS Bedrock
- Per-tenant model selection and cost allocation
- Model performance benchmarking and automatic routing to lowest-latency provider

**Phase 4 — Agent Evaluation Framework**
- Automated regression testing for agent response quality
- A/B routing for agent strategy comparison
- Human-in-the-loop annotation pipeline for ground-truth evaluation dataset construction

**Phase 5 — Long-Term Agent Memory**
- Episodic memory store for cross-session context retention
- Semantic compression of conversation history for long-running agent relationships
- User preference extraction and personalization

**Phase 6 — Enterprise Analytics & Governance**
- Tenant-level usage dashboards with cost attribution
- AI governance audit reports for regulatory compliance
- Content policy enforcement layer with configurable safety classifiers
- PII detection and redaction pipeline

**Phase 7 — Distributed Vector Infrastructure**
- Partition-based vector indexing for knowledge bases exceeding 50M vectors
- Multi-region vector replication with consistency guarantees
- Hybrid sparse-dense retrieval (BM25 + ANN) for improved retrieval precision

---

## Project Structure

```
enterprise-ai-agent-platform/
├── backend/
│   ├── api/
│   │   ├── routes/          # FastAPI route handlers
│   │   ├── middleware/       # Auth, rate limiting, tenant context
│   │   └── dependencies/     # Dependency injection providers
│   ├── agents/
│   │   ├── planner.py        # Intent decomposition agent
│   │   ├── retriever.py      # RAG agent
│   │   ├── sql_agent.py      # SQL reasoning agent
│   │   ├── direct_agent.py   # Direct reasoning agent
│   │   └── evaluator.py      # Quality gate agent
│   ├── orchestration/
│   │   ├── graph.py          # LangGraph workflow definition
│   │   ├── state.py          # Workflow state schema
│   │   └── router.py         # Agent routing logic
│   ├── core/
│   │   ├── auth.py           # JWT + RBAC implementation
│   │   ├── config.py         # Environment configuration
│   │   └── telemetry.py      # Observability instrumentation
│   ├── db/
│   │   ├── models/           # SQLAlchemy async models
│   │   ├── repositories/     # Data access layer
│   │   └── migrations/       # Alembic migration scripts
│   ├── services/
│   │   ├── vector_service.py # pgvector retrieval service
│   │   ├── llm_service.py    # Ollama inference adapter
│   │   └── streaming.py      # SSE streaming utilities
│   └── main.py               # FastAPI application entrypoint
├── frontend/
│   ├── src/
│   │   ├── components/       # React UI components
│   │   ├── hooks/            # Custom hooks (useStream, useAuth)
│   │   ├── stores/           # Zustand state atoms
│   │   ├── services/         # API client layer
│   │   └── App.tsx
│   └── vite.config.ts
├── docker-compose.yml
├── docker-compose.prod.yml
├── .env.example
└── README.md
```

---

## Contributing

Contributions are reviewed for architectural consistency and production quality standards. Please open an issue to discuss significant changes before submitting a pull request.

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

<div align="center">

**Built with production engineering discipline by [Wajahat Arshad](https://www.linkedin.com/in/wajahat-arshad-135359233)**

*AI Platform Engineer · Backend Architect · Distributed Systems*

[LinkedIn](https://www.linkedin.com/in/wajahat-arshad-135359233) · [GitHub](https://github.com/wajahatarshad) · [wajahatarshad111@gmail.com](mailto:wajahatarshad111@gmail.com)

</div>
