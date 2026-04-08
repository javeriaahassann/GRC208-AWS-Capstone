# GRC208 AWS Capstone Project - Deployment Guide

---

## Overview
This guide provides step-by-step instructions to deploy the AWS-based GRC (Governance, Risk & Compliance) automation system.

---

## AWS Services Used
- AWS CloudFormation  
- Amazon VPC  
- Amazon RDS (MySQL)  
- AWS Lambda  
- AWS Config  
- Amazon EventBridge  
- Amazon S3  
- IAM  

---

## Deployment Steps

### Step 1: AWS Account Setup
- Create AWS account  
- Enable MFA on root account  
- Create IAM administrative user  
- Configure AWS CLI  

---

### Step 2: Network Deployment
- Deploy VPC using CloudFormation  
- Create public and private subnets  
- Configure security groups  

---

### Step 3: Database Deployment
- Deploy RDS MySQL instance using CloudFormation  
- Configure database credentials  
- Verify database status (Available)  

---

### Step 4: Lambda Setup
- Create IAM role for Lambda  
- Attach required policies  
- Deploy Lambda function  
- Test Lambda execution  

---

### Step 5: AWS Config Setup
- Create IAM role for AWS Config  
- Create S3 bucket for logs  
- Enable configuration recorder  
- Verify recording status  

---

### Step 6: Automation Setup
- Create EventBridge rule  
- Set schedule (e.g., hourly)  
- Link rule with Lambda function  

---

## Workflow / System Flow

AWS Resources  
↓  
AWS Config (tracks configurations)  
↓  
Lambda Function (runs compliance checks)  
↓  
Database (RDS stores results)  
↓  
S3 (stores reports/logs)  
↓  
CloudWatch / EventBridge (monitoring & scheduling)  

---

## Output
- Automated compliance monitoring system  
- Serverless architecture  
- Continuous governance checks  

---

## Notes
- Ensure all resources are deployed in **us-east-1 region**  
- Database may not be publicly accessible due to security configuration  
- Follow least privilege principle for IAM roles  

---

## Conclusion
The deployment successfully creates a secure and automated GRC monitoring system using AWS services.