## Needs before simplifying
- Need plugin system just like obsidian - for our plugin system will be working with and making or existing plugin system better, breaking changes is fully allowed as this is not production yet. Will be working with src/lib/api
- Need theme system like obsidian in our settings page, and reactivity needs to be done perfectly will be working with src/lib/stores/ui/theme.svelte.ts.
- Need ways to monitor when theme changes.
- So things like app-shell can reflect that.
- Just like the plugin system, need an page-pack system, and a page for it on settings, to activate some and remove some.

- For the plugin system, I need you to research how obsidian, logseq, joplin, make there api look like, and I need you make it similar to the best one. I'm looking for simplicity and professional. I'd also like mine to have sandbox and permission ability, sandbox, if needed have different levels of sandboxing, but what I had in mind was strict or non-strict. And for the permissions ability I'd like it to be similar to mobile and phones, or tauri, where you basically ask permission for any access, file access, mic access.
  
  And another way I'd like the plugin, to have possible of submodules, these submodules could be made to access files, but have a special way of knowing no content will be leak, everything is offline, the submodules have a simple job, just to run the file content, do whatever it needs to do withit, the main plugin can only send request to it get no result back. This could be used for things like edit-proposal in my app, etc. Giving features to notes, while we know everything is private. This could be useful if one of the plugin have access to web, yet also have access to content, but ensuring everything is private. I'm unsure how to make this work.


## Stores Folder

The only thing I want to keep in there are: 

- theme.svelte.ts
- titlebar.svelte.ts
- viewport.svelte.ts
- app-shell.ts
- constants.ts
- edge.svelte.ts
- notification.svelte.ts
- panel.svelte.ts
- core folder
- context folder
- core-folder
- _services folder_
- core-blueprint

### Need to figure out what's features worthy
**Core**
- app.svelte.ts
- navigation.svelte.ts
- router.svelte.ts
- tabs.svelte.ts
- vault.svelte.ts
- theme.svelte.ts
- workbench.svelte.ts _Not used anywhere_
- workspace.svelte.ts _Not used anywhere_
- app-shell.ts from outside needs to be moved inside core.
- Same with src/lib/stores/constants.ts
- The context folder or its content should be moved to core, and well organized.

**Features**
- notification.svelte.ts
- reminder.svelte.ts
- sync.svelte.ts
- tasks.svelte.ts
- blueprints folder from outside should be moved to features
- page-pack as well
- src/lib/stores/markdown content as well but we'll need a better name for it.
- Everything else inside src/lib/stores/features should be moved to src/lib/features

**Questions**
- What is active-file-sync.ts
- How do I make panel.setup
- Looking to simplify code, and shorten codes.
- What do you think we should do with types folder inside the store folder, do we keep it and organize the structure of the folder? or what do we do with it?

## Blueprints

## Config.yaml and API

- Need to figure out panel.setup should be make and onDestroyFeature

## Pages

Need to figure out how to page setup config


## Separate app features Outside of core

- Nav-Sheet should be outside of core.

