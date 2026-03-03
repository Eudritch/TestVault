I need it fully fixed, that navigation fully work, I want everything bellow implemented.
- I want it to be able to work with storage syncing with active vault
- I want settings to be per vault
- I want it to manage multiple settings with our app such as having an appearance and theme page



## Tabs I'd likely need, and order should be organized

- **General**
	- Show things like version, automatic update, language, help, etc
	- Account related things like login, signup, commercial license etc
- **Editor**
	- Helps manage editor settings
- **Files and links**
- **Keyboard Shortcut** _Similar to vscode_


## Everything I will need

- Sync














We are only working with src/routes/(app)/settings




You are my product + UX architect. I’m building an Obsidian/Notion-style knowledge app, and it’s already built. I’m currently implementing the Settings experience.

IMPORTANT CONSTRAINTS
- Do NOT assume a permanent left settings sidebar.
- Settings must use an UPPER NAV (top navigation) for the primary sections.
- Some individual settings pages MAY optionally use an internal LEFT PANEL for sub-navigation (example: Plugins page can have a left panel to browse plugins/categories and a right panel for plugin details & settings).
- You must adapt everything to match how my app is already structured. Do not propose an IA that conflicts with my existing navigation patterns.
- You must read and align with my nav-sheet. Use the nav-sheet as the source of truth for naming, routes, hierarchy, and screen patterns.
- Where the nav-sheet already defines an area, fit your plan into it. Only suggest new routes if absolutely required, and clearly justify them.

YOUR TASK
Rewrite a complete “Settings Page Feature Universe” for my app, but organized for an upper-nav settings layout, with optional internal left panels only where they make sense. Produce a structured spec that I can implement immediately.

DELIVERABLES (output format)
1) SETTINGS IA (Upper Nav)
   - Propose 5–8 top-level Settings tabs (upper nav items).
   - Under each tab, list subpages and subsections.
   - For every tab/page, note the scope of its settings:
     - App-wide vs Workspace/Vault vs This device vs This account (or whatever scopes exist in my app per nav-sheet).
   - Keep labels consistent with the nav-sheet naming.

2) PAGE LAYOUT RULES
   - Define the standard “Settings Shell” layout used by all settings pages:
     - header contents (title, description, settings search, save status, reset section)
     - content layout (cards/sections)
     - error states + “saved” indicator behavior
   - Define when to use:
     a) no subnav (simple page)
     b) in-page tabs/segmented control
     c) anchor mini-nav
     d) optional internal left panel
   - Include mobile behaviors for upper nav and optional left panels.

3) OPTIONAL LEFT PANEL PAGES (ONLY WHERE NEEDED)
   - Identify which pages should have an internal left panel (e.g., Plugins, Integrations, Devices, Workspaces, Members, Templates).
   - For each, define:
     - what appears in the left panel (search, filters, lists, categories)
     - what appears in the main panel (details, settings form, actions)
     - empty states (no item selected), loading, and error states
     - routing/deep-linking behavior (select item via URL)

4) COMPLETE SETTINGS CATALOG (THE FULL LIST)
   - Provide a comprehensive settings catalog (Obsidian/Notion-level) mapped into the IA above.
   - For each setting item, specify:
     - Setting name (user-facing)
     - Type (toggle, dropdown, text, number, multi-select, action button)
     - Default value (reasonable defaults)
     - Scope (app/workspace/device/account)
     - Dependencies (e.g., appears only if sync enabled)
     - “Danger” classification (safe, caution, destructive) + confirmation needs
   - Use groups like:
     - Account & Profile
     - Workspace/Vault
     - Appearance & UI
     - Editor & Writing (markdown/rich text behaviors)
     - Notes/Files/Attachments
     - Search & Indexing
     - Sync & Offline + conflict handling
     - Collaboration & Sharing
     - Notifications
     - Databases/Properties (if my app has them)
     - Shortcuts & Input
     - Integrations & API
     - Plugins/Extensions (if supported)
     - Publishing (if supported)
     - Security & Privacy
     - Performance & Diagnostics
     - Billing (if applicable)
     - About

5) SETTINGS SEARCH + DEEP LINKING (CRITICAL)
   - Specify how “Settings Search” should work without a left sidebar:
     - search indexing fields (setting name, synonyms, descriptions)
     - result grouping (by tab/page)
     - click behavior (navigate to correct tab/page + highlight setting)
     - deep link format (routes + hash anchors)
   - Make sure it’s compatible with the nav-sheet routing patterns.

6) IMPLEMENTATION NOTES (PRACTICAL)
   - Provide a “ship order”:
     - MVP settings (minimum to ship)
     - Next wave
     - Later/enterprise
   - Provide UI guardrails:
     - max controls per page before requiring sub-structure
     - reset-to-default patterns
     - import/export settings JSON patterns
   - Mention any tricky edge cases:
     - sync conflicts
     - multi-device scope vs account scope
     - plugin safe mode
     - accessibility defaults

HOW TO ADAPT TO MY APP
- First, summarize what you infer from my app’s structure and nav-sheet: section names, existing routes, where Settings lives, and how nested navigation works.
- Then produce the deliverables above strictly aligned to that structure.
- If something is unknown from the nav-sheet, make the smallest reasonable assumption and clearly mark it as an assumption.

INPUTS YOU MUST USE
- My app’s current navigation structure (inspect what’s already built).
- My nav-sheet (treat as source of truth).
- Any existing Settings screens already in the app (match visual patterns).

OUTPUT QUALITY BAR
- Be concrete. Avoid vague “could” lists—give exact settings, defaults, and routing.
- Keep it implementable: clear groupings, consistent labels, and minimal contradictions.
- Do not exceed 8 upper-nav tabs unless my nav-sheet already exceeds that.