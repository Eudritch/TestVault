**Todo**:

Kanban
Mobile
+config.yaml

Page Group Layers, Layout and Pages - Nested Path Layers and page id or category replacement

Also have features list weather it support fully swapping for another page, and versioning

weather page has changable header or footer.



Clear understanding of app core and reminder, document, chat


Working ai chat page.


Clear understanding of how overlay work, z-index and ability to overlay only canvas and avoid Panes



## +Config.yaml

Here's what I want from, rename +layout.yaml to +config.yaml and add these features:

I want PageGroup and Pages

Basically a tree PageGroup contains multiple other pages, for example clock, calendar, etc, page group is essenstially a navigator, of pages, with a clean ui.

A Pack can contain many pages without a page group, but this navigator is really nice to have especially for mobile

I'd like it if we had a path just like this, but it'll be in yaml each dir representing a different group, and an optional navigator layout.

root/
├── src/
│   ├── main.py
│   ├── utils.py
│   └── config/
│       └── settings.json
├── tests/
│   └── test_main.py
├── docs/
│   └── README.md
├── .gitignore
└── requirements.txt


We should also be able to import another +config.yaml from another +config.yaml


There will be multiple +config.yaml available and each nested dir override there parent, well not complete overide have a higher specity just like css, and we should be able to clear out specifict labels, or property from all parent inside +config.yaml. also we should have sveltekit features of advence layout where we use +config@ to completely reset config.yaml into anew or we use +config@name to get parent (name).

Here's some more feature I'd like, +config.yaml to be able to filter out what files get shown in NavigationPanel.svelte looking at transform of line 143.

NavigationPanel.svelte will have support of navigating to a specifict dir and +config.yaml should have actions and what to do on action, and redirect NavigationPanel.svelte to a directory, and bread crumbs will show of the current path it is on



For Pages may support updating its header, or footer, and +config.yaml should be able to deside header and footer, of pane as canvas.svelte support and if its available replace and let the defined be what is active.


For NavigationPanel.svelte I'd like breadcrumbs capability and ability to set different secion like header and footer, we currently have primary-nav-section along with footer-section, but I'd like these moved into +config.yaml default.

And the BreadCrumbs part must be at the top most of component.