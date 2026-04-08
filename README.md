# GRC208 AWS Capstone Project

## Project Title
Cloud-Based GRC (Governance, Risk & Compliance) Automation System on AWS

---

## Overview
This project implements a cloud-based GRC solution using AWS services to automate compliance monitoring, configuration tracking, and scheduled governance checks.

---

## Services Used
- AWS CloudFormation  
- Amazon VPC  
- Amazon RDS (MySQL)  
- AWS Lambda  
- AWS Config  
- Amazon EventBridge  
- Amazon S3  
- IAM  

---

## Implementation Summary
- Secured AWS account with MFA and IAM administrative user  
- Deployed network and database infrastructure using CloudFormation  
- Provisioned Amazon RDS MySQL instance  
- Created AWS Lambda function for compliance monitoring  
- Enabled AWS Config for continuous resource tracking  
- Configured EventBridge for automated scheduled checks  
- Implemented IAM roles and policies for secure service access  

---

## Key Features
- Automated compliance monitoring  
- Serverless architecture  
- Infrastructure as Code (IaC)  
- Secure role-based access control  
- Continuous scheduled governance checks  

---

## Database Access Note
The RDS database was deployed successfully. Direct external access testing through MySQL Workbench was attempted, but connectivity remained restricted due to subnet design and managed cloud environment limitations. In a production-grade deployment, database access would typically be handled through private networking, VPN, or a bastion host.

---

## Result
The project successfully deployed the core GRC automation workflow:

- CloudFormation stacks completed successfully  
- Lambda function deployed and active  
- AWS Config recorder enabled  
- EventBridge scheduling enabled  
- Lambda test invocation returned status code 200  

---

## Conclusion
This project demonstrates a scalable, automated, and secure framework for continuous compliance monitoring in AWS cloud environments.

---

## Final Statement
**The implemented solution provides a scalable, automated, and secure framework for continuous compliance monitoring in cloud environments.**
