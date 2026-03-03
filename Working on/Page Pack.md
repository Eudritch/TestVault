Setup MessagePage and sdk multiple options and support
Same with meeting page
Same with Reminders Page grocery etc

Need a pack of sdks and which functionality pages supports.


## Support

- SDK of zod and interface plugin support, ability to hot swap, weather some functionallity are required or optional.
- HotSwapable Page with ai able to assist migrating.
- Need better support in onboarding
- Onboarding since we have navigational bar able to go nested deep we should make use of that support, also on boarding should help us setup our folder structure. Which would mean +config.yaml will be used with folders.
  
  
  
  You know how vscode has settings page, with ui, and optional to switch to json form. I'd like the same for when opening +config.yaml files. I've made src/routes/(app)/config specifically for that, and we'd likely have to change DEFAULT_VAULT_CONFIG DEFAULT_ROUTES for that. And we'll need the page to have a header that allow to choose weather we render code mirror, or we render ui. Also I'd like you to update  the ui make it nicer, and make it up to date with the new vault_config features. I'll also like to make it ux friendly, and make it fully functional, able to manage the file properly.