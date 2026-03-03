## Rewritten prompt (in paragraphs)

You are a senior staff engineer and LLM-systems architect. I’m building an assistant that must _feel like_ a single-model, single-thread conversation: one coherent “brain,” one continuous intent, one authoritative narrative, and one consistent set of decisions over time. Today our multi-agent approach breaks that illusion—context gets fragmented, intent drifts, follow-through is inconsistent, and debugging is painful. I want you to design a refactor that restores the single-thread qualities while still letting us use agents for speed and scale.

Start by explaining—concisely but convincingly—why a single model + single thread tends to be more reliable in practice. Make it explicit that the benefit is not just “simplicity,” but _coherence_: one source of truth for intent, one stable chain of reasoning, fewer contradictory partial plans, fewer duplicated tool calls, fewer race conditions on state, and a clearer causal link between input, memory, and output. Emphasize the operational benefits too: deterministic state transitions, easier evaluation, easier incident triage, predictable latency, and safer behavior because there’s a single gatekeeper for what gets said to the user.

Next, acknowledge the real advantages of agents (parallelism, throughput, background work), but frame them as _supporting systems_ rather than competing “voices.” Agents should not be allowed to directly steer or mutate user-facing state in an uncontrolled way. The primary assistant thread must remain the only author of user-visible responses and the only writer to canonical conversation state. Agents can exist, but they must produce bounded artifacts (summaries, retrieval bundles, memory candidates, tool results) that are merged into the main thread through a deterministic, auditable process.

Then propose how we can get “more context and smarter context” without losing single-thread coherence. Use the idea of a background context pipeline (for example `runtime/background-pamphlet.ts`) that continuously categorizes information, compacts history, and produces a small set of structured memory artifacts: short rolling summaries, long-term facts, open loops/tasks, user preferences, and pinned constraints. These artifacts must be versioned, attributable, and safe to include in the main context window. The goal is that the assistant becomes _more_ consistent and context-rich than a naive single-thread system, while remaining just as predictable and reliable.

Finally, produce a complete refactor plan that gets us from our current “agents module that adds negatives” to a “super assistant” architecture: single-thread quality, faster via parallel work, larger effective context via memory compaction, and higher reliability via strict state ownership and better evaluation. Include the target architecture, the key module boundaries and contracts, the runtime concurrency model, the memory strategy, the migration steps, and the testing/evaluation plan. Be opinionated and specific.

---

## The single-model, single-thread benefits you should make unmistakable (and why they’re real)

A single thread is basically the “single-writer principle” applied to cognition: one authoritative state machine decides what the assistant believes the user wants, what it has committed to do, and what it will say next. That prevents intent bifurcation (two sub-agents interpreting the ask differently), prevents contradictory partial plans, and prevents “plan churn” where parallel workers each revise direction without a shared commit point.

It also makes context _meaningfully composable_. With one thread, you can guarantee ordering (“this came after that”), guarantee what was known at decision time, and create reproducible runs. In multi-agent systems, “context” often becomes an eventually-consistent soup—fast, but frequently wrong in subtle ways (stale summaries, racing memory writes, duplicate tool calls, conflicting interpretations). Reliability drops not because agents are “dumb,” but because the system loses a single causal chain and a single commit log.

Operationally, single-thread systems are easier to evaluate and harden. You can build deterministic snapshots (input → built context → model output → tool calls → final) and write regression tests against them. When something breaks, you can trace _one_ decision path. With multiple voices, failures are emergent: the bug is “in the interaction,” which is the hardest kind of bug.

---

Write me a complete refactor plan to get to a “super assistant agents” without losing single-thread coherence

Produce a complete refactor plan that gets us from our current “agents module that adds negatives” to a “super assistant” architecture: single-thread quality, faster via parallel work, larger effective context via memory compaction, and higher reliability via strict state ownership and better evaluation. Include the target architecture, the key module boundaries and contracts, the runtime concurrency model, the memory strategy, the migration steps, and the testing/evaluation plan. Be opinionated and specific.