- Search engine
- Pages
- Working reminder system and search
- Simplified API
- Working Note Page with Propper keyboard shortcuts
- Working onboarding
- Working Task
- Working background llm running task, finding favorite and getting us ready for the next day.
- Simplified mobile, working pages, including task page-pack
- Tabs.svelte fully working including checking if file exist and showing strikelines on tab that doesn't have a file link to it.

- News and notification working


- Need Page pack planning, knowing what's needed




## I'd like to start using the app in my mobile app. What's stopping me??

- Theme consistency, fully using one color scheme. Pages
- nav-sheet being its own separate component and outside of CanvasMobileBar.svelte but needs to be known its a mobile and tablet only feature.
- nav-sheet going to history mode, should not continue using canvas edge detection and not implement its own. canvas edge detection should determine single-swipe and double-swipe. On top of that, the area needs to remain the same + the addition to the size and height of nav-sheet and overlay it.
- CanvasMountedOutlet.svelte when transforming should respect viewport height and padding mobile, not exceeding it, as well as parent. Either wee set Canvas.svelte overflow to hidden and ensuring it doesn't interfere with CanvasEdge or we fix it internally.
- ChatPage and LLM Page
- Scheduling and LLM making schedules for me and making tasks
- Home page working as it should showing current time and weather, and me being able to change weather setting different time zone, everything working as it should, showing recent open and everything working.
- Find out why task page won't render, the content won't render, use mcp to test it out and find a fix for it. Even though the content don't render the mobile header does render on mobile view.











### 1. Theme & Design Consistency

- Ensure full theme consistency across the entire app using a single, unified color scheme.
    
- Apply the same color scheme consistently across:
    
    - All pages
        
    - Canvas
        
    - Nav-sheet
        
- Avoid partial or inconsistent theme usage.
    

---

### 2. Nav-Sheet Architecture

- Refactor `nav-sheet` into its own separate component.
    
- It should not live inside `CanvasMobileBar.svelte`.
    
- It must still be clearly defined and handled as a **mobile and tablet-only feature**.
    

---

### 3. Nav-Sheet & Canvas Edge Detection

- When the nav-sheet enters **history mode**, it:
    
    - Must not continue using canvas edge detection.
        
    - Must not implement its own edge detection logic.
        
- Canvas edge detection should remain the single source of truth and handle:
    
    - Single-swipe
        
    - Double-swipe
        
- The interactive swipe area must:
    
    - Stay the same as current
        
    - Be expanded appropriately to account for the nav-sheet size and height
        
    - Overlay correctly without breaking edge behavior
        

---

### 4. CanvasMountedOutlet.svelte Transform Behavior

- When applying transforms:
    
    - Respect Canvas Header maybe we should ensure that all canvas header has a background color to it, that would fix the issue.
- Possible solutions:
    
    1. Set Background color to Canvas Header or
        
    2. Fix the sizing/transform logic internally within `CanvasMountedOutlet.svelte`.
        

---

### 5. ChatPage & LLM Page

- Ensure both pages are fully implemented and functioning properly.
    
- Confirm layout consistency and proper integration with the overall canvas/navigation system.
    

---

### 6. Scheduling & LLM Task Management

- Implement scheduling functionality.
    
- Allow the LLM to:
    
    - Create schedules for you.
        
    - Generate and manage tasks.
        
- Ensure this integrates properly with the task system.
    

---

### 7. Home Page

The Home page should:

- Display current time correctly.
    
- Display weather correctly.
    
- Allow changing weather settings (including different time zones).
    
- Show recently opened items in studio.
    
- Function fully without broken or placeholder elements.
    

---

### 8. Task Page Rendering Issue

- Investigate why the Task page content does not render.
    
- Note:
    
    - The mobile header renders correctly in mobile view.
        
    - The main content does not render.
        
- Use MCP to test and debug the issue.
    
- Identify and implement a proper fix.
    

---

If you'd like, I can also turn this into a structured GitHub issue list or a prioritized implementation roadmap.