# GRC Platform - Architecture Diagrams

## System Architecture Overview

```mermaid
graph TB
    subgraph "Internet"
        User["👤 Users/Administrators"]
    end
    
    subgraph "AWS Account"
        subgraph "VPC"
            subgraph "Public Subnets"
                IGW["Internet Gateway"]
                ALB["Application Load Balancer"]
                NAT["NAT Gateway"]
            end
            
            subgraph "Private Subnets"
                ECS["ECS Fargate<br/>GRC Application"]
                RDS["RDS MySQL<br/>Database"]
                Lambda["Lambda Functions<br/>Compliance Monitor"]
            end
        end
        
        subgraph "AWS Services"
            Config["AWS Config<br/>Compliance Monitoring"]
            SecurityHub["Security Hub<br/>Security Findings"]
            CloudTrail["CloudTrail<br/>Audit Logging"]
            S3["S3 Buckets<br/>Evidence & Reports"]
            DynamoDB["DynamoDB<br/>Compliance Status"]
            KMS["KMS<br/>Encryption"]
            IAM["IAM<br/>Access Control"]
        end
        
        subgraph "Monitoring"
            CloudWatch["CloudWatch<br/>Metrics & Logs"]
            SNS["SNS<br/>Notifications"]
            EventBridge["EventBridge<br/>Event Routing"]
        end
    end
    
    User -->|HTTPS| IGW
    IGW --> ALB
    ALB --> ECS
    ECS --> RDS
    ECS --> S3
    ECS --> DynamoDB
    
    EventBridge -->|Triggers| Lambda
    Lambda --> Config
    Lambda --> DynamoDB
    Lambda --> SNS
    
    Config --> SecurityHub
    CloudTrail --> S3
    RDS -->|Encrypted| KMS
    S3 -->|Encrypted| KMS
    DynamoDB -->|Encrypted| KMS
    
    Lambda --> CloudWatch
    ECS --> CloudWatch
    RDS --> CloudWatch
    CloudWatch --> SNS
    
    ECS -->|Audit| CloudTrail
    Lambda -->|Audit| CloudTrail
    
    IAM -->|Permissions| ECS
    IAM -->|Permissions| Lambda
    IAM -->|Permissions| CloudTrail
```

## Data Flow Diagram

```mermaid
graph LR
    subgraph "Data Sources"
        AWS["AWS Resources"]
        User["User Input"]
    end
    
    subgraph "Processing"
        Config["AWS Config<br/>Resource Compliance"]
        Lambda["Lambda<br/>Compliance Analysis"]
        Risk["Risk Assessment<br/>Engine"]
    end
    
    subgraph "Storage"
        RDS["RDS<br/>GRC Data"]
        DynamoDB["DynamoDB<br/>Compliance Status"]
        S3["S3<br/>Evidence"]
    end
    
    subgraph "Output"
        Dashboard["Dashboard<br/>Visualization"]
        Reports["Compliance<br/>Reports"]
        Alerts["SNS<br/>Alerts"]
    end
    
    AWS --> Config
    User --> RDS
    
    Config --> Lambda
    RDS --> Lambda
    
    Lambda --> Risk
    Risk --> DynamoDB
    
    RDS --> Dashboard
    DynamoDB --> Dashboard
    S3 --> Dashboard
    
    RDS --> Reports
    DynamoDB --> Reports
    
    DynamoDB -->|Threshold| Alerts
    
    style AWS fill:#ff9900
    style Lambda fill:#ff9900
    style Config fill:#ff9900
    style RDS fill:#527fff
    style DynamoDB fill:#527fff
    style S3 fill:#527fff
    style Dashboard fill:#00a300
    style Reports fill:#00a300
    style Alerts fill:#ff6b6b
```

## Deployment Architecture

```mermaid
graph TB
    subgraph "Phase 1: Network"
        VPC["Create VPC<br/>10.0.0.0/16"]
        Subnets["Create Subnets<br/>Public & Private"]
        IGW["Create Internet Gateway"]
        NAT["Create NAT Gateway"]
        SG["Create Security Groups"]
    end
    
    subgraph "Phase 2: Database"
        RDS["Deploy RDS MySQL"]
        S3["Create S3 Buckets"]
        DynamoDB["Create DynamoDB Tables"]
        KMS["Create KMS Keys"]
    end
    
    subgraph "Phase 3: Application"
        ECS["Deploy ECS Fargate"]
        ALB["Deploy ALB"]
        Lambda["Deploy Lambda Functions"]
    end
    
    subgraph "Phase 4: Monitoring"
        Config["Enable AWS Config"]
        CloudTrail["Enable CloudTrail"]
        SecurityHub["Enable Security Hub"]
        CloudWatch["Configure CloudWatch"]
    end
    
    subgraph "Phase 5: Data"
        LoadData["Load Sample Data"]
        ConfigRules["Configure Config Rules"]
        Alarms["Set CloudWatch Alarms"]
    end
    
    VPC --> Subnets
    Subnets --> IGW
    IGW --> NAT
    NAT --> SG
    
    SG --> RDS
    SG --> S3
    SG --> DynamoDB
    RDS --> KMS
    S3 --> KMS
    
    SG --> ECS
    SG --> ALB
    ALB --> ECS
    ECS --> Lambda
    
    ECS --> Config
    ECS --> CloudTrail
    Config --> SecurityHub
    CloudTrail --> CloudWatch
    
    LoadData --> RDS
    LoadData --> DynamoDB
    ConfigRules --> Config
    CloudWatch --> Alarms
    
    style VPC fill:#e1f5ff
    style RDS fill:#c8e6c9
    style ECS fill:#fff9c4
    style Config fill:#f8bbd0
    style LoadData fill:#b2dfdb
```

## Compliance Monitoring Flow

```mermaid
graph LR
    subgraph "1. Detection"
        AWSRes["AWS Resources"]
        Config["AWS Config Rules"]
        AWSRes -->|Monitor| Config
    end
    
    subgraph "2. Analysis"
        Lambda["Lambda Function"]
        Risk["Risk Calculator"]
        Config -->|Compliance Data| Lambda
        Lambda --> Risk
    end
    
    subgraph "3. Storage"
        DynamoDB["DynamoDB<br/>Compliance Status"]
        RDS["RDS<br/>GRC Database"]
        Risk -->|Store| DynamoDB
        Risk -->|Store| RDS
    end
    
    subgraph "4. Reporting"
        Dashboard["Dashboard"]
        Reports["Reports"]
        DynamoDB -->|Display| Dashboard
        RDS -->|Generate| Reports
    end
    
    subgraph "5. Alerting"
        SNS["SNS Topic"]
        Email["Email Notification"]
        DynamoDB -->|Alert| SNS
        SNS -->|Send| Email
    end
    
    style AWSRes fill:#ff9900
    style Config fill:#ff9900
    style Lambda fill:#ff9900
    style Risk fill:#ff9900
    style DynamoDB fill:#527fff
    style RDS fill:#527fff
    style Dashboard fill:#00a300
    style Reports fill:#00a300
    style SNS fill:#ff6b6b
    style Email fill:#ff6b6b
```

## Risk Assessment Workflow

```mermaid
graph TD
    Start["Start Risk Assessment"]
    
    GetCompliance["Get Compliance Status<br/>from AWS Config"]
    CalcRatio["Calculate Non-Compliance Ratio"]
    
    Decision{Non-Compliance<br/>Ratio}
    
    Critical["Risk Level: CRITICAL<br/>Score: 8-10"]
    High["Risk Level: HIGH<br/>Score: 6-8"]
    Medium["Risk Level: MEDIUM<br/>Score: 4-6"]
    Low["Risk Level: LOW<br/>Score: 1-4"]
    
    Store["Store in DynamoDB<br/>& RDS"]
    Alert{Score > 7?}
    SendAlert["Send SNS Alert"]
    NoAlert["Log Only"]
    
    End["End"]
    
    Start --> GetCompliance
    GetCompliance --> CalcRatio
    CalcRatio --> Decision
    
    Decision -->|>= 75%| Critical
    Decision -->|50-75%| High
    Decision -->|20-50%| Medium
    Decision -->|< 20%| Low
    
    Critical --> Store
    High --> Store
    Medium --> Store
    Low --> Store
    
    Store --> Alert
    Alert -->|Yes| SendAlert
    Alert -->|No| NoAlert
    
    SendAlert --> End
    NoAlert --> End
    
    style Start fill:#e1f5ff
    style Critical fill:#ff6b6b
    style High fill:#ff9900
    style Medium fill:#f59e0b
    style Low fill:#10b981
    style End fill:#e1f5ff
```

## Control Implementation Tracking

```mermaid
graph LR
    subgraph "Framework"
        ISO["ISO 27001:2022"]
        NIST["NIST CSF"]
        PCI["PCI DSS"]
        HIPAA["HIPAA"]
    end
    
    subgraph "Controls"
        C1["Control A.5.1"]
        C2["Control A.7.1"]
        C3["Control PR.AC-1"]
        C4["Control 1.1"]
    end
    
    subgraph "Implementation Status"
        Impl["Implemented"]
        InProg["In Progress"]
        NotStart["Not Started"]
    end
    
    subgraph "Evidence"
        Docs["Documentation"]
        Logs["Audit Logs"]
        Tests["Test Results"]
    end
    
    ISO --> C1
    ISO --> C2
    NIST --> C3
    PCI --> C4
    
    C1 --> Impl
    C2 --> InProg
    C3 --> Impl
    C4 --> NotStart
    
    Impl --> Docs
    InProg --> Logs
    NotStart --> Tests
    
    style Impl fill:#10b981
    style InProg fill:#f59e0b
    style NotStart fill:#ef4444
```

## Security Architecture

```mermaid
graph TB
    subgraph "External"
        Internet["Internet"]
    end
    
    subgraph "Edge"
        IGW["Internet Gateway"]
        ALB["ALB<br/>HTTPS/TLS"]
    end
    
    subgraph "Network"
        PublicSN["Public Subnets"]
        PrivateSN["Private Subnets"]
        SG["Security Groups<br/>Least Privilege"]
        NACL["Network ACLs"]
    end
    
    subgraph "Application"
        ECS["ECS Fargate<br/>Containerized App"]
    end
    
    subgraph "Data"
        RDS["RDS MySQL<br/>Encrypted"]
        S3["S3 Buckets<br/>Encrypted"]
        DynamoDB["DynamoDB<br/>Encrypted"]
    end
    
    subgraph "Encryption"
        KMS["AWS KMS<br/>Key Management"]
    end
    
    subgraph "Access Control"
        IAM["IAM Roles<br/>& Policies"]
    end
    
    subgraph "Audit"
        CloudTrail["CloudTrail<br/>API Logging"]
        Logs["CloudWatch Logs<br/>Application Logs"]
    end
    
    Internet --> IGW
    IGW --> ALB
    ALB -->|HTTPS| PublicSN
    PublicSN --> PrivateSN
    PrivateSN --> SG
    SG --> NACL
    
    NACL --> ECS
    ECS --> RDS
    ECS --> S3
    ECS --> DynamoDB
    
    RDS -->|Encrypted| KMS
    S3 -->|Encrypted| KMS
    DynamoDB -->|Encrypted| KMS
    
    ECS -->|Permissions| IAM
    RDS -->|Permissions| IAM
    S3 -->|Permissions| IAM
    
    ECS -->|Audit| CloudTrail
    RDS -->|Audit| CloudTrail
    ECS -->|Logs| Logs
    
    style Internet fill:#ff9900
    style IGW fill:#e1f5ff
    style ALB fill:#e1f5ff
    style PublicSN fill:#c8e6c9
    style PrivateSN fill:#c8e6c9
    style SG fill:#fff9c4
    style NACL fill:#fff9c4
    style ECS fill:#f8bbd0
    style RDS fill:#b2dfdb
    style S3 fill:#b2dfdb
    style DynamoDB fill:#b2dfdb
    style KMS fill:#ffccbc
    style IAM fill:#ffccbc
    style CloudTrail fill:#d1c4e9
    style Logs fill:#d1c4e9
```

## Database Schema Relationships

```mermaid
erDiagram
    FRAMEWORKS ||--o{ CONTROLS : contains
    FRAMEWORKS ||--o{ COMPLIANCE_STATUS : tracks
    CONTROLS ||--o{ AUDIT_LOGS : logs
    RISKS ||--o{ AUDIT_LOGS : logs
    ASSETS ||--o{ RISKS : threatens
    COMPLIANCE_STATUS ||--o{ AUDIT_LOGS : records
    
    FRAMEWORKS {
        int id PK
        string name
        string version
        text description
    }
    
    CONTROLS {
        int id PK
        string control_id UK
        int framework_id FK
        string title
        string implementation_status
        string owner
    }
    
    RISKS {
        int id PK
        string risk_id UK
        string title
        string category
        string probability
        string impact
        decimal risk_score
        string status
    }
    
    ASSETS {
        int id PK
        string asset_id UK
        string name
        string type
        string classification
        string criticality
    }
    
    COMPLIANCE_STATUS {
        string timestamp PK
        decimal compliance_percentage
        int total_rules
        int compliant_rules
        int non_compliant_rules
    }
    
    AUDIT_LOGS {
        int id PK
        string action
        string entity_type
        string entity_id
        string user
        timestamp timestamp
    }
```

## Deployment Pipeline

```mermaid
graph LR
    Code["Code Repository<br/>GitHub"]
    
    Test["Run Tests<br/>test_cases.py"]
    
    Build["Build Artifacts<br/>CloudFormation"]
    
    Deploy["Deploy to AWS<br/>deploy.sh"]
    
    Verify["Verify Deployment<br/>Health Checks"]
    
    Monitor["Monitor<br/>CloudWatch"]
    
    Code --> Test
    Test -->|Pass| Build
    Build --> Deploy
    Deploy --> Verify
    Verify -->|Success| Monitor
    Verify -->|Failure| Code
    
    style Code fill:#e1f5ff
    style Test fill:#c8e6c9
    style Build fill:#fff9c4
    style Deploy fill:#f8bbd0
    style Verify fill:#b2dfdb
    style Monitor fill:#d1c4e9
```

## Component Interaction Matrix

| Component | Interacts With | Purpose |
|-----------|----------------|---------|
| AWS Config | Lambda, RDS, DynamoDB | Provides compliance data |
| Lambda | Config, RDS, DynamoDB, SNS | Analyzes compliance and triggers alerts |
| RDS | ECS, Lambda, Dashboard | Stores GRC data |
| DynamoDB | Lambda, Dashboard | Stores real-time compliance status |
| S3 | ECS, CloudTrail | Stores evidence and audit logs |
| CloudTrail | S3, CloudWatch | Logs all API calls |
| Security Hub | Config, CloudWatch | Aggregates security findings |
| CloudWatch | All services | Monitors metrics and logs |
| SNS | Lambda, CloudWatch | Sends notifications |
| IAM | All services | Controls access |
| KMS | RDS, S3, DynamoDB | Encrypts data |

## Key Integration Points

1. **AWS Config → Lambda**: Compliance data triggers Lambda for analysis
2. **Lambda → DynamoDB**: Real-time compliance status storage
3. **Lambda → SNS**: Alert notifications for non-compliance
4. **RDS ↔ Dashboard**: Display GRC data and user input
5. **CloudTrail → S3**: Audit logging for compliance
6. **Security Hub → Dashboard**: Security findings aggregation
7. **CloudWatch → Alarms**: Threshold-based alerting

This architecture ensures a secure, scalable, and compliant GRC platform that integrates seamlessly with AWS services for continuous monitoring and reporting.
