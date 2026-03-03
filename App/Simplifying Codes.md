- Remove the folder src/lib/actions while finding a respectable place within our lib in our existing folders for the files within.
- Thinking anything store related from src/lib/types needs to be moved to src/lib/stores/types
- Remove src/types if necessary, if its not use full, unsure why its even there
- Remove /plugins folder and make sure nothing is broken doing so.
- Need a src/lib/stores/app-shell.ts that handle shell, titlebar sizes, and tauri background color to match app bg color, and TitleBarArea to match background color.
	- src/routes/(app)/+layout.svelte line 18 to line 29 should be there, also the px amount should not be set by this, it should be set by TitleBar.svelte using a svelte bind:clientWidth, should not define a fixed width, and if we need to update src-tauri/src/titlebar.rs we may do so.
	- For app frame color we need to use tauri api if isTauri import { isTauri } from '@tauri-apps/api/core'; if isTauri we set the frameBackground color to match our theme.
- Find what /scripts is about if nothing we need remove it



- Need to figure out what to do with src/styles
- Might need an src/lib/sdk or blueprint for all app sdk, and pages blueprint



- 






- Remove non needed code
- - Remove RightSidebar.svelte
- Better structure and making things easier to follow through
- Simplify Api and functionality
- Simplify while keeping all functionality basically better structure and organize api, and api inside +config.yaml.





- Organize pages into page-pack

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
