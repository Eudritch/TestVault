I want you to do the todo, and help me get the app to production by making a clean api out of src/lib/stores. I need you to look at bellow and figure out how to make stores a clean api. stores is an api and is missing some of the functionality that I need. stores folder is pretty neat and pretty clean and I'd like what ever you plan to do with it to remain neat. I'm looking to make my app an obsidian alternative and on top of that a notion alternative, and as an open source app we do need api levels. Look at the my unform thoughts markdown bellow and plan out a perfect api and managable things and all features I mention but better, these are my ideas but I'm sure you can make them 1000 times better.



## Todo

Make a rigid api and that is what +config.yaml  will use

Do not forget root layout and layout

Need to explain layout as well in App Insight

Need to make it a clear document that any body can understand


Add layout which is +config.yaml root and +config.yaml basically integreated to api, I use layout as name rn but I would like a better name for the api.

Need a section of api that is meant for only one active and not merge and will need a name for it, inside +config.yaml we may declare active layout links this is use full for PageGroups, Layouts, that display links to what we support



## App Insight

The app is a modular app able to take in whatever your wishes is. The app is mean't to help assist and organize, and helps manage whatever you mean, its meant to be your dream app and website and should be able to replace all your needs, google services etc, weather self hosted, or still using the same services with a new shell that is 100% customizable to your needs and wonders.

It's an all in one notion, obsidian, slack and zoom alternative packed in. With the app being the shell and as a modular you can navigate through switching platform and navigator as you wish.


The app core are:
**Features**
- Documents
- Chat
- Reminder
- Panes _(Which are Panels, Canvas, and TitleBarAreas)_
- Layout and Root Layout
- LLM
**Display**
- Panes which are Panels and Canvas, and TitlebarAreas.

**Pages**
- Clock
- Note
- People Friends
- Grocery

## Onboarding

Here's how I want the on boarding to be. We will have two path, standard and advanced.

We will have an option to click advanced.

**Standard**

On standard we have a section where we select why we want to use the app, up most 3 so we don't make it ambigous for our use. standard is mean't to guide you, and ease you into the app.

We'll have option like, note, calendar, manage task, leisure, teams, organize, etc.

Then when you select continue you get moved to the next section where it'll guide you to how you want navigation panel setup, weather you want it to have Pages aligned on top of navigation, and weather you want navigation to be able to dive deeper and swap pane to shows directory of chat, or slack etc.

It'll help you structure your directory with folders and labels and order that meats your needs. and help you get the pages you need and things setup in a fine manner, please take a look at src/routes/(app)/config this could be useful.

And the final page of directory setup is what I had in mind is it helps you sync, or ask you to pay for subscribtion to help sync, or it helps setup plugin that helps you sync it.


Ongoing to the next step of onboarding, we display how the app work and on the last page, we ask with a check box to skip this onboarding on new vault creation with a continue button.

**Advanced**

We get our selection pack, and we select which pack we'd like to add, pack like the one that contain settings page get's auto added.

### Page Package UI Structure

When you open a **Page Package**, you see two main sections:

#### 1. Pages Section

This section displays:

- **Shared Pages**
    
- **Singleton Pages**
    

Each page is shown as a **card**, with an icon in the bottom-right corner indicating its type (Shared or Singleton).

#### 2. Layouts Section

This section lists all **Layouts (Page Groups)**.

Each layout card displays:

- The number of **shared pages** it uses
    
- The number of **layout-specific singleton pages** it contains
    

---

### Page Type Rules

- **Layout Singleton Pages**  
    Belong only to a single layout and cannot be reused elsewhere.
    
- **Global Singleton Pages**  
    Standalone pages that are not part of any layout.
    
- **Shared Pages**  
    Pages that are referenced by multiple layouts.
    

and just like standard, we should get asked how navigation panel is setup, and display how our app work. and have  Ongoing to the next step of onboarding, we display how the app work and on the last page, we ask with a check box to skip this onboarding on new vault creation with a continue button.

**Standard**

On on boarding you get to select what you'll be using it for, and it should limit you to how many selection you can make in each section to not be overwhelmed from start.

It'll guide you to how you want navigation panel setup, weather you want it to have Pages aligned on top of navigation, and weather you want navigation to be able to dive deeper and swap pane to shows directory of chat, or slack etc.

It'll help you structure your directory with folders and labels and order that meats your needs.

And the final page of directory setup is what I had in mind is it helps you sync, or ask you to pay for subscribtion to help sync, or it helps setup plugin that helps you sync it.


Ongoing to the next step of onboarding, we display how the app work and on the last page, we ask with a check box to skip this onboarding on new vault creation with a continue button.


## Panes and Config Setup

Depends on what mounting allow, we may have header, footer, and additional option that are managable and interchangable weather through +config.yaml or through settings through api.

**Config.yaml**
We should have an option that let us know if it only manage directory and no subdirectory, or an array of subdirectory to manage including its directory.

If it is a layout we should be able to set navigations of layout.

Need a custom page for +config.yaml inside src/routes/(app) that helps manage it without having to use yaml. Just like vscode has its settings you can view it as json or as ui.
## Panes and functionality

App Panes are:

**navigation** _(Helps navigate directory)_,
**support** _(Gives info, like time, and tasks)_,
**insight** (Assist a page useful as a sidebar overlay, that get shown on click actions),
**activity** (Basically a secondary panel, useful like using it as assistant panel)

When setting insight overlay or not


## Api, +config.yaml  and .page



#### Features
Api needs to be robust and we should be able to have things like storage encryption and syncing among other things. And will need to make packages



#### .page and +config.yaml

Ability to import a +config.yaml and have api ability in javascript just like +config.yaml import, +config.yaml already have that feature now .page should have it aswell

**Page**:
Some page may have there own feature for example note page, may allow setting predefine component, like for example by default we'll have a component at top of renderer, that shows the name of content, and is editable, but we could also swap that out with a component that shows icon plus the name that is editable, or other things.

I'm thinking we use Yaml instead of the object we had prior. That'll make things easier to manage.







## Mobile

Padding top visualViewport implemented in +layout.svelte same for padding bottom

Implement padding top or margin top, do the same for bottom with visualViewport for mobile, tablet, (and desktop??)


In settings page for mobile have option to make the height of tapping bellow footer to be able to fuse with viewport bottom, works with certain android including samsung, need to make notes of what it does and for android.


## Settings





## Pages and how they React

Pages can use api to set up panel and by default when pages change panel revert to there previous state, but page can also say to keep panel permenantly unless specifically removed later.

Pages may also do the same with headers.

Page like kanban use insight Panel as overlay to help with making ui easy.

## Page Extension

### Page Package Overview

A **Page Package** contains three types of entities:

1. **Shared Pages**  
    Pages that can be used by multiple layouts.
    
2. **Singleton Pages**  
    Standalone pages that do **not** belong to any layout.
    
3. **Layouts (Page Groups)**  
    A layout is a group of pages and can contain:
    
    - Its own **layout-specific singleton pages**
        
    - References to **shared pages**
        

---

### Page Package UI Structure

When you open a **Page Package**, you see two main sections:

#### 1. Pages Section

This section displays:

- **Shared Pages**
    
- **Singleton Pages**
    

Each page is shown as a **card**, with an icon in the bottom-right corner indicating its type (Shared or Singleton).

#### 2. Layouts Section

This section lists all **Layouts (Page Groups)**.

Each layout card displays:

- The number of **shared pages** it uses
    
- The number of **layout-specific singleton pages** it contains
    

---

### Page Type Rules

- **Layout Singleton Pages**  
    Belong only to a single layout and cannot be reused elsewhere.
    
- **Global Singleton Pages**  
    Standalone pages that are not part of any layout.
    
- **Shared Pages**  
    Pages that are referenced by multiple layouts.
    





