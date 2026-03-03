src/routes/(app)/meeting

## Modified prompt (with new features + UI beauty guidance)

**Redesign the Meeting Page into a 3-column layout: Left (Context), Center (Call), Right (Collaboration). The UI should feel modern, calm, and premium—like a blend of Zoom familiarity + Discord community clarity + Notion-level polish. Keep visual noise low, emphasize hierarchy, and make the interface feel “guided” by smart defaults.**

### Global layout + visual language

* Use a **three-column grid** with clear visual hierarchy:

  * **Left column:** narrow (context feed)
  * **Center column:** dominant (live meeting)
  * **Right column:** medium (collaboration rail)
* Style: **soft surfaces, subtle shadows, generous padding, rounded corners**, minimal borders. Use restrained accent color only for “live” states (recording/streaming, active speaker, pinned items).
* Typography: **strong hierarchy** (section titles, timestamps, microlabels). UI should read like a story: *what happened → what’s happening → what we’re doing about it*.
* Motion: short, smooth transitions (200–300ms) for panel switching, expanding cards, and pinning.

---

## 1) Left column — “Meeting Highlights” (high-signal timeline)

**Purpose:** Show only the *important* things that happened since the meeting started. This is not a raw log; it’s a curated narrative.

### Content types (cards in a timeline)

* **Files shared** (icon + filename + sharer + open button)
* **Links opened** (title + domain + preview + open button)
* **Decisions captured** (“Decided: …”)
* **Action items created** (“Owner: … Due: …”)
* **AI summary checkpoints** (every ~10–15 minutes, 2–3 bullets)
* **Pinned items** (anything pinned from chat or meeting)

### “Importance” rules (must be visible and explainable)

An event is “highlight-worthy” if:

* It was referenced by multiple people, or
* It was opened/shared during the meeting, or
* It became an output (decision/task), or
* Someone pinned it, or
* It relates to the current shared content/topic.

Each highlight card includes a subtle **reason label** (tiny text like: “Pinned”, “Referenced 3×”, “Opened by host”, “Decision”).

### Controls at top

* Filter chips: **All / Files / Decisions / Actions / Summaries**
* Toggle: **Highlights (default) / Full activity**
* Optional: search field
* Include a **“Catch me up”** button: generates an AI summary since the user joined or last 10 minutes.

### Noise control

* Automatically group repeated actions:
  “BC opened 5 files” → collapsible group card with expand.

---

## 2) Center column — “Call” (Zoom-like, familiar)

**Purpose:** The live meeting experience should be instantly recognizable.

### Layout

* **Top strip:** participant tiles (square or rounded-square). Auto-collapse with “+12” when crowded.
* **Main stage:** shared screen/video/content is the primary focus.
* **Bottom control bar:** circular interactive buttons:

  * Mute, Camera, Share, React, Raise Hand, Record/Stream, More
* Live indicators:

  * “Now sharing: [name]” label on stage
  * Recording/streaming badge (subtle but visible)
  * Active speaker outline (thin accent ring)

### Behavior

* When screen sharing: stage prioritizes shared content.
* When not sharing: stage prioritizes active speaker or grid view.

---

## 3) Right column — “Collaboration Rail” (mode-based tabs)

**Purpose:** This area is for coordination—chat, queue, notes, tasks—without cluttering the call.

### Tabs (single rail, not stacked panels)

Default tabs:

1. **Chat** (Discord-like)
2. **Queue** (appears/promotes when streaming/video is relevant)
3. **Notes / Decisions**
4. **Tasks**

#### Chat tab

* Chat supports **inline event cards** (file shared, decision, task), but these are *references* that link back to the canonical highlights on the left.
* Support threads for side discussions.
* Pin any message → becomes a pinned highlight in Left.

#### Queue tab (for streaming / video sharing)

* A collapsible playlist with:

  * Add video file or paste link
  * Drag to reorder
  * “Play next”, “Remove”, “Clear”
* Permissions:

  * Presenter controls (host/co-host)
  * View-only for others
* If no streaming/video: Queue tab hides or stays dormant.

#### Notes / Decisions tab

* Lightweight collaborative doc.
* “Decisions” section supports one-click conversion into tasks.

#### Tasks tab

* Action items list: Owner, due date, status.
* Tasks can be created from chat, highlights, or notes.

---

## Adaptive layout modes (automatic, calm)

The page can shift emphasis based on what’s happening:

### Mode A: Standard Meeting (default)

* Right defaults to **Chat**
* Left shows curated highlights

### Mode B: Working Session (screen share heavy)

* Left becomes more valuable; highlights bias toward links/files opened and tasks created.

### Mode C: Streaming / Watch Party

* Right defaults to **Queue**
* Queue becomes first-class; chat remains accessible.

Switching modes should feel seamless and not disruptive.

---

## End-of-meeting outputs (automatic)

When the meeting ends, generate a clean summary panel:

* Top 5 highlights
* Decisions made
* Action items (with owners and due dates)
* Files and links shared

