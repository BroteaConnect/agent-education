# Action Item Platform - Basic Flow (Easy Version)

```mermaid
sequenceDiagram
    participant Person as Person #LightBlue
    participant Omi as Omi #LightGreen
    participant Agent as Agent #Gold
    participant SocialMedia as Social Media #Purple
    participant Database as Database #LightGray
    participant Community as Community #Orange
    
    %% Phase 1: Project Discovery
    rect rgb(230, 240, 255)
    Note over Person,Agent: Project Discovery
    Person->>Omi: Starts conversation
    Omi->>Agent: Shares conversation
    Agent->>Agent: Identifies project
    end
    
    %% Phase 2: Builder Contact
    rect rgb(255, 240, 230)
    Note over Agent,SocialMedia: Finding Builders
    Agent->>SocialMedia: Contacts builders
    end
    
    %% Phase 3: Storage
    rect rgb(230, 255, 230)
    Note over Agent,Person: Saving Project
    Agent->>Database: Saves project
    Agent-->>Person: Confirms project
    end
    
    %% Phase 4: Community Support
    rect rgb(255, 230, 255)
    Note over Community,Person: Community Support
    Community->>Database: Likes & funds project
    Agent-->>Person: Updates on funding
    end
```

# Builder Process - Basic Flow (Easy Version)

```mermaid
sequenceDiagram
    participant Builder as Builder #LightBlue
    participant Agent as Agent #Gold
    participant Plan as Project Plan #LightGreen
    participant Funding as Funding #Pink
    participant Marketing as Marketing #Purple
    
    %% Phase 1: Planning
    rect rgb(230, 240, 255)
    Note over Builder,Plan: Planning Phase
    Builder->>Agent: Starts project
    Agent->>Plan: Creates plan
    Agent-->>Builder: Shows plan
    end
    
    %% Phase 2: Execution
    rect rgb(255, 230, 240)
    Note over Agent,Marketing: Execution Phase
    Agent->>Funding: Applies for funding
    Agent->>Marketing: Starts marketing
    end
    
    %% Phase 3: Completion
    rect rgb(230, 255, 230)
    Note over Builder,Agent: Completion Phase
    Agent-->>Builder: Connects with funders
    Agent-->>Builder: Provides final report
    end
