# AWS Integrated GRC Platform - Capstone Project

## Overview

The AWS Integrated GRC Platform is a comprehensive Governance, Risk, and Compliance solution designed for GRC208 students. This capstone project demonstrates how to build, deploy, and manage an enterprise-grade GRC platform using Amazon Web Services (AWS) and industry best practices.

## Project Objectives

Students completing this capstone will learn to:

1. Design and implement a multi-tier application architecture on AWS
2. Integrate AWS native security and compliance services
3. Implement Infrastructure as Code (IaC) using CloudFormation
4. Develop serverless functions for compliance automation
5. Map technical controls to compliance frameworks
6. Create comprehensive compliance monitoring and reporting solutions
7. Implement audit logging and evidence collection
8. Manage risk assessment and mitigation strategies

## Key Features

### Governance Module
- Framework management (ISO 27001, NIST, PCI DSS, HIPAA, GDPR, SOC 2)
- Control library and mapping
- Policy management and tracking
- Role-based access control (RBAC)

### Risk Management Module
- Risk identification and assessment
- Risk scoring matrix (probability × impact)
- Mitigation strategy tracking
- Risk register and reporting

### Compliance Module
- Compliance status monitoring
- AWS Config integration
- Automated compliance checks
- Control implementation tracking
- Compliance reporting

### Audit & Evidence Module
- Centralized evidence repository (S3)
- Audit trail logging (CloudTrail)
- Assessment evidence collection
- Audit report generation

## Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Infrastructure** | AWS CloudFormation | Infrastructure as Code |
| **Compute** | AWS Lambda, ECS Fargate | Application and automation |
| **Database** | Amazon RDS MySQL | Relational data storage |
| **Data Storage** | Amazon S3 | Evidence and reports |
| **Cache/NoSQL** | Amazon DynamoDB | Compliance status and risks |
| **Encryption** | AWS KMS | Data encryption |
| **Monitoring** | AWS Config, Security Hub | Compliance monitoring |
| **Audit** | AWS CloudTrail | Audit logging |
| **Networking** | VPC, ALB, NAT Gateway | Network infrastructure |
| **Automation** | Python, AWS Lambda | Compliance automation |

## Project Structure

```
grc-capstone-project/
├── README.md                              # Project overview
├── DEPLOYMENT_GUIDE.md                    # Step-by-step deployment
├── architecture_design.md                 # Architecture documentation
├── cloudformation-network-stack.yaml      # Network infrastructure
├── cloudformation-database-stack.yaml     # Database infrastructure
├── lambda_compliance_monitor.py           # Compliance monitoring function
├── sample_data.sql                        # Sample data for testing
├── test_cases.py                          # Comprehensive test suite
├── deploy.sh                              # Automated deployment script
├── requirements.txt                       # Python dependencies
├── docs/
│   ├── AWS_SERVICES_GUIDE.md             # AWS services explanation
│   ├── COMPLIANCE_FRAMEWORKS.md          # Framework details
│   ├── BEST_PRACTICES.md                 # Implementation best practices
│   └── TROUBLESHOOTING.md                # Common issues and solutions
└── examples/
    ├── compliance_report_template.md     # Report template
    ├── risk_assessment_template.md       # Risk template
    └── audit_checklist.md                # Audit checklist
```

## Quick Start

### Prerequisites
- AWS Account with appropriate permissions
- AWS CLI v2 installed and configured
- Python 3.8+
- Git

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd grc-capstone-project

# Install Python dependencies
pip install -r requirements.txt

# Configure AWS credentials
aws configure

# Deploy infrastructure
./deploy.sh
```

### First Steps

1. **Review Architecture**: Read `architecture_design.md` to understand the system design
2. **Follow Deployment Guide**: Use `DEPLOYMENT_GUIDE.md` for step-by-step instructions
3. **Load Sample Data**: Execute `sample_data.sql` to populate the database
4. **Run Tests**: Execute `test_cases.py` to validate the deployment
5. **Explore AWS Console**: Navigate to AWS Console to see deployed resources

## AWS Services Used

### Core Services
- **AWS CloudFormation**: Infrastructure as Code for resource provisioning
- **Amazon VPC**: Isolated network environment with public/private subnets
- **Amazon RDS**: MySQL database for GRC data
- **Amazon S3**: Storage for evidence and compliance reports
- **AWS Lambda**: Serverless functions for compliance automation

### Security & Compliance Services
- **AWS Config**: Continuous monitoring of resource configurations
- **AWS Security Hub**: Centralized security findings and compliance
- **AWS CloudTrail**: API activity and audit logging
- **AWS IAM**: Identity and access management
- **AWS KMS**: Key management and encryption

### Monitoring & Alerting
- **Amazon CloudWatch**: Metrics, logs, and alarms
- **Amazon SNS**: Alert notifications
- **Amazon EventBridge**: Event-driven automation

## Compliance Frameworks Supported

The platform supports mapping to the following compliance frameworks:

- **ISO 27001:2022** - Information Security Management System
- **NIST Cybersecurity Framework** - Risk management and security controls
- **PCI DSS 3.2.1** - Payment Card Industry Data Security Standard
- **HIPAA** - Health Insurance Portability and Accountability Act
- **GDPR** - General Data Protection Regulation
- **SOC 2** - Service Organization Control Framework

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     AWS Account                              │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                    VPC (10.0.0.0/16)                 │   │
│  │                                                       │   │
│  │  ┌─────────────────────────────────────────────┐    │   │
│  │  │        Public Subnets (ALB, NAT)            │    │   │
│  │  │  ┌──────────────────────────────────────┐   │    │   │
│  │  │  │   Application Load Balancer (ALB)    │   │    │   │
│  │  │  └──────────────────────────────────────┘   │    │   │
│  │  └─────────────────────────────────────────────┘    │   │
│  │                        ↓                             │   │
│  │  ┌─────────────────────────────────────────────┐    │   │
│  │  │      Private Subnets (ECS, RDS)             │    │   │
│  │  │  ┌──────────────────────────────────────┐   │    │   │
│  │  │  │  ECS Fargate (GRC Application)       │   │    │   │
│  │  │  └──────────────────────────────────────┘   │    │   │
│  │  │                                              │    │   │
│  │  │  ┌──────────────────────────────────────┐   │    │   │
│  │  │  │  RDS MySQL (GRC Database)           │   │    │   │
│  │  │  └──────────────────────────────────────┘   │    │   │
│  │  └─────────────────────────────────────────────┘    │   │
│  │                                                       │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              AWS Native Services                      │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌────────────┐ │   │
│  │  │ AWS Config   │  │ Security Hub │  │ CloudTrail │ │   │
│  │  └──────────────┘  └──────────────┘  └────────────┘ │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌────────────┐ │   │
│  │  │ Lambda       │  │ DynamoDB     │  │ S3         │ │   │
│  │  └──────────────┘  └──────────────┘  └────────────┘ │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

## Data Flow

```
AWS Resources
    ↓
AWS Config (Compliance Status)
    ↓
Lambda Function (Compliance Monitor)
    ↓
DynamoDB (Compliance Status Table)
    ↓
RDS Database (GRC Platform)
    ↓
S3 (Evidence & Reports)
    ↓
CloudWatch (Monitoring)
    ↓
SNS (Alerts)
```

## Testing

The project includes comprehensive test cases covering:

- Compliance monitoring functionality
- Risk assessment and scoring
- Data validation
- Database operations
- Framework mapping
- Audit logging
- Report generation
- Integration workflows

### Run Tests

```bash
# Run all tests
python3 test_cases.py

# Run with verbose output
python3 -m unittest test_cases -v

# Run specific test class
python3 -m unittest test_cases.TestComplianceMonitoring -v
```

## Deployment

### Automated Deployment

```bash
# Make script executable
chmod +x deploy.sh

# Run deployment script
./deploy.sh
```

### Manual Deployment

Follow the step-by-step instructions in `DEPLOYMENT_GUIDE.md`

## Configuration

### Environment Variables

Create a `.env` file with the following variables:

```bash
AWS_REGION=us-east-1
AWS_ACCOUNT_ID=123456789012
DB_HOST=grc-capstone-db.xxxxx.us-east-1.rds.amazonaws.com
DB_PORT=3306
DB_NAME=grcdb
DB_USER=grcadmin
DB_PASSWORD=SecurePassword123!
```

## Documentation

- **DEPLOYMENT_GUIDE.md**: Complete step-by-step deployment instructions
- **architecture_design.md**: System architecture and design decisions
- **docs/AWS_SERVICES_GUIDE.md**: Detailed explanation of AWS services
- **docs/COMPLIANCE_FRAMEWORKS.md**: Compliance framework details
- **docs/BEST_PRACTICES.md**: Implementation best practices
- **docs/TROUBLESHOOTING.md**: Common issues and solutions

## Learning Outcomes

Upon completing this capstone, students will be able to:

1. Design scalable cloud architectures for GRC applications
2. Implement AWS native security and compliance services
3. Write Infrastructure as Code using CloudFormation
4. Develop serverless applications with AWS Lambda
5. Implement comprehensive audit logging and monitoring
6. Map technical controls to compliance frameworks
7. Create automated compliance checking and reporting
8. Manage cloud security and governance

## Best Practices Implemented

- **Infrastructure as Code**: All infrastructure defined in CloudFormation
- **Security**: Encryption at rest and in transit, least privilege access
- **High Availability**: Multi-AZ deployment with load balancing
- **Monitoring**: Comprehensive CloudWatch metrics and alarms
- **Automation**: Serverless functions for compliance automation
- **Audit Trail**: Complete audit logging with CloudTrail
- **Disaster Recovery**: Automated backups and recovery procedures
- **Cost Optimization**: Use of serverless and managed services

## Troubleshooting

For common issues and solutions, see `docs/TROUBLESHOOTING.md`

## Support

For questions or issues:
1. Check the troubleshooting guide
2. Review AWS documentation
3. Check CloudWatch logs
4. Review CloudFormation events

## References

- AWS GRC Documentation: https://docs.aws.amazon.com/grc/
- AWS Security Best Practices: https://aws.amazon.com/security/best-practices/
- AWS Config User Guide: https://docs.aws.amazon.com/config/
- AWS Lambda Developer Guide: https://docs.aws.amazon.com/lambda/
- AWS CloudFormation User Guide: https://docs.aws.amazon.com/cloudformation/

## License

This project is provided as educational material for GRC208 students.

## Acknowledgments

This project incorporates best practices from:
- AWS Security Reference Architecture
- NIST Cybersecurity Framework
- ISO 27001:2022 Standard
- AWS Well-Architected Framework

## Version History

- **v1.0** (March 2026): Initial release
  - Core GRC platform
  - AWS Config integration
  - Compliance monitoring
  - Risk assessment
  - Audit logging

## Contact

For questions about this capstone project, contact your GRC208 instructor.

---

**Last Updated**: March 2026
**Status**: Production Ready
**Maintenance**: Actively Maintained
