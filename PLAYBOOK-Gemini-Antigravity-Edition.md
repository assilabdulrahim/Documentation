# Project Lifecycle Playbook v1.0 — Gemini / Antigravity Edition
## AI-Guided Structured Project Kickoff (Optimized for Multimodal + Massive Context)

**Model:** Gemini (via Antigravity, or Gemini directly) — 1M+ token context, multimodal, Google ecosystem
**Philosophy:** Exploit Gemini's ability to read diagrams and screenshots, its enormous context window, and its native Google Workspace integration. Rein in verbosity with explicit discipline.

---

## How to Use This Playbook with Gemini / Antigravity

1. **Upload this entire document to Gemini (or load it into Antigravity).**
2. **Tell it:** "Start Phase 1 of the Project Lifecycle Playbook (Gemini Edition). Ask me the intake questions one at a time."
3. **Answer each question fully.** You can also upload sketches, screenshots, or mockups — Gemini can read them.
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
- **External services?** (APIs, LLMs, auth, payment, **Google Workspace?**)
- **Why these choices?** (expertise? skill? performance? cost?)

> **Gemini advantage:** If your project touches Gmail, Google Drive, Docs, Calendar, or Sheets, say so — Gemini's native integration makes those skills far easier to design and wire.

### 1.4 Known Constraints & Risks
- **Hard constraints?** (deadlines, budget, regulatory, locked architecture?)
- **Architectural constraints?** (solo dev, minimal deps, scale?)
- **Team constraints?** (learning curve, domain expertise?)
- **What scares you most?** (what breaks the project?)

---

## Phase 2: Spike Identification (15 minutes)

Gemini analyzes your tech stack and flags risky assumptions needing proof-of-concept.

### 2.1 Spike Analysis
For each tech choice, Gemini asks:
- Is this proven in your use case?
- What's the failure mode?
- What's a quick 1–2 hour test that proves it?

> **Gemini advantage — multimodal spikes:** Upload a UI mockup or wireframe and ask "does this match requirement X?" Gemini reads the image and validates. It can also spike against screenshots of existing systems.

### 2.2 Spike Execution
Gemini will:
1. List the spikes
2. Propose tests for each
3. Write spike code (and read any visual references you provide)
4. Report: pass or fail, and lessons learned

### 2.3 Gate
You review spike results. Approve or pivot. Gemini awaits approval.

---

## Phase 3: Requirements Elicitation (45 minutes)

Gemini asks targeted questions to build a **complete, unambiguous SRS**.

### 3.1 Functional Requirements
For each feature:
- **What inputs?** (user actions, data, events, **uploaded images?**)
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
- **Cost:** monthly budget? (Note: Gemini is API-only — factor token costs.)

### 3.3 Gap Detection
Gemini scans for ambiguous definitions, missing edge cases, implicit assumptions, scope creep, and asks: "Is [gap] in V1? If yes, describe. If no, move to V2."

> **Discipline note:** Gemini can be verbose and over-eager to add features. Push back: "Is that V1 or V2?" Keep scope tight.

### 3.4 Output
SRS with ~50 functional requirements, ~8 non-functional, clear V1/V2 boundaries, acceptance criteria.

---

## Phase 4: Architecture Design (1–2 hours)

> **Gemini strength:** This is where multimodal shines. Sketch your architecture on paper or a whiteboard, photograph it, and upload it. Gemini reads the sketch and formalizes it into a proper architecture document and clean diagrams. It can also generate draw.io-compatible XML or Mermaid diagrams directly.

Gemini asks:
- **Monolith or services?**
- **Data flow:** reads/writes?
- **Failure modes:** recovery?
- **Deployment:** machine to production?
- **Scaling:** what breaks at 10x load?

### 4.1 Artifacts
- **Architecture diagram** (Gemini can generate Mermaid or draw.io XML — and read your hand-drawn version)
- **Data model** (SQL or document schema)
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

### 4.3 Output
Architecture document, diagrams (Mermaid/draw.io), data schema, decision log, open items.

---

## Phase 5: Build Sequence & Prompts (1 hour)

Gemini designs the phased build and writes self-contained prompts.

### 5.1 Phase Structure
- **Phase 0:** Environment (install, verify)
- **Phase 1–N:** Each builds one piece with a **Gate** (manual test)

Example gates:
- Phase 1: `/health` returns green
- Phase 2: OIDC login works
- Phase 3: data survives restart
- Phase N: all acceptance criteria pass

### 5.2 Prompt Discipline (enforce strictly with Gemini)
Each prompt must:
- **Read existing code first**
- **Report severity-rated findings** (Critical/High/Medium/Low)
- **Await approval** on flagged items
- **Minimal changes — no scope expansion** (Gemini's main failure mode is doing too much; constrain it)
- **Reference SRS + Architecture** in every session

> **Antigravity note:** As an agentic environment, Antigravity can execute and iterate autonomously. Set tight gates and require approval checkpoints so it doesn't run ahead and over-build.

### 5.3 Output
- Phased build plan (Phase 0 → N with gates)
- One prompt per phase (~12 prompts)
- Self-contained prompts
- Estimated total build time

---

## Phase 6: Handoff (15 minutes)

Gemini delivers:
1. ✅ Complete SRS v1.0 (locked)
2. ✅ Architecture document + diagrams (Mermaid/draw.io)
3. ✅ Data schema
4. ✅ Twelve build prompts
5. ✅ Phase 0 checklist
6. ✅ User manual skeleton
7. ✅ V2 requirements document

**Ready to build.**

---

## Gemini / Antigravity-Specific Guidance

### Strengths to Leverage
- **Multimodal:** reads diagrams, screenshots, mockups, whiteboard photos — use it in Phases 2 and 4
- **1M+ token context:** hold the entire codebase, all docs, and conversation history at once
- **Google ecosystem:** native Gmail/Drive/Docs/Calendar/Sheets integration — huge for projects touching Workspace
- **Diagram generation:** produces Mermaid and draw.io XML directly
- **Structured output:** strong at generating clean JSON specs, OpenAPI, schemas

### Weaknesses to Mitigate
- **Verbosity:** tends to over-explain and over-build; enforce "minimal changes" and "is this V1?" relentlessly
- **Over-eagerness:** may add unrequested features; lock scope in the SRS and hold the line
- **Closed/API-only:** no self-hosting; factor token costs for large-context calls
- **Over-confidence:** occasionally asserts uncertain things; ask it to flag confidence levels

### Workflow Pattern
1. **You:** Phase 1 intake (30 min) — upload any sketches/mockups you already have
2. **Gemini:** Phase 2 spikes, including multimodal validation (15 min + execution)
3. **You:** review spike results, approve or pivot
4. **Gemini:** Phase 3 requirements (45 min)
5. **Gemini:** Phase 4 architecture — upload your hand-drawn sketch, it formalizes (1–2 hours)
6. **Gemini:** Phase 5 build prompts (1 hour)
7. **Gemini / Antigravity:** Phases 0–N build (20–40 hours), with tight gates

**Total planning + design:** 4–6 hours. **Build:** 20–40 hours.

---

## When to Use This Playbook with Gemini

- ✅ Project touches Google Workspace (Gmail, Drive, Docs, Calendar)
- ✅ You have visual assets (mockups, diagrams, screenshots) to feed it
- ✅ Very large codebase that benefits from 1M+ context
- ✅ You want diagram generation built in

When NOT:
- ❌ You need self-hosting / air-gapped (Gemini is cloud-only)
- ❌ Strict token budget on huge-context calls
- ❌ Quick 2-hour spike

---

## Quick Start

**Tell Gemini / Antigravity:**

> Start Phase 1 of the Project Lifecycle Playbook (Gemini Edition). Ask me the intake questions one at a time, waiting for my full answer before the next. I may upload sketches or screenshots — read them. When Phase 1 is complete, summarize and confirm before Phase 2.

Answer each question fully. Upload visuals where useful. Review outputs at every gate. Hold scope discipline — Gemini wants to add more than V1 needs.

**Ready?** Upload this document to Gemini and begin.
