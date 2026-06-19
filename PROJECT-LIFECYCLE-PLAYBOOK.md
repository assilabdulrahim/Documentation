# Project Lifecycle Playbook v1.6
## A universal, stack-agnostic framework for turning a clear idea into complete, working code

**Purpose:** A repeatable, disciplined process for *any* new project, in any language, on any stack. It forces clarity upfront, proves risky assumptions early, and generates a complete chain — Requirements → Architecture → Build Prompts — with zero "figure it out during code" moments.

**Outcome:** Code that is complete and wired end-to-end on the first build, because every entry point, dependency, and requirement was accounted for before a line was written.

### What this playbook is — and is not
- It **is** stack-agnostic. Nothing here assumes a particular language, framework, database, or deployment target. Where a concrete example is given (a specific framework, a code snippet), it is clearly marked as an *example* and is not part of the method.
- It **is not** a guarantee of flawless code. No document can prevent a wrong domain decision or an execution slip. What it *does* eliminate is the class of *systematic* failures: missing layers, unwired dependencies, ambiguous specs, dropped requirements, unverified assumptions, and unhandled entry points. Those are removed by design and caught by gates; correctness of business logic is still proven by tests and review, not by this file.

> **v1.6 changes.** Two additions to §5: **fallback-on-failure, never fake-on-failure** — when a real operation fails at runtime (an API call, content extraction, a computation), the code returns a genuine fallback or the standard error envelope, never placeholder/fake/sample content presented as success; and **integration tests for the core happy path** are mandatory — "tests pass" is not satisfied by unit tests alone, the primary end-to-end flow must have a passing integration test. (The "every frontend call has a backend endpoint" rule was already enforced as the §5.3/§5.4 caller→handler check.)
>
> **v1.5 changes.** Added the **placeholder policy** (§5.3): code carries **no** placeholders/`TODO`s/stubs — anything the spec allows is implemented; the only values left unfilled are human-supplied configuration values, which go in config files as explicit, greppable placeholders and are catalogued in a **Human-Input Register** (file, key, purpose, why human-only, where to obtain). Enforced in §5.4. Nothing is silently missing.
>
> **v1.4 changes.** Consolidated into a single A-to-Z document. The full end-to-end verification routine lives entirely in §5.3 (Completeness Protocol) and §5.4 (Self-Verification Gate) — there is no separate protocol file. Added the explicit rule that any failed gate check is a blocker.
>
> **v1.3 changes.** Folded in fault classes observed when the generated code was tested: **persistence-chain completeness** (every entity → contract → registered implementation → correct lifetime → fulfils every member → mapped + migrated; no orphan or stub implementations; no unverified reliance on a generic data layer), **data provenance** (presented/consumed values bound to a real source, never a leftover placeholder or default), **exact config-key matching** (a key the code reads must exist verbatim), **full DI-graph closure with lifetime checks** (no captive dependencies, no cycles), and **embedded-template/syntax-collision** checks (the host parser must not consume sequences meant for a template engine). All stated generically; framework names appear only as examples.
>
> **v1.2 changes.** Made fully stack-agnostic. Generalized completeness from "UI → database" to **every entry point**. Added the Requirements Clarity Gate, Traceability Matrix, Security Review, and version-control checkpoint discipline. Framework-specific code recipes retained only as illustrative examples in Appendix A.1.

---

## How to use this playbook (with Claude)

1. Tell Claude: *"Start the Project Lifecycle Playbook for [ProjectName]."*
2. Claude asks the Phase 1 intake questions **one at a time**, waiting for a full answer before the next.
3. Claude proceeds through Phases 2–6, **stopping at every gate** for your review and approval.
4. You approve or iterate *that phase* before moving on. No gate is skipped.
5. Once all six phases are approved, the build prompts from Phase 5 are run, one phase at a time, each behind its gate.

---

## Phase 1: Project Intake

Ask in order. Do not skip any. Wait for a complete answer before the next question.

### 1.1 Project Identity
- **Project name?** (short, memorable)
- **One-sentence purpose?** (what it does, for whom)
- **Success criteria?** (three bullets: what "done" means)
- **Stakeholders?** (solo, team, external users?)
- **Timeline?** (deadline or open-ended?)

### 1.2 Deployment Context
- **Where does it run?** (local, VM, container, serverless, web, desktop, mobile, embedded, hybrid?)
- **How many users / what load?** (solo, team, public; expected concurrency?)
- **Connectivity?** (always online, intermittent, air-gapped?)
- **Data residency / regulatory?** (local-only, cloud, hybrid, compliance regime?)

### 1.3 Tech Stack Inventory
- **Language(s)?**
- **Framework(s)?**
- **Data stores?** (relational, document, key-value, cache, blob, queue?)
- **Deployment mechanism?** (container, orchestrator, function, binary, app store?)
- **External services?** (auth, payments, email/SMS, storage, LLMs, third-party APIs?)
- **Why these choices?** (expertise, performance, cost, constraint?)

### 1.4 Known Constraints & Risks
- **Hard constraints?** (deadline, budget, regulatory, locked architecture?)
- **Architectural constraints?** (solo maintainer, minimal dependencies, scale targets?)
- **Team / skill constraints?** (learning curve acceptable? domain expertise required?)
- **What scares you most?** (which assumption, if wrong, tanks the project?)

### 1.5 Entry-Point Inventory
List **every way the system can be triggered** — this is the spine of the completeness check in Phase 5, and missing one here is the most common cause of "missing functionality." For each, note the trigger and who/what invokes it:
- **Synchronous request/response** (HTTP/REST/GraphQL/gRPC/RPC endpoints)
- **User-interface actions** (buttons, form submits, page loads that fetch or mutate data)
- **Scheduled / time-based** (cron, timers, recurring jobs)
- **Message / event consumers** (queues, topics, event bus, webhook receivers)
- **Command-line / batch** (CLI commands, scripts, one-off tasks)
- **Agent / function calls** (tools an LLM or automation can invoke)
- **Lifecycle tasks** (startup, migration, seeding, shutdown, health/readiness probes)

---

## Phase 2: Spike Identification

Based on the stack (1.3), the constraints (1.4), and the entry points (1.5), Claude flags assumptions that must be proven with a small proof-of-concept **before** architecture is locked.

### 2.1 Spike Heuristic (tech → required spike)
For each unfamiliar or load-bearing choice, ask: *Is this proven in this exact use case? What is its failure mode? What 1–2 hour test proves it?* The table below is illustrative — substitute your own rows.

| Example tech choice | Example spike | Gate (pass/fail test) |
|---|---|---|
| Async runtime + chosen data store | Does async work with this driver, with pooling + transactions? | A toy service does concurrent reads/writes correctly |
| ORM / data-access layer | Does it load/save/query efficiently and play well with async? | A non-trivial query + transaction round-trips correctly |
| API style (REST vs GraphQL vs RPC) | Which fits the access patterns? | Schema/queries sketched; tradeoffs understood |
| External integration (auth, payments, LLM, etc.) | Can you actually call, stream, and handle failure? | End-to-end call works; errors handled; cost/latency known |
| Auth mechanism | Does login work end-to-end in this deployment? | A full login → protected-resource flow succeeds |
| Real-time transport (WebSocket/SSE/gRPC stream) | Which transport fits latency/scale/complexity? | Bidirectional message round-trips |
| Async messaging (queue/event bus) | Can you operate producer + consumer reliably (solo)? | Message published, consumed, retried on failure |
| Schema migrations | Can you generate, apply, and roll back a migration? | A migration applies and reverts cleanly against a real DB |
| Novel pattern (RAG, tool-loop, secrets rotation) | Does the pattern actually work with real inputs? | A proof-of-concept survives realistic data |

### 2.2 Spike Ordering
- **Blocking spikes** (architecture depends on the answer) run first.
- **Independent spikes** run in parallel.
- Typical budget: 4–8 hours for a medium project.

### 2.3 Gate
Claude lists the spikes with durations and gates, runs them, and reports **pass/fail + lessons learned**. You approve or pivot. Architecture does not start until blocking spikes pass.

---

## Phase 3: Requirements Elicitation

The goal is a **complete, unambiguous specification** before any architecture. Vague requirements are the root cause of code that "works" but does the wrong thing.

### 3.1 Functional Requirements
For each capability implied by the one-sentence purpose, and **for each entry point from 1.5**:
- **Inputs?** (actions, data, events; shape and source)
- **Outputs / outcomes?** (results, side effects, persisted changes)
- **Who may invoke it?** (permissions, isolation, default-deny)
- **Failure behavior?** (graceful degradation, retry, notify, fail-closed)
- **What persists?** (and what must survive restart/redeploy)
- **Integrations required?** (external APIs, webhooks, other services)

### 3.2 Non-Functional Requirements
- **Performance:** explicit latency/throughput targets (numbers, not adjectives)
- **Scale:** concurrent users, data volume, growth forecast
- **Reliability:** uptime target, recovery time, durability
- **Security/Privacy:** data sensitivity, compliance regime, encryption in transit/at rest
- **Maintainability:** solo or team, expected lifespan
- **Cost:** budget envelope (compute, storage, per-call/API costs)

### 3.3 Gap Detection
Claude scans the draft for: ambiguous definitions, missing edge cases (empty/malformed/oversized input, dependency down), implicit assumptions ("the user is always online"), and scope creep. For each: *"Is [gap] in V1? If yes, describe it. If no, it moves to V2."*

### 3.4 Requirements Clarity Gate (must pass before Phase 4)
No requirement advances to architecture unless it is:
1. **Atomic and testable** — it has explicit, measurable acceptance criteria.
2. **Free of undefined vague terms** — every "fast", "secure", "scalable", "robust", "user-friendly" is replaced by a number, threshold, or precise definition (e.g. "p95 latency < 200 ms", "supports 1,000 concurrent sessions", "meets OWASP ASVS L2").
3. **Failure-defined** — behavior on error, edge, empty, and malformed input is specified.
4. **Scope-bounded** — clearly V1 or V2.

Anything that cannot meet all four is reworded until it can, or explicitly deferred to V2. Ambiguity does not pass this gate.

### 3.5 Output
A specification with the functional requirements (each with acceptance criteria), the non-functional requirements, clear V1/V2 boundaries, and every entry point from 1.5 covered. Each requirement carries a stable **ID** (used by the Traceability Matrix in Phase 4).

---

## Phase 4: Architecture Design

Spikes are proven and requirements are locked. Now Claude designs the system and binds it to the requirements.

### 4.1 Questions Claude Resolves
- **Where does business logic live?** (monolith, services, functions?)
- **Data flow:** what reads from where; what writes?
- **Entry-point topology:** how each trigger from 1.5 enters and is handled.
- **Failure modes:** what breaks, and what recovers it?
- **Deployment path:** from a developer machine to each target environment.
- **Scaling path:** at 10× load, what breaks first — plan for it or accept the risk explicitly?

### 4.2 Artifacts Claude Produces
- **Architecture description + diagram:** components, communication, external systems, data stores; at least presentation / logic / data tiers.
- **Data model:** schema (relational) or collection/index design (non-relational), with keys, indexes, and constraints — **and the migration strategy** (how schema changes are versioned and applied).
- **Completeness Map:** one row per entry point from 1.5, traced through every layer (see the §5.3 contract). This is the build contract.
- **Traceability Matrix:** every requirement **ID → architecture component(s) → planned build phase → acceptance test**. Any requirement with no downstream link is either a gap (add it) or out of scope (mark V2). This is the structural guarantee that no requirement is silently dropped.
- **Security Review (design-time):** the §4.3 security standards applied to this design — auth on every entry point, secret handling, data protection, trust boundaries.
- **Decision Log:** every significant choice with the spike evidence or rationale that justified it.
- **Open Items:** things deliberately deferred to resolve during build.

### 4.3 Engineering Standards (apply to every project, every stack)
- **Dependency injection / inversion of control** — components receive dependencies; they do not construct them.
- **Non-blocking I/O** wherever the platform supports it.
- **One standardized error envelope** — a single shape for all errors, everywhere.
- **Structured logging** — consistent, queryable fields; never log secrets.
- **Configuration & secrets externalized** — nothing hardcoded; secrets never in code or version control.
- **Versions pinned** — no floating/"latest"; reproducible builds.
- **One source of truth per concern** — no duplicated, drifting state.
- **Every entry point is validated and authorized** — input checked at the boundary; default-deny on access.
- **Migrations are first-class** — schema changes are versioned and applied; no manual drift.
- **No invented APIs** — every external library/framework symbol used is verified to exist in the pinned version before relying on it.
- **Configuration keys match exactly** — the key a component reads is the key the configuration defines, verbatim (no renamed or differently-prefixed keys).
- **No placeholders in code** — only human-supplied configuration values are left unfilled, and those are explicit, greppable, and recorded in the Human-Input Register.

#### Security standards (design-time checklist; enforced again in §5.4)
- AuthN + AuthZ on every protected entry point (default-deny).
- Secrets externalized, never in code/repo/logs; rotation-ready.
- All input validated; all output encoded for its sink; queries parameterized (no string-built SQL/commands).
- Encryption in transit; encryption at rest where the data sensitivity requires it.
- Dependency vulnerability scan; pinned versions.
- Least privilege for every credential, role, and service identity.
- No sensitive data in URLs, query strings, or client-visible state.

### 4.4 Output
Architecture document, diagrams, data model + migration strategy, **Completeness Map**, **Traceability Matrix**, **Security Review**, decision log, open items.

---

## Phase 5: Build Sequence & Prompts

Claude designs the phased build and writes self-contained prompts that produce complete, wired code.

### 5.1 Phase Structure
- **Phase 0:** Environment setup (no code) — tools to install, what to verify, repository initialized.
- **Phases 1–N:** each builds one cohesive piece and ends in a **Gate** — a manual test proving the phase is done before moving on.

Example gates (illustrative): skeleton up and health/readiness probe green → auth works and rejects forged credentials → data survives restart → final integration: all acceptance criteria pass.

**Version-control checkpoint discipline:** commit at every **green** gate; never build on top of a **red** gate. Each phase must be independently revertible. "Fearless" means it is always safe to undo the last phase.

### 5.2 Prompt Discipline (locked into every prompt)
- **Read existing code first** — never touch anything before understanding it.
- **Audit before writing** — produce or refresh the Completeness Map and report gaps *before* generating code.
- **Report severity-rated findings** (Critical / High / Medium / Low) and **await approval** on flagged items.
- **Build bottom-up, never skip a layer** (see §5.3).
- **Inline the matching trap entries** from the project's Known-Traps Library (Appendix A) — verbatim, code included — for every entry whose trigger matches this stack.
- **Implement, don't placeholder** — leave no `[placeholder]`, `TODO`, or stub in code; the only legitimate gaps are human-supplied configuration values, which go in config as explicit placeholders and into the Human-Input Register (see §5.3 placeholder policy).
- **Minimal changes** — do not refactor code unrelated to the task.
- **Pass the Self-Verification Gate** (§5.4) before declaring any phase done.
- **Integration-test the happy path** — include at least one integration test exercising the feature's primary end-to-end flow, not unit tests alone.
- **Carry the spec, the Completeness Map, and the Traceability Matrix** in every session's context.

### 5.3 The Completeness Protocol (the backbone of every build prompt)

This is the core defense against "missing functionality" and "broken wiring." It works for **every** kind of entry point, not just web UIs. Every build prompt instructs the model to:

**Step 1 — Inventory and trace (no code yet).** For **every entry point** in scope (from 1.5 / the Completeness Map), record the full vertical slice and mark any `MISSING` or `NOT WIRED` cell as a gap:

| Dimension | What to record |
|---|---|
| **1. Trigger & contract** | How it is invoked (route+verb, schedule, topic, CLI signature, tool signature); input shape + serialization/encoding; output/result; required permission |
| **2. Handler** | The function/endpoint/consumer that receives it — or `MISSING` |
| **3. Logic** | The use-case/service method(s) it calls — or `MISSING` |
| **4. Dependencies & wiring** | Every dependency the handler/logic needs is registered and **resolvable** in the composition root, with the **whole graph closing** (every constructor parameter type is itself registered, recursively, no cycles) and **lifetimes correct** (a longer-lived component never captures a shorter-lived one) — or `NOT WIRED` |
| **5. Data & persistence** | Entities/tables touched, **and** each is mapped in the data context so it has a schema and an **applied migration** (live schema matches the model). Detailed in dimension 9. |
| **6. Config & secrets** | Every config value / key / connection string the slice reads is present in **each** target environment and **named exactly** as the code reads it — a key the code reads must exist verbatim, not under a renamed or differently-prefixed key; nothing hardcoded |
| **7. Validation & errors** | Inputs validated at the boundary; every failure path returns the standard error envelope; no path silently swallows errors |
| **8. Authorization** | Protected entry points enforce the correct permission; default-deny |
| **9. Persistence chain** | For **every** entity the slice touches: contract/abstraction exists → implementation exists → it is **registered** → its **lifetime is correct** → the implementation **fulfills every member** (no stub / not-implemented behind a registered type) → **no orphan implementation** that is never registered. Do not assume a generic or base data-access layer covers an entity unless a working generic implementation is **verified registered** for it. Any `NO` in this chain is a blocker — a caller cannot use a contract that has no working, registered implementation. |
| **10. Data provenance** | Every value the slice **presents or consumes** is bound to its **real source** — a caller-supplied input, populated state/store, or a fetched response — **not** a leftover default or placeholder when real data is available. Suspect hardcoded defaults, placeholder strings, and inputs the caller never actually sets. |

**Step 2 — Implement gaps bottom-up, never skipping a layer:**
1. Data access + **migrations** (schema exists and is applied)
2. Infrastructure dependencies / external clients (storage, email, payment, LLM, etc.)
3. Wiring: register every dependency from steps 1–2 in the composition root
4. Application/business logic that depends on the above
5. Wiring: register the application services
6. Handlers / endpoints / consumers — thin: they validate, delegate to logic, and return the standard result/error shape
7. Verify **every** trigger now has exactly one matching handler

**Step 3 — Hard rules (never violated):**
- NEVER leave an entry point the system exposes unimplemented.
- NEVER register a component without also registering all of its dependencies.
- NEVER register a type whose implementation is a stub or not-implemented placeholder.
- NEVER leave a placeholder, `TODO`, or stub in code where the spec allows implementation — implement it. The only values left unfilled are human-supplied configuration values, placed in config files as explicit placeholders and listed in the Human-Input Register.
- NEVER assume an implementation exists — search for it first.
- NEVER assume a generic or base data-access layer covers an entity unless a working generic implementation is verified registered for that entity.
- NEVER leave a presented or consumed value bound to a placeholder/default when a real source is available.
- NEVER substitute placeholder, fake, or sample content when a real operation fails at runtime (an API call, content extraction, a computation) — implement a genuine fallback path, or fail with the standard error envelope. Faking success on failure is a blocker.
- ALWAYS trace every outbound call a client/UI/agent makes to a concrete handler.
- ALWAYS run the full test suite after every batch of changes.

**Placeholder policy — code carries no placeholders; configuration carries explicit, registered ones.**
- **In code: implement, never placeholder.** No `[placeholder]`, `TODO`, `FIXME`, empty/stub bodies, not-implemented throws, or fake/sample values standing in for real logic or content. If the spec gives enough to build it, build it now. A placeholder in code is a gap, and a gap is a blocker.
- **In configuration: placeholders are allowed, but explicit and never invented.** Values only a human can supply after the build — secrets, API keys, connection strings, tenant IDs/URLs, license keys, externally issued identifiers — live in config files as clearly marked placeholders in one consistent, greppable form (e.g. `<SET_ME: purpose>` or an environment-variable reference), never scattered through code. Never fabricate a real-looking value; a plausible-but-wrong key fails more confusingly than an obvious blank.
- **Everything left unfilled is inventoried.** Maintain a **Human-Input Register**: for each surviving placeholder — the file and key, what it is, why only a human can supply it, and where to obtain it. Nothing is silently missing: if it is not implemented, it is because it provably requires a human, and it appears in the register.

### 5.4 Self-Verification Gate (runs at every phase gate, before "done")

A prompt may not declare a phase complete until **all** of the following pass. State each explicitly in the prompt and use the project's own commands:

- **Build is clean** — the project's build command reports zero errors (warnings-as-errors where configured).
- **Tests pass** — the full automated suite is green.
- **Integration tests cover the core happy path** — the primary end-to-end flow has at least one integration test, and it passes; a green suite of unit tests alone does not satisfy this check.
- **Caller → handler mapping is complete** — every invocation a client/UI/job/agent makes resolves to exactly one handler whose contract matches (method/route/params, message schema, CLI args, or tool signature), and the **serialization/content encoding the caller sends matches what the handler expects to bind** (a mismatch is a classic source of silent failures and all-null binds).
- **Dependency graph closes** — every component's constructor parameters are themselves registered, recursively, until the graph closes; no missing registrations, **no cycles**, and **no captive-lifetime mismatch** (a longer-lived component must not depend on a shorter-lived one).
- **Persistence chain closes** — every entity a service uses has a contract, a registered implementation that fulfills every member (no stubs), a correct lifetime, and is mapped + migrated; no orphan implementations; no reliance on an unverified generic data-access layer.
- **Config keys match exactly** — every config/secret key read in code exists verbatim in the configuration source. (Example: code reading `ConnectionStrings:BlobStorage` requires a key named exactly `ConnectionStrings:BlobStorage`, not a renamed or differently-prefixed key.)
- **Schema matches the model** — every persisted entity has an applied migration; the live schema matches the code.
- **Config/secrets present** — every required config value and secret exists in each target environment; nothing secret is in code or version control.
- **Data provenance is correct** — every presented or consumed value comes from its real source, not a placeholder, hardcoded default, or input the caller never sets.
- **Embedded templates parse** — any generated code that embeds another language or templating (template literals, JSX, Jinja/ERB/Handlebars, format strings, shell heredocs, etc.) is scanned so the **host parser does not consume sequences meant for the template engine** before it runs. (Example: `${...}` inside a backtick template literal is a host-language expression, so a literal `${{ value }}` breaks; a brace in a format string may need doubling.) Escape, relocate, or split as needed.
- **No invented APIs** — every external library/framework/API symbol referenced actually exists in the pinned version.
- **No placeholders in code** — the output is scanned for `[placeholder]`/`TODO`/`FIXME`/stub/not-implemented markers and fake sample values; each is either implemented or, if it is a human-supplied config value, relocated to a config file as an explicit placeholder and recorded in the Human-Input Register. Code carries none.
- **Config placeholders are explicit and registered** — every value left for the human is marked in the one consistent form and listed in the Human-Input Register with its purpose and how to obtain it; no value the build could itself supply is left blank, and no placeholder is a fabricated real-looking value.
- **Failure behavior is real, not faked** — when a real operation fails at runtime, the code returns a genuine fallback or the standard error envelope; it never returns placeholder/fake/sample content dressed as success.
- **Security holds** — authorization enforced on every protected entry point; inputs validated; outputs encoded; errors use the standard envelope.
- **Nothing exposed is unimplemented; nothing silently swallows errors.**

Treat every check as a pass/fail diff: any failure is a **blocker** — fix it, do not skip a check as "probably fine" and do not claim "it will work in production." This gate proves the **wiring** is complete and safe; the per-phase functional gate in §5.1 proves the **feature** works. Both must pass.

### 5.5 Output
- Phased build plan (Phase 0 → N, each with a gate)
- One self-contained prompt per phase, each embedding the Completeness Protocol, the Self-Verification Gate, and the matching Appendix A entries, and referencing the spec + Traceability Matrix
- Total estimated build time

---

## Phase 6: Handoff

Claude delivers:
1. ✅ Complete specification (locked; not changed during build)
2. ✅ Architecture document + diagrams + data model + migration strategy
3. ✅ Completeness Map (every entry point traced — the build contract)
4. ✅ Traceability Matrix (requirement → component → phase → test)
5. ✅ Security Review
6. ✅ Build prompts (one per phase), each embedding the protocol, the gate, and the matching trap entries
7. ✅ Phase 0 checklist
8. ✅ User/operator manual skeleton (filled in after build)
9. ✅ V2 requirements (parking lot of deferred scope)

**Ready to build.**

---

## Key Differences from an Ad-Hoc Approach

| Ad-hoc | Playbook |
|---|---|
| "I have an idea, let me code it" | Phase 1: define the idea and every entry point rigorously |
| Discover architectural problems mid-build | Phase 2: spike risky assumptions upfront |
| "Fast" / "secure" left undefined | Phase 3.4: clarity gate — every term is measurable |
| Requirements quietly dropped | Phase 4: traceability matrix — nothing untraced ships |
| Missing endpoints/wiring found at runtime | Phase 5.3: completeness protocol — gaps found before coding |
| "Works on my machine" schema/config surprises | Phase 5.4: migrations + config verified at every gate |
| Security bolted on later | Phase 4.3 + 5.4: security designed in and gated |
| ~30% rework | <5% rework (changes are scope decisions, not discoveries) |

---

## When to Use This Playbook

- ✅ Any project beyond a throwaway script
- ✅ Anything with multiple entry points, persistence, or integrations
- ✅ Systems that will scale, evolve, or be maintained long-term
- ✅ Anything where "it silently doesn't work" is unacceptable

When **not** to:
- ❌ A genuine 2-hour throwaway spike (just code it)
- ❌ A prototype with no intent to ship (though Phase 1 clarity still helps)

---

## Who Runs This

**You + Claude.** You answer Phase 1; Claude asks the Phase 3 questions; you iterate Phases 2–4 until locked; Claude writes Phase 5 and delivers Phase 6. You review and approve at every gate. It is collaborative, not a form filled out alone.

---

## Appendix A — Known-Traps Library (mechanism & schema)

A **per-project** catalogue of bugs that have already cost real debugging time on *this* project's stack, each paired with a fix **verified to work**. It is not part of the universal method — every project grows its own. Its job is to stop known failures from recurring in generated code.

**Rule:** every Phase 5 build prompt must inline — verbatim, code included — each entry whose **"When it applies"** trigger matches the project's stack. Treat each as a non-negotiable acceptance check.

**Entry schema** (use this when adding a trap):
- **When it applies** — the exact stack / configuration trigger
- **Symptom** — what the developer actually sees (error code, blank page, null bind, …)
- **Root cause** — why it happens
- **Verified fix** — the working code / configuration
- **Verify it worked** — the concrete check proving the fix landed

**The one hard rule for this library:** never add a trap from memory or assumption. An entry without an *exact, verified* fix is worse than no entry — it gives false confidence. If you have only a symptom, log it as an open question, not a fix.

---

## Appendix A.1 — Worked examples (illustrative only)

> These are **examples** of what a real, filled-in trap entry looks like. They belong to a specific stack (here: a particular .NET web configuration) and are included **only to demonstrate the schema** — they are not part of the universal playbook and would live in a per-project library. Delete them when adapting this document; replace them with your own project's verified traps.

### Example 1 — Web MVC framework: CSS / JS not loading

**When it applies:** A server-rendered MVC framework serving static CSS/JS via root-relative (`~/`) paths inside a shared layout (illustrated with ASP.NET Core MVC). Check this **before** debugging the stylesheet itself.

One symptom, two distinct root causes — confirm both before going deeper.

**Cause A — the `~/` path is emitted literally and 404s.** The framework's tag-helper that rewrites `~/` to a root-relative URL is not active.
- **Fix A:** ensure the view-imports file registers the framework tag helpers, e.g.:
  ```cshtml
  @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

**Cause B — nothing is requested at all; the page renders unstyled.** The `<link>`/`<script>` tags live in the layout, but the layout is never applied, so they never reach the browser.
- **Fix B:** ensure the view-start file exists and selects the layout, e.g.:
  ```cshtml
  @{ Layout = "_Layout"; }
  ```

**Also confirm** the static-file middleware is in the pipeline and the asset physically exists in the web-root folder.

**Verify it worked:** the page source / network tab shows the link resolved to a root-relative path (no `~`) returning HTTP 200, and the page is styled.

---

### Example 2 — OIDC provider with pure authorization-code flow (no implicit/hybrid)

**When it applies:** An OIDC integration using **pure** authorization-code flow (`response_type=code`, PKCE, no implicit/hybrid grant) where the identity provider's app registration has **both Access tokens and ID tokens unchecked** (illustrated with Azure AD B2C + a .NET OIDC middleware). Such providers can deviate from baseline OpenID Connect in this exact configuration; the fixes below are applied together.

**Symptoms & root causes:**

| Error / symptom | Cause | Fix |
|---|---|---|
| `State is null` (e.g. `IDX21329`) | Provider's `form_post` response mode may not echo `state` back when implicit grant is fully disabled; base validation requires it | Disable the state requirement in a custom protocol validator |
| `id_token missing` (e.g. `IDX21336`) | Token endpoint returns only `access_token` with `response_type=code` + implicit disabled; base validation requires both | Override the token-response validation to skip the both-tokens requirement |
| No user identity after login | Without an `id_token` the middleware can't build a principal; the `access_token` carries the same `sub`/`name`/`email` | Decode the `access_token` on token-validated and build the principal |

**Verified fix — registration + post-configure (example):**
```csharp
// 1. Pure code + PKCE
builder.Services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApp(options =>
    {
        builder.Configuration.GetSection("AzureAdB2C").Bind(options);
        options.ResponseType = OpenIdConnectResponseType.Code;
        options.UsePkce = true;
        options.SaveTokens = true;
    });

// 2. Custom protocol validator + build principal from the access token
builder.Services.PostConfigure<OpenIdConnectOptions>(
    OpenIdConnectDefaults.AuthenticationScheme, options =>
{
    options.ProtocolValidator = new B2CProtocolValidator();

    options.Events.OnTokenValidated = context =>
    {
        if (context.SecurityToken is null &&
            context.ProtocolMessage?.AccessToken is not null)
        {
            var handler = new JsonWebTokenHandler();
            var jwt = handler.ReadJsonWebToken(context.ProtocolMessage.AccessToken);
            var claims = jwt.Claims
                .Where(c => c.Type is not ("aud" or "iss" or "iat" or "exp" or "nbf" or "jti"))
                .Select(c => new Claim(c.Type, c.Value));
            context.Principal = new ClaimsPrincipal(
                new ClaimsIdentity(claims, "OpenIdConnect"));
        }
        return Task.CompletedTask;
    };
});
```

**Verified fix — the protocol validator (example):**
```csharp
sealed class B2CProtocolValidator : OpenIdConnectProtocolValidator
{
    public B2CProtocolValidator()
    {
        // Provider with form_post may not echo state back.
        RequireState = false;
    }

    public override void ValidateTokenResponse(
        OpenIdConnectProtocolValidationContext ctx)
    {
        // Token endpoint doesn't return id_token with response_type=code
        // when implicit grant is fully disabled.
        // Skip base validation that requires both id_token and access_token.
    }
}
```

**Configuration (example):**
```json
"AzureAdB2C": {
    "Instance": "https://{tenant}.b2clogin.com",
    "ClientId": "{app-id}",
    "Domain": "{tenant}.onmicrosoft.com",
    "SignUpSignInPolicyId": "B2C_1_SI",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath": "/signout-callback-oidc",
    "ResponseType": "code",
    "UsePkce": true
}
```

**Verify it worked:** login completes with no state/`id_token` errors; the user is authenticated; `sub`/`name`/`email` claims are present (sourced from the access token).
