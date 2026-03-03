






- **Goal**
    
    - Build a **SvelteKit AI assistant/coach app** with “AI everywhere” (core UX, automation, suggestions, proactive help).
        
- **Core principles**
    
    - **Privacy-first**
        
        - Users should be able to **self-host**.
            
        - Support **local/owned storage** and clear data boundaries.
            
    - **Resilience**
        
        - Provide **backup LLMs** if the primary model is offline.
            
        - Ensure **redundant storage + backups**.
            
    - **Mobile constraints**
        
        - Mobile clients should **not do heavy/offline processing**.
            
        - Push compute to server / background services.
            
- **Key product features**
    
    - **News ready for you**
        
        - Personalized feed surfaced proactively.
            
    - **To-do ready for you**
        
        - Tasks suggested/organized automatically.
            
    - **Event-based notifications**
        
        - Notify users based on certain triggers/events.
            
    - **Weather awareness**
        
        - Use weather data to infer context, recommend actions, or learn patterns.
            
- **Automation priority**
    
    - Focus automation on discovering:
        
        - **User needs**
            
        - **Habits**
            
        - **Preferences**
            
    - Use **NLP** when necessary to extract intent, entities, routines, and patterns.
        
    - Automation should feel like a **background coach** (predictive + proactive).
        
- **AI architecture requirements**
    
    - A **tooling / tool-calling system** so the assistant can:
        
        - Determine from the request **what is needed**
            
        - Decide **which tool** to use (news, todo, calendar, weather, notifications, etc.)
            
        - Decide **which model** to use (fast vs deep, local vs hosted, etc.)
            
        - Choose the **best route** to produce results
            
    - Support **LLM memory**
        
        - Use `packages/llm` with a memory layer involved.
            
    - Track model availability
        
        - Determine whether **chat models are active** inside the app.
            
- **Interaction intelligence**
    
    - Decide when to:
        
        - **Infer and proceed** (no extra questions)
            
        - **Ask clarifying questions** (when needed)
            
    - Decide response strategy:
        
        - Provide **quick answers immediately**
            
        - Continue processing for **better results in the background**
            
    - Adapt to user preferences:
        
        - Learn **how the user wants responses**
            
            - tone, format, brevity, coaching style, etc.
                
- **App structure thoughts**
    
    - Create an **automation folder inside `lib`**
        
        - e.g. `src/lib/automation/*`
            
    - Hook it into the app shell:
        
        - `+layout.svelte` should import:
            
            - automation orchestration, or
                
            - a store that wires automation into the UI lifecycle.
                
- **Backend / integration requirements**
    
    - App should be able to **link with another app/service** for background work
        
        - Example: separate worker service for automation jobs, ingestion, long-running tasks.
            
    - Support **background processing**
        
        - Especially for long or expensive tasks and proactive automation.
            
- **Storage requirements**
    
    - Define how storage is handled with:
        
        - **privacy**
            
        - **redundancy**
            
        - **backups**
            
    - Ensure the system can survive:
        
        - model downtime
            
        - service downtime
            
        - partial data loss scenarios
            
- **Open decisions to cover in the structure**
    
    - How to determine:
        
        - **when to learn from a user request** vs ignore it
            
        - **when to run background tasks**
            
        - **when to ask follow-up questions**
            
        - **how to choose response tone/style per user**
            
    - How to implement:
        
        - tool registry + router
            
        - model registry + fallback
            
        - memory store
            
        - automation scheduler + triggers
            
        - notifications pipeline
            
        - news/todo/weather ingestion pipelines
            
- **Overall goal for the codebase**
    
    - A structure that is:
        
        - **easy to navigate**
            
        - **easy to extend**
            
        - supports adding new tools/features without rewiring everything