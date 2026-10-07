# Cleanup Guide

## 📌 Overview

This document provides the steps required to safely clean up the AWS resources created or used for the three-tier application monitoring project.

The cleanup should be performed only after all testing, screenshots, demonstrations, and documentation are complete.

---

## ⚠️ Important

Before deleting resources:

- Make sure all required screenshots have been saved.
- Make sure the GitHub repository is updated.
- Make sure the CloudWatch dashboard is no longer required.
- Make sure CloudWatch alarm evidence has been collected.
- Make sure SNS notification evidence has been collected.
- Confirm that the original three-tier application project does not depend on resources being deleted.

The monitoring project reuses the deployed three-tier application infrastructure, so deleting ECS resources may also affect the original application.

---

## ☁️ 1. Remove CloudWatch Dashboard

Go to:

    AWS Console
    → CloudWatch
    → Dashboards

Open:

    Three-Tier-App-Monitoring

Delete the dashboard if it is no longer required.

---

## 🚨 2. Delete CloudWatch Alarms

Go to:

    CloudWatch
    → Alarms
    → All alarms

Select:

    Backend-CPU-High
    Frontend-CPU-High
    MongoDB-CPU-High

Choose:

    Delete

Confirm the deletion.

---

## 📧 3. Remove SNS Subscription

Go to:

    Amazon SNS
    → Topics
    → ThreeTierMonitoringAlerts

Review the subscriptions.

If the notification system is no longer required:

1. Delete the email subscription.
2. Delete the SNS topic if it was created only for this project.

Topic:

    ThreeTierMonitoringAlerts

---

## 📋 4. Delete CloudWatch Log Group

Go to:

    CloudWatch
    → Logs
    → Log Management

Locate:

    /ecs/three-tier-backend

If the logs are no longer required:

1. Select the log group.
2. Choose Delete.
3. Confirm the deletion.

### Important

CloudWatch Logs may be useful for future troubleshooting, so keep the log group if the application is still being used.

---

## 🐳 5. ECS Task Definition

The monitoring configuration created a new backend task definition revision:

    three-tier-backend:5

Do not delete the task definition revision if the application is still using it.

If the monitoring project is completely finished and the service is no longer required, the revision can be deregistered from:

    ECS
    → Task Definitions
    → three-tier-backend

---

## 🖥️ 6. ECS Services and Cluster

The application uses the following ECS resources:

    Cluster:
    three-tier-cluster

    Services:
    three-tier-frontend-service
    three-tier-backend-service
    three-tier-mongodb-service-2

### Important

These resources belong to the deployed three-tier application and were reused for this monitoring project.

Therefore:

**Do not delete the ECS cluster or services only for cleaning up the monitoring project.**

Delete them only when the original application is also no longer required.

---

## 📦 7. Amazon ECR

The application images are stored in Amazon ECR repositories:

    three-tier-frontend
    three-tier-backend
    three-tier-mongodb

These repositories are part of the original application deployment.

Do not delete them unless the original application is no longer required.

---

## 🔎 8. AWS Cloud Map

The application uses the Cloud Map namespace:

    three-tier.local

Services include:

    backend
    mongodb

Cloud Map should only be deleted after the ECS application and service discovery configuration are no longer required.

---

## 🔐 9. IAM Resources

The monitoring project does not require deletion of the existing application IAM roles.

Review IAM resources before removing anything.

Never delete an IAM role that is still being used by ECS, GitHub Actions, or another AWS service.

---

## 🧹 Recommended Cleanup Order

If the complete application and monitoring environment are no longer required, use the following general order:

    1. Stop application usage
            ↓
    2. Delete CloudWatch alarms
            ↓
    3. Delete CloudWatch dashboard
            ↓
    4. Remove SNS subscription/topic
            ↓
    5. Delete CloudWatch log group
            ↓
    6. Remove ECS services
            ↓
    7. Remove ECS cluster
            ↓
    8. Remove Cloud Map services/namespace
            ↓
    9. Remove ECR repositories
            ↓
    10. Review and remove unused IAM/network resources

---

## 💰 Cost Considerations

AWS resources may incur charges depending on usage.

Particular resources to review include:

- Amazon ECS / Fargate
- Amazon ECR storage
- CloudWatch Logs
- CloudWatch custom monitoring or dashboards where applicable
- Amazon SNS usage
- Other supporting AWS resources

Before finishing the project, review the AWS Billing dashboard and confirm that no unnecessary resources remain active.

---

## ✅ Final Cleanup Checklist

    [ ] CloudWatch dashboard reviewed
    [ ] CloudWatch alarms reviewed
    [ ] SNS topic reviewed
    [ ] SNS subscription reviewed
    [ ] CloudWatch log group reviewed
    [ ] ECS services reviewed
    [ ] ECS cluster reviewed
    [ ] ECR repositories reviewed
    [ ] Cloud Map resources reviewed
    [ ] IAM resources reviewed
    [ ] AWS Billing reviewed

---

## 📌 Important Final Note

This monitoring project was built on top of an existing three-tier application deployment.

Therefore, monitoring resources and application resources should be treated separately.

Deleting CloudWatch monitoring resources is generally safe once monitoring is no longer required.

Deleting ECS, ECR, Cloud Map, networking, or IAM resources may affect the underlying three-tier application and should only be performed when the complete application environment is no longer needed.
