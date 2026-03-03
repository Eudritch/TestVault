I want to refactor to better packages/llm, packages/llm-memory and packages/llm-ui, Please come up with a perfect plan.


Below is a **fully reworked “v3” spec** that defines how everything _should_ work—especially **Orchestrator (Kernel)** and **Memory**—in a way that is:

- **Deterministic, testable, observable**
    
- **Fast by default**, but can dial up correctness
    
- **Simple-to-author pamphlets**
    
- **Neat TypeScript boundaries**, easy to change later
    
- **Designed for parallel multi-model / multi-agent execution** with proper logs
    

After the spec, you’ll get a **single Copilot/Code-Agent prompt** that tells it to refactor your current system into this new architecture, implement the scaffolding, and add a benchmarking/logging harness to validate agent counts, speed, and quality.

---

# Assistant Runtime v3 Spec

## Kernel, Agents, Memory, UI, Multimodal, Tracing

## 0) Invariants (the “laws” of the runtime)

These rules make the system feel “god-built” instead of fragile:

1. **Kernel is deterministic.**  
    LLMs can propose; **Kernel decides**. Kernel never depends on model prose for control flow.
    
2. **Everything important is a typed artifact.**  
    Intermediate outputs are not random strings; they are stored, referenced, verified, and replayable.
    
3. **Pamphlets compile to an executable plan.**  
    Nested authoring is allowed; runtime executes **flat DAG/sequence** with explicit semantics.
    
4. **Memory writes are gated and centralized.**  
    Only the **MemoryWriter pipeline** commits memory; other agents can only _suggest_ writes.
    
5. **Context is budgeted and assembled from chips, not raw history.**  
    No “entire conversation in prompt.” Ever.
    
6. **Parallelism is controlled by resource semaphores.**  
    No unbounded concurrency; per-model/per-tool limits exist.
    
7. **Quorum-based commitment.**  
    Final response cannot commit unless mode-specific verification requirements are satisfied.
    
8. **Observability is first-class.**  
    Every run produces a trace with timings, decisions, budgets, and artifacts.
    

---

# 1) Packages and Boundaries

You currently have:

- `packages/llm`
    
- `packages/llm-memory`
    
- `packages/llm-ui`
    

That’s okay, but v3 benefits from one more package:

- `packages/llm-observability` (optional but strongly recommended)
    

If you want to keep it to 3 packages, fold observability into `packages/llm/runtime/trace/*`.  
But the cleanest split is:

### 1.1 `packages/llm` (Kernel + Orchestration + Agents + Features)

Owns:

- deterministic kernel state machine
    
- plan compiler + scheduler
    
- agent runner + pool
    
- tools, capabilities, plugins
    
- multimodal routing (at least the orchestration hooks)
    

### 1.2 `packages/llm-memory` (Memory platform)

Owns:

- tiered memory (session/short/long)
    
- retrieval planning and ranking
    
- memory write pipeline (extract → score → dedupe → store)
    
- adapters for vector DB / KV / graph store
    
- policy enforcement (privacy, retention, consent)
    

### 1.3 `packages/llm-ui` (UI spec + renderer)

Owns:

- UI schema/spec
    
- widget registry
    
- UI directive/pamphlet
    
- structured output parsing and rendering
    
- streaming-safe parsing
    

### 1.4 `packages/llm-observability` (Tracing + metrics + replay) — recommended

Owns:

- trace event schemas
    
- run storage (jsonl / sqlite / postgres)
    
- metrics aggregation
    
- “bench harness” utilities
    
- run replay viewer hooks
    

---

# 2) Canonical Runtime Objects (TypeScript contracts)

This section is your “single source of truth.” Every subsystem consumes these types.

## 2.1 Request envelope (every turn, all modalities)

```ts
export type Modality = "text" | "image" | "audio" | "file";

export interface RequestEnvelope {
  requestId: string;          // stable across retries
  sessionId: string;
  userId?: string;

  timestampMs: number;
  channel: "chat" | "voice" | "api" | "automation";

  input: {
    text?: string;            // user text or transcript
    attachments?: Array<{
      id: string;
      modality: Modality;
      mimeType: string;
      bytesRef?: string;      // pointer, not inline bytes
      filename?: string;
      meta?: Record<string, unknown>;
    }>;
  };

  // runtime toggles
  mode: "auto" | "fast" | "balanced" | "right" | "aggressive";

  // optional user preferences per request
  preferences?: {
    responseStyle?: "concise" | "normal" | "detailed";
    allowUI?: boolean; // user can disable UI generation
  };

  // safety / privilege context (device control, sandbox access, etc.)
  auth?: {
    roles?: string[];
    deviceScope?: string[];
  };
}
```

## 2.2 Policy + budgets (kernel-enforced)

```ts
export interface RunBudgets {
  timeMs: number;           // overall time budget
  llmCalls: number;         // total LLM calls allowed
  toolCalls: number;        // total tool calls allowed
  tokensIn?: number;        // optional - provider dependent
  tokensOut?: number;
  parallelismCap: number;   // overall concurrency cap
}

export interface ModePolicy {
  mode: RequestEnvelope["mode"];

  // effort scaling
  minQuorum: number;     // evidence threshold to commit
  maxAgents: number;     // hard cap
  maxVerifyPasses: number;

  // quality knobs
  requireVerifier: boolean;
  requireCitations: boolean;
  requireToolEvidence: boolean;

  // safety knobs
  approvalsRequiredFor: Array<
    "filesystem_write" | "network" | "device_control" | "payments"
  >;

  budgets: RunBudgets;
}
```

## 2.3 Artifacts (everything intermediate)

Artifacts are immutable outputs produced by agents/tools/kernel steps.

```ts
export type ArtifactKind =
  | "planGraph"
  | "toolResult"
  | "workerResult"
  | "claims"
  | "evidence"
  | "diff"
  | "uiSpec"
  | "memoryWriteCandidates"
  | "verificationReport"
  | "finalAnswer";

export interface ArtifactRef {
  id: string;
  kind: ArtifactKind;
  createdAtMs: number;
  producer: {
    type: "kernel" | "agent" | "tool";
    name: string;  // "planner", "verifier", "web.search", etc.
  };
  summary: string;
  // pointer to payload storage (file, db, in-mem) to avoid bloating state
  payloadRef: string;
  tags?: string[];
}
```

## 2.4 PlanGraph (compiled from pamphlets + planner)

The plan graph is what the kernel actually runs.

```ts
export interface PlanGraph {
  id: string;
  nodes: PlanNode[];
  edges: Array<{ from: string; to: string }>;
  entryNodeId: string;
}

export interface PlanNode {
  id: string;
  goal: string;

  run: "agent" | "tool" | "kernel";
  agentProfile?: AgentProfileId;  // if run=agent
  toolName?: string;             // if run=tool

  // scheduling controls
  parallelGroup?: string;         // nodes with same group may run in parallel
  when?: PredicateExpr;           // skip logic
  retryPolicy?: RetryPolicy;

  // speed/quality hints (advisory) - kernel still decides
  hints?: {
    speed: 0 | 1 | 2 | 3;
    quality: 0 | 1 | 2 | 3;
    expectedMs?: number;
  };

  // verification and commit rules
  acceptance?: AcceptanceRule[];
  produces?: ArtifactKind[];

  // steering hooks
  pitfalls?: string[];
}
```

## 2.5 Agents and profiles (capability + permissions)

```ts
export type AgentProfileId =
  | "triage"
  | "planner"
  | "generalist"
  | "researcher"
  | "coder"
  | "verifier"
  | "uiComposer"
  | "memoryWriter"
  | "steeringMonitor";

export interface AgentProfile {
  id: AgentProfileId;
  description: string;
  allowedTools: string[];
  maxTokensOut?: number;
  temperature?: number;

  // used by effort scaler
  typicalLatencyMs?: number;
  typicalQuality?: 0 | 1 | 2 | 3;
}
```

---

# 3) Orchestrator (Kernel) v3 — The definitive design

Your current kernel is close conceptually, but v3 makes the “control plane” explicit and testable.

## 3.1 Kernel is a state machine with phases

**Phases:**

1. Normalize request → load policy & budgets
    
2. Triage (cheap) → classify: complexity, risk, tool needs, UI desirability
    
3. Resolve pamphlets/features/tools → compile capability digest
    
4. Memory retrieval planning → retrieve chips
    
5. Context assembly → build bounded context
    
6. Plan compile → PlanGraph (from pamphlet + planner)
    
7. Execute plan → scheduler runs nodes (parallel/sequential)
    
8. Verify quorum → verifier checks claims/evidence/contracts
    
9. Render output → (text + optional UI spec)
    
10. Memory writeback → write pipeline
    
11. Commit turn → emit final artifacts + trace replay
    

## 3.2 Determining “answer immediately” vs “plan”

Kernel uses triage output to choose pattern:

- **Direct Answer Pattern** if:
    
    - complexity low AND
        
    - risk low AND
        
    - no tools required AND
        
    - budgets tight (fast mode)
        

Otherwise:

- **Plan Pattern** → compile PlanGraph and run it.
    

## 3.3 Effort scaling (how many agents to run)

### 3.3.1 Triage output (single cheap call)

Triage produces:

```ts
export interface TriageSignals {
  complexity: 0 | 1 | 2 | 3;
  risk: 0 | 1 | 2 | 3;
  needsTools: boolean;
  needsMemory: boolean;
  uiWouldHelp: boolean;

  // helpful for routing
  categories: string[]; // "coding", "research", "scheduling", etc.
}
```

### 3.3.2 Base agent budget formula (simple + predictable)

```text
baseAgents = 1 + complexity + risk
N = clamp(baseAgents, 1, policy.maxAgents)
```

Then adjust:

- Reduce if tool bottlenecked (rate limits, single sandbox)
    
- Reduce if steps are not parallelizable
    
- Increase if early disagreement occurs OR verifier fails OR “Right” mode
    

### 3.3.3 Adaptive scaling (the “don’t waste agents” rule)

After first parallel wave, compute:

- agreement score between worker outputs
    
- verifier confidence
    
- unresolved claims count
    

If:

- agreement high AND verifier ok → stop early  
    Else:
    
- spawn additional worker(s) only for the disputed sub-questions
    

This prevents “always run 6 agents.”

## 3.4 Scheduler and resource control (parallel done correctly)

You need **resource semaphores**, not just concurrency numbers.

### 3.4.1 Resource manager

Kernel owns a `ResourceManager` that enforces:

- global concurrency: `policy.budgets.parallelismCap`
    
- per-model concurrency (e.g. “ollama/llama3.1: max 2”)
    
- per-tool concurrency (e.g. “web.search max 1”)
    
- per-sandbox concurrency (single shell session)
    

### 3.4.2 Plan node execution

Each node execution produces:

- artifacts (refs)
    
- timing metrics
    
- tool call list
    
- errors
    

and appends them to the trace.

## 3.5 Quorum commitment (mode-driven)

Define quorum rules per mode:

- **fast**: quorum=1, verifier optional
    
- **balanced**: quorum=2 OR verifier pass
    
- **right**: quorum=2+ AND verifier required AND tool evidence required for factual claims or actions
    
- **aggressive**: quorum still required, but allow more parallelism and retries
    

Quorum is not “votes.” It’s “evidence threshold met.”

## 3.6 Tool calling model (safe, bounded, observable)

Tool calls are a kernel-mediated service:

- model can propose tool calls
    
- kernel validates permissions/budgets
    
- kernel executes tool calls
    
- tool outputs become artifacts (untrusted data)
    

**Key security rule:** tool output never becomes instructions.

## 3.7 Optional steering (kernel-integrated but feature-gated)

Steering runs only when:

- policy says `steering=on` OR
    
- step contains pitfalls OR
    
- verifier signals drift
    

Steering actions:

- raise “drift detected” event
    
- request a micro re-plan
    
- insert a “constraint reminder” chip into context
    
- block commit if output violates contract
    

Steering must be cheap and bounded.

---

# 4) Memory System v3 — definitive design

Your existing `llm-memory` is the right direction, but v3 makes memory **governed, tiered, and testable**.

## 4.1 Memory tiers and their strict roles

### 4.1.1 Session memory (ephemeral, ultra-relevant)

- compact event log (“what happened”)
    
- active constraints (“user wants concise”, “timezone”, “current task”)
    
- no raw transcript in prompt
    

### 4.1.2 Short-term memory (cross-session, high precision)

- preferences and stable working context
    
- “current projects”
    
- things user explicitly wants remembered (opt-in)
    

### 4.1.3 Long-term memory (heavy, searchable, auditable)

- episodic store (vector + metadata)
    
- optional graph store (entities/relations)
    
- audit log + retention controls
    

## 4.2 Memory read path (deterministic + budgeted)

### 4.2.1 Retrieval planning

A deterministic planner decides retrieval mode:

- none
    
- session only
    
- short only
    
- hybrid (short + long)
    
- targeted (long only, if needed)
    

Inputs:

- triage signals
    
- policy mode (fast vs right)
    
- prompt cues (e.g. “as before”, “my preference”, “last time”)
    

Outputs a **MemoryQueryPlan**:

```ts
export interface MemoryQueryPlan {
  mode: "none" | "session" | "short" | "long" | "hybrid" | "targeted";
  budget: { maxItems: number; maxTokens: number; maxLatencyMs: number };
  queries: Array<{
    kind: "preference" | "project" | "episodic" | "entity";
    queryText: string;
    filters?: Record<string, unknown>;
  }>;
}
```

### 4.2.2 Retrieval produces ContextChips

Memory never injects raw text; it returns chips:

```ts
export interface ContextChip {
  id: string;
  kind: "memory" | "file" | "tool" | "diff" | "note";
  title: string;
  summary: string;
  payloadRef: string; // hydrate on demand
  relevance: number;  // 0..1
  trust: "trusted" | "untrusted"; // memory is “trusted-ish” but still not instructions
}
```

## 4.3 Memory write path (centralized, gated, minimal)

Only **MemoryWriter** can commit. It runs after the response (or mid-run for long tasks).

Pipeline stages:

1. **Extract candidates** (from artifacts + final answer)
    
2. **Classify** (preference / project / fact / commitment / event)
    
3. **Score**:
    
    - usefulness
        
    - stability
        
    - sensitivity/PII risk
        
    - confidence (evidence-backed?)
        
4. **Dedupe/merge**:
    
    - update existing memory instead of adding duplicates
        
5. **Apply policy**:
    
    - consent flags
        
    - retention
        
    - “do not store” categories
        
6. **Write**:
    
    - KV for preferences/projects
        
    - vector store for episodic
        
    - graph store for entities (optional)
        
7. **Emit audit event**:
    
    - what changed and why
        

### 4.3.1 Memory records (practical shape)

```ts
export type MemoryKind = "preference" | "project" | "fact" | "event" | "entity";

export interface MemoryRecord {
  id: string;
  kind: MemoryKind;
  content: string;          // compact, not raw transcript
  summary: string;
  tags: string[];

  createdAtMs: number;
  updatedAtMs: number;

  source: {
    sessionId: string;
    requestId: string;
    artifactRefs: string[];  // evidence/provenance
  };

  governance: {
    sensitivity: "low" | "medium" | "high";
    retention: "session" | "short" | "long";
    userVisible: boolean; // can user view/delete
  };

  scores: {
    usefulness: number; // 0..1
    confidence: number; // 0..1
  };
}
```

## 4.4 Memory correctness + poisoning defense

Memory ingestion must treat:

- tool outputs
    
- web pages
    
- OCR’d text
    
- third-party content
    

as **untrusted** and never store as “preference/fact” without verification.

Rule of thumb:

- store “user preference” only if user said it
    
- store “fact” only if evidence exists or explicitly labeled as “unverified note”
    
- store “episodic event summary” is okay but must not become a “fact”
    

---

# 5) Logs, tracing, and replay (required for tuning agent counts)

You asked for logs to understand:

- how models behave
    
- parallelism
    
- correct agent count vs speed/quality
    

This requires **structured traces + aggregated metrics**.

## 5.1 Trace event model (JSONL-friendly)

Every turn produces a `RunTrace` with events like:

- `turn.started`
    
- `policy.resolved`
    
- `triage.completed`
    
- `memory.plan.created`
    
- `memory.retrieve.completed`
    
- `context.assembled`
    
- `plan.compiled`
    
- `scheduler.node.started`
    
- `scheduler.node.completed`
    
- `llm.call.started`
    
- `llm.call.completed`
    
- `tool.call.started`
    
- `tool.call.completed`
    
- `verifier.completed`
    
- `output.rendered`
    
- `memory.write.completed`
    
- `turn.completed`
    

Each event includes:

- timestamps
    
- correlation ids (requestId, nodeId, agentRunId)
    
- budgets before/after
    
- latency
    
- token usage
    
- model id/provider id
    
- error data (typed)
    

## 5.2 Metrics you must record

At minimum per run:

**Latency**

- total turn time
    
- time in memory retrieval
    
- time in planning
    
- time in tool calls
    
- time in verification
    

**Cost**

- LLM calls
    
- tokens in/out
    
- tool calls
    

**Parallelism**

- peak concurrent LLM calls
    
- peak concurrent tool calls
    
- queue time per node (how long waiting on semaphore)
    

**Quality signals**

- verifier pass/fail
    
- number of disputed claims
    
- “disagreement score” across workers
    
- user follow-up corrections (later)
    

This is what you’ll use to tune policies.

---

# 6) Testing “right amount of agents” (bench harness)

You want the architecture to “test itself” and learn the right agent count.

## 6.1 Add a bench harness (not fancy, just real)

Create a `bench/` suite that can run:

- same prompt across modes
    
- same prompt across agent counts
    
- same prompt across model combinations
    

Outputs:

- trace logs
    
- summary report (csv/json)
    
- recommended `effortScaling` adjustments
    

## 6.2 Quality scoring options (practical)

You can start with:

- deterministic checks (json schema valid, tests pass, citations present)
    
- rubric judge (an LLM judge with fixed rubric) — optional but useful
    
- user satisfaction proxies (follow-up rate, corrections)
    

Keep it simple at first:

- “verifier passed” is your initial quality metric.
    

---

# 7) What changes from your current architecture (mapping)

Your doc is solid, but v3 changes the _control contracts_.

### Major changes:

1. **Planner output must be PlanGraph**, not ad-hoc worker lists.
    
2. **Pamphlets compile to PlanGraph nodes** with explicit `sequence/parallel/choice`.
    
3. **ContextChips becomes universal** (files/memory/tools/diffs all use same mechanism).
    
4. **Memory write is centralized** (MemoryWriter pipeline only).
    
5. **Tracing schema becomes mandatory**, not optional telemetry.
    
6. **ResourceManager controls concurrency**, especially for multi-model parallel runs.
    
7. **Quorum gates final commit** and is mode-dependent.
    
8. **UI generation becomes a dedicated plan node** producing schema-valid `uiSpec`.
    

---

# 8) The Copilot/Code-Agent Refactor Prompt

Copy/paste this as the _single instruction prompt_ for your refactor agent.

> This prompt is written to force the agent to:
> 
> - refactor aggressively (breaking changes allowed),
>     
> - keep code neat,
>     
> - implement v3 kernel + memory contracts,
>     
> - add tracing/bench harness,
>     
> - and validate agent-count logic with logs.
>     

```text
You are a senior TypeScript runtime architect and refactoring agent.

GOAL
Refactor the existing repo’s LLM system into “Assistant Runtime v3” with a deterministic kernel, compiled PlanGraph execution, centralized memory write pipeline, ContextChips, quorum-based verification, and first-class tracing + bench harness.

You MUST:
- Keep the code neat, minimal, readable, and easy to change.
- Prefer small well-named modules over large files.
- Prefer strict TypeScript types and schema validation where needed.
- Breaking changes are allowed (and expected) if they simplify the system.
- Pamphlets must remain simple (or simpler) even with new features.
- Do not introduce overengineering: implement the smallest complete v3 that passes tests and has logs/bench.

DELIVERABLES
1) Update architecture docs to match v3 (Kernel, Memory, UI, Pamphlets, Tracing).
2) Implement v3 contracts (RequestEnvelope, ModePolicy, ArtifactRef, PlanGraph, PlanNode, ContextChip, MemoryRecord, MemoryQueryPlan).
3) Implement Kernel v3 as a deterministic state machine with phases:
   - normalize → triage → resolve features/tools → memory plan/retrieve → context assemble → plan compile → execute plan with scheduler → verify quorum → render output → memory writeback → commit trace
4) Implement ResourceManager that enforces:
   - global parallelismCap
   - per-model concurrency
   - per-tool concurrency
   - per-sandbox concurrency
5) Implement effort scaling:
   - triage outputs complexity/risk/tool/memory/UI needs
   - agent count formula: N = clamp(1 + complexity + risk, 1, policy.maxAgents)
   - adaptive scaling: if verifier fails or disagreement high, spawn additional targeted worker(s)
6) Implement Memory v3 in packages/llm-memory:
   - tiered stores: session, short, long
   - deterministic retrieval planner producing MemoryQueryPlan
   - retrieval returns ContextChips (not raw text)
   - MemoryWriter pipeline (extract → score → dedupe/merge → policy → write → audit)
   - Only MemoryWriter can commit memory; others can only propose candidates
7) Implement tracing:
   - structured trace events (JSONL-friendly)
   - record timings, token usage, model id, tool calls, node scheduling, queue time, budgets deltas
   - store trace per run with replay references to artifacts
8) Implement bench harness:
   - run a list of prompts across modes and/or agent counts
   - emit summary metrics (latency, llmCalls, toolCalls, verifier pass/fail, disagreement score, peak concurrency)
   - output a report file
9) UI:
   - keep existing llm-ui rendering approach
   - add a dedicated plan node “ui.compose” that produces a schema-validated uiSpec artifact (no raw JSX/HTML)
10) Migration:
   - keep a thin compatibility wrapper for current runtime entrypoints (invoke/stream/session) that calls Kernel v3
   - remove or deprecate obsolete v2 pipeline components after v3 is working

PROCESS (MANDATORY)
A) Repository analysis:
   - identify existing kernel/responder/worker pool/pamphlets/memory integration points
   - produce a brief “Current → v3 mapping plan” in a new doc: docs/migration-v3.md
B) Implement in small safe steps:
   1) Add v3 types and trace event schema + minimal trace logger
   2) Add ResourceManager + scheduler skeleton
   3) Add PlanGraph compiler (start by compiling simplest pamphlets)
   4) Add Kernel v3 with minimal execution of a tiny PlanGraph
   5) Integrate tool calling under kernel mediation
   6) Integrate memory plan/retrieve + ContextChips
   7) Integrate MemoryWriter pipeline
   8) Add verifier + quorum gates
   9) Add UI compose node + uiSpec validation
   10) Add bench harness + report output
C) Tests:
   - Add unit tests for:
     - pamphlet compile to PlanGraph (sequence/parallel/choice)
     - ResourceManager concurrency behavior
     - Memory write gating/dedupe/merge
     - deterministic trace event ordering (where applicable)
   - Add an integration test that runs a full turn with a mocked model/tool/memory adapter.
D) Documentation:
   - rewrite the architecture doc in docs/architecture-v3.md matching the implemented system
   - ensure it describes orchestrator + memory in detail and matches code

IMPLEMENTATION RULES
- Kernel must not rely on model prose for control decisions.
- All intermediate data must be stored as typed artifacts with ArtifactRefs.
- Tool outputs are untrusted and must not become instructions.
- Memory retrieval returns ContextChips; ContextBuilder decides what to hydrate into prompt under budget.
- Concurrency must be bounded by ResourceManager semaphores.
- Keep pamphlets simple and compiled; only allow nesting via sequence/parallel/choice.
- Prefer deterministic planners (heuristics) unless a model is explicitly used and bounded.
- Log enough to tune agent counts and parallelism.

OUTPUT
When you finish, provide:
- a summary of what changed
- how to run bench
- where traces are stored
- how to tune mode policies / agent scaling
- what is left as TODOs (only if truly nonessential)
```

---

# 9) What you should expect after this refactor (practically)

When v3 is done, you’ll have:

- A kernel that can **prove** what it did (trace + artifacts)
    
- A memory system that is **controllable**, not “magic”
    
- A way to **measure** whether 2 agents beats 5 agents for a given task class
    
- A bench harness that helps you tune:
    
    - quorum thresholds
        
    - parallelism caps
        
    - per-model concurrency limits
        
    - default policies per mode
        

---

If you want one extra “god-tier” addition to include in the refactor prompt (optional):  
Add a **ModelRegistry** with model metadata (latency, context size, cost class, reliability rating) so the scheduler can route “fast tasks” to cheap models and reserve “right mode verification” for reliable models. This pairs perfectly with your goal of multi-model parallel execution and will show up clearly in your traces.








You’re right to call this out. The _real_ difference between “a pile of agents” and a god-tier assistant is **behavioral contracts**:

- what the **Orchestrator** does _every turn_,
    
- when it answers immediately vs plans,
    
- how it chooses tools/pamphlets/agents,
    
- how it parallelizes safely,
    
- what “voting” actually means (and when it’s a trap),
    
- and how **Memory** behaves so it’s useful, fast, and not a hallucination amplifier.
    

Below is a **behavior spec** (not code) that you can treat as your runtime “constitution.”

---

# Orchestrator v3 Behavior Spec

## The Orchestrator is an OS kernel, not an agent

## 1) Orchestrator identity and non-negotiables

### 1.1 What the Orchestrator is

A deterministic control plane that:

- interprets **policies** and **pamphlets**
    
- constructs and executes a **PlanGraph**
    
- enforces budgets, permissions, concurrency
    
- collects artifacts and evidence
    
- decides when to stop, retry, or escalate
    
- commits final output only after meeting quorum rules
    

### 1.2 What the Orchestrator is NOT

- It is not “one big prompt.”
    
- It does not trust unstructured model text for control flow.
    
- It does not pass full conversation history to the model.
    

### 1.3 The 3 “bio-inspired” behaviors you want, translated to engineering rules

1. **Stigmergy = Shared Workspace**  
    Agents don’t “talk”; they write artifacts into a shared workspace that the Orchestrator reads and routes on.
    
2. **Response thresholds = Activation gating**  
    Agent routing is driven by thresholds (risk/complexity/disagreement), not vibes.
    
3. **Quorum = Commit protocol**  
    You don’t go fast until the evidence threshold is met.
    

---

# 2) The Orchestrator lifecycle (per turn)

Every request executes this exact deterministic phase sequence. Some phases can be skipped, but **only by deterministic rules**.

## Phase A — Normalize

**Inputs:** `RequestEnvelope`  
**Outputs:** `RunContext` (policy, budgets, trace IDs, user mode, channel mode)

Behavior:

- Normalize modality (text/audio/image) into canonical envelope.
    
- Determine `ModePolicy` (auto/fast/balanced/right/aggressive).
    
- Initialize trace, counters, semaphores.
    

**Rule:** if policy budgets are exceeded at any time, Orchestrator must degrade to a simpler plan or stop.

---

## Phase B — Triage (cheap, bounded, optional model)

Goal: Decide “answer now vs plan” + what kind of work it is.

### B1) Deterministic triage first (always)

- Input length, attachments count
    
- keywords: “last time”, “remember”, “as before” → memory signal
    
- verbs implying action: “book”, “pay”, “delete”, “run commands” → risk signal
    
- “compare”, “research”, “best” → tool/research signal
    
- “code”, “refactor”, “bug” → coding signal
    

### B2) If still uncertain: call **Triage Agent** (very small budget)

Outputs `TriageSignals`:

- `complexity` (0–3)
    
- `risk` (0–3)
    
- `needsTools` boolean
    
- `needsMemory` boolean
    
- `uiWouldHelp` boolean
    
- `categories` (coding/research/device/creative/etc.)
    

**Rule:** triage must never produce a plan; it only labels the problem.

---

## Phase C — Decide: “Answer Immediately?”

This is the first key behavior you asked for.

### Immediate Answer Gate (deterministic)

Answer immediately if **all** are true:

- `complexity <= 1`
    
- `risk == 0`
    
- `needsTools == false`
    
- `needsMemory == false` _(or memory can be ignored safely)_
    
- request doesn’t require structured output / UI
    
- policy mode is `fast` OR `auto` with tight budgets
    

If any fail → plan.

**Important:** Immediate answer still runs _minimum checks_:

- output formatting contract check (basic)
    
- policy guardrails check (PII / disallowed actions)
    
- if user asked for citations/verification explicitly, it’s not immediate
    

---

## Phase D — Resolve “What is even available?”

This phase answers your: **“Filter out Tools and Pamphlet based on prompt knowing which we run.”**

### D1) Build Capability Digest (no LLM needed)

From:

- installed features/plugins (tools, sandboxes, UI, memory backends)
    
- user/device permissions
    
- channel constraints (voice vs chat)
    
- current runtime state (tool rate limits, model availability)
    

Produce a compact structure:

- tool list with permissions + latency class + cost class
    
- pamphlet catalog with tags/capabilities
    
- known agent profiles available
    

### D2) Filter tools deterministically (permission + relevance)

Hard filters:

- permissions not granted
    
- tool disabled in policy/mode
    
- tool blocked by channel (e.g., no terminal on mobile)
    

Soft filters (relevance):

- use simple keyword matching + tags
    
- optionally rerank with tiny model if huge toolset
    

**Rule:** the model never sees the full tool universe unless needed.  
The Orchestrator shows the model a **shortlist** plus a way to request expansion.

---

## Phase E — Memory Plan + Retrieval (budgeted)

This phase answers your: **“Weather we run to save context… AI struggles with long process.”**

Behavior:

- If `needsMemory == false`, skip retrieval entirely.
    
- Else run deterministic `MemoryPlanner`:
    
    - chooses tier(s): session/short/long/hybrid
        
    - sets strict budgets: max chips, max tokens, max latency
        
    - emits `MemoryQueryPlan`
        

Retrieval returns **ContextChips**, not raw text.

**Rule:** memory retrieval is always _cheaper than dumping history_.

---

## Phase F — Context Assembly (Context Builder)

Build the smallest prompt that can succeed.

Inputs:

- request
    
- chips (memory/file/tool previews)
    
- selected pamphlets/tools shortlist
    
- policy reminders (budgets, safety)
    
- active task state (if continuing)
    

Output:

- `ModelContext` with strict token budgets and explicit sections:
    
    - Instruction
        
    - Task state
        
    - Evidence/data (quoted)
        
    - Allowed tools
        
    - Output contract
        

**Rule:** “data” is quoted/untrusted; “instructions” are kernel-controlled.

---

## Phase G — Plan Compilation

This phase answers your: **“Orchestrator looks at instruction and determines steps needed in order and a Planning Diagram.”**

### G1) Pamphlet selection behavior

Pick pamphlets by:

1. deterministic tag match (category + tool needs + output type)
    
2. fallback to a “General Assistant” pamphlet
    
3. if multiple match, choose highest confidence / lowest complexity first
    

### G2) Plan sources

PlanGraph can come from:

- compiled pamphlet alone (if pamphlet fully prescriptive)
    
- compiled pamphlet skeleton + Planner agent fills small gaps
    
- fully generated plan (only in complex cases)
    

**Rule:** the planner must output structured `PlanGraph`, never prose.

### G3) PlanGraph behavior requirements

Each node includes:

- goal
    
- run type (agent/tool/kernel)
    
- tool permission envelope
    
- parallel group id (optional)
    
- acceptance rules (what “done” means)
    
- pitfalls list (steering hooks)
    
- expected cost/latency hint
    

---

# 3) Agent Management Behavior

## How the Orchestrator decides “how many agents” and how to run them

You want “extremely fast + right answers.” That requires **effort scaling** that is simple and testable.

## 3.1 Agent count baseline (deterministic)

Let:

- `C = complexity (0..3)`
    
- `R = risk (0..3)`
    
- `Mode = fast/balanced/right/aggressive`
    

Baseline:

- `N = clamp(1 + C + R, 1, policy.maxAgents)`
    

Then adjust:

- If plan has >1 independent branches: increase parallel workers
    
- If tools are bottlenecked (web rate limit / single sandbox): decrease
    
- If user explicitly wants speed: cap N even if complex
    
- If “right mode”: add verifier and increase quorum
    

## 3.2 Parallelism behavior (resource-semaphore based)

Orchestrator does not “just Promise.all” agents.

It enforces:

- global concurrency cap
    
- per-model concurrency cap
    
- per-tool concurrency cap
    
- per-sandbox concurrency cap
    

This prevents self-DDoS and also creates clean metrics.

## 3.3 “Voting” behavior: what it should mean (and when not to)

Voting is useful only when:

- the task has **multiple plausible solutions**
    
- solutions are **independent**
    
- there is an objective verifier or rubric
    

**Bad voting:** “3 agents answer, majority wins.”  
This often picks the most fluent hallucination.

**Good voting (recommended): “Quorum with evidence”**

- Each worker must output:
    
    - answer
        
    - claims list
        
    - evidence refs (tool outputs, file snippets, memory chips)
        
    - uncertainty list
        
- A verifier agent merges:
    
    - aligns claims
        
    - flags conflicts
        
    - runs tools/tests if needed
        
- Commit only if quorum threshold is met **for the critical claims**
    

So it’s not democracy; it’s **evidence convergence**.

## 3.4 Disagreement behavior (when agents disagree)

If worker outputs disagree:

1. Extract conflict set of claims
    
2. Spawn targeted sub-workers only for the disputed claims
    
3. Run verifier again
    
4. If still unresolved:
    
    - ask user clarifying question OR
        
    - present multiple options with confidence + tradeoffs
        

This stops infinite loops and avoids wasted agent floods.

---

# 4) Execution Behavior

## How the Orchestrator runs the PlanGraph

## 4.1 Node execution behavior

Each node run must:

- receive only the minimal context (chips relevant to node)
    
- produce typed artifacts
    
- report timings, tokens, tool calls
    
- obey budgets
    

## 4.2 Tool calling behavior

Tools are kernel-mediated:

- agent proposes tool call intent
    
- kernel checks permissions, budgets, and policy
    
- kernel executes tool call
    
- tool output becomes an **untrusted artifact**
    

**Rule:** tool output can inform decisions but cannot override instructions.

## 4.3 “Steering” behavior (optional but powerful)

Steering is not always-on. It’s triggered by:

- high risk
    
- long plans
    
- repeated failures
    
- pitfalls flagged in nodes
    
- drift signals (plan no longer matches goal)
    

Steering monitor actions:

- add a constraint reminder chip
    
- insert a micro re-plan node
    
- block commit if output violates contract
    

Steering is a feature that can be toggled:

- per mode
    
- per pamphlet
    
- per node
    

---

# 5) Commit Behavior (Quorum + Verification)

This is where “Right answers” actually come from.

## 5.1 Quorum rules by mode

- **fast:** quorum=1, minimal checks
    
- **balanced:** quorum=1 + verifier OR 2 independent workers
    
- **right:** quorum=2+ + verifier + tool evidence for critical claims
    
- **aggressive:** higher parallelism, but verifier still required if risk>0
    

## 5.2 Verifier behavior (what it must do)

Verifier must:

- check output contract (format, completeness)
    
- check policy (safety, approvals)
    
- check claim-evidence links (for important claims)
    
- check internal consistency
    
- request additional nodes if needed
    

**Rule:** verifier can force additional tool calls if policy requires.

---

# 6) UI Behavior

## “Generate UI when necessary” as a kernel decision

UI is not inline tags by default; it’s a plan node:

- `ui.compose` generates a schema-valid `uiSpec` artifact
    

Orchestrator triggers UI generation if:

- data is structured (tables, comparisons)
    
- user needs to choose (options)
    
- task is multi-step (progress + checklist)
    
- channel supports UI and user didn’t disable it
    

**Rule:** UI output must validate against a component registry; unknown components are rejected.

---

---

# Memory v3 Behavior Spec

## Memory is a governed data product with strict write/read policies

Your memory design is good conceptually, but the **behavior** must be explicit or memory becomes:

- expensive (context bloat)
    
- wrong (hallucination amplifier)
    
- unsafe (poisoning + privacy issues)
    

So memory needs these behaviors.

---

# 1) Memory tiers and what each is allowed to do

## 1.1 Session Memory (ephemeral)

Purpose:

- keep the assistant coherent inside a session without passing full transcript
    

Allowed content:

- compact “events” (who did what)
    
- active constraints (“use concise style”, “project is X”)
    
- current task state pointers
    

Not allowed:

- storing raw transcript for prompt reuse
    

Behavior:

- always updated each turn (cheap)
    
- retrieval is extremely fast and deterministic
    
- TTL = end of session (or user-defined)
    

## 1.2 Short-Term Memory (cross-session, high precision)

Purpose:

- stable preferences + “what user is working on”
    

Allowed content:

- preferences (tone, format)
    
- active projects
    
- stable user facts explicitly provided/approved
    

Behavior:

- write requires high confidence + low sensitivity
    
- retrieval is KV-first (fast, deterministic)
    
- periodic consolidation into fewer records
    

## 1.3 Long-Term Memory (episodic + optional graph)

Purpose:

- searchable archive and richer recall
    

Allowed content:

- episodic summaries (not full logs)
    
- structured entity notes (optional)
    
- “learned” patterns with provenance
    

Behavior:

- retrieval is hybrid (vector + metadata)
    
- strict “provenance and confidence” tags
    
- explicit user controls (view/delete/disable)
    
- offline consolidation jobs allowed
    

---

# 2) Memory read behavior (when do we retrieve?)

This is crucial: you don’t want retrieval every turn.

## 2.1 Retrieval gate (deterministic)

Retrieve only if one is true:

- user references past: “as before”, “last time”, “my preference”
    
- task is long-running and needs continuity
    
- user identity personalization is needed (tone, format)
    
- planner indicates missing constraints that likely exist in memory
    

Otherwise retrieval is **skipped**.

## 2.2 MemoryQueryPlan behavior

MemoryPlanner outputs:

- tier(s) to query
    
- number of items
    
- max tokens
    
- latency budget
    
- query types (preference/project/episodic/entity)
    

**Important:** planner is deterministic by default.  
A small model can assist only if ambiguous.

## 2.3 Retrieval output is ContextChips only

Memory does not dump text into prompt.

It returns chips with:

- title
    
- 1–2 sentence summary
    
- payloadRef (hydrate if needed)
    
- relevance score
    
- provenance/confidence
    

ContextBuilder decides:

- which chips to include
    
- how much to hydrate (snippet)
    
- where to place them (data section)
    

---

# 3) Memory write behavior (centralized + gated)

This is where most systems die.

## 3.1 Only MemoryWriter commits

Other agents can output `memoryWriteCandidates` artifacts.  
But only MemoryWriter pipeline may:

- score
    
- dedupe/merge
    
- store
    
- audit
    

## 3.2 Write pipeline stages (required)

1. **Extract** candidates from artifacts:
    
    - user preferences stated
        
    - project state updates
        
    - tasks completed
        
    - commitments (“remind me tomorrow…”)
        
2. **Classify**:
    
    - preference / project / fact / event / entity
        
3. **Score**:
    
    - usefulness
        
    - stability (will it remain true?)
        
    - confidence (supported by evidence?)
        
    - sensitivity/PII risk
        
4. **Dedupe & merge**:
    
    - update existing record
        
    - avoid duplicates
        
    - resolve conflicts deterministically:
        
        - prefer newer
            
        - prefer user-stated over inferred
            
        - keep both if uncertain with confidence tags
            
5. **Policy enforce**:
    
    - consent
        
    - retention tier
        
    - “never store” categories
        
6. **Commit + audit**:
    
    - emit “memory.write” event with reason
        

## 3.3 What counts as “safe memory”

### Preferences (safe-ish)

Store when:

- explicitly stated by user
    
- repeated across turns
    
- low sensitivity
    

### Facts (dangerous)

Only store as “fact” when:

- user states it explicitly OR
    
- tool evidence supports it  
    Otherwise store as “note” with low confidence.
    

### Episodic summaries (generally safe)

Store:

- compact “what happened”
    
- not raw content
    
- keep provenance pointers
    

---

# 4) Memory consolidation + forgetting behavior

You want it “bio-inspired,” so implement **decay and consolidation**.

## 4.1 Decay (“pheromones”)

Every memory item has:

- lastUsedAt
    
- reinforcementCount
    
- decay score
    

If unused for long time:

- reduce retrieval priority
    
- eventually consolidate into a higher-level summary
    
- optionally delete if policy says so
    

## 4.2 Consolidation jobs (offline)

Periodic jobs:

- merge duplicate preferences
    
- compress episodic items into “project timeline” summaries
    
- build entity graph edges from stable references
    

**Rule:** offline jobs never silently change user-facing preferences; they propose merges and keep provenance.

---

# 5) Memory security behaviors (poisoning resistance)

Memory and retrieval are attack surfaces.

## 5.1 Untrusted sources rule

Never store as preference/fact:

- web page claims
    
- tool outputs
    
- OCR text
    
- other agents’ outputs
    

unless verified and labeled.

## 5.2 Instruction/data separation

Retrieved memory is always presented to models as:

- “data / notes” (quoted)  
    Not as instructions.
    

## 5.3 Audit + user control

User must be able to:

- see what is stored
    
- delete items
    
- disable memory tier(s)
    

---

# 6) How Orchestrator + Memory cooperate (the missing glue)

This is the “behavioral harmony” layer.

## 6.1 Orchestrator never assumes memory is correct

Memory chips are inputs; verifier can challenge them.

## 6.2 Memory retrieval is budgeted like tools

Memory retrieval has:

- latency budget
    
- max chips
    
- max hydration tokens
    

If retrieval exceeds budget:

- fall back to “ask user” or “no memory used”
    

## 6.3 Memory writes are part of the plan (not a side effect)

MemoryWriter is a final node:

- `memory.writeback`  
    It produces:
    
- `memoryWriteReport` artifact
    
- audit events
    

---

# 7) Concrete behavior examples

## Example A: “What’s 15% of 80?”

Triage: complexity 0, risk 0, needsTools false, needsMemory false  
Immediate Answer: yes  
Agents: 1  
Memory: none  
UI: none  
Verifier: optional/basic

## Example B: “Refactor my LLM runtime architecture and add bench harness”

Triage: complexity 3, risk 1, needsTools maybe, needsMemory maybe  
Immediate Answer: no  
PlanGraph:

- plan.compile
    
- architecture.propose (parallel workers: kernel/memory/ui)
    
- verifier.consistency
    
- output.render (doc + diagrams)
    
- memory.writeback (project update)  
    Agents:
    
- planner + 3 specialists + verifier + (optional UI composer)  
    Parallel:
    
- 2–4 workers max, bounded by policy
    

## Example C: “Book me a flight and pay”

Risk high → approvals required  
PlanGraph includes:

- gather constraints
    
- present options UI
    
- require approval
    
- then tool execution  
    Memory: store preferences only if user explicitly opts in
    

---

# 8) The single rule that will make this feel “perfect”

**Do not optimize for agent count. Optimize for:**

- _expected value per ms_ under a policy, with _quorum to prevent mistakes_.
    

That’s how biology does it: **fast until uncertainty rises**, then recruit more effort.

