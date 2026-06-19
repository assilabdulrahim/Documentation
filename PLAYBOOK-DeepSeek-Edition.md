# Project Lifecycle Playbook v1.0 — DeepSeek Edition
## AI-Guided Structured Project Kickoff (Optimized for Reasoning + Self-Hosting)

**Model:** DeepSeek (applies to current DeepSeek reasoning and chat models) — strong reasoning, excellent code, MIT-licensed, self-hostable, cost-effective
**Philosophy:** Exploit DeepSeek's reasoning strength for architecture and complex logic — it is the best non-Claude option here for tradeoff analysis. Use reasoning mode for hard thinking, chat mode for routine code. Self-host for privacy.

> **Version note:** Written to apply across recent DeepSeek models. Use the reasoning model (chain-of-thought) for Phases 2 and 4; use the faster chat model for routine code generation in Phase 5 to avoid over-thinking simple tasks.

---

## How to Use This Playbook with DeepSeek

1. **Upload this entire document to DeepSeek (hosted API or self-hosted).**
2. **Tell it:** "Start Phase 1 of the Project Lifecycle Playbook (DeepSeek Edition). Ask me the intake questions one at a time."
3. **Answer each question fully.**
4. **It will proceed through Phases 2–6** as described below.
5. **At each phase gate, review the output before approving.**

---

## Phase 1: Project Intake (30 minutes)

Ask these questions in order. Do not skip any.

### 1.1 Project Identity
- **Project name?** (short, memorable)
- **One-sentence purpose?** (what does it do, for whom)
- **Success criteria?** (three bullet points)
- **Stakeholders?** (solo, team, external?)
- **Timeline?** (deadline or open-ended?)

### 1.2 Deployment Context
- **Where does it run?** (laptop, VPS, Docker, cloud, mobile, hybrid?)
- **How many users?** (solo, team, public?)
- **Internet access?** (always, occasionally, air-gapped?)
- **Data residency?** (local, cloud, hybrid, regulatory?)

> **DeepSeek advantage:** If privacy or air-gapping matters, DeepSeek is MIT-licensed and self-hostable — you can run the entire planning + build loop with zero data leaving your infrastructure.

### 1.3 Tech Stack Inventory
- **Language(s)?**
- **Framework?**
- **Databases?**
- **Deployment?** (Docker, K8s, serverless, binary?)
- **External services?** (APIs, LLMs, auth, payment?)
- **Why these choices?** (expertise? skill? performance? cost?)

### 1.4 Known Constraints & Risks
- **Hard constraints?** (deadlines, budget, regulatory, locked architecture?)
- **Architectural constraints?** (solo dev, minimal deps, scale?)
- **Team constraints?** (learning curve, domain expertise?)
- **What scares you most?** (what breaks the project?)

---

## Phase 2: Spike Identification (15 minutes) — use reasoning mode

DeepSeek's reasoning model analyzes your tech stack and flags risky assumptions needing proof-of-concept. This is a strength: it reasons through failure modes thoroughly.

### 2.1 Spike Analysis
For each tech choice, DeepSeek reasons:
- Is this proven in your use case?
- What's the failure mode, traced step by step?
- What's the minimal test that proves or disproves it?

### 2.2 Spike Execution
DeepSeek will:
1. List the spikes with reasoning for why each matters
2. Propose tests for each
3. Write spike code (excellent code quality)
4. Report: pass or fail, and lessons learned

### 2.3 Gate
You review spike results. Approve or pivot. DeepSeek awaits approval.

---

## Phase 3: Requirements Elicitation (45 minutes)

DeepSeek asks targeted questions to build a **complete, unambiguous SRS**.

### 3.1 Functional Requirements
For each feature:
- **What inputs?** (user actions, data, events?)
- **What outputs?** (results, side effects?)
- **Who accesses what?** (permissions, isolation?)
- **Failure handling?** (graceful, retry, notify?)
- **Persistence?** (what survives restarts?)

### 3.2 Non-Functional Requirements
- **Performance:** latency targets? Throughput?
- **Scale:** concurrent users? Data volume?
- **Reliability:** uptime SLA? Recovery time?
- **Privacy/Security:** sensitivity? Compliance? Encryption?
- **Maintainability:** solo or team? Long-term?
- **Cost:** monthly budget? (DeepSeek is low-cost hosted, or free self-hosted.)

### 3.3 Gap Detection
DeepSeek's reasoning excels here — it traces logical gaps, contradictions, and unhandled cases methodically. It asks: "Is [gap] in V1? If yes, describe. If no, move to V2."

> **Watch for over-thinking:** The reasoning model may over-analyze simple requirements. If it spirals, tell it "this one is simple — give the short answer."

### 3.4 Output
SRS with ~50 functional requirements, ~8 non-functional, clear V1/V2 boundaries, acceptance criteria.

---

## Phase 4: Architecture Design (1–2 hours) — DeepSeek's sweet spot

> **Trust DeepSeek more here than most non-Claude models.** Its reasoning model is genuinely strong at architecture tradeoff analysis — comparing monolith vs. microservices, evaluating failure modes, reasoning about scaling bottlenecks. Use reasoning mode and let it think.

DeepSeek reasons through:
- **Monolith or services?** (with explicit tradeoff analysis)
- **Data flow:** reads/writes?
- **Failure modes:** traced step by step, with recovery strategy
- **Deployment:** machine to production?
- **Scaling:** what breaks first at 10x, and why?

### 4.1 Artifacts
- **Architecture description** (text — DeepSeek is text-only; provide your own draw.io, or have it generate Mermaid)
- **Data model** (SQL or document schema — DeepSeek's code quality is high here)
- **Decision log** (with genuine reasoning, not just assertions)
- **Open items** (things to resolve during build)

### 4.2 Standards (non-negotiable)
- Dependency injection
- Async/await for all I/O
- Standardized error envelope
- JSON structured logging
- Configuration externalized
- No `latest` image tags
- One source of truth per concern

### 4.3 Output
Architecture document with reasoned decision log, data schema, open items.

---

## Phase 5: Build Sequence & Prompts (1 hour) — use chat mode for routine code

DeepSeek designs the phased build and writes self-contained prompts. Switch to the faster chat model here for routine generation; reserve reasoning mode for genuinely hard implementation problems.

### 5.1 Phase Structure
- **Phase 0:** Environment (install, verify)
- **Phase 1–N:** Each builds one piece with a **Gate** (manual test)

Example gates:
- Phase 1: `/health` returns green
- Phase 2: OIDC login works
- Phase 3: data survives restart
- Phase N: all acceptance criteria pass

### 5.2 Prompt Discipline
Each prompt must:
- **Read existing code first**
- **Report severity-rated findings** (Critical/High/Medium/Low)
- **Await approval** on flagged items
- **Minimal changes** (no unrelated refactoring)
- **Reference SRS + Architecture** in every session

> **Context note:** DeepSeek's context window (128k) is smaller than Gemini/MiniMax. For large codebases, don't paste everything — paste the relevant files plus the SRS/Architecture sections for the current phase. Chunk strategically.

### 5.3 Output
- Phased build plan (Phase 0 → N with gates)
- One prompt per phase (~12 prompts)
- Self-contained prompts
- Estimated total build time

---

## Phase 6: Handoff (15 minutes)

DeepSeek delivers:
1. ✅ Complete SRS v1.0 (locked)
2. ✅ Architecture document with reasoned decision log
3. ✅ Data schema
4. ✅ Twelve build prompts
5. ✅ Phase 0 checklist
6. ✅ User manual skeleton
7. ✅ V2 requirements document

**Ready to build.**

---

## DeepSeek-Specific Guidance

### Strengths to Leverage
- **Reasoning:** best non-Claude option for architecture tradeoffs and complex logic — trust it in Phase 4
- **Code quality:** strong, clean implementations
- **Self-hostable (MIT):** run the whole loop air-gapped for privacy-sensitive work
- **Cost-effective:** cheap hosted, free self-hosted
- **Two modes:** reasoning for hard thinking, chat for fast routine code

### Weaknesses to Mitigate
- **128k context:** smaller than Gemini/MiniMax; chunk large codebases, don't paste everything
- **Over-thinking:** the reasoning model can spiral on simple tasks; tell it when to be terse, or switch to chat mode
- **Text-only:** no multimodal; describe diagrams in text or use Mermaid
- **Smaller ecosystem:** fewer turnkey integrations than OpenAI/Claude; verify tooling

### Workflow Pattern
1. **You:** Phase 1 intake (30 min) — flag privacy/air-gap needs
2. **DeepSeek (reasoning):** Phase 2 spikes (15 min + execution)
3. **You:** review spike results, approve or pivot
4. **DeepSeek (reasoning):** Phase 3 requirements (45 min)
5. **DeepSeek (reasoning):** Phase 4 architecture (1–2 hours) — its strongest phase
6. **DeepSeek (chat):** Phase 5 build prompts + routine code (1 hour)
7. **DeepSeek (chat + reasoning as needed):** Phases 0–N build (20–40 hours)

**Total planning + design:** 4–6 hours. **Build:** 20–40 hours.

---

## When to Use This Playbook with DeepSeek

- ✅ Privacy-sensitive or air-gapped (self-host it)
- ✅ Architecture-heavy projects needing real tradeoff reasoning
- ✅ Complex logic / algorithmic work
- ✅ Cost-sensitive (cheap hosted or free self-hosted)

When NOT:
- ❌ Very large codebase that must fit entirely in context (use Gemini/MiniMax)
- ❌ Multimodal input is central
- ❌ Quick 2-hour spike

---

## Quick Start

**Tell DeepSeek:**

> Start Phase 1 of the Project Lifecycle Playbook (DeepSeek Edition). Ask me the intake questions one at a time, waiting for my full answer before the next. Use reasoning mode for Phases 2 and 4 (spikes and architecture), and the faster chat mode for routine code in Phase 5. When Phase 1 is complete, summarize and confirm before Phase 2. If a task is simple, give the short answer rather than over-analyzing.

Answer each question fully. Lean on DeepSeek for architecture reasoning. Chunk context for large codebases. Review outputs at every gate.

**Ready?** Upload this document to DeepSeek and begin.
