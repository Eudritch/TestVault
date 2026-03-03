## Where we at

How will planning diagram be executed, and how will we manage agents, knowing what agent to run, where speed or quality is needed. Pamphlet will need to determine speed or quality with levels.

How do we make pamphlet 3d diagram to 1d, maybe we have instruction, and a step section. llm run through the steps as if they were prompt tools, but instruction is basically a prompt.


Question how do we make steering optional feature?? Helping guide and making sure we are untrack where common pitfalls happen??
{
	instruction : string;
	steps: Array<[{speed, reliable, contextSize, etc}]> 
	// can be nested array but will be flatted on setup, and needs documented, that nested array doesn't help in any cause, just helps when developing. Steps may have nested array but when reading, seen as singleton and flatline. So when defining similar step with different llm feedback, its useful in the prompt to determine when to use which.
}


## Pamphlets

Steps may have nested array but when reading, seen as singleton and flatline. So when defining similar step with different llm feedback, its useful in the prompt to determine when to use which.

Pamphlet can still have other pamphlet within

How do we implement steering features in pamphlets???



How exactly do we determine which step may take a long time to run.

Or which step can benefits from lots of agent running, weather through voting, or helping with speed, or light work small agent can run.

Maybe each steps comes with meta???


But how does orchestrator understand how to handled it, maybe pamphlet should have instruction for orchestrator, determining how to handle it, how many agents to allow, and how to handle the prompt, undestanding steps and determining a planning diagram with section of what agents to run.



How do we make a planning diagram and follow through knowing which step will run and which will not.




<INSTRUCTION>
</INSTRUCTION>

<AGENT>
</AGENT>

## Orchestrator

Weather we can answer question immediatly.

Orchestrator looks at instruction and determines which steps needed in what order and a Planning Diagram. And what agents get ran.

Filter out Tools and Pamphlet based on prompt knowing which we run.

Find out weather we run to save context, in an efficient manner as ai struggle with long process. Weather we split work with agents to keep accuracy, as ai struggle with lots of work. Weather we run multiple agents simultaneously for speed, as ai is limited to bandwidth and multiple agents can help with speed. Weather we use voting, as multiple model coming up with question and voting which is best answer, can give better result then big models in a lot of places which you'll need to do research on.