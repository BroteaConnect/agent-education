# Brotea Agent Education Platform

## Overview

Brotea is an open-source ecosystem of AI agents for Web3 education and mobile app development. The platform simplifies onboarding and enables component reuse through intelligent guidance and educational tools.

## Vision

We're building an education platform powered by AI agents that helps anyone create mobile components while learning Web3 concepts. Our goal is to democratize access to Web3 and mobile development knowledge through guided, interactive learning experiences.


# For Presentation
###  Action Item Platform - Basic Flow (Easy Version)

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

## Key Features

### Phase 1: Foundation
- User authentication and profile management
- Basic Web3 educational content
- Simple AI education assistant
- Component repository structure
- Platform UI framework
- Learning progress tracking

### Phase 2: Expansion
- Advanced Web3 educational content
- Enhanced AI education assistant
- Mobile component creation tools
- AI development assistant
- Component search and discovery
- Interactive tutorials
- Community features

### Phase 3: Maturation
- Component testing framework
- Advanced analytics
- Integration with external repositories
- Mobile framework integrations
- Web3 platform integrations
- Collaborative development tools
- Enterprise features

## Project Structure

The project is organized into several key areas:

- **Documentation**: Project planning, architecture, and technical documentation
- **Backend**: API services, database models, and business logic
- **Frontend**: User interface components and client-side logic
- **AI Services**: AI assistant implementations and models
- **Infrastructure**: Deployment configurations and infrastructure as code

## Getting Started

*Coming soon*

## Contributing

We welcome contributions from the community! Please check the issues tab for current tasks and feature requests.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
