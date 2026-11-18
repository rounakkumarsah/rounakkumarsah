# Rounak Kumar Sah — AI & Full-Stack Systems Engineer

Production-grade AI systems, multi-agent orchestration frameworks, and scalable web applications built with Python, FastAPI, Next.js, LangGraph, Qdrant, PostgreSQL, and Docker.

### Technical Focus & Core Stack

- **Backend & Systems**: Python (3.12+), FastAPI, Asyncio, Pydantic v2, PostgreSQL, Redis, Docker, Docker Compose
- **AI Architecture & Retrieval**: LangGraph, Qdrant (Vector & Hybrid Search), Neo4j, Model Context Protocol (MCP), RAG Evaluation
- **Frontend & Web**: Next.js (App Router), TypeScript, Tailwind CSS, React Server Components

---

### Featured Production Projects

| Project & Repository | System Architecture | Core Tech Stack | Engineering Verification & Focus |
| :--- | :--- | :--- | :--- |
| **[ASEP — Autonomous Software Engineering Platform](https://github.com/rounakkumarsah/ASEP)** | Multi-agent state orchestration engine handling long-running software workflows, state persistence, dynamic tool routing, and sandboxed execution. | Python, FastAPI, LangGraph, PostgreSQL, Redis, Neo4j, Qdrant, Ollama | Stateful agent DAGs, graph + vector memory, containerized tool execution. |
| **[Production-RAG-Agent](https://github.com/rounakkumarsah/Production-RAG-Agent)** | Dockerized multi-format RAG pipeline featuring semantic chunking, deterministic chunk IDs, duplicate detection, and citation-grounded retrieval. | Python, FastAPI, Next.js, LangGraph, Qdrant, PostgreSQL, Docker | Exponential-backoff rate-limit recovery, resumable indexing, similarity-threshold filtering. |

---

### Featured System Design & Engineering Decisions

#### 1. Hybrid Vector Retrieval Architecture (Dense + Sparse Fusion)
When constructing production Retrieval-Augmented Generation (RAG) systems, relying solely on dense vector embeddings often leads to low precision on exact-keyword domain queries (e.g., error codes, legal identifiers, specific API symbols).

To resolve this limitation, hybrid retrieval combines dense semantic vectors (Qdrant) with sparse lexical tokens (BM25) fused via Reciprocal Rank Fusion (RRF):

$$RRF\_Score(d \in D) = \sum_{m \in M} \frac{1}{k + r_m(d)}$$

Where $M$ represents the set of retrieval systems (dense and sparse), and $r_m(d)$ is the rank of document $d$ in system $m$. Using $k=60$ dampens outlier rankings and produces a balanced context set before cross-encoder reranking.

#### 2. Governed Model Context Protocol (MCP) Tool Integration
Exposing internal APIs directly to LLM agents without strict governance creates prompt-injection and data-exfiltration vectors. Production MCP servers built with FastAPI/FastMCP enforce:
- **Context Schema Minimization**: Stripping redundant OpenAPI metadata and loading tool schemas on demand to prevent context-window bloat.
- **Stateless Transport & Auth**: Enforcing OAuth 2.1 token validation and fine-grained read/write separation over Streamable HTTP / SSE transports.
- **Deterministic Tool Contracts**: Validating every tool payload with strict Pydantic v2 models and human-in-the-loop (HITL) approval gates for mutating actions.

---

### Technical Sprints & Roles

- **Full-Time Engineering Roles**: Open to AI Engineer, Backend Python/FastAPI, and Full-Stack AI Engineer roles (Bengaluru, Hyderabad, or Remote).
- **Fixed-Scope Technical Sprints ($1,200+ / Scope-Based)**:
  - **Production RAG Pipeline & Audit**: Hybrid retrieval (Qdrant + BM25), chunking optimization, and automated RAG evaluation setup.
  - **MCP Server & Agent Orchestration**: Wrapping existing FastAPI/REST services into governed, schema-validated MCP tools and stateful LangGraph workflows.
  - **Prototype-to-Production SaaS Rescue**: Fixing auth, webhook idempotency, Docker deployment, and automated API contract testing (`pytest` / `Schemathesis`).

---

### Connect & Inquiries

- **LinkedIn**: [linkedin.com/in/rounak-kumar-sah](https://www.linkedin.com/in/rounak-kumar-sah)
- **Email**: `rounakkrsah429@gmail.com`
