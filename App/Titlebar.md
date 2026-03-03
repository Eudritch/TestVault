- I want title bar dragging to be smooth, so that when you drag over an element, half over any direction we set our dragging tab there. Needs fixing
- I also want us when dragging tab, if we set it to aside of a canvas, we create a canvas a there, and we must get visual representawtion that that's what will happen just like obsidian.
- When we drag tab to a different canvas, the tab moves there to the canvas tab, but if we drag to the active canvas, the tab continue behaving like it should normaly do, just dragging with other tabs.
- I also want +layout.svelte to better handle canvases, make sure there is always at least one canvas pane. 



- Ghost canvas area should only be displayed on the canvas if hovered, but if hovering the tab of the canvas, it should not be displayed.
- I need you to make +layout.svelte line 165 setting panes use MultiSplitView.svelte, and on top of that, I'd like the TitleBarArea.svelte to be still working normally with Tabs, as it already have, basically I don't want you to break TitleBarArea.svelte while making us use MultiSplitView.svelte.
-  And we are still having issues with this so I want the code to be fully looked at and get this done for me --> Here’s the “do it like Chrome” recipe, rewritten as an implementation guide you can follow.

## The Chrome rule in one sentence

**While dragging, keep a “gap” (placeholder) in the tab strip and move that gap to a new index only when the dragged tab crosses the midpoint between two neighboring tab slots (and don’t allow rapid back-and-forth moves).**

That “midpoint” is the _halfway-over_ behavior you’re describing.

---

## 1) Use the same mental model Chrome uses

When a drag starts:

- The dragged tab becomes a **floating tab** (drawn above others, following the pointer).
    
- The strip keeps a **placeholder gap** at the tab’s current index (same width as the dragged tab).
    
- Other tabs slide left/right to fill around that gap.
    

Reordering is just: **move the placeholder index**.

---

## 2) Track the dragged tab by its anchored X (not hover)

Chrome-style drag math is based on the dragged tab’s X in strip coordinates, anchored by where the user grabbed the tab.

On drag start:

- Save `grabOffsetX = pointerX - tabLeftX` (where inside the tab the user grabbed).  
    On drag move:
    
- `draggedLeftX = pointerX - grabOffsetX`
    

That’s the key value you use to decide when to reorder.

---

## 3) Compute “slot boundaries” and reorder at the halfway point

At any moment, the tab strip has **ideal slot positions** (where each tab _should_ be if you weren’t dragging). Think:

- `slotLeft[i]` = ideal left X position for a tab inserted at index `i`
    
- There are boundaries between slots:
    
    - `boundary[i] = (slotLeft[i] + slotLeft[i+1]) / 2`
        

**Chrome-like decision:**

- Find which slot the dragged tab belongs to by seeing which boundary it’s past.
    
- When `draggedLeftX` crosses `boundary[i]`, you move the placeholder index across that boundary.
    

This is the “halfway” behavior, but done in a way that stays correct even when tabs have different widths.

### Practical version (very close to Chrome)

Instead of manually walking boundaries, you can do what Chrome effectively does today:

> Pick the insertion index whose **ideal slotLeft** is _closest_ to the current `draggedLeftX`.

That produces the same “midpoint between slots” behavior automatically.

---

## 4) Add the anti-jitter rule (this is why Chrome feels stable)

If you only use midpoint crossing, tiny pointer noise can cause flip-flopping.

So: **don’t allow a reorder unless the user has moved enough since the last reorder.**

A good Chrome-like heuristic:

- require `abs(draggedLeftX - lastReorderX) >= threshold`
    
- threshold can be something like `max(16px, tabWidth * 0.1)` (tune it)
    

This hysteresis is a big part of the “Chrome feel.”

---

## 5) Full flow (copy/paste logic)

```js
// State set on drag start
let dragIndexOriginal;
let placeholderIndex;
let grabOffsetX;
let lastReorderX = null;

function onDragStart(pointerX, tabLeftX, tabIndex) {
  dragIndexOriginal = tabIndex;
  placeholderIndex = tabIndex;
  grabOffsetX = pointerX - tabLeftX;

  // visually: set tab to floating, leave a gap at placeholderIndex
}

function onDragMove(pointerX, getSlotLefts, tabWidth) {
  const draggedLeftX = pointerX - grabOffsetX;

  // 1) Compute current ideal slotLeft positions for each possible insertion index.
  //    (getSlotLefts should reflect the strip layout *with a gap* at placeholderIndex.)
  const slotLeft = getSlotLefts(); // array length n (possible insertion positions)

  // 2) Choose target insertion index = nearest ideal slotLeft to draggedLeftX.
  let target = 0;
  let bestDist = Infinity;
  for (let i = 0; i < slotLeft.length; i++) {
    const d = Math.abs(draggedLeftX - slotLeft[i]);
    if (d < bestDist) {
      bestDist = d;
      target = i;
    }
  }

  // 3) Hysteresis: block rapid flip-flops
  const threshold = Math.max(16, tabWidth * 0.1);
  if (target !== placeholderIndex) {
    if (lastReorderX === null || Math.abs(draggedLeftX - lastReorderX) >= threshold) {
      placeholderIndex = target;
      lastReorderX = draggedLeftX;

      // update model/layout: animate other tabs to new ideal positions
    }
  }

  // 4) Render floating tab at draggedLeftX
}
```

---

## 6) Mapping to your “halfway over active + hovered tab center” statement

If you want the simplest conceptual check:

- “Hovered tab center” is basically representing the **midpoint boundary** between positions.
    
- Chrome’s real behavior is: **you reorder when you cross the midpoint between the two neighboring slots**, not merely when you overlap a tab.
    


- 


- We should be able to drag tab from one canvas, to another canvas tab area.
-
- I don't want to have to create another store just for src/lib/stores/ui/tab-drag.svelte.ts, find out if you can integrate it into src/lib/stores/context/pane.svelte.ts, but make sure everything is neat, and we only have necessary things inside.
- Lastly I need you to fix TitlebarArea.svelte to work correctly with Canvas Tabs and look into Canvas.svelte line 56 - 57. And do test it out via mcp localhost:1620/?mcp



  
  
  
  
### Tab Dragging & Canvas Behavior Improvements

1. **Smooth Title Bar Dragging**
    
    - Dragging a tab from the title bar should feel smooth and responsive.
        
    - When the dragged tab is moved over another element, and crosses halfway (from any direction), it should snap/position itself into that target area.
        
    - The current snapping/placement behavior needs to be fixed.
        
2. **Creating a New Canvas by Dragging**
    
    - If a tab is dragged to the side of an existing canvas (e.g., left, right, top, or bottom), a new canvas pane should be created in that position.
        
    - There must be a clear visual indicator (similar to Obsidian) showing where the new canvas will be created before the tab is dropped.
        
3. **Dragging Between Canvases**
    
    - If a tab is dragged to a different (non-active) canvas, it should move into that canvas’s tab bar.
        
    - If a tab is dragged within the currently active canvas, it should behave normally — meaning it should simply reorder among the existing tabs without triggering canvas movement behavior.
        
4. **Canvas Management in `+layout.svelte`**
    
    - Improve canvas state handling in `+layout.svelte`.
        
    - Ensure that there is always at least one canvas pane present.
        
    - Prevent situations where all canvases are removed or the layout becomes invalid.
        