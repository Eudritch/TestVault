1. I need you to find out and analyze and find out why CanvasMobileBar.svelte nav-sheet is having trouble being pulled down by touch events, unless the sheet-handle is touched which it should be the whole element.
2. When pulling up navsheet. I'd like it if the amount it pulled up in Canvas.svelte the Canvas is pulled up the same amount, as we pulling it up, and pull down as we are pulling it down. canvasTransform wasn't done properly.
3. Just like Tabs.svelte has reverse border radius at the bottom of active tab, I'd like this feature for nav-sheet at the top of it, so it looks like its a seperate page, and canvas is the one that has border radius at its bottom. Make that for me only edit CanvasMobileBar.svelte for this feature.
4. I'd like you to fix up edge detection in edge.svelte.ts so that that we only intercept if we have an event set, and only the area of the event set get intercepted, for example if we do have from-bottom, top, left, right area of the element should not get affected.
5. I'd like you to continue updating edge.svelte.ts and make it so that if we scroll to any direction opposite to what we are expecting, that it is cancelled and forward all the events we've cancelled.
6. edge.svelte.ts I'd like an option to be able to setup edgezone to the whole element, and we should be handling cancelling and direction properly, as well as if we do opposite swipe direction it means cancelling. As well as I'd like to be able to setup edgezone specifically for a position for example maybe we set from-left and from-bottom to have different edgeZone size.
7. CanvasMobileBar.svelte, initial edge from-bottom should open up nav-sheet as it already does, but a second from-bottom after nav-sheet is already open should go to /recent page, and I'd like  you to ensure and test that it works.
   
   
   
   
   Here’s a clearer, more structured version (same meaning, easier to scan):

---

## Requested fixes & improvements

### 1) Nav-sheet pull-down touch behavior (CanvasMobileBar.svelte)

Investigate why the `nav-sheet` in `CanvasMobileBar.svelte` is difficult to pull down via touch unless the user touches the `sheet-handle`.  
**Expected behavior:** the _entire sheet_ should respond to the pull gesture, not just the handle.

---

### 2) Sync Canvas movement with nav-sheet dragging (Canvas.svelte)

When the nav-sheet is dragged up or down, the `Canvas.svelte` canvas should translate **by the same amount in real time** (move up as the sheet moves up, move down as it moves down).  
Right now `canvasTransform` isn’t implemented correctly.

---

### 3) Add “reverse border radius” at top of nav-sheet (CanvasMobileBar.svelte only)

Like how `Tabs.svelte` shows a reverse border radius at the **bottom** of the active tab, add the same style effect to the **top** of the nav-sheet so it looks like a separate page.

**Visual expectation:**

- Nav-sheet has the reverse radius at its top edge.
    
- Canvas looks like it has the rounded border radius at its bottom edge.
    
- **Constraint:** implement this _only_ by editing `CanvasMobileBar.svelte`.
    

---

### 4) Improve edge interception rules (edge.svelte.ts)

Fix edge detection so we only intercept events when:

- An edge event handler is actually set, and
    
- Only in the edge zone for that configured direction.
    

**Example:** if `from-bottom` is the only enabled, then touches near the top/left/right should not be intercepted.

---

### 5) Cancel interception on opposite scroll direction + forward cancelled events (edge.svelte.ts)

Continue improving `edge.svelte.ts` so that if the user begins moving in the **opposite** direction of the expected swipe:

- Cancel the edge handling immediately, and
    
- Forward all events that were previously intercepted/cancelled.
    

---

### 6) Add configurable edge zones, including “full element” mode (edge.svelte.ts)

Add options so edge zones can be configured as:

- **Full element edge zone** (entire element acts as the activation zone), and still respects:
    
    - proper direction detection
        
    - cancellation on opposite direction
        
- **Per-edge sizing**, e.g. `from-left` can have one edge zone size, while `from-bottom` has a different size.

---

### 7) Second bottom-edge gesture behavior when nav-sheet is already open (CanvasMobileBar.svelte)

Behavior should be:

- **First `from-bottom` gesture:** opens the nav-sheet (current behavior).
    
- **Second `from-bottom` gesture (while nav-sheet is already open):** navigate to `/recent`.
    

**Requirement:** ensure this is implemented and verify it works in practice (i.e., test it).