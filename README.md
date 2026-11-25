# Rounak Kumar Sah — AI & Full-Stack Engineer

Production AI systems, stateful multi-agent platforms, observability tooling, and full-stack web applications built with Python, FastAPI, Next.js 15, TypeScript, LangGraph, OpenTelemetry, Qdrant, PostgreSQL, Neo4j, and Docker.

### Technical Focus & Core Stack

- **Agentic AI, RAG & Observability**: LangGraph (`StateGraph`, `interrupt()`), Model Context Protocol (MCP), OpenTelemetry (`gen_ai.*` OTLP), RAG Pipelines, Tool Calling, OpenAI, Google Gemini, Anthropic Claude, Pydantic v2, PyMuPDF, PaddleOCR
- **Backend & APIs**: Python 3.12 (`asyncio`), FastAPI, REST, WebSockets, Server-Sent Events (SSE), JWT Auth & RBAC, Ephemeral Docker Sandboxing
- **Databases & Search**: PostgreSQL (`SQLAlchemy 2.0`, `asyncpg`, `Alembic`), SQLite WAL (`aiosqlite`), Qdrant (Vector Search), Redis (Pub/Sub & Caching), Neo4j (AST Knowledge Graphs), Supabase
- **Frontend, Testing & DevSecOps**: Next.js 15, React 19, TypeScript, Tailwind CSS, Zustand, Docker (Multi-stage), GitHub Actions CI/CD, `pytest`, `Vitest`, CodeQL, Semgrep, Trivy, Bandit, n8n

---

### Featured Production Projects

| Project & Repository | System Architecture | Core Tech Stack | Code-Verified Engineering Proof |
| :--- | :--- | :--- | :--- |
| **[ASEP — Autonomous Software Engineering Platform](https://github.com/rounakkumarsah/ASEP)** | 32-node LangGraph multi-agent runtime across 33 FastAPI routers with bounded `critic ↔ debugger` self-healing loops, HITL pause/resume, and an in-browser Next.js 15 IDE. | Python, FastAPI, Next.js 15, React 19, LangGraph, PostgreSQL, Redis, Qdrant, Neo4j, Docker | **706 passing tests** (555 `pytest`, 151 `Vitest`), **7 CI security gates** (CodeQL, Semgrep, Trivy, Bandit, Cosign), ephemeral Docker sandbox (`network_mode="none"`, `cap_drop=["ALL"]`, 50MB RAM / 50 PID limits). |
| **[TraceAgent — OTel GenAI Flight Recorder & Replay Debugger](https://github.com/rounakkumarsah/traceagent)** | Local-first OpenTelemetry (`gen_ai.*`) flight recorder, transparent MCP stdio proxy (`traceagent wrap`), SHA-256 tamper-evident SQLite WAL ledger, and bundled Next.js 15 static flamegraph UI. | Python 3.12, FastAPI, `aiosqlite`, Next.js 15 (Static Export), TypeScript, OpenTelemetry, MCP | **22 passing tests**, 1,000-step SHA-256 chain verify in **`9.17 ms`**, Mode-1 `$0.00` deterministic stub replay + Mode-2 live fork, CI regression gate (`traceagent check`), zero-Node wheel install. |
| **[Production-RAG-Agent](https://github.com/rounakkumarsah/Production-RAG-Agent)** | Crash-resilient multi-format RAG platform supporting 8 file formats (16 extensions) with PyMuPDF + dynamic PaddleOCR/Tesseract fallback and real-time SSE token/citation streaming. | Python, FastAPI, Next.js 15, LangGraph, Qdrant, PostgreSQL, Gemini, Docker | SHA-256 deduplication + deterministic **UUIDv5** Qdrant WAL checkpointing, `asyncio.Semaphore` + dynamic batch halving for HTTP 429 recovery, dual cosine cutoffs (`0.63` floor / `0.05` gap). |
| **[AI-Review-Management-System](https://github.com/rounakkumarsah/AI-Review-Management-System)** | Event-driven customer review automation pipeline performing sentiment classification, draft response generation, and negative-review escalation before publishing. | n8n, OpenAI LLMs, Google Business Profile API, Google Sheets API, Telegram Bot Webhooks | Real-time webhook ingestion with mandatory Telegram Human-in-the-Loop (HITL) approval routing for low-rating reviews. |

---

### Featured Code-Verified Engineering Decisions

#### 1. Non-Blocking HITL Governance & Ephemeral Execution Sandboxing (`ASEP`)
Executing LLM-generated code or destructive commands inside an async API backend creates two severe failure modes: Remote Code Execution (RCE) and blocked worker threads during human approval waits.
- **Defense-in-Depth Sandboxing**: Isolated code runs inside ephemeral Docker containers configured with `network_mode="none"`, `read_only=True` rootfs (`noexec,nosuid,nodev` tmpfs), `cap_drop=["ALL"]`, `50MB` RAM, `50` PIDs, and a 5-second hard watchdog.
- **Non-Blocking Interrupts**: High-risk MCP tool calls trigger LangGraph `interrupt()` and snapshot state via `AsyncPostgresSaver` into PostgreSQL, freeing the FastAPI event loop until an approval decision resumes the graph via `Command(resume=...)`.

#### 2. Tamper-Evident Audit Ledger & Honest Two-Mode Replay (`TraceAgent`)
Debugging multi-step agent failures repeatedly burns tokens and leaks prompts to external SaaS clouds.
- **Cryptographic Local Ledger**: Every OTLP `gen_ai.*` span and MCP stdio JSON-RPC tool call is persisted in SQLite WAL with a canonical-JSON SHA-256 hash chain (`traceagent verify` catches single-byte tampering in `<10 ms` across 1,000 steps).
- **Strict Replay Separation**: Mode 1 (`stub_replay`, `$0.00`) tests Python parsers/validators offline against recorded outputs, while Mode 2 (`live_fork`) replays prefix steps from cache and invokes a configured LLM only from Step N onward.

#### 3. Crash-Resilient Ingestion & Rate-Limit Recovery (`Production-RAG-Agent`)
Mid-upload crashes and provider HTTP `429` quotas break naive RAG pipelines and waste paid embedding calls.
- **Idempotent Vector Checkpointing**: Every chunk maps to a deterministic `UUIDv5` derived from `filename::page::section::heading::index::text`. Re-uploads query Qdrant's WAL via `get_existing_point_ids()` and skip already-indexed chunks.
- **Adaptive Backoff & Batch Halving**: Outbound Gemini embedding requests (3072-dim) are serialized via `asyncio.Semaphore`, parse exact gRPC `retry_delay` / HTTP `Retry-After` headers, and dynamically halve batch sizes (`5 → 2 → 1`) on rate limits instead of aborting ingestion.

---

### Full-Time Roles & Technical Sprints

- **Full-Time Engineering Roles**: Open to AI Engineer, Full-Stack AI Engineer, and Backend Python/FastAPI roles (**Bengaluru, Hyderabad, Pune, Chennai, Mumbai, or Global Remote**).
- **Fixed-Scope Technical Sprints ($1,200+ / Scope-Based)**:
  - **Production RAG Pipeline & Audit**: Crash-resilient multi-format ingestion, OCR fallback, Qdrant retrieval tuning, and streaming citation UIs.
  - **Governed MCP, Observability & Multi-Agent Systems**: Wrapping FastAPI/REST APIs into RBAC-enforced MCP tools, sandboxed execution, OTel/TraceAgent flight recording, and CI regression gates.
  - **Prototype-to-Production Backend Rescue**: Fixing async bottlenecks, Docker multi-stage hardening, automated `pytest` suites, and CI security gates.

---

### Connect & Inquiries

- **LinkedIn**: [linkedin.com/in/rounak-kumar-sah](https://www.linkedin.com/in/rounak-kumar-sah)
- **Email**: `rounakkrsah429@gmail.com`
