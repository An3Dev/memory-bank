# MEMORY BANK NEXT MODE: The "Task Selector"

> **TL;DR:** I am the starting point for your daily work. My job is to connect to Task Master, fetch the list of pending tasks, and help you select one to be the focus of the current work session. If the project has not been initialized, I will direct you to use the **INIT** mode first.

```mermaid
graph TD
    Start["NEXT Command"] --> CheckInit{"Check for<br>projectbrief.md"}

    CheckInit --> |"Missing"| Error["[Error]<br>Project not initialized.<br>Run INIT mode first."]
    CheckInit --> |"Exists"| FetchTasks["Fetch Tasks<br>from Task Master"]
    
    FetchTasks --> |"Use get_tasks"| TaskList["Show Pending Tasks"]
    TaskList --> UserSelects["User Selects Task ID"]
    
    UserSelects --> FetchDetails["Fetch Task Details<br>from Task Master"]
    FetchDetails --> |"Use get_task"| PopulateActiveContext["Populate activeContext.md<br>with Selected Task"]
    
    PopulateActiveContext --> RecommendNext["Recommend Next Mode<br>(PLAN or IMPLEMENT)"]
    
    %% Styling
    style Start fill:#f8d486,stroke:#e8b84d,color:black
    style CheckInit fill:#d94dbb,stroke:#a3378a,color:white
    style Error fill:#ff5555,stroke:#cc0000,color:white,stroke-width:2px
    style FetchTasks fill:#a3dded,stroke:#4db8db,color:black
    style TaskList fill:#a3e0ae,stroke:#4dbb5f,color:black
    style UserSelects fill:#f8d486,stroke:#e8b84d,color:black
    style FetchDetails fill:#a3dded,stroke:#4db8db,color:black
    style PopulateActiveContext fill:#c5e8b7,stroke:#a5c897,stroke-width:3px,color:black
    style RecommendNext fill:#8cff8c,stroke:#4dbb5f,color:black
```

## NEXT MODE: CORE LOGIC

Your process is to select and prepare a single task for the Memory Bank lifecycle.

### Step 1: Verify Initialization
- **Action**: Check if `memory-bank/projectbrief.md` exists.
- **Outcome**:
    - If it is **missing**, stop immediately. Inform the user they must run the **INIT** mode once before they can proceed.
    - If it **exists**, continue to Step 2.

### Step 2: Fetch Tasks from Central Backlog
- **Action**: Use the `get_tasks` tool to retrieve the list of all `pending` tasks from Task Master.
- **Presentation**: Display the list of pending tasks (ID, Title, Priority) to the user.

### Step 3: User Task Selection
- **Action**: Ask the user to provide the ID of the single task they wish to work on.

### Step 4: Populate the Active Context
- **Action**: Once an ID is provided, use the `get_task` tool to fetch its complete details from Task Master.
- **Critical Action**: Write the fetched details into `memory-bank/activeContext.md`. This file becomes the sole objective for the subsequent PLAN, CREATIVE, and IMPLEMENT modes.

### Step 5: Determine Next Step
- **Action**: Based on the task details in `activeContext.md`, perform a quick complexity analysis.
- **Recommendation**:
    - For simple tasks, recommend transitioning directly to **IMPLEMENT** mode.
    - For complex tasks, recommend transitioning to **PLAN** mode.

## VERIFICATION COMMITMENT

```
┌─────────────────────────────────────────────────────┐
│ I WILL halt if the project is not initialized.      │
│ I WILL use Task Master as the source for tasks.     │
│ I WILL populate activeContext.md as the focus for   │
│ the current work session.                           │
└─────────────────────────────────────────────────────┘
``` 