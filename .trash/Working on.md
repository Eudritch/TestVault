Obsidian have I'm looking to build my own for my app and I'm thinking, also I'd like it to be better organize instead of leaving it like obsidian docs where all types, class and everything is all laid out and ambigous to look at and understand how exactly the whole thing work https://docs.obsidian.md/Reference/TypeScript+API/AbstractInputSuggest/getSuggestions I'd like mine to be better laid out, organized, secionized etc.

I'll like my own Typescript Api inside a folder within src/lib and I'd like my own docs inside src/routes/(docs)/docs

I need you to look at Obsidian docs, and look at my src/lib/stores especially src/lib/stores/core/router.svelte.ts analyzing how +config.yaml work and making api on that as well

You will also need to declare a variable with the app name in window in src/app.d.ts, you will define the app name to use in declare in .env
 
---

I'd like the css and style of src/routes/(app)/(mobile)/history/+page.svelte, packages/markdown-editor, src/styles/foundations.scss to be changed, and I'd like all reference of obsidian to be removed.

Hey this is what I want. I'd like you to remove the purple color and replace it with some other color. I'd also like you to change some of the other colors to make it better suited. Most of the color was copied from obsidian and I'd like it to be changed so it does not look like I copy it, I'd  color that are creative and professional that are better suited.

You are to change all color to make them better suited, and work in harmony with the app, and remove all the keyword obsidian.

---

I'd like the meeting page to be redone. It should have 3 section, left, center, right.

On the left I'd like it to be a section on what we working on, containing events done in since the meeting started, things like files shared, things opened, events summary looked in an event manner, that ai adds, etc. It should only contain important things in the left side.

On the center, it should be just similar to zoom, where at the top we have the people on the zoom, a list of square boxes of people, on the middle we have the content or screen being shared, at the bottom we have circle interactable buttons.

On the right at the top , that will contain 70% or 100% if no bottom of the view. If we are sharing video we have a collapsable section at the bottom of queue, where we can add videos to queue, we can rearange etc, we may even add link video to queue.


I'm thinking if we not streaming it may look like discord, where we have a section that shows people in the call, and at the bottom just a regular chat, that may contain events integrated in the chat.

## Beta release what's stopping us

- Harmony pages working in harmony ui etc, drive
- UI consistency and scss
- ~~Pane consistency - width, min-width, max-width~~
- Mobile
- ~~Obsidian Typescript SDK but better, better researched etc~~
- Better LLM integration with context-chip
- Pane disparity between pages and tabs.
- Theme and Plugin support, and theme coming in with extra css
- ~~Remove obsidian purple color in scss and in markdown-editor make more blueish??~~
- ~~Move default panel to default config instead of having it defined in +layout.svelte~~
- ~~Remove having to set panel state to true in +layout.svelte~~
- ~~Set auto panel active state saved need to figure out how to handle the width so it don't expand beyond what it should be~~
- ~~Need to setup right panel in titlebararea~~
- Pages Specifically for mobile
- Selective when to show MobileNav and fully functional
- How to handle mobile-nav toggle, ability to swipe-up and a second swipe-up toggle history
- Make history page functional and make use of tabs
- Make meeting functional and better looking, with sdk/plugin support
- Make use of document and make me an obsidian graph-view
- Make me an insight panel icon, that is grayed out by default, if has no active component within. Same for all other panel

- Usefulness making calendar and etc without any body asking, scheduling etc.



- Fully functional component, stability including
	- ~~directory tree~~
	- tabs
	- ~~multi-split-view~~
	- ~~+layout scroll pattern~~
- App name, icon and website origin
- Pages
	- Meeting Page
	- Grocery Page
	- People Page
	- Calendar Page
	- Jot Page
	- Etc

Later creating typescript api

analyzing obsidian.md typescript api

and seeing how we can make ours based on how our app work
also obsidian api typescript is not well organized, doesn't have a core , and types, etc all is organized alphabetically which shouldn't be the case for mine, it should be easier to navigate mine, everything should be better, easier, and neater




### LLM

What else is stopping us from running as swift as claude code runs mcp, and as smooth as LangGraph and Cursor runs agents? Without using regex or any that sort of tools, I'd like from initial prompt for orchestrator-pamphlets.ts and its orchestrateInternal function to do a better job. Maybe when before running agents we create a task graph of agents what will be running, or something. How do you propose we get to level of claude code, LangGraph, Cursor, and OpenClaw?


What else is stopping us from getting reliable responses just like a human would, handling context, memory, intent, everything well, and being able to trust that what ever we ask a human, the answer we get is reliable.



The llm model should be what handle the path and what to do as a human. We should not have things like isActiveNoteQuestion, isLastMessageQuestion, isGreetingPrompt in orchestrator-helpers.ts nor have if else statement, if clock etc inside orchestrator-pamphlets.ts

Everything should be handled by model knowing what todo etc, I said to remove regex for that reason, not to be replaced by another sort that does what regex was doing. We should not have prompts like these

<PromptsNotToHave>
	If a prompt explicitly asks for current time/date/day (clock lookup), and clock is allowed, return d="t" with t="clock".

	Do NOT use clock for planning/advice phrasing like "when should I ...
</PromptsNotToHave>

What else is stopping us from getting reliable responses just like a human would, handling context, memory, intent, everything well, and being able to trust that what ever we ask a human, the answer we get is reliable.





Surely this project packages/llm have a bunch of unnecessary things or step within it, that could be removed and won't affect anything for the worst, only for the best. I'd like them find. Could be code files, certain codes, or anything.



The pros of a single model single thread are that its more reliable, better context understanding, better follow through,  fast, better intent understanding.

However there are pros to agents, speed, more task being done at once, parallelism, etc.

And I'd like more pros like better context, smarter context, larger context, by using agents for handling context in runtime/background-pamphlet.ts by categorizing memory compacting, summary etc

I'd like all the pros of a single thread model, we currently don't have that, our agents module only bring us negative to the point where it is not worth it, and we need a complete refactor.

I'd like to make this rich into a super llm, assistant, that is on par with single model, single thread, while being faster, more context, lots of agents, and more reliable.



Plan the complete refactor we need to get us there



I need you to figure out and plan out what's left before production. You must find out every single weakness and plan out improvements to be made. Make sure everything is perfect, we know all available pamphlet and tools and able to work with them perfectly, and that we spin off as many agents needed to finish task quickly, and none if not needed. That conversation and context is top shelf and production grade and can replace human at keeping track of what was said and done, and intent with result.

Make sure that we have the best of the best, and every weakness is tackled.

Make me that plan evaluate what improvement is needed, what weakness to tackle, what changes to be made in that plan.










### Api

Something rigid and nice

something that work fluently with .page, +config.yaml and api 

Need to compare notion sdk/api and obsidian, need to make it supper neat and features pack


Need to figure out features that I will need, including features like theme, extension etc, but these won't be available just yet.

App needs to be released for beta, need to figure out what's stopping betta.


## Untitled (8)