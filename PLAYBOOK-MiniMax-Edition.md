# Project Lifecycle Playbook v1.0 — MiniMax Edition
## AI-Guided Structured Project Kickoff (Optimized for Long Context + Multilingual)

**Model:** MiniMax — very large context window, strong multilingual capability, cost-effective
**Philosophy:** Exploit MiniMax's long context and multilingual strength (valuable for non-English and Arabic content). Be more prescriptive in prompts, since the English-language tooling ecosystem and example base are thinner than for Western models.

> **Calibration note (honest):** Public, detailed knowledge of MiniMax's exact current capabilities is more limited than for Claude/Gemini/DeepSeek. This edition therefore leans on MiniMax's well-established strengths (long context, multilingual) and deliberately makes prompts more explicit and verification-heavy. Treat MiniMax's claims about niche libraries or framework specifics as "verify before trusting."

---

## How to Use This Playbook with MiniMax

1. **Upload this entire document to MiniMax.**
2. **Tell it:** "Start Phase 1 of the Project Lifecycle Playbook (MiniMax Edition). Ask me the intake questions one at a time."
3. **Answer each question fully.** You may answer in English, Arabic, or mixed — MiniMax handles multilingual input well.
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

> **MiniMax advantage:** If your project handles **multilingual content** (Arabic, Chinese, mixed scripts), say so — MiniMax is strong here, including correct handling of non-Latin text and right-to-left scripts.

### 1.4 Known Constraints & Risks
- **Hard constraints?** (deadlines, budget, regulatory, locked architecture?)
- **Architectural constraints?** (solo dev, minimal deps, scale?)
- **Team constraints?** (learning curve, domain expertise?)
- **What scares you most?** (what breaks the project?)

---

## Phase 2: Spike Identification (15 minutes)

MiniMax analyzes your tech stack and flags risky assumptions needing proof-of-concept.

### 2.1 Spike Analysis
For each tech choice, MiniMax asks:
- Is this proven in your use case?
- What's the failure mode?
- What's a quick 1–2 hour test that proves it?

> **Verification discipline:** Because MiniMax's knowledge of niche Western libraries may be less current, every spike here doubly serves to **verify the library actually behaves as assumed**. Run the code; trust the result over the model's recollection.

### 2.2 Spike Execution
MiniMax will:
1. List the spikes
2. Propose tests for each
3. Write spike code
4. Report: pass or fail, and lessons learned

### 2.3 Gate
You review spike results. Approve or pivot. MiniMax awaits approval.

---

## Phase 3: Requirements Elicitation (45 minutes)

MiniMax asks targeted questions to build a **complete, unambiguous SRS**.

### 3.1 Functional Requirements
For each feature:
- **What inputs?** (user actions, data, events?)
- **What outputs?** (results, side effects?)
- **Who accesses what?** (permissions, isolation?)
- **Failure handling?** (graceful, retry, notify?)
- **Persistence?** (what survives restarts?)
- **Languages?** (which languages must inputs/outputs support?)

### 3.2 Non-Functional Requirements
- **Performance:** latency targets? Throughput?
- **Scale:** concurrent users? Data volume?
- **Reliability:** uptime SLA? Recovery time?
- **Privacy/Security:** sensitivity? Compliance? Encryption?
- **Maintainability:** solo or team? Long-term?
- **Cost:** monthly budget? (MiniMax is typically cost-effective — note actual pricing.)
- **Localization:** multilingual UI? RTL rendering? Locale formats?

### 3.3 Gap Detection
MiniMax scans for ambiguous definitions, missing edge cases, implicit assumptions, scope creep, and asks: "Is [gap] in V1? If yes, describe. If no, move to V2."

> **Be prescriptive:** Spell out edge cases explicitly rather than expecting MiniMax to infer Western-convention defaults. State them.

### 3.4 Output
SRS with ~50 functional requirements, ~8 non-functional, clear V1/V2 boundaries, acceptance criteria.

---

## Phase 4: Architecture Design (1–2 hours)

**Note:** Pair architecture decisions with human review. MiniMax can implement and reason, but ecosystem-specific best-practice knowledge (framework idioms, deployment conventions) may be thinner — verify against current docs.

MiniMax asks:
- **Monolith or services?**
- **Data flow:** reads/writes?
- **Failure modes:** recovery?
- **Deployment:** machine to production?
- **Scaling:** what breaks at 10x load?

### 4.1 Artifacts
- **Architecture outline** (text description; provide your own draw.io if you need visuals)
- **Data model** (SQL or document schema — confirm dialect-specific syntax)
- **Decision log** (why each choice?)
- **Open items** (things to resolve during build)

### 4.2 Standards (non-negotiable)
- Dependency injection
- Async/await for all I/O
- Standardized error envelope
- JSON structured logging
- Configuration externalized
- No `latest` image tags
- One source of truth per concern
- **UTF-8 end to end** (critical for multilingual/RTL correctness)

### 4.3 Output
Architecture document, data schema, decision log, open items.

---

## Phase 5: Build Sequence & Prompts (1 hour)

MiniMax designs the phased build and writes self-contained prompts.

### 5.1 Phase Structure
- **Phase 0:** Environment (install, verify)
- **Phase 1–N:** Each builds one piece with a **Gate** (manual test)

Example gates:
- Phase 1: `/health` returns green
- Phase 2: OIDC login works
- Phase 3: data survives restart
- Phase N: all acceptance criteria pass — including a multilingual/RTL round-trip test if relevant

### 5.2 Prompt Discipline (make prompts MORE explicit for MiniMax)
Each prompt must:
- **Read existing code first**
- **Report severity-rated findings** (Critical/High/Medium/Low)
- **Await approval** on flagged items
- **Minimal changes** (no unrelated refactoring)
- **Reference SRS + Architecture** in every session
- **Spell out conventions explicitly** — don't assume MiniMax infers the Western-default idiom; state the expected pattern

### 5.3 Output
- Phased build plan (Phase 0 → N with gates)
- One prompt per phase (~12 prompts)
- Self-contained, explicit prompts
- Estimated total build time

---

## Phase 6: Handoff (15 minutes)

MiniMax delivers:
1. ✅ Complete SRS v1.0 (locked)
2. ✅ Architecture document
3. ✅ Data schema
4. ✅ Twelve build prompts
5. ✅ Phase 0 checklist
6. ✅ User manual skeleton
7. ✅ V2 requirements document

**Ready to build.**

---

## MiniMax-Specific Guidance

### Strengths to Leverage
- **Very large context:** hold the whole codebase and all docs at once
- **Multilingual:** strong on Arabic, Chinese, mixed scripts, RTL — use it for any internationalized project
- **Cost-effective:** typically cheaper per token — good for high-iteration work
- **Solid code + reasoning:** competent across general tasks

### Weaknesses to Mitigate
- **Thinner English ecosystem knowledge:** verify niche library/framework specifics against current docs
- **Fewer worked examples:** be prescriptive; spell out conventions rather than relying on inference
- **Best-practice gaps for Western stacks:** pair architecture with human review or a second model
- **Variable tooling integration:** confirm any IDE/agent integration works before relying on it

### Workflow Pattern
1. **You:** Phase 1 intake (30 min) — flag multilingual needs
2. **MiniMax:** Phase 2 spikes (15 min + execution) — spikes double as library verification
3. **You:** review spike results, approve or pivot
4. **MiniMax:** Phase 3 requirements (45 min) — state edge cases explicitly
5. **Human + MiniMax:** Phase 4 architecture (1–2 hours) *[verify best practices]*
6. **MiniMax:** Phase 5 explicit build prompts (1 hour)
7. **MiniMax:** Phases 0–N build (20–40 hours)

**Total planning + design:** 4–6 hours. **Build:** 20–40 hours.

---

## When to Use This Playbook with MiniMax

- ✅ Multilingual project (Arabic, Chinese, mixed, RTL)
- ✅ Very long context needs (whole-codebase reasoning)
- ✅ Cost-sensitive, high-iteration work
- ✅ You're comfortable verifying library specifics yourself

When NOT:
- ❌ You need authoritative Western-framework best-practice guidance with no verification
- ❌ Multimodal (diagram/screenshot) input is central
- ❌ Quick 2-hour spike

---

## Quick Start

**Tell MiniMax:**

> Start Phase 1 of the Project Lifecycle Playbook (MiniMax Edition). Ask me the intake questions one at a time, waiting for my full answer before the next. I may answer in English or Arabic. When Phase 1 is complete, summarize and confirm before Phase 2. Throughout, when you rely on a library or framework detail, state your confidence and suggest a quick test to verify it.

Answer each question fully. Be explicit about multilingual needs. Verify library behavior via spikes. Review outputs at every gate.

**Ready?** Upload this document to MiniMax and begin.
