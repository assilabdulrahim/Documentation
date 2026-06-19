# Project Lifecycle Playbook v1.0 — MiMo V2.5 Edition
## AI-Guided Structured Project Kickoff (Optimized for Low-Cost, Long-Horizon Autonomous Builds)

**Model:** Xiaomi MiMo V2.5 / V2.5-Pro — Pro: 1T MoE (42B active); standard V2.5: 310B (15B active). 1M context, native multimodal (text/image/audio/video), MIT, exceptionally token-efficient
**Philosophy:** MiMo's edge is cost and endurance — frontier-class agentic coding at a fraction of the per-token price, sustaining 1,000+ tool calls without losing coherence. That changes the economics of this playbook: spikes, iterations, and full autonomous phase-runs become cheap enough to do liberally.

> **Tier guidance:** Use **MiMo-V2.5-Pro** for complex software engineering and architecture. Use the standard **MiMo-V2.5** (310B) for everyday coding — it matches Pro on routine tasks at roughly half the cost. Pick per phase: Pro for Phases 2/4 (spikes, architecture), standard for routine Phase 5 generation.
>
> **Deployment note (honest):** Even the 310B standard model is ~155GB+ at 4-bit — over a single 128GB GB10's capacity, and the Pro is far larger. So for your workstation, MiMo is API/cloud (Xiaomi API, OpenRouter, Kilo Code), not a local embedded runtime. Its strength here is *build-time* assistance at very low cost.
>
> **Speed tradeoff:** MiMo is token-*efficient* but not the fastest per token (~40 tok/s, higher time-to-first-token). It shines in autonomous, batch-style runs more than snappy interactive back-and-forth.

---

## How to Use This Playbook with MiMo V2.5

1. **Upload this entire document to MiMo (Xiaomi API, OpenRouter, or Kilo Code).**
2. **Tell it:** "Start Phase 1 of the Project Lifecycle Playbook (MiMo V2.5 Edition). Ask me the intake questions one at a time."
3. **Answer each question fully.** You can attach images, audio, or video — MiMo is natively multimodal.
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

## Phase 2: Spike Identification (15 minutes) — run liberally; it's cheap

MiMo analyzes your tech stack and flags risky assumptions needing proof-of-concept. Because MiMo is so cost-efficient, you can afford *more* spikes than usual — use that to de-risk thoroughly.

### 2.1 Spike Analysis
For each tech choice, MiMo asks:
- Is this proven in your use case?
- What's the failure mode?
- What's a quick test that proves or disproves it?

> **MiMo advantage — autonomous spikes:** MiMo sustains 1,000+ tool calls in a single run, so a spike can be a genuine end-to-end build-and-test executed autonomously, not a toy. Multimodal input means you can attach mockups, screenshots, or even an audio description of the requirement.

### 2.2 Spike Execution
MiMo will:
1. List the spikes
2. Propose tests for each
3. Execute spike builds autonomously (long tool-call chains are its strength)
4. Report: pass or fail, and lessons learned

### 2.3 Gate
You review spike results. Approve or pivot. MiMo awaits approval.

---

## Phase 3: Requirements Elicitation (45 minutes)

MiMo asks targeted questions to build a **complete, unambiguous SRS**.

### 3.1 Functional Requirements
For each feature:
- **What inputs?** (user actions, data, events, images/audio/video?)
- **What outputs?** (results, side effects?)
- **Who accesses what?** (permissions, isolation?)
- **Failure handling?** (graceful, retry, notify?)
- **Persistence?** (what survives restarts?)

### 3.2 Non-Functional Requirements
- **Performance:** latency targets? Throughput? (Note MiMo's lower tokens/sec — design async/batch where latency matters.)
- **Scale:** concurrent users? Data volume?
- **Reliability:** uptime SLA? Recovery time?
- **Privacy/Security:** sensitivity? Compliance? Encryption?
- **Maintainability:** solo or team? Long-term?
- **Cost:** monthly budget? (MiMo's low per-token cost and token efficiency make high-iteration workflows genuinely affordable.)

### 3.3 Gap Detection
MiMo scans for ambiguous definitions, missing edge cases, implicit assumptions, scope creep, and asks: "Is [gap] in V1? If yes, describe. If no, move to V2."

### 3.4 Output
SRS with ~50 functional requirements, ~8 non-functional, clear V1/V2 boundaries, acceptance criteria.

---

## Phase 4: Architecture Design (1–2 hours) — use MiMo-V2.5-Pro here

> **Use the Pro tier for architecture.** Its reasoning is frontier-comparable on complex software engineering. Let it reason through tradeoffs; the cost of a thorough architecture session is low.

MiMo reasons through:
- **Monolith or services?**
- **Data flow:** reads/writes?
- **Failure modes:** recovery?
- **Deployment:** machine to production?
- **Scaling:** what breaks first at 10x?

### 4.1 Artifacts
- **Architecture description** (MiMo can generate Mermaid; it also reads sketches/screenshots)
- **Data model** (SQL or document schema)
- **Decision log** (why each choice?)
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
Architecture document, diagrams, data schema, decision log, open items.

---

## Phase 5: Build Sequence & Prompts (1 hour) — standard tier for routine generation

MiMo designs the phased build and writes self-contained prompts. For routine generation, switch to standard MiMo-V2.5 to halve cost; reserve Pro for genuinely hard implementation problems.

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

> **MiMo-specific prompt notes:**
> - **Exploit autonomy:** a single prompt can drive a long, multi-step build (it holds coherence over 1,000+ tool calls) — but still bound it with the phase Gate so it stops at a verifiable checkpoint.
> - **Prefer async/batch** framing over expecting fast interactive turns, given throughput.
> - **Tier switch:** note in each prompt whether it should run on Pro (hard) or standard (routine).

### 5.3 Output
- Phased build plan (Phase 0 → N with gates)
- One prompt per phase (~12 prompts)
- Self-contained prompts, tier-tagged
- Estimated total build time

---

## Phase 6: Handoff (15 minutes)

MiMo delivers:
1. ✅ Complete SRS v1.0 (locked)
2. ✅ Architecture document + diagrams
3. ✅ Data schema
4. ✅ Twelve build prompts (tier-tagged Pro/standard)
5. ✅ Phase 0 checklist
6. ✅ User manual skeleton
7. ✅ V2 requirements document

**Ready to build.**

---

## MiMo V2.5-Specific Guidance

### Strengths to Leverage
- **Cost efficiency:** frontier-class output at a fraction of the per-token price; 40–60% fewer tokens than comparable frontier models — run spikes and iterations liberally
- **Long-horizon autonomy:** sustains 1,000+ sequential tool calls without losing the thread — true end-to-end autonomous builds
- **1M context:** entire codebase plus docs in one window
- **Native multimodal:** text, image, audio, and video unified — attach mockups, screenshots, audio specs
- **MIT license + two tiers:** Pro for hard work, standard (310B) for routine at half the cost

### Weaknesses to Mitigate
- **Throughput/latency:** ~40 tokens/sec and higher time-to-first-token — design async/batch; not ideal for snappy interactive chat
- **Not local on a single 128GB box:** API/cloud for the GB10
- **Tier discipline needed:** decide Pro-vs-standard per task or you'll overpay (or underpower)
- **Bound the autonomy:** its endurance means a loose prompt can run a long way before you can course-correct — always anchor to a phase Gate

### Workflow Pattern
1. **You:** Phase 1 intake (30 min) — attach any visual/audio assets
2. **MiMo-Pro:** Phase 2 spikes, run liberally and autonomously (15 min + execution)
3. **You:** review spike results, approve or pivot
4. **MiMo-Pro:** Phase 3 requirements (45 min)
5. **MiMo-Pro:** Phase 4 architecture (1–2 hours)
6. **MiMo standard:** Phase 5 routine build prompts; **Pro** for hard ones (1 hour)
7. **MiMo (Kilo Code / API):** Phases 0–N autonomous build, gated per phase (20–40 hours, very low cost)

**Total planning + design:** 4–6 hours. **Build:** 20–40 hours at a fraction of typical cost.

---

## When to Use This Playbook with MiMo V2.5

- ✅ Cost-sensitive projects with heavy iteration
- ✅ Long-horizon autonomous builds (many hundreds/thousands of tool calls)
- ✅ Mixed-media requirements (images, audio, video)
- ✅ High-volume agentic coding pipelines

When NOT:
- ❌ You need fast, snappy interactive turns (throughput is modest)
- ❌ Must run fully local on a single modest box
- ❌ Quick 2-hour spike

---

## Quick Start

**Tell MiMo V2.5:**

> Start Phase 1 of the Project Lifecycle Playbook (MiMo V2.5 Edition). Ask me the intake questions one at a time, waiting for my full answer before the next. I may attach images, audio, or video — read them. Use MiMo-V2.5-Pro for spikes and architecture (Phases 2 and 4) and standard MiMo-V2.5 for routine code (Phase 5), and tell me which tier each later prompt should run on. When Phase 1 is complete, summarize and confirm before Phase 2. Anchor every autonomous run to its phase Gate so it stops at a verifiable checkpoint.

Answer each question fully. Exploit the low cost — spike liberally. Pick tiers deliberately. Anchor autonomy to gates. Review outputs at every gate.

**Ready?** Upload this document to MiMo and begin.
