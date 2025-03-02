# Brotea Platform - Technical Flow (Advanced Version)

```mermaid
sequenceDiagram
    participant Person as Person #LightBlue
    participant Omi as Omi #LightGreen
    participant Agent as Agent #Gold
    participant Eliza as Eliza #Pink
    participant Twitter as Twitter #LightBlue
    participant Farcaster as Farcaster #Purple
    participant Telegram as Telegram #LightBlue
    participant Database as Database #LightGray
    participant Community as Community #Orange
    
    %% Phase 1: Project Detection & Analysis
    rect rgb(230, 240, 255)
    Note over Person,Eliza: Project Detection & Analysis Phase
    Person->>Omi: Initiates conversation (natural language)
    activate Omi
    Omi->>Agent: Forwards conversation data (JSON)
    activate Agent
    Agent->>Eliza: Sends conversation for NLP analysis
    activate Eliza
    Eliza->>Eliza: Performs intent recognition
    Eliza->>Eliza: Extracts project parameters
    Eliza->>Eliza: Classifies project type
    Eliza->>Agent: Returns project detection results (structured data)
    deactivate Eliza
    Agent->>Agent: Validates project viability
    Agent->>Agent: Generates project metadata
    end
    
    %% Phase 2: Builder Contact & Network Integration
    rect rgb(255, 240, 230)
    Note over Agent,Telegram: Builder Contact & Network Integration Phase
    Agent->>Twitter: API call to identify relevant builders
    activate Twitter
    Twitter-->>Agent: Returns builder profiles and contact info
    deactivate Twitter
    Agent->>Farcaster: API call to identify relevant builders
    activate Farcaster
    Farcaster-->>Agent: Returns builder profiles and contact info
    deactivate Farcaster
    Agent->>Telegram: API call to identify relevant builders
    activate Telegram
    Telegram-->>Agent: Returns builder profiles and contact info
    deactivate Telegram
    Agent->>Agent: Filters and ranks builders by relevance
    Agent->>Twitter: Sends project proposal to selected builders
    Agent->>Farcaster: Sends project proposal to selected builders
    Agent->>Telegram: Sends project proposal to selected builders
    end
    
    %% Phase 3: Data Persistence & Synchronization
    rect rgb(230, 255, 230)
    Note over Agent,Database: Data Persistence & Synchronization Phase
    Agent->>Database: Stores project metadata (JSON)
    activate Database
    Agent->>Database: Stores builder contact records
    Agent->>Database: Stores conversation history
    Agent->>Database: Creates project timeline
    Database-->>Agent: Confirms successful transaction
    deactivate Database
    Agent-->>Omi: Sends project status update (structured data)
    deactivate Agent
    Omi-->>Person: Renders project information (natural language)
    deactivate Omi
    end
    
    %% Phase 4: Community Engagement & Tokenized Funding
    rect rgb(255, 230, 255)
    Note over Community,Person: Community Engagement & Tokenized Funding Phase
    Community->>Database: Submits project likes (engagement metrics)
    activate Database
    Database->>Database: Updates project popularity score
    Community->>Database: Initiates token-based funding transaction
    Database->>Database: Validates token transfer
    Database->>Database: Updates project funding status
    Database-->>Agent: Triggers funding status webhook
    deactivate Database
    activate Agent
    Agent->>Agent: Calculates funding milestones
    Agent->>Agent: Updates project roadmap
    Agent-->>Omi: Sends funding notification (structured data)
    deactivate Agent
    activate Omi
    Omi-->>Person: Renders funding confirmation (natural language)
    deactivate Omi
    end
```

# Builder Process - Technical Flow (Advanced Version)

```mermaid
sequenceDiagram
    participant Builder as Builder #LightBlue
    participant Agent as Agent #Gold
    participant ExecutionPlan as Execution Plan #LightGreen
    participant GrantApplications as Grant Applications #Pink
    participant MarketingCampaigns as Marketing Campaigns #Purple
    participant GrantFounder as Grant Founder #Orange
    participant Database as Database #LightGray
    participant Analytics as Analytics Engine #Red
    
    %% Phase 1: Plan Initiation & Project Architecture
    rect rgb(230, 240, 255)
    Note over Builder,Builder: Plan Initiation & Project Architecture Phase
    Builder->>Agent: Submits execution request (authenticated API call)
    activate Agent
    Agent->>Agent: Validates builder credentials
    Agent->>Agent: Retrieves project metadata
    Agent->>ExecutionPlan: Initiates plan generation algorithm
    activate ExecutionPlan
    ExecutionPlan->>ExecutionPlan: Analyzes project requirements
    ExecutionPlan->>ExecutionPlan: Generates task dependency graph
    ExecutionPlan->>ExecutionPlan: Calculates resource requirements
    ExecutionPlan->>ExecutionPlan: Optimizes execution timeline
    ExecutionPlan-->>Agent: Returns comprehensive execution plan (JSON)
    deactivate ExecutionPlan
    Agent->>Analytics: Logs plan generation event
    Agent-->>Builder: Renders interactive execution plan
    Builder->>Agent: Submits plan approval (digital signature)
    end
    
    %% Phase 2: Grant Applications & Funding Strategy
    rect rgb(255, 230, 240)
    Note over Agent,Agent: Grant Applications & Funding Strategy Phase
    Agent->>GrantApplications: Initiates application workflow
    activate GrantApplications
    GrantApplications->>GrantApplications: Matches project to grant opportunities
    GrantApplications->>GrantApplications: Generates application documents
    GrantApplications->>GrantApplications: Submits applications to grant platforms
    GrantApplications->>GrantApplications: Tracks application status
    GrantApplications-->>Agent: Returns application status matrix
    deactivate GrantApplications
    Agent->>Analytics: Logs application metrics
    end
    
    %% Phase 3: Marketing Campaign Orchestration
    rect rgb(240, 230, 255)
    Note over Agent,Agent: Marketing Campaign Orchestration Phase
    Agent->>MarketingCampaigns: Initiates marketing strategy generation
    activate MarketingCampaigns
    MarketingCampaigns->>MarketingCampaigns: Analyzes target audience
    MarketingCampaigns->>MarketingCampaigns: Creates content calendar
    MarketingCampaigns->>MarketingCampaigns: Deploys cross-platform campaigns
    MarketingCampaigns->>MarketingCampaigns: Monitors engagement metrics
    MarketingCampaigns->>MarketingCampaigns: Optimizes campaign parameters
    MarketingCampaigns-->>Agent: Returns performance analytics dashboard
    deactivate MarketingCampaigns
    Agent->>Analytics: Logs marketing performance data
    end
    
    %% Phase 4: Grant Founder Matching & Negotiation
    rect rgb(255, 240, 230)
    Note over Agent,Builder: Grant Founder Matching & Negotiation Phase
    Agent->>GrantFounder: Initiates founder matching algorithm
    activate GrantFounder
    GrantFounder->>GrantFounder: Analyzes project-founder compatibility
    GrantFounder->>GrantFounder: Ranks potential founders
    GrantFounder->>GrantFounder: Generates introduction materials
    GrantFounder-->>Agent: Returns founder recommendations with match scores
    deactivate GrantFounder
    Agent-->>Builder: Presents founder matches with compatibility metrics
    Builder->>GrantFounder: Initiates secure communication channel
    activate GrantFounder
    Builder->>GrantFounder: Conducts negotiation process
    deactivate GrantFounder
    end
    
    %% Phase 5: Project Status Synchronization & Reporting
    rect rgb(230, 255, 230)
    Note over Builder,Builder: Project Status Synchronization & Reporting Phase
    Builder->>Database: Submits encrypted status update
    activate Database
    Database->>Database: Validates update signature
    Database->>Database: Applies transactional updates
    Database->>Database: Triggers status webhooks
    Database-->>Agent: Notifies of status change
    deactivate Database
    Agent->>Analytics: Requests comprehensive analytics
    activate Analytics
    Analytics->>Analytics: Aggregates project metrics
    Analytics->>Analytics: Generates performance visualizations
    Analytics->>Analytics: Compiles recommendation engine output
    Analytics-->>Agent: Returns analytical report package
    deactivate Analytics
    Agent-->>Builder: Delivers interactive project dashboard
    deactivate Agent
    end
