# Project Lifecycle Playbook v1.0
## AI-Guided Structured Project Kickoff Framework

**Purpose:** A repeatable, disciplined process for any future project. Forces smart questions upfront; identifies risky assumptions early; generates complete SRS → Architecture → Build Prompts with zero "figure it out during code" moments.

**Outcome:** A complete project ready to build, not a partial spec that discovers problems mid-implementation.

---

## Phase 1: Project Intake (30 minutes)

Ask these questions in order. Do not skip any. Wait for complete answers before moving to Phase 2.

### 1.1 Project Identity
- **Project name?** (short, memorable, what you'll call it)
- **One-sentence purpose?** (what does it do, for whom)
- **Success criteria?** (three bullet points: what does "done" mean?)
- **Stakeholders?** (just you? a team? external users?)
- **Timeline?** (deadline, or open-ended?)

### 1.2 Deployment Context
- **Where does it run?** (local laptop, cloud VM, Docker, web app, desktop app, mobile, hybrid?)
- **How many users?** (solo, team, public?)
- **Internet access?** (always online, occasionally online, air-gapped, choice?)
- **Data residency?** (local only, cloud, hybrid, regulatory constraints?)

### 1.3 Tech Stack Inventory
- **Language(s)?** (Python, TypeScript, Rust, C#, Go, Kotlin, etc.)
- **Framework?** (FastAPI, Django, Express, React, Flutter, etc.)
- **Databases?** (PostgreSQL, MongoDB, SQLite, Redis, etc.)
- **Deployment?** (Docker, K8s, Lambda, executable, mobile app store?)
- **External services?** (Stripe, Auth0, SendGrid, cloud storage, LLMs, etc.)
- **Why these choices?** (owner's expertise, team skill, performance requirements, cost?)

### 1.4 Known Constraints & Risks
- **Hard constraints?** (immovable deadlines, budget limits, regulatory, architecture locked-in)
- **Architectural constraints?** (solo maintainer, minimal dependencies, scale requirements, etc.)
- **Team constraints?** (learning curve acceptable? domain expertise required?)
- **What scares you most?** (what assumption, if wrong, would tank the project?)

---

## Phase 2: Spike Identification (15 minutes, AI-driven)

Based on the tech stack from 1.3 and the constraints from 1.4, I (Claude) identify which spikes are **required** before locking architecture.

### 2.1 Spike Matrix (tech → required spikes)

| Tech Choice | Required Spike | Duration | Gate |
|---|---|---|---|
| Python async/FastAPI | Does async work with your DB choice? | 1 hour | Can I write a toy service with async+pooling+transactions? |
| ORM (SQLAlchemy, Django ORM, etc.) | Does the ORM play well with async? | 1 hour | Can I load/save/query efficiently in the ORM? |
| GraphQL vs REST | Which API style fits your use case? | 1 hour | Can I sketch the schema/queries and understand the tradeoffs? |
| LLM integration (Claude, GPT, local Ollama) | Can I actually call and stream the LLM? | 2 hours | Does streaming work? Token counting accurate? Costs predictable? |
| Auth (OAuth, JWT, mTLS, local) | Does the auth mechanism work in your deployment? | 1–2 hours | Can I implement a login flow end-to-end? |
| Real-time (WebSocket, gRPC, Server-Sent Events) | Which transport fits (latency, scalability, complexity)? | 1 hour | Can I build a bidirectional chat in 1 hour? |
| Distributed system (microservices, event bus) | Can you actually operate it solo? | 2 hours | Can you write one service + message queue + another service, test it? |
| Novel pattern (secrets rotation, tool-calling loop, RAG) | Does the pattern actually work? | 2–4 hours | Proof-of-concept with real assumptions. |

For each tech choice in your stack, I ask: "Is there a spike needed?" If yes, add it to the list with estimated duration and a clear gate (pass/fail test).

### 2.2 Spike Ordering
- **Blocking spikes** (architecture depends on the answer) run first.
- **Independent spikes** (nice-to-know) run in parallel.
- **Total spike budget:** usually 4–8 hours for a medium project.

### 2.3 Output from Phase 2
A numbered list: "Your tech stack requires these spikes. Est. 6 hours total. Ready to proceed?"

---

## Phase 3: Requirements Elicitation (45 minutes, iterative)

I ask targeted questions about what the system must do. The goal is a **complete, unambiguous SRS** before architecture.

### 3.1 Functional Requirements Template
For each major feature from the one-sentence purpose:

- **What inputs does the user provide?**
- **What outputs/outcomes must the system produce?**
- **Who can see/access what?** (multi-user → permissions; solo → skip)
- **What happens on failure?** (graceful degradation? retry? notify?)
- **What data must persist?** (conversations? uploads? audit trail?)
- **What integrations are required?** (external APIs, webhooks, etc.)

### 3.2 Non-Functional Requirements Template
- **Performance:** latency targets? Throughput? (if not specified, why?)
- **Scale:** concurrent users? Data size? Growth forecast?
- **Reliability:** uptime SLA? Recovery time on failure?
- **Privacy/Security:** data sensitivity? Compliance (GDPR, HIPAA, etc.)? Encryption?
- **Maintainability:** solo dev or team? Long-term support expected?
- **Cost:** budget per month? (compute, storage, services)

### 3.3 I Ask for Gaps
After you describe requirements, I scan for:
- Ambiguous definitions (what does "fast" mean? 100ms? 1s?)
- Missing edge cases ("what if the database is down?")
- Implicit assumptions ("the user will always have internet" — is that true?)
- Scope creep ("users can customize everything" — is that in V1?)

I ask: "Is [gap X] in scope for V1? If yes, describe it. If no, move to V2."

### 3.4 Output from Phase 3
An SRS outline with ~40–60 functional requirements, ~8 non-functional requirements, clear scope boundaries (V1 vs V2), and acceptance criteria. Gaps filled or explicitly deferred.

---

## Phase 4: Architecture Design (1–2 hours)

Now that spikes are validated and requirements are locked, I design the system.

### 4.1 Questions I Ask
- **Where does the business logic live?** (monolith, microservices, serverless functions?)
- **Data flow:** what reads from where? What writes?
- **Failure modes:** what breaks, and what's the recovery?
- **Deployment:** how does it get from my machine to production?
- **Scaling path:** if load increases 10x, what breaks first? (Plan for it or accept the risk?)

### 4.2 Artifacts I Produce

**Architecture Diagram (draw.io):** boxes for services, arrows for communication, external systems labeled, data stores marked. Three tiers minimum (presentation, business logic, data). Exported as `.drawio` and rasterized as PNG.

**C4 Component Diagram:** if architecture is complex (multiple services), one level deeper — show internal components per service.

**Decision Log:** every architectural choice with the spike evidence that justified it.

**Data Model (draw.io):** schema diagram if relational, or collection/index structure if NoSQL. Primary keys, foreign keys, indexes, constraints.

**Deployment Topology:** where does each piece run? Docker, VMs, Lambda, browser, mobile? What's the network?

### 4.3 Standards from Brain Nest 365 (reused for all projects)
- Dependency injection (services don't create dependencies; they receive them).
- Async/await for all I/O (no blocking calls).
- Error envelope (standardized shape for all errors).
- JSON structured logging (consistent fields for querying).
- Configuration externalized (environment, no hardcoding).
- No `latest` image tags (pin versions).

### 4.4 Output from Phase 4
- Architecture document (~3–5 pages)
- Diagrams (C4, data model, deployment)
- Decision log (5–10 entries with spike references)
- Open items list (things to resolve during build)

---

## Phase 5: Build Sequence & Prompts (1 hour)

I design the phased build and write the self-contained prompts.

### 5.1 Phase Structure
**Phase 0:** Environment setup (no code). What tools to install, what to verify.
**Phase 1–N:** Each phase builds one cohesive piece and has a **Gate** — a manual test proving the phase is done before moving forward.

Example:
- Phase 1: Skeleton (container, processes, config) → Gate: `/health` endpoint returns green
- Phase 2: Auth → Gate: OIDC login works, JWT validation rejects fakes
- Phase 3: Database persistence → Gate: data survives container restart
- ...
- Phase N: Integration & acceptance → Gate: acceptance criteria 1–9 all pass

### 5.2 Prompt Discipline (locked in every prompt)
- **Read existing code first** before touching anything
- **Report severity-rated findings** (Critical/High/Medium/Low)
- **Await approval** on flagged items
- **Minimal changes** — don't refactor code not directly related to the task
- **Attach the SRS + Architecture** to every session context

### 5.3 Output from Phase 5
- Phased build plan (Phase 0 → N with gates)
- One prompt per phase (~12 prompts for a medium project)
- Each prompt is self-contained and references the SRS + Architecture
- Total estimated build time (e.g., "30 hours across 6 weeks")

---

## Phase 6: Handoff (15 minutes)

I deliver:
1. ✅ Complete SRS v1.0 (locked, not to be changed during build)
2. ✅ Architecture document with diagrams
3. ✅ Thirteen build prompts (or however many phases)
4. ✅ Phase 0 checklist (what to install/configure before touching code)
5. ✅ User manual / installation guide (skeleton, filled in after build)
6. ✅ V2 requirements document (parking lot of deferred features)

**You are now 100% ready to build.**

---

## How to Run This Playbook (for a new project)

**Step 1:** Tell me: "Start the Project Lifecycle Playbook for [ProjectName]."

**Step 2:** I will:
- Ask the Phase 1 questions (30 min intake)
- Identify required spikes from your tech stack (Phase 2)
- Ask requirements-elicitation questions (Phase 3)
- Design the architecture (Phase 4)
- Write the build prompts and phases (Phase 5)
- Deliver the complete package (Phase 6)

**Step 3:** You review and approve at each phase gate. If something doesn't feel right, we iterate *that phase* before moving forward — no phase gates get skipped.

**Step 4:** Once all six phases are approved, you go to Claude Code with Prompt 1 and build.

**Total time:** 4–6 hours of iterative discussion (across multiple sessions, if preferred), then clean, unambiguous code-ready artifacts.

---

## Key Differences from Ad-Hoc Approach

| Ad-hoc | Playbook |
|---|---|
| "I have an idea, let me code it" | Phase 1: Define the idea rigorously |
| Discover architectural problems mid-build | Phase 2: Spike risky assumptions upfront |
| Missing requirements surface in code review | Phase 3: Complete SRS before touching code |
| Architecture changes mid-project | Phase 4: Locked design before any prompt |
| Prompts say "figure it out" | Phase 5: Prompts reference frozen specs |
| 30% rework rate | <5% rework rate (changes are scope, not discovery) |

---

## When to Use This Playbook

- ✅ Any project >1 week of work
- ✅ Anything with multiple services or data persistence
- ✅ Systems that might scale or evolve
- ✅ Anything you'll maintain long-term

When NOT to use it:
- ❌ Quick 2-hour spike (just code it)
- ❌ Copy-paste templates (use spikes instead)
- ❌ Prototypes with no intention to ship (but even prototypes benefit from Phase 1 clarity)

---

## Who Runs This

**You + Claude.** You answer Phase 1, I ask Phase 3 questions, we iterate Phases 2–4 until locked, then I write Phase 5 and deliver Phase 6. It's collaborative, not a form you fill out alone.
