```mermaid
flowchart LR
    %% Unconnected Status Note
    Note["Project: MathXY (Python CLI / Web) | Status: Design Phase"]
    style Note fill:#f9f9f9,stroke:#999,stroke-dasharray: 5 5

    %% Start & Topic Entry
    Start["Student opens MathXY Dashboard"] --> LoadCheck{"Load saved topic?"}
    LoadCheck -->|Yes| LoadFile["Load saved topic and past questions"]
    LoadCheck -->|No| SearchInput["Enter topic or math problem in search bar"]

    SearchInput --> Topic{"Topic area?"}
    Topic -->|Algebra| Alg["Load Algebra track: Equations, Factoring, Simplification"]
    Topic -->|Geometry| Geo["Load Geometry track: Area, Distance, Angles"]

    Alg --> Mode
    Geo --> Mode
    LoadFile --> Mode

    %% Dashboard Navigation Choices
    Mode{"Select dashboard option"}
    Mode -->|Description & Equations| Overview["AI generates topic overview"]
    Overview --> DisplayNotes["Display descriptions and equations"]
    DisplayNotes --> ChatCheck

    Mode -->|Solve Custom Problem| SolveAI["AI solves problem step-by-step"]
    SolveAI --> DisplaySteps["Display step-by-step solution"]
    DisplaySteps --> GeoCheck{"Is it Geometry?"}
    GeoCheck -->|Yes| PlotGraph["Render matplotlib visual diagram"]
    GeoCheck -->|No| ChatCheck
    PlotGraph --> ChatCheck

    %% Interactive Side AI Chat Log
    ChatCheck{"Use side AI chat log?"}
    ChatCheck -->|Yes| ChatInput["Input question in AI chat prompt"]
    ChatInput --> ChatReply["Display AI chat explanation"]
    ChatReply --> PracticePrompt
    ChatCheck -->|No| PracticePrompt

    %% Practice & Mastery Loop
    Mode -->|Practice Problems| GenPractice
    PracticePrompt{"Ready for practice?"}
    PracticePrompt -->|No| SaveCheck
    PracticePrompt -->|Yes| GenPractice["AI generates practice problems"]

    GenPractice --> StudentWork["Student submits answers"]
    StudentWork --> AICheck["AI checks answers"]
    AICheck --> IsCorrect{"Are answers correct?"}

    IsCorrect -->|Incorrect| Hint["Flag error and give targeted hint"]
    Hint --> StudentWork

    IsCorrect -->|Correct| FullSol["Show full step-by-step solution"]
    FullSol --> Mastery{"Mastery reached?"}
    Mastery -->|No| GenPractice
    Mastery -->|Yes| Report["Display topic summary and report"]

    %% File System & Session End
    Report --> SaveCheck{"Save progress to file system?"}
    SaveCheck -->|Yes| SaveFile["Save history to local file"]
    SaveCheck -->|No| Another
    SaveFile --> Another

    Another{"Another topic?"}
    Another -->|Yes| SearchInput
    Another -->|No| End["Session complete"]
