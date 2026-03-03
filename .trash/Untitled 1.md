# Runtime

**Steps**


Need main process rules, that gets pick as needed. For example for math, never use ai to solve math rather use tools.


We get context and also based on intent, and we also get active rules notes context.

Based on context we get real intent of the question asked.

From the question and intent asked, we get all tools that might be useful to answer question. and we also use tools that will most likely needed, and lightweight and add to the context, for example current time, if needed.

At the same time, we are running lightweight tools. we asked user question about their intent, if its too complicated we asked them to clear it out and make it better understandable for ai.

We reingeneer prompt with context and tools, if it is hard to understand.

We make write notes, and rules if the prompt says too, or we likely would need too.

We break down tasks, and simplify tasks, and add to tasks notes, current process context.

We set order agents may possibly run, with reliance order etc, based on what we got, we make the orchestrator planning agent, which models runs, and tree branches.

We transform and put all these agents answer together, and we refine, if we are expected to respond with special way. Before refinement if task was hard, we check if we solve what was asked of us.


**Needs**

Is context intent and question clear to understand intent?
How hard is the tasks, do we require agents?
If task hard, do we require voting, or bigger model?
Can we give quick answer while also following current rules context?

Context tools, notes and task.



Intent/Context
Tools Needed
Rules
Memory
Task completion
Reruns, Rules, Fetching
active loaded models

Context requrirement and fetching ability for each agent


router

preprocess
transform
refine




do we have rules to follow, do we have specifict way of text output

response task
prompt task
unfinished task

types.ts (root folder)
pamphlets.ts
- intent
- context
- intent
- any user question?
- any notes/rules need to be added? weather have conditional rules
- prompt tasks added to context task
- prompt re-engineer including some tool uses
- tools to show that might be needed.
- agents
- order time consuming and reliance.
- transform
- refine

background-pamphlet.ts
- summarize context
- group prior message
- organize a rag context (query vs rag, google)

core.ts
- tools
- pamphlets
	- (Needs a new class so I know steps that been added to ai model, or we need agent for example refinement can be done later or at start)
	- organizer
	- 100 steps
	- refinement
- models
index.ts

## Core Tools/Pamphlets (maybe core.ts that import these)

# Pamphlets

## Skills

### Wordsmith

- specialized for programming
- specialized for folders

### Fetching

### Ai Voting

### Simplify

## Brain/Core/Focus/Thinker

## Specialty (and solve database?)
- Folder Organizer

## Refinements and way to output


# Models

Ai levels 1 to 5
ai skills

Embeding
Light
Thinking



# Tools



# Thoughts

Pamphlet need a way to dynamically chose agents, weather one or multiple. and model that is right for it.

Need way to solve reliance on agent.




[3:01 PM, 1/17/2026] Eudritch Benjamin: To avoid slow down rank on how long it'll take
[3:02 PM, 1/17/2026] Eudritch Benjamin: Context requirement for whole and for each agent
[3:02 PM, 1/17/2026] Eudritch Benjamin: Multi and agent
\[3:03 PM, 1/17/2026] Eudritch Benjamin: And main process which solves what is asked


[3:09 PM, 1/17/2026] Eudritch Benjamin: Main process rules like using tool to solve math never rely on ai
[4:09 PM, 1/17/2026] Eudritch Benjamin: Busybox
[5:12 PM, 1/18/2026] Eudritch Benjamin: Prompt reengineer part reliance and researches.