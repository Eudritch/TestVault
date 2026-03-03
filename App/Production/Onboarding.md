## Onboarding process

Seemless, a ui that have steps integrated to it that make it seemless in a way you don't even know that you are setting it up for your self.

The first few steps may have different ui and doesn't show what steps you are on so you can move to it quickly without realizing


I'd like you to help me come up with a creative way to make my onboarding process in a way that you move extremelly quickly through it, without realizing your setting the app for your self, or that your performing an activity. I'm talking about the different pages and layout so your brain don't realize that your performing a task.



## Onboarding Flow Overview

  

The onboarding experience offers **two paths**:

  

- **Standard** — guided, opinionated, and beginner-friendly

- **Advanced** — flexible, modular, and power-user focused

  

Users can choose **Advanced** at any time from the start.

  

---

  

## Standard Onboarding

  

The **Standard** path is designed to guide users, reduce ambiguity, and ease them into the app with sensible defaults.

  

### Step 1: Usage Intent

  

Users select **up to three** reasons for using the app.  

This helps tailor structure and defaults without overfitting.

  

Example options:

  

- Notes

- Calendar

- Task management

- Leisure

- Team collaboration

- Organization

  

---

  

### Step 2: Navigation Setup

  

Users are guided through setting up the **navigation panel**, including:

  

- Whether pages are displayed:

    - At the top of the navigation

    - Within nested navigation levels

- Whether navigation supports:

    - Deep navigation

    - Pane swapping (e.g. directory view for chats, Slack, etc.)

  

---

  

### Step 3: Directory Structure

  

Users configure how their workspace is organized:

  

- Folder structure

- Labels and categories

- Ordering and hierarchy

  

This step helps users:

  

- Create a directory that matches their workflow

- Set up essential pages and views in a clean, intentional way

  

> Reference: `src/routes/(app)/config` for implementation alignment.

  

---

  

### Step 4: Sync & Integrations

  

Users are guided through synchronization options:

  

- Enable syncing (free or paid, if applicable)

- Upgrade to a subscription for advanced sync features

- Set up plugins that enable syncing and integrations

  

---

  

### Step 5: App Overview

  

A short walkthrough explaining:

  

- How the app works

- Core concepts (pages, navigation, directories)

  

---

  

### Step 6: Onboarding Preferences

  

On the final screen:

  

- A checkbox allows users to **skip onboarding for future vault creation**

- A **Continue** button completes onboarding

  

---

  

## Advanced Onboarding

  

The **Advanced** path is designed for experienced users who want full control from the start.

  

### Step 1: Page Pack Selection

  

Users chooses how many  **Page Packs** to add to their workspace and when open a pack can choose to omit singleton, layout, or going deeper to omit certain things.

  

- Packs bundle pages, layouts, and configuration

- Essential packs (e.g. Settings) are auto-added

  

---

  

### Step 2: Page Pack Configuration

  

When opening a **Page Pack**, users see two sections:

  

#### Pages Section

  

Displays:

  

- **Shared Pages**

- **Singleton Pages**

  

Each page appears as a card, with an icon indicating its type.

  

#### Layouts Section

  

Displays all **Layouts (Page Groups)**.

  

Each layout card shows:

  

- Number of shared pages used

- Number of layout-specific singleton pages

  

---

  

### Page Type Rules

  

- **Layout Singleton Pages**  

    Belong exclusively to a single layout and cannot be reused.

- **Global Singleton Pages**  

    Standalone pages that are not part of any layout.

- **Shared Pages**  

    Pages that can be referenced by multiple layouts.

  

---

  

### Step 3: Navigation Setup

  

Just like the Standard path, users configure:

  

- Navigation structure

- Page placement

- Depth and pane behavior

  

---

  

### Step 4: App Overview

  

A concise walkthrough explaining:

  

- Core concepts

- How layouts, pages, and navigation interact

  

---

  

### Step 5: Onboarding Preferences

  

On the final screen:

  

- A checkbox to **skip onboarding for future vault creation**

- A **Continue** button to finish onboarding

  

---

  

## Key Differences Between Paths

  

|Standard|Advanced|

|---|---|

|Guided, opinionated|Modular and flexible|

|Intent-based setup|Pack-based setup|

|Defaults first|Full control upfront|

|Beginner-friendly|Power-user focused|