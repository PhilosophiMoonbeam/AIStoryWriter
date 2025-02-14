# AI Story Generator System Flow

```mermaid
graph TB
    %% Main Input
    Start([Start]) --> Config[Load Configuration]
    Config --> ModelSelect[Select LLM Provider]
    
    %% Model Provider Selection
    ModelSelect --> |Local| Ollama[Ollama Models]
    ModelSelect --> |Cloud| Cloud[Cloud Providers]
    Cloud --> Google[Google]
    Cloud --> OpenRouter[OpenRouter]
    
    %% Main Pipeline Start
    ModelSelect --> OutlineGen[Outline Generator]
    
    %% Outline Generation Process
    subgraph OutlineGeneration[Outline Generation Pipeline]
        OutlineGen --> BaseContext[Extract Base Context]
        BaseContext --> StoryElements[Generate Story Elements]
        StoryElements --> InitialOutline[Create Initial Outline]
        InitialOutline --> OutlineQC{Quality Check}
        OutlineQC --> |Pass| OutlineComplete[Outline Complete]
        OutlineQC --> |Fail| OutlineRevision[Revision Cycle]
        OutlineRevision --> OutlineQC
    end
    
    %% Chapter Generation Process
    OutlineComplete --> ChapterGen[Chapter Generator]
    
    subgraph ChapterGeneration[Chapter Generation Pipeline]
        ChapterGen --> Stage1[Stage 1: Initial Plot]
        Stage1 --> Stage2[Stage 2: Character Development]
        Stage2 --> Stage3[Stage 3: Dialogue Integration]
        Stage3 --> Stage4[Stage 4: Pre-revision Edit]
        Stage4 --> Stage5[Stage 5: Quality Revision]
        Stage5 --> ChapterQC{Quality Check}
        ChapterQC --> |Pass| ChapterComplete[Chapter Complete]
        ChapterQC --> |Fail| ChapterRevision[Revision Cycle]
        ChapterRevision --> ChapterQC
    end
    
    %% Scene Generation System
    ChapterComplete --> SceneGen[Scene Generator]
    
    subgraph SceneGeneration[Scene Generation Pipeline]
        SceneGen --> OutlineToScenes[Break Chapter into Scenes]
        OutlineToScenes --> SceneJSON[Convert to JSON Structure]
        SceneJSON --> GenerateScenes[Generate Individual Scenes]
        GenerateScenes --> IntegrateScenes[Integrate into Chapter]
    end
    
    %% Quality Control System
    IntegrateScenes --> QualityControl[Quality Control System]
    
    subgraph QualityControlSystem[Quality Control Pipeline]
        QualityControl --> ContentCheck[Content Verification]
        ContentCheck --> Scrubber[Content Refinement]
        Scrubber --> TranslationCheck[Translation Check]
        TranslationCheck --> |Needs Translation| Translator[Translation System]
        TranslationCheck --> |No Translation| FinalQC[Final Quality Check]
        Translator --> FinalQC
        FinalQC --> |Pass| FinalContent[Final Content]
        FinalQC --> |Fail| EditingSystem[Novel Editor System]
        EditingSystem --> FinalQC
    end
    
    %% Support Systems
    subgraph SupportSystems[Support Systems]
        Logger[Logging System]
        Stats[Statistics System]
        MetaData[Story Metadata System]
    end
    
    %% Connect Support Systems
    OutlineGeneration --> Logger
    ChapterGeneration --> Logger
    SceneGeneration --> Logger
    QualityControlSystem --> Logger
    FinalContent --> Stats
    FinalContent --> MetaData
    
    %% Final Output
    FinalContent --> Complete([Story Complete])
    
    %% Styling
    classDef process fill:#f9f,stroke:#333,stroke-width:2px
    classDef decision fill:#bbf,stroke:#333,stroke-width:2px
    classDef system fill:#bfb,stroke:#333,stroke-width:2px
    
    class OutlineGen,ChapterGen,SceneGen,QualityControl process
    class OutlineQC,ChapterQC,FinalQC decision
    class Logger,Stats,MetaData system
```

This flowchart represents the complete system architecture of the AI Story Generator, including:

1. Initial Configuration and Model Selection
2. Outline Generation Pipeline with quality control
3. Chapter Generation Pipeline with its 5-stage process
4. Scene Generation System
5. Quality Control System with content refinement and translation
6. Support Systems integration
7. Final output generation

The flowchart uses different styles to distinguish between:
- Process nodes (pink)
- Decision points (blue)
- Support systems (green)

Each major component is organized into subgraphs for clarity, showing the relationships and data flow between different parts of the system.
