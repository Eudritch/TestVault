I'd like you to make me a package folder that relies on the packages/llm that contains ui to be able to use with llm. And I'd like us to move the ui from our apps into this packages, and our app will be using this  in chat composer etc. We will move out src/lib/components/widgets into that package as well as src/lib/components/llm/MessageRenderer.svelte 

This package will contain as well ReasoningBlock and MessageRenderer.svelte and takes in a markdown-it optional prop


ReasoningBlock is already inside widget folder which I'm thinking it'll need to be moved.



Remenmber to make it align with packages/llm/runtime ui features, and make changes to that feature to make everything work.



This new package should also use packages/llm pamphlets accordingly containing its own pamphlet and orchestrator to interact with the ui well, knowing when it be used, and when not to use it.