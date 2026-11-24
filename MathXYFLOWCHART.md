```mermaid
flowchart LR
    %% Unconnected Status Note (Outside Execution Flow)
    Note["Project: MathXY (Python CLI / Web) | Status: Design Phase"]
    style Note fill:#f9f9f9,stroke:#999,stroke-dasharray: 5 5

    %% Start & Topic Entry
    Start(["Student opens MathXY Dashboard"]) --> LoadCheck{"Load saved topic from file system?"}
    LoadCheck -->|Yes| LoadFile["Load saved topic & past questions"]
    LoadCheck -->|No| SearchInput[/"Enter topic or math problem in search bar"/]

    SearchInput --> Topic{"Topic area?"}
    Topic -->|Algebra| Alg["Load Algebra track<br/>(Equations, Factoring, Simplification)"]
    Topic -->|Geometry| Geo["Load Geometry track<br/>(Area, Distance, Angles, Intersections)"]

    Alg --> Mode
    Geo --> Mode
    LoadFile --> Mode

    %% Dashboard Navigation Choices
    Mode{"Select dashboard option"}
    Mode -->|Description & Equations| Overview["AI generates topic overview & custom shortcuts"]
    Overview --> DisplayNotes[/"Display descriptions & equations"/]
    DisplayNotes --> ChatCheck

    Mode -->|Solve Custom Problem| SolveAI["AI solves problem step-by-step"]
    SolveAI --> DisplaySteps[/"Display step-by-step solution & final answer"/]
    DisplaySteps --> GeoCheck{"Is it a Geometry problem?"}
    GeoCheck -->|Yes| PlotGraph[/"Render visual diagram via matplotlib<br/>(Shapes & intersection coordinates)"/]
    GeoCheck -->|No| ChatCheck
    PlotGraph --> ChatCheck

    %% Interactive Side AI Chat Log
    ChatCheck{"Confused? Use side AI chat log?"}
    ChatCheck -->|Yes| ChatInput[/"Input question in AI chat prompt"/]
    ChatInput --> ChatReply[/"Display AI chat explanation"/]
    ChatReply --> PracticePrompt
    ChatCheck -->|No| PracticePrompt

    %% Practice & Mastery Loop
    Mode -->|Practice Problems| GenPractice
    PracticePrompt{"Ready for practice problems?"}
    PracticePrompt -->|No| SaveCheck
    PracticePrompt -->|Yes| GenPractice["AI generates practice problems"]

    GenPractice --> StudentWork[/"Student works through items & submits answers"/]
    StudentWork --> AICheck["AI checks the answers"]
    AICheck --> IsCorrect{"Are the answers correct?"}

    IsCorrect -->|Incorrect| Hint["Flag error & give targeted hint"]
    Hint --> StudentWork

    IsCorrect -->|Correct| FullSol["Show full step-by-step solution"]
    FullSol --> Mastery{"Mastery threshold reached?"}
    Mastery -->|No| GenPractice
    Mastery -->|Yes| Report[/"Display topic summary & progress report"/]

    %% Built-in File System & Session End
    Report --> SaveCheck{"Save topic & progress to file system?"}
    SaveCheck -->|Yes| SaveFile["Save topic, notes & history to local file"]
    SaveCheck -->|No| Another
    SaveFile --> Another

    Another{"Another topic?"}
    Another -->|Yes| SearchInput
    Another -->|No| End(["Session complete"])
