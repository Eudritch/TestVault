We are refering to files inside src/lib/features, I need you to make me plugins, based on src/lib/api. Please plan out how you will get the job done right, and with simplicity.


- I want ai-context.svelte.ts to be turn into a plugin but we needs better name for that files.
- I want src/lib/features/constant.ts to be spread out to multiple files inside src/lib/constant.ts
- I want blueprint move out of src/lib/features/constant.ts into src/lib/blueprint folder into multiple files.
- context-chips.svelte.ts should be the same plugin as ai-context.svelte.ts, I'm unsure how we will manage it, weather the file will be merged or not.
- src/lib/features/edit-proposals.svelte.ts might rely on ai-context.svelte or context-chips.svelte.ts but it should be a seperate plugin.
- I want src/lib/features/sync to become a plugin.
- I want a src/lib/plugins that contains app core plugins.



The reason that edit-propolsals would be a separate plugins, is that other pages, and other things may access it in the future, to support edit proposal, on non markdown pages.





I'm thinking src/lib/automation should be moved into the plugins folder, and be made a plugin as well.