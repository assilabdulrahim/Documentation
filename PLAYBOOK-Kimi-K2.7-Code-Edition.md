# Project Lifecycle Playbook v1.0 — Kimi K2.7-Code Edition
## AI-Guided Structured Project Kickoff (Optimized for Agentic Coding + MCP Tool-Use)

**Model:** Kimi K2.7-Code (Moonshot AI) — 1T MoE (32B active), 256K context, multimodal (text/image/video), Modified MIT, mandatory thinking mode
**Philosophy:** Exploit Kimi's purpose-built agentic-coding and MCP tool-calling strength. It is designed for long-horizon software engineering with tool loops — lean into that. Budget for its mandatory reasoning tokens; they are the point, not overhead.

> **Deployment note (honest):** Kimi K2.7-Code is ~1T parameters and needs roughly 4×H100-class hardware to self-host. It will **not** run locally on a single 128GB workstation. For your setup you reach it via API (Moonshot, or shells like Kimi Code CLI / OpenCode / Kilo Code). That's fine for *build-time* coding assistance, but it cannot serve as a fully local embedded runtime if data-never-leaves-the-box is a hard requirement.
>
> **Benchmark caveat:** As of its June 2026 release, most published K2.7-Code numbers are Moonshot's own benchmarks. Validate it on *your* representative task before trusting headline figures.

---

## How to Use This Playbook with Kimi K2.7-Code

1. **Upload this entire document to Kimi (Kimi Code CLI, OpenCode, Kilo Code, or the API).**
2. **Tell it:** "Start Phase 1 of the Project Lifecycle Playbook (Kimi K2.7-Code Edition). Ask me the intake questions one at a time."
3. **Answer each question fully.** You can attach screenshots or even a recorded repro — Kimi reads image and video input.
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
- **Language(s)?** (Kimi is especially strong in Rust, Go, Python)
- **Framework?**
- **Databases?**
- **Deployment?** (Docker, K8s, serverless, binary?)
- **External services?** (APIs, LLMs, auth, payment?)
- **MCP servers in play?** (Kimi excels at Model Context Protocol tool-use — list any you'll integrate)
- **Why these choices?** (expertise? skill? performance? cost?)

### 1.4 Known Constraints & Risks
- **Hard constraints?** (deadlines, budget, regulatory, locked architecture?)
- **Architectural constraints?** (solo dev, minimal deps, scale?)
- **Team constraints?** (learning curve, domain expertise?)
- **What scares you most?** (what assumption breaks the project?)

---

## Phase 2: Spike Identification (15 minutes)

Kimi analyzes your tech stack and flags risky assumptions needing proof-of-concept.

### 2.1 Spike Analysis
For each tech choice, Kimi asks:
- Is this proven in your use case?
- What's the failure mode?
- What's a quick 1–2 hour test that proves it?

> **Kimi advantage — MCP and multimodal spikes:** If a spike involves tool-calling through MCP (CI checks, ticket updates, file edits in one loop), Kimi is in its element — have it build and run the tool loop. For UI/visual assumptions, attach a screenshot or recorded repro; Kimi reads it directly.

### 2.2 Spike Execution
Kimi will:
1. List the spikes
2. Propose tests for each
3. Write and run spike code (including MCP tool loops)
4. Report: pass or fail, and lessons learned

### 2.3 Gate
You review spike results. Approve or pivot. Kimi awaits approval.

---

## Phase 3: Requirements Elicitation (45 minutes)

Kimi asks targeted questions to build a **complete, unambiguous SRS**.

### 3.1 Functional Requirements
For each feature:
- **What inputs?** (user actions, data, events, images/video?)
- **What outputs?** (results, side effects?)
- **Who accesses what?** (permissions, isolation?)
- **Failure handling?** (graceful, retry, notify?)
- **Persistence?** (what survives restarts?)
- **Tool/agent steps?** (which features require autonomous multi-step tool use?)

### 3.2 Non-Functional Requirements
- **Performance:** latency targets? Throughput?
- **Scale:** concurrent users? Data volume?
- **Reliability:** uptime SLA? Recovery time?
- **Privacy/Security:** sensitivity? Compliance? Encryption?
- **Maintainability:** solo or team? Long-term?
- **Cost:** monthly budget? (Kimi is API-priced — factor input/output tokens *including mandatory thinking tokens*.)

### 3.3 Gap Detection
Kimi scans for ambiguous definitions, missing edge cases, implicit assumptions, scope creep, and asks: "Is [gap] in V1? If yes, describe. If no, move to V2."

### 3.4 Output
SRS with ~50 functional requirements, ~8 non-functional, clear V1/V2 boundaries, acceptance criteria.

---

## Phase 4: Architecture Design (1–2 hours)

> **Kimi strength:** Agentic-coding-aware architecture. It reasons well about tool-calling topologies, MCP server boundaries, and long-horizon agent loops — design these explicitly with it. It can also read a photographed whiteboard sketch.

Kimi reasons through:
- **Monolith or services?**
- **Data flow:** reads/writes?
- **Tool/agent topology:** which MCP servers, what the tool-calling loop looks like, loop-depth limits
- **Failure modes:** recovery?
- **Deployment:** machine to production?
- **Scaling:** what breaks at 10x load?

### 4.1 Artifacts
- **Architecture description** (Kimi can generate Mermaid; it reads your sketches)
- **Data model** (SQL or document schema)
- **MCP/tool integration map** (which tools, which scopes, which loops)
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
- **Tool-call loops bounded** (max depth in config; Kimi's agentic eagerness needs an explicit ceiling)

### 4.3 Output
Architecture document, diagrams, MCP/tool map, data schema, decision log, open items.

---

## Phase 5: Build Sequence & Prompts (1 hour)

Kimi designs the phased build and writes self-contained prompts.

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

> **Kimi-specific prompt notes:**
> - **Do not try to disable thinking mode** — it's mandatory and disabling it errors out. Write prompts expecting visible reasoning; keep only the final answer in multi-turn history.
> - **Lean on MCP** for skills/integrations rather than bespoke glue where a Model Context Protocol server already exists.
> - **Cap tool-loop depth** explicitly in every agentic prompt so a long-horizon run can't spiral.

### 5.3 Output
- Phased build plan (Phase 0 → N with gates)
- One prompt per phase (~12 prompts)
- Self-contained prompts
- Estimated total build time

---

## Phase 6: Handoff (15 minutes)

Kimi delivers:
1. ✅ Complete SRS v1.0 (locked)
2. ✅ Architecture document + diagrams + MCP/tool map
3. ✅ Data schema
4. ✅ Twelve build prompts
5. ✅ Phase 0 checklist
6. ✅ User manual skeleton
7. ✅ V2 requirements document

**Ready to build.**

---

## Kimi K2.7-Code-Specific Guidance

### Strengths to Leverage
- **Agentic software engineering:** purpose-built for long-horizon, multi-step coding
- **MCP tool-use:** best-in-class at Model Context Protocol loops (CI, tickets, file edits)
- **Multimodal:** text, image, and video in one prompt — share docs, screenshots, a recorded repro together
- **256K context:** holds large codebases and long sessions
- **Agent-swarm heritage:** can decompose big builds into parallel sub-tasks

### Weaknesses to Mitigate
- **Not locally self-hostable on modest hardware:** ~1T params, ~4×H100 minimum — API only for a single workstation
- **Mandatory thinking tokens:** real cost per call; budget for it, don't fight it
- **Agentic eagerness:** bound tool-loop depth or a long run can over-execute
- **First-party benchmarks:** validate on your own task before trusting headline numbers
- **Pure-math reasoning:** trails the very top frontier models on standalone math (rarely relevant to building)

### Workflow Pattern
1. **You:** Phase 1 intake (30 min) — list MCP servers, attach any visuals
2. **Kimi:** Phase 2 spikes incl. MCP tool loops (15 min + execution)
3. **You:** review spike results, approve or pivot
4. **Kimi:** Phase 3 requirements (45 min)
5. **Kimi:** Phase 4 architecture + tool/agent topology (1–2 hours)
6. **Kimi:** Phase 5 build prompts (1 hour)
7. **Kimi (Kimi Code CLI / OpenCode):** Phases 0–N build (20–40 hours), tool loops bounded

**Total planning + design:** 4–6 hours. **Build:** 20–40 hours.

---

## When to Use This Playbook with Kimi K2.7-Code

- ✅ Heavily agentic projects with real tool-calling / MCP workflows
- ✅ Mixed-media requirements (screenshots, recorded repros, docs together)
- ✅ Long-horizon autonomous software engineering
- ✅ Rust/Go/Python-centric builds

When NOT:
- ❌ Must run fully local on a single modest box (use a small local model instead)
- ❌ Hard token budget where mandatory thinking tokens hurt
- ❌ Quick 2-hour spike

---

## Quick Start

**Tell Kimi K2.7-Code:**

> Start Phase 1 of the Project Lifecycle Playbook (Kimi K2.7-Code Edition). Ask me the intake questions one at a time, waiting for my full answer before the next. I may attach screenshots or recordings — read them. List any MCP servers we'll use. When Phase 1 is complete, summarize and confirm before Phase 2. In every agentic prompt later, cap tool-loop depth explicitly.

Answer each question fully. Lean on MCP and multimodal input. Bound tool loops. Review outputs at every gate.

**Ready?** Upload this document to Kimi and begin.
