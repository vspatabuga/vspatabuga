# 🛠️ Prototype: Unified Sovereign AI Governance Stack

## 📌 Project Overview
This prototype demonstrates a fully self-hosted, private, and auditable AI orchestration environment. It bridges the gap between raw local intelligence and enterprise-grade governance by unifying orchestration, context management, and observability into a single sovereign perimeter.

The goal is to provide a "Private Intelligence Center" where data never leaves the controlled infrastructure, and every AI decision is traced and evaluated for compliance and accuracy.

---

## 🏗️ System Architecture

```mermaid
graph TD
    subgraph "Secure Access Layer (Zero-Trust)"
        User((User/Admin)) -->|Tailscale/Cloudflare| Gateway[Reverse Proxy / Ingress]
    end

    subgraph "Orchestration & Automation Layer"
        Gateway --> OpenClaw[OpenClaw Orchestrator]
        n8n[n8n Automation] <-->|Webhooks/API| OpenClaw
    end

    subgraph "Intelligence & Memory Layer"
        OpenClaw -->|Query| LlamaIndex[LlamaIndex RAG Engine]
        LlamaIndex -->|Fetch| VectorDB[(Private Vector Store)]
        OpenClaw -->|Inference| Ollama[Ollama Local LLM]
    end

    subgraph "Governance & Observability Layer"
        OpenClaw -.->|Tracing/Logs| Phoenix[Arize Phoenix]
        LlamaIndex -.->|Eval| Phoenix
        Phoenix -->|Insights| User
    end

    subgraph "Infrastructure"
        Ollama --- GPU[GPU/NPU Resources]
        OpenClaw --- Docker[Docker Engine]
    end
```

---

## 🚀 Key Components & Logic

### 1. The Brain: OpenClaw
Acts as the central nervous system. It manages agentic workflows, deciding when to search the local knowledge base or trigger an external automation. It ensures that LLM interactions follow predefined safety and logic bounds.

### 2. The Engine: Ollama
Provides the raw inference power using local models like **DeepSeek-R1** or **Qwen-2.5**. By running Ollama within the same network, we eliminate latency and data privacy risks associated with third-party APIs.

### 3. The Memory: LlamaIndex
Provides "Context Sovereignty." It indexes private documents (PDFs, Markdown, Wikis) into a local vector database. When a query is made, LlamaIndex injects only the relevant private context into the prompt, ensuring the LLM remains grounded in factual, private data.

### 4. The Auditor: Arize Phoenix
This is the "Governance" anchor. It records every trace, span, and retrieval step. 
- **Audit:** Who asked what, and what context was retrieved?
- **Evaluation:** Did the LLM hallucinate? Was the retrieved context relevant?
- **Sovereignty:** Unlike SaaS alternatives, Phoenix runs locally, keeping the audit trail private.

### 5. The Connector: n8n
Handles the "External World" integration. It acts as the sensor and actuator, pulling data from GitHub, SQL databases, or internal APIs and feeding it into the OpenClaw orchestration loop.

---

## 🛡️ Governance Workflow

1.  **Request:** A user or an n8n trigger sends a request to OpenClaw.
2.  **Context Retrieval:** OpenClaw asks LlamaIndex for relevant private data.
3.  **Inference:** OpenClaw sends the prompt + context to Ollama.
4.  **Tracing:** Throughout the process, Arize Phoenix captures the metadata (Prompts, Token usage, Retrieval accuracy).
5.  **Validation:** Arize Phoenix runs automated Evals to ensure the response is safe and accurate before it is delivered.
6.  **Response:** The final, audited answer is sent back to the user/trigger.

---

## 🛠️ Tech Stack Employed
- **Orchestration:** OpenClaw
- **Automation:** n8n
- **RAG Framework:** LlamaIndex
- **Inference Engine:** Ollama (Local Models)
- **Observability:** Arize Phoenix
- **Infrastructure:** Docker, Terraform, Tailscale
- **Security:** Vaultwarden (Secrets Management)
