I want a complete refactor of packages/llm/runtime/kernel.ts, not a new version, but a complete breaking changes. I need new feature new kernel functionality, and I need you to plan out how you will structure the code, how you will make sure nothing is left out of my plan, how everything will work in harmony, and nothing broken and no interuption. Completly plan out the new design and architecture from my plan be sure to keep every single things from my plan.



My Plan:


Below is a **remade, robust architecture spec** (text-only) that bakes in:

* **Multimodal from the first millisecond** (deterministic canonicalization front-end)
* **Deterministic model routing** (fast vs slow, tier rules, modality constraints)
* **“Only one slow model” survivability** (MinCalls execution + DAG collapsing)
* **Safe large fan-out** (progressive speculation + backpressure + commit protocol)
* **Pamphlets + MCP** (pamphlets advisory only; MCP always proxied & scoped)
* **Separate-work evaluation** (measurable outputs, not “more opinions”)

No diagrams, no flowcharts—just the system.

---

## 0) System goals and invariants

### Primary goals

1. **Never fail closed-loop safety** (no unauthorized tool/MCP use, no prompt injection taking control).
2. **Bounded latency and cost** even under heavy fan-out.
3. **Correctness via evidence + verifiable artifacts**, not just consensus.
4. **Multimodal reliability**: deterministic handling of PDFs/images/audio/video.
5. **Operational stability**: backpressure, circuit breakers, cancellation, traceability.

### Non-negotiable invariants (must be enforced in code)

1. Agents **never** call tools/MCP directly—only via **Tool Runtime + MCP Proxy**.
2. Pamphlets are **advisory only** (cannot spawn, cannot grant permissions, cannot override policy).
3. Every run has **hard budgets** (latency, cost, tool calls, concurrency).
4. All writes to shared state are **staged and committed** via a **commit protocol** (rollback possible).
5. Every claim includes **provenance + evidence pointers** (model/tool trace IDs).
6. High-risk actions require **explicit approval gates** (policy-enforced), regardless of planner output.
7. Backpressure exists end-to-end (planner cannot flood scheduler/tools).
8. Cancellation is first-class (kill low-value work when budgets tighten).

---

## 1) Two planes (rigid separation)

### Deterministic Control Plane (enforcement + stability)

Owns:

* authentication/tenant isolation
* modality detection + canonicalization
* policy, budgets, permissions
* model/tool routing enforcement
* scheduling/admission/backpressure
* sandboxing + secrets handling
* tracing/audit/eval hooks
* commit protocol (shared-state correctness)

### LLM Work Plane (adaptive problem solving)

Owns:

* decomposition
* drafting Plans (within constraints)
* producing artifacts/claims
* verification attempts (within allowed policies)
* integration/synthesis

**Rule:** LLM proposes; deterministic plane enforces.

---

## 2) Front-end multimodal pipeline (mandatory, deterministic)

This is the part that prevents “initial phase confusion” and ensures multimodal is actually accounted for.

### 2.1 Asset Manifest (deterministic)

On every request, the system produces an `AssetManifest`:

* asset id, source, MIME type
* byte size, page count or duration
* cryptographic hash (for provenance)
* sensitivity tags (PII likelihood, policy signals)
* safe preview metadata (never raw execution)

### 2.2 Canonicalization (deterministic tools, bounded)

The control plane converts assets into a **Canonical Working Set**:

* PDF:

  * attempt text extraction
  * OCR only if extraction is insufficient (bounded pages/time)
  * optional page image snapshots if needed for diagrams/tables
* Images:

  * OCR for text-heavy images
  * vision caption/object pass only if needed (bounded)
* Audio/Video:

  * transcript with timestamps
  * optional speaker labeling (bounded)
* Result:

  * `CanonicalText` + `EvidencePointers` (asset id + offsets/time/page refs)

**Important:** canonicalization produces *inputs for reasoning*, not final interpretation.

### 2.3 Modality Router (deterministic)

Based on the manifest and the user request, it sets:

* which modalities must be used (text-only vs needs vision/audio)
* which conversions are required before planning
* hard caps (max pages, max frames, max minutes of audio)

If a required modality is unavailable, it emits a **fallback plan** (e.g., OCR only + text reasoning, or “cannot safely proceed without X”).

---

## 3) Capability Registry (the backbone of “fast vs slow”)

A deterministic registry describing every available model tier and tool:

For each model:

* supported modalities: text / vision / audio
* context limits
* measured latency p50/p95 by token ranges
* cost profile
* reliability score (from eval + incident feedback)
* concurrency caps, provider throttles
* “best-at” tags (extraction, planning, deep reasoning, tool use, integration)

For each tool/MCP server:

* allowed intents
* rate limits and quotas
* reliability/latency stats
* scopes (read/write/execute) and constraints

This registry is what makes routing enforceable and non-hand-wavy.

---

## 4) Core control-plane components

### 4.1 Gateway

* auth, tenant isolation, rate limiting
* request size caps
* initial safety filters (known exploit patterns)

### 4.2 Input Guardrails

* injection detection signals
* PII detection hints (for policy decisions)
* mark untrusted segments (so they cannot become instructions)

### 4.3 Context Builder

* conversation context
* retrieval seeds (if permitted)
* memory summaries (if enabled by product)
* produces a bounded `ContextPack`

### 4.4 Pamphlet Engine (advisory)

* selects relevant playbooks by request type + context signals
* outputs recommendations only:

  * suggested agent roles
  * suggested tool intents
  * verification checklist templates
* cannot create work; cannot escalate privileges

### 4.5 Planner/Router LLM (low privilege)

* takes Canonical Working Set + ContextPack + pamphlet recommendations
* outputs `PlanSpec` (strict schema)

### 4.6 Plan Validator (strict)

Rejects any plan that:

* violates schema
* requests forbidden tool classes or missing evidence requirements
* exceeds theoretical budgets or concurrency caps
* attempts to route around canonicalization/modality constraints

### 4.7 Policy Engine (least privilege)

Transforms plan requests into:

* allowed model tiers per node
* allowed tool menus per node (capabilities, not endpoints)
* approvals required for irreversible actions
* enforced budgets (latency/cost/tool calls)

### 4.8 Resource Manager

Computes hard caps each run:

* `max_parallel_agents` (by CPU/GPU/provider limits)
* `max_parallel_tool_calls` (per tool/MCP server)
* `tier_quota` (how many calls allowed to “slow” tier)
* degrade rules when throttled or near budget

### 4.9 Admission Control + Backpressure

* queues per role (planner/worker/verifier/integrator)
* credit-based issuance (planner gets limited “spawn credits”)
* circuit breakers on tool/MCP p95 spikes
* load shedding rules (drop low-value speculation first)

### 4.10 Scheduler

* executes the DAG respecting dependencies
* supports work-stealing within concurrency caps
* progressive fan-out (speculation windows)
* cancellation and timeboxing

### 4.11 Shared Workspace (with commit protocol)

Holds:

* TaskGraph state
* claims/evidence
* intermediate artifacts
* tool outputs (as evidence pointers)
* commit log (staged → committed → rolled back)

### 4.12 Tool Runtime + Sandbox

* filesystem/network restrictions
* secrets injection only for allowed scopes
* output sanitization (tool output cannot become instructions)
* deterministic retries with idempotency keys

### 4.13 MCP Proxy (always in the middle)

* maps “tool intents” to allowed MCP server + scoped calls
* enforces rate limits, payload caps
* strips/labels untrusted outputs
* records trace IDs for every call

### 4.14 Output Guardrails

* policy compliance
* PII redaction as required
* format checks
* “evidence contract” completeness check (if required)

### 4.15 Tracing/Audit + Evaluation

* full trace: prompts, tool calls, diffs, commit log
* online eval hooks (pass/fail checks)
* offline eval to update Capability Registry reliability scores

---

## 5) Work-plane agent roles (how you actually split work)

Use agents by **artifact type**, not “more opinions”:

1. **Extractor agents (fast/small)**
   Output: structured facts + evidence pointers only.
2. **Tool runner agents (fast/small)**
   Output: tool results + logs + normalized outputs.
3. **Reasoner agents (slow/strong, few)**
   Output: decisions, tradeoffs, proposed actions with evidence.
4. **Verifier agents (independent)**
   Output: checks, contradictions, missing evidence, pass/fail.
5. **Integrator (strong)**
   Output: final synthesis + explicit evidence mapping + compliance.

This is how you evaluate “separate work” cleanly: each role has measurable output constraints.

---

## 6) Data contracts (the rigidity)

### 6.1 PlanSpec (required sections)

* mode: fast / efficient / right
* assets: list from Asset Manifest (ids only)
* canonicalization_plan: required; references assets and caps
* task_graph:

  * nodes: id, deps, objective, required output schema
  * per node requirements:

    * modalities needed
    * reasoning depth (low/med/high)
    * risk level (low/med/high)
    * verification level (none/check/quorum/approval)
    * latency budget (ms)
* agent_requests:

  * role, count, tier preference, max tokens
* tool_intents:

  * capability-level intents per node (not endpoints)
* fanout_policy:

  * progressive steps + cancellation thresholds
* slow_model_only_policy:

  * whether MinCalls mode allowed/required
* evidence_requirements:

  * what must be cited/pointered for acceptance

### 6.2 Claim/Evidence object (workspace)

* claim text (short)
* confidence
* evidence pointers (tool trace IDs, asset refs with offsets)
* provenance (model id/tier, prompt hash, timestamps)
* status: proposed / challenged / verified / rejected

### 6.3 Commit record (workspace)

* staged changes summary
* affected artifacts
* required verifications satisfied (yes/no)
* approvals satisfied (yes/no)
* commit/rollback trace IDs

---

## 7) Model routing rules (fast vs slow, and multimodal)

### 7.1 Node-level routing (deterministic enforcement)

Given node requirements:

* If modality requires vision/audio → only models/tools that support it are eligible.
* If reasoning depth is low and output is structured extraction → force fast/small tier.
* If risk is high or verification requires quorum/approval → allow slow/strong tier (within tier quotas).
* If nearing latency budget → downshift tiers and reduce speculation automatically.

### 7.2 Escalation rules (when to use slow model)

Escalate only when:

* disagreement crosses threshold (structured mismatch)
* verifier flags missing evidence
* action is irreversible/high-risk
* deterministic tests fail repeatedly

Otherwise, keep work on small/fast models + deterministic checks.

---

## 8) “Only one slow model” survival: MinCalls execution mode

When the registry/resource manager indicates:

* only one eligible model for key steps, or
* the slow tier is heavily throttled, or
* p95 latency budget is tight,

the scheduler switches to **MinCalls**:

1. Collapse adjacent LLM nodes into one macro node where safe.
2. Push everything possible into deterministic steps first:

   * canonicalization
   * retrieval/ranking
   * parsing/validation
   * lightweight consistency checks
3. Use **1 slow call** to produce:

   * structured intermediate outputs + decisions + a verification checklist
4. Use **optional 2nd slow call** only if needed:

   * integration + final formatting + evidence mapping
5. Simulate “multi-agent separation” inside one call by requiring:

   * multiple independent drafts + critique + reconciled output
     (validated by schema; rejected if missing)

This prevents “multi-step × slow model” from crushing you.

---

## 9) Fan-out without failure: speculation windows + backpressure

### Progressive fan-out policy (default)

* Start with a small fan-out (e.g., 4–8 agents) on ready nodes.
* Expand only if:

  * uncertainty remains high, or
  * disagreement detected, or
  * there are many independent sections to process.
* Cancel branches when:

  * latency budget remaining is low,
  * tool queues are saturated,
  * the marginal value drops below threshold.

### Backpressure contract

* Planner receives a limited number of “spawn credits.”
* Scheduler issues credits based on measured service rates and current queue depth.
* Tool runtime enforces per-tool concurrency tokens (especially MCP).

This is what keeps p95 stable.

---

## 10) Verification and evaluation (how you know separate work is correct)

### Default verification strategy

* Low risk: 1 producer + 1 verifier + deterministic schema checks
* Medium risk: 1 producer + 1 verifier + targeted independent reproduction
* High risk: quorum only on critical nodes + approval gates for irreversible actions

### Disagreement detection (cheap and effective)

* compare structured outputs (not prose)
* if mismatch > threshold → spawn a verifier or escalate tier
* if verifier still can’t reconcile → integrator flags uncertainty explicitly

---

## 11) Reliability mechanics (production hardening)

* retries only on idempotent operations (idempotency keys)
* tool circuit breakers on sustained failures/latency spikes
* hedged requests only for critical, time-sensitive calls (budgeted)
* caching of canonicalization outputs (by asset hash) to avoid repeated OCR/ASR
* strict timeouts per node; global deadline enforcement
* graceful degradation rules per mode (fast/efficient/right)

---

## 12) Operating modes (policy presets)

### Fast

* smallest speculation window
* minimal verification
* strict latency cap
* slow tier rarely allowed

### Efficient

* progressive fan-out with strict marginal-value thresholds
* moderate verification
* balanced tier usage

### Right

* higher verification requirements
* slow tier allowed for reasoning/integration
* approvals for risky actions enforced

Policy Engine can override planner-selected mode upward when risk is high.

---

If you want the next step now, I can convert this into:

1. a **strict PlanSpec schema** (field-by-field, required/optional, validation rules), and
2. a **deterministic routing table** (node requirements → eligible model tiers/tools → escalation triggers), and
3. the **MinCalls algorithm** as an executable scheduler policy.
