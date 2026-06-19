# Project Lifecycle Playbook v1.0 — Big Pickle Edition
## AI-Guided Structured Project Kickoff (Optimized for Open-Source Coding)

**Model:** Big Pickle (OpenCode Zen, 200k context, MIT-licensed, free)
**Philosophy:** Leverage the 200k context window for code-heavy work; use Big Pickle for implementation spikes and code generation; pair with strategic human review for architecture.

---

## How to Use This Playbook with Big Pickle

1. **Upload this entire document to Big Pickle (OpenCode Zen or self-hosted).**
2. **Tell Big Pickle:** "Start Phase 1 of the Project Lifecycle Playbook (Big Pickle Edition). Ask me the intake questions one at a time."
3. **Answer each question fully.**
4. **Big Pickle will proceed through Phases 2–6** exactly as described below.
5. **At each phase gate, review Big Pickle's output before approving.**

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
- **Language(s)?** (Python, TypeScript, Go, Rust, etc.)
- **Framework?** (FastAPI, Django, Express, React, etc.)
- **Databases?** (PostgreSQL, MongoDB, SQLite, Redis, etc.)
- **Deployment?** (Docker, K8s, serverless, binary?)
- **External services?** (APIs, LLMs, auth, payment, etc.)
- **Why these choices?** (expertise? skill? performance? cost?)

### 1.4 Known Constraints & Risks
- **Hard constraints?** (deadlines, budget, regulatory, locked architecture?)
- **Architectural constraints?** (solo dev, minimal deps, scale?)
- **Team constraints?** (learning curve, domain expertise?)
- **What scares you most?** (what breaks the project?)

---

## Phase 2: Spike Identification (15 minutes)

Big Pickle analyzes your tech stack and flags risky assumptions needing proof-of-concept.

### 2.1 Spike Analysis
For each tech choice, Big Pickle asks:
- Is this proven in your use case?
- What's the failure mode?
- What's a quick 1–2 hour test that proves it?

**Big Pickle's strength:** Writing complete spike code fast.

### 2.2 Spike Execution
Big Pickle will:
1. List the spikes
2. Propose tests for each
3. Write spike code
4. Report: pass or fail, and lessons learned

### 2.3 Gate
You review spike results. Approve or pivot tech stack. Big Pickle awaits approval.

---

## Phase 3: Requirements Elicitation (45 minutes)

Big Pickle asks targeted questions to build a **complete, unambiguous SRS**.

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
- **Cost:** monthly budget?

### 3.3 Gap Detection
Big Pickle scans for:
- Ambiguous definitions
- Missing edge cases
- Implicit assumptions
- Scope creep

Big Pickle asks: "Is [gap] in V1? If yes, describe. If no, move to V2."

### 3.4 Output
SRS with ~50 functional requirements, ~8 non-functional, clear V1/V2 boundaries, acceptance criteria.

---

## Phase 4: Architecture Design (1–2 hours)

**Note:** For architecture decisions, pair with human review or Claude. Big Pickle can implement but may miss tradeoffs.

Big Pickle asks:
- **Monolith or services?**
- **Data flow:** reads/writes?
- **Failure modes:** recovery?
- **Deployment:** machine to production?
- **Scaling:** what breaks at 10x load?

### 4.1 Artifacts
- **Architecture outline** (text description)
- **Data model** (SQL or document schema, in code)
- **Decision log** (why each choice?)
- **Open items** (things to resolve during build)

**You should add:** draw.io diagrams yourself or request from Claude.

### 4.2 Standards (non-negotiable)
- Dependency injection
- Async/await for all I/O
- Standardized error envelope
- JSON structured logging
- Configuration externalized
- No `latest` image tags
- One source of truth per concern

### 4.3 Output
Architecture document, data schema, decision log, open items.

---

## Phase 5: Build Sequence & Prompts (1 hour)

Big Pickle designs the phased build and writes self-contained prompts.

### 5.1 Phase Structure
- **Phase 0:** Environment (install, verify)
- **Phase 1–N:** Each builds one piece with a **Gate** (manual test)

Example gates:
- Phase 1: `/health` returns green
- Phase 2: OIDC login works
- Phase 3: data survives restart
- ...
- Phase N: all acceptance criteria pass

### 5.2 Prompt Discipline (Big Pickle enforces)
Each prompt must:
- **Read existing code first**
- **Report severity-rated findings**
- **Await approval** on flagged items
- **Minimal changes** (no unrelated refactoring)
- **Reference SRS + Architecture** in every session

Big Pickle's 200k context makes this natural.

### 5.3 Output
- Phased build plan (Phase 0 → N with gates)
- One prompt per phase (~12 prompts)
- Self-contained prompts
- Estimated total build time

---

## Phase 6: Handoff (15 minutes)

Big Pickle delivers:
1. ✅ Complete SRS v1.0 (locked)
2. ✅ Architecture document
3. ✅ Data schema (code)
4. ✅ Twelve build prompts
5. ✅ Phase 0 checklist
6. ✅ User manual skeleton
7. ✅ V2 requirements document

**Ready to build.**

---

## Big Pickle-Specific Guidance

### Strengths to Leverage
- **200k context:** upload entire codebase + requirements in one prompt
- **Coding-optimized:** spike implementations are complete and fast
- **No safety filters:** free to suggest novel patterns
- **Free:** no API costs, unlimited iterations

### Weaknesses to Mitigate
- **Architecture decisions:** may miss subtle tradeoffs; use human judgment or Claude for Phase 4
- **Novel patterns:** may suggest incomplete implementations; add explicit spike gates
- **Error handling:** tends to skip edge cases; ask explicitly about failure modes
- **No diagrams:** can't generate draw.io; describe architecture in text

### Workflow Pattern
1. **You:** Phase 1 project intake (30 min)
2. **Big Pickle:** Phase 2 spike identification (15 min)
3. **Big Pickle:** spike code execution (1–2 hours)
4. **You:** review results, approve or pivot
5. **Big Pickle:** Phase 3 requirements (45 min)
6. **Claude or You:** Phase 4 architecture (1–2 hours) *[Critical]*
7. **Big Pickle:** Phase 5 build prompts (1 hour)
8. **Big Pickle:** Phases 0–N full build (20–40 hours)

**Total planning + design:** 4–6 hours. **Build:** 20–40 hours.

---

## When to Use This Playbook with Big Pickle

- ✅ Any project >1 week of work
- ✅ Multiple services or data persistence
- ✅ Systems that scale or evolve
- ✅ Long-term maintenance
- ✅ You want free, unlimited iterations

When NOT:
- ❌ Quick 2-hour spike
- ❌ Copy-paste templates
- ❌ Prototypes with no intention to ship

---

## Quick Start

**Tell Big Pickle:**

> Start Phase 1 of the Project Lifecycle Playbook (Big Pickle Edition). Ask me the intake questions one at a time, waiting for my full answer before asking the next. When I've answered all Phase 1 questions, summarize them and confirm you understand the project before proceeding to Phase 2.

Then follow along. Answer each question fully. Review outputs at every gate. Approve before moving forward.

**Ready?** Upload this document to Big Pickle and begin.
