Remenmber I've asked you to simplify pamphlet, simplifying defineFlow and defineSkill. You did not do as I asked bellow?

## Prompt processing

We know we might sometimes need a planning thought on which steps will be ran to get the best result from prompt processing llm, and how many agents will run, what memory is needed from text, and a fast llm that grab context, and history based on prompt, and grab events memory from what recently had happened. We do need a small llm to filter out tools, and pamphlets, mcp etc.

We know that if we need to plan thought we must also plan what agents will be ran, import necessary tools, and agents.

## Pamphlet Here's how I'd want it to be

I'd like a better design than what I present but the idea is, we have a simplified version than what we currently have. With steps being small prompts of what to run, and instruction letting us know how we run the steps. steps basically contain steps that may run in the process, and since it has prompt, I'm wondering if the steps can comes with tools as well, or tools is outside steps in the object. Thinking outside may actually be better but unsure.

Pamphlet can still contain other pamphlet just like we had before.

The new architecture would be something like this

{
	instruction : string;
	steps: Array<[{speed, reliable, contextSize, etc, prompt}]> 
	// can be nested array but will be flatted on setup, and needs documented, that nested array doesn't help in any cause, just helps when developing. Steps may have nested array but when reading, seen as singleton and flatline. So when defining similar step with different llm feedback, its useful in the prompt to determine when to use which.
}

when we run defineSkill or defineFlow the steps are flatten.


How will planning diagram be executed, and how will we manage agents, knowing what agent to run, where speed or quality is needed. Pamphlet will need to determine speed or quality with levels.

How do we make pamphlet 3d diagram to 1d, maybe we have instruction, and a step section. llm run through the steps as if they were prompt tools, but instruction is basically a prompt.


Question how do we make steering optional feature?? Helping guide and making sure we are untrack where common pitfalls happen??
{
	instruction : string;
	steps: Array<[{speed, reliable, contextSize, etc}]> 
	// can be nested array but will be flatted on setup, and needs documented, that nested array doesn't help in any cause, just helps when developing. Steps may have nested array but when reading, seen as singleton and flatline. So when defining similar step with different llm feedback, its useful in the prompt to determine when to use which.
	schema:
}
