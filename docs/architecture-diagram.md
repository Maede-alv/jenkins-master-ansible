# Jenkins Architecture Diagram

## System Overview

This diagram illustrates the complete Jenkins automation architecture including the master controller, agents, and Job DSL repository integration.

```mermaid
graph TB
    subgraph "Jenkins Master (Controller)"
        A[Jenkins Master<br/>Port 8080] --> B[JCasC Configuration<br/>jenkins.yaml]
        A --> C[Plugin Manager<br/>plugins.txt]
        A --> D[Admin User Setup<br/>init.groovy.d]
        A --> E[GitHub Integration<br/>Token-based]
        A --> Z[Seed Job<br/>Job DSL Loader]
    end
    
    subgraph "External Repositories"
        F[Job DSL Repository<br/>https://github.com/Maede-alv/jenkins-jobdsl] --> G[Pipeline Jobs<br/>*.groovy]
        F --> H[Freestyle Jobs<br/>*.groovy]
        F --> I[Multi-branch Jobs<br/>*.groovy]
        F --> Y[Job Views<br/>*.groovy]
    end
    
    subgraph "Jenkins Agents (Slaves)"
        J[Agent 1<br/>Port 50000] --> K[Build Executor 1]
        L[Agent 2<br/>Port 50000] --> M[Build Executor 2]
        N[Agent N<br/>Port 50000] --> O[Build Executor N]
    end
    
    subgraph "Ansible Automation"
        P[Master Ansible<br/>jenkins-master-ansible] --> A
        Q[Agent Ansible<br/>jenkins-agent-ansible] --> J
        Q --> L
        Q --> N
    end
    
    subgraph "Version Control"
        R[GitHub] --> F
        R --> P
        R --> Q
    end
    
    A --> F
    A --> J
    A --> L
    A --> N
    Z --> F
    
    style A fill:#4CAF50,stroke:#333,stroke-width:2px,color:#fff
    style F fill:#2196F3,stroke:#333,stroke-width:2px,color:#fff
    style J fill:#FF9800,stroke:#333,stroke-width:2px,color:#fff
    style L fill:#FF9800,stroke:#333,stroke-width:2px,color:#fff
    style N fill:#FF9800,stroke:#333,stroke-width:2px,color:#fff
    style P fill:#9C27B0,stroke:#333,stroke-width:2px,color:#fff
    style Q fill:#9C27B0,stroke:#333,stroke-width:2px,color:#fff
    style R fill:#607D8B,stroke:#333,stroke-width:2px,color:#fff
```

## Component Details

### Jenkins Master (Controller)
- **Port**: 8080 (Web UI), 50000 (Agent communication)
- **Configuration**: JCasC (Jenkins Configuration as Code)
- **Plugins**: Managed via `plugins.txt` with specific versions
- **Admin**: Automated setup via Groovy init scripts
- **GitHub**: Token-based authentication for repository access

### Job DSL Repository
- **URL**: https://github.com/Maede-alv/jenkins-jobdsl
- **Purpose**: Infrastructure-as-code for Jenkins job definitions
- **Content**: Groovy scripts defining pipelines, freestyle jobs, and views
- **Integration**: Loaded via seed job on Jenkins master

### Jenkins Agents (Slaves)
- **Communication**: JNLP (Java Web Start) or SSH
- **Port**: 50000 (default agent port)
- **Purpose**: Distributed build execution
- **Management**: Automated via separate Ansible playbook

### Ansible Automation
- **Master Playbook**: `jenkins-master-ansible` - Deploys and configures Jenkins master
- **Agent Playbook**: `jenkins-agent-ansible` - Deploys and configures Jenkins agents
- **Configuration**: Declarative configuration management
- **Idempotency**: Safe to run multiple times

## Data Flow

1. **Initial Setup**: Ansible deploys Jenkins master with JCasC configuration
2. **Plugin Installation**: Jenkins installs plugins from `plugins.txt`
3. **Admin Setup**: Groovy init script creates admin user
4. **Seed Job Creation**: JCasC creates seed job for Job DSL loading
5. **Job Loading**: Seed job fetches and executes DSL scripts from repository
6. **Agent Connection**: Jenkins agents connect to master for build execution
7. **Build Execution**: Jobs run on available agents based on labels and resources

## Security Considerations

- **Authentication**: Local user authentication with admin setup
- **Authorization**: Logged-in users can do anything (configurable)
- **Credentials**: GitHub token stored securely in Jenkins credentials
- **Network**: Agent communication on dedicated port (50000)
- **SSH**: Key-based authentication for Ansible automation

## Scalability

- **Horizontal Scaling**: Add more agents as needed
- **Load Distribution**: Jobs distributed across available agents
- **Resource Management**: Agent labels for job routing
- **High Availability**: Master can be clustered (advanced setup) 