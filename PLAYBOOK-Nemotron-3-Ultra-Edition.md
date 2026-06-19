# Project Lifecycle Playbook v1.0 — Nemotron 3 Ultra Edition
## AI-Guided Structured Project Kickoff (Optimized for Structured Reasoning + Speed)

**Model:** NVIDIA Nemotron 3 Ultra — 550B MoE (55B active), 1M context, hybrid Mamba-Transformer, NVFP4/Blackwell-optimized, open (OpenMDW-1.1)
**Philosophy:** Nemotron rewards explicit structure and punishes vagueness — which makes it the most natural match for this playbook's disciplined, gated approach. Its edge is speed at strong-reasoning quality: use it for fast, structured, long-running agent work.

> **Key behavioral note (from NVIDIA's own guidance):** Ultra needs structure. Do **not** treat it as a free-form chat model and hope it infers the workflow. Every prompt should state the goal, the constraints, the expected output shape, and the gate. This playbook already does that — Nemotron is built for exactly this style.
>
> **Deployment note (honest):** At ~550B params it's roughly ~275GB even at FP4 — it will **not** fit on a single 128GB GB10. Its NVFP4 quantization is optimized for Blackwell Tensor Cores, so on *larger* NVIDIA hardware it self-hosts beautifully; on your workstation you'd reach it via API (build.nvidia.com / OpenCode). Fine for build-time assistance; not a local embedded runtime for a single GB10.

---

## How to Use This Playbook with Nemotron 3 Ultra

1. **Upload this entire document to Nemotron (build.nvidia.com, OpenCode, or your NVIDIA deployment).**
2. **Tell it:** "Start Phase 1 of the Project Lifecycle Playbook (Nemotron 3 Ultra Edition). Ask me the intake questions one at a time."
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

> **Nemotron note:** If you have larger NVIDIA/Blackwell hardware, Ultra can self-host air-gapped under its open license — relevant if data residency is strict.

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
- **What scares you most?** (what assumption breaks the project?)

---

## Phase 2: Spike Identification (15 minutes)

Nemotron analyzes your tech stack and flags risky assumptions needing proof-of-concept. Its reasoning is strong here, and its speed means spikes run fast.

### 2.1 Spike Analysis
For each tech choice, Nemotron asks (in structured form):
- Is this proven in your use case?
- What's the failure mode, traced explicitly?
- What's the minimal test that proves or disproves it?

> **Give it structure:** Phrase each spike as "Goal → Assumption under test → Pass/fail criterion → Output expected." Nemotron performs markedly better with this shape than with a loose "try X."

### 2.2 Spike Execution
Nemotron will:
1. List the spikes in structured form
2. Propose pass/fail tests for each
3. Write spike code (fast generation)
4. Report: pass or fail, and lessons learned

### 2.3 Gate
You review spike results. Approve or pivot. Nemotron awaits approval.

---

## Phase 3: Requirements Elicitation (45 minutes)

Nemotron asks targeted questions to build a **complete, unambiguous SRS**.

### 3.1 Functional Requirements
For each feature:
- **What inputs?** (user actions, data, events?)
- **What outputs?** (results, side effects?)
- **Who accesses what?** (permissions, isolation?)
- **Failure handling?** (graceful, retry, notify?)
- **Persistence?** (what survives restarts?)

### 3.2 Non-Functional Requirements
- **Performance:** latency targets? Throughput? (Nemotron's own speed is an asset for high-throughput agent workloads.)
- **Scale:** concurrent users? Data volume?
- **Reliability:** uptime SLA? Recovery time?
- **Privacy/Security:** sensitivity? Compliance? Encryption?
- **Maintainability:** solo or team? Long-term?
- **Cost:** monthly budget?

### 3.3 Gap Detection
Nemotron's reasoning traces logical gaps, contradictions, and unhandled cases methodically. It asks: "Is [gap] in V1? If yes, describe. If no, move to V2."

### 3.4 Output
SRS with ~50 functional requirements, ~8 non-functional, clear V1/V2 boundaries, acceptance criteria.

---

## Phase 4: Architecture Design (1–2 hours) — a strong phase for Nemotron

> **Trust Nemotron's reasoning here**, provided you give it structure. Its Multi-Teacher distillation makes it strong across planning, code generation, tool use, and verification. Frame the architecture task with explicit tradeoff axes and it reasons cleanly across them.

Nemotron reasons through:
- **Monolith or services?** (with explicit tradeoff analysis)
- **Data flow:** reads/writes?
- **Failure modes:** traced step by step, with recovery
- **Deployment:** machine to production?
- **Scaling:** what breaks first at 10x, and why?

### 4.1 Artifacts
- **Architecture description** (text; Nemotron can produce Mermaid — it's text-only, so provide your own draw.io if needed)
- **Data model** (SQL or document schema)
- **Decision log** (with genuine reasoning)
- **Open items**

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

## Phase 5: Build Sequence & Prompts (1 hour)

Nemotron designs the phased build and writes self-contained prompts. Its speed makes high-volume, many-turn build loops practical to run interactively rather than as slow batch jobs.

### 5.1 Phase Structure
- **Phase 0:** Environment (install, verify)
- **Phase 1–N:** Each builds one piece with a **Gate** (manual test)

Example gates:
- Phase 1: `/health` returns green
- Phase 2: OIDC login works
- Phase 3: data survives restart
- Phase N: all acceptance criteria pass

### 5.2 Prompt Discipline (Nemotron thrives on this)
Each prompt must:
- **Read existing code first**
- **Report severity-rated findings** (Critical/High/Medium/Low)
- **Await approval** on flagged items
- **Minimal changes** (no unrelated refactoring)
- **Reference SRS + Architecture** in every session
- **State goal, constraints, output shape, and gate explicitly** — Nemotron's single biggest performance lever is structure; never hand it a loose, open-ended instruction

### 5.3 Output
- Phased build plan (Phase 0 → N with gates)
- One prompt per phase (~12 prompts)
- Self-contained, highly structured prompts
- Estimated total build time

---

## Phase 6: Handoff (15 minutes)

Nemotron delivers:
1. ✅ Complete SRS v1.0 (locked)
2. ✅ Architecture document with reasoned decision log
3. ✅ Data schema
4. ✅ Twelve build prompts
5. ✅ Phase 0 checklist
6. ✅ User manual skeleton
7. ✅ V2 requirements document

**Ready to build.**

---

## Nemotron 3 Ultra-Specific Guidance

### Strengths to Leverage
- **Structure-rewarding reasoning:** the most disciplined match for this playbook — explicit prompts pay off
- **Speed:** ~140 tokens/sec with low time-to-first-token — fast enough for interactive, many-turn agent loops
- **1M context:** holds entire codebases and long histories
- **Open (OpenMDW):** weights, training data, and recipes published — fine-tune and domain-adapt freely
- **Blackwell/FP4:** self-hosts efficiently on larger NVIDIA hardware
- **Long-running agents:** built for multi-step planning, tool use, synthesis, verification, recovery

### Weaknesses to Mitigate
- **Punishes vagueness:** loose, free-form prompts degrade output sharply — always give structure
- **Not local on a single 128GB box:** ~275GB at FP4; API for the GB10
- **Text-only:** no multimodal; describe diagrams in text or use Mermaid
- **Trails the very top on raw intelligence index:** a few points behind the leading open model — it trades a little peak capability for several-times-faster output (usually the right trade for building)

### Workflow Pattern
1. **You:** Phase 1 intake (30 min)
2. **Nemotron:** Phase 2 spikes in structured form (15 min + fast execution)
3. **You:** review spike results, approve or pivot
4. **Nemotron:** Phase 3 requirements (45 min)
5. **Nemotron:** Phase 4 architecture — strong phase, give it tradeoff axes (1–2 hours)
6. **Nemotron:** Phase 5 highly structured build prompts (1 hour)
7. **Nemotron (OpenCode):** Phases 0–N build (fast iteration; 20–40 hours wall-clock, less compute-bound)

**Total planning + design:** 4–6 hours. **Build:** 20–40 hours (faster per turn than most).

---

## When to Use This Playbook with Nemotron 3 Ultra

- ✅ You'll write tightly structured prompts (this playbook does)
- ✅ Speed and high-throughput, many-turn agent loops matter
- ✅ Long-running autonomous agents with planning + verification
- ✅ You have larger NVIDIA/Blackwell hardware for self-hosting (optional)

When NOT:
- ❌ You want a forgiving, free-form chat partner that infers intent
- ❌ Multimodal input is central
- ❌ Quick 2-hour spike

---

## Quick Start

**Tell Nemotron 3 Ultra:**

> Start Phase 1 of the Project Lifecycle Playbook (Nemotron 3 Ultra Edition). Ask me the intake questions one at a time, waiting for my full answer before the next. Throughout, expect highly structured prompts from me — goal, constraints, expected output shape, and a pass/fail gate — and hold me to that structure. When Phase 1 is complete, summarize and confirm before Phase 2.

Answer each question fully. Always give Nemotron explicit structure. Lean on its reasoning in Phase 4. Review outputs at every gate.

**Ready?** Upload this document to Nemotron and begin.
