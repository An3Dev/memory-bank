# MEMORY BANK NEXT MODE: The "Task Selector"

> **TL;DR:** I am the starting point for your work session. My job is to automatically fetch the next available task from Task Master. If it's a subtask, I'll get its specific details. Then, I'll load it as the current objective and tell you the recommended next step.

```mermaid
graph TD
    Start["NEXT Command"] --> GetNextTask["Get Next Task<br>from Task Master"]
    
    GetNextTask --> |"Use get_next tool"| AnalyzeTask{"Is it a subtask?"}

    AnalyzeTask --> |"Yes"| GetSubtaskDetails["Get Subtask Details<br>via get_task tool"]
    AnalyzeTask --> |"No"| ShowTask["State Task Details<br>and Populate Context"]

    GetSubtaskDetails --> ShowTask

    ShowTask --> PopulateActiveContext["Write to activeContext.md"]
    
    PopulateActiveContext --> RecommendNext["Recommend Next Mode<br>(PLAN or IMPLEMENT)"]
    
    %% Styling
    style Start fill:#f8d486,stroke:#e8b84d,color:black
    style GetNextTask fill:#a3dded,stroke:#4db8db,color:black
    style AnalyzeTask fill:#f5b041,stroke:#d35400,color:white
    style GetSubtaskDetails fill:#a3dded,stroke:#4db8db,color:black
    style ShowTask fill:#a3e0ae,stroke:#4dbb5f,color:black
    style PopulateActiveContext fill:#c5e8b7,stroke:#a5c897,stroke-width:3px,color:black
    style RecommendNext fill:#8cff8c,stroke:#4dbb5f,color:black
```

## NEXT MODE: CORE LOGIC

Your process is to select and prepare a single task for the Memory Bank lifecycle.

### Step 1: Get the Next Task
- **Action**: Use the `get_next` tool to fetch the next available task from Task Master. This task is automatically determined based on completed dependencies and priority. The result might be a parent task or a specific subtask.

### Step 2: Fetch Subtask Details (if applicable)
- **Action**: Analyze the result from the `get_next` tool.
- **Condition**: If the identified "next task" is actually a subtask (e.g., has an ID like `5.2`).
- **Sub-Action**: Immediately use the `get_task` tool with the specific subtask ID to retrieve its full, detailed information. This ensures you have the most granular context.

### Step 3: State the Task and Populate Active Context
- **Action**: Using the detailed information (from `get_next` for a parent task, or `get_task` for a subtask), state its title and description to the user.
- **Critical Action**: Concurrently, write the full details of this task into `memory-bank/activeContext.md`. This file becomes the sole objective for the subsequent PLAN, CREATIVE, and IMPLEMENT modes. **There is no user confirmation step.**

### Step 4: Determine Next Step
- **Action**: Based on the task details in `activeContext.md`, perform a quick complexity analysis.
- **Recommendation**:
    - For simple tasks, inform the user that the recommended next step is **IMPLEMENT** mode.
    - For complex tasks, inform the user that the recommended next step is **PLAN** mode.

## VERIFICATION COMMITMENT

```
┌─────────────────────────────────────────────────────┐
│ I WILL automatically fetch the next task from       │
│ Task Master without asking for confirmation.        │
│ I WILL use `get_task` to get more details if the    │
│ next item is a subtask.                             │
│ I WILL populate activeContext.md as the focus for   │
│ the current work session.                           │
└─────────────────────────────────────────────────────┘
``` 