# Setup Guide

## 📌 Overview

This guide explains the setup and configuration required to monitor the existing three-tier application using Amazon CloudWatch.

The application is already deployed on Amazon ECS using AWS Fargate. This project adds the CloudWatch monitoring, logging, dashboard, alarm, and notification components.

---

## 🏗️ Existing Application

The monitoring project uses the existing three-tier application consisting of:

    Frontend → Nginx
    Backend  → Node.js / Express
    Database → MongoDB

The application runs using Amazon ECS and AWS Fargate.

### ECS Cluster

    three-tier-cluster

### ECS Services

    three-tier-frontend-service
    three-tier-backend-service
    three-tier-mongodb-service-2

---

## ☁️ AWS Region

The project uses the following AWS region:

    us-east-1

Make sure the AWS Console is set to the same region while configuring the monitoring resources.

---

## 🔐 Prerequisites

Before configuring CloudWatch monitoring, ensure that:

- The three-tier application is deployed and running.
- ECS services are active.
- ECS tasks are running.
- The application can be accessed through the frontend.
- Amazon ECR images are available.
- AWS Cloud Map service discovery is configured.
- Appropriate IAM permissions are available.
- Access to Amazon CloudWatch is available.

---

## 📊 Step 1 — Create CloudWatch Dashboard

Open:

    AWS Console
    → CloudWatch
    → Dashboards

Choose:

    Create dashboard

Use the following dashboard name:

    Three-Tier-App-Monitoring

Select:

    Metrics

The dashboard is used to provide a centralized view of the three ECS services.

---

## 📈 Step 2 — Add ECS Metrics

Inside the CloudWatch dashboard:

    Add widget
    → Metrics
    → ECS
    → ClusterName, ServiceName

Select the ECS cluster:

    three-tier-cluster

Monitor the following services:

    three-tier-frontend-service
    three-tier-backend-service
    three-tier-mongodb-service-2

Select the following metrics:

    CPUUtilization
    MemoryUtilization
    LiveTaskCount

The metrics can be displayed together in an ECS monitoring widget.

---

## 🚨 Step 3 — Create CPU Alarms

Go to:

    CloudWatch
    → Alarms
    → All alarms
    → Create alarm

Select the ECS service metric:

    ECS
    → ClusterName, ServiceName
    → CPUUtilization

Configure the alarm with:

    Statistic:
    Average

    Period:
    5 minutes

    Threshold:
    Greater than 70%

Create alarms for:

    Backend-CPU-High
    Frontend-CPU-High
    MongoDB-CPU-High

---

## 📧 Step 4 — Configure SNS Notifications

During alarm creation, configure the notification action.

Create an SNS topic:

    ThreeTierMonitoringAlerts

Add an email subscription to the topic.

AWS sends a confirmation email to the configured email address.

Open the email and confirm the subscription.

After confirmation, the SNS topic can be used by all three CloudWatch alarms.

---

## 📋 Step 5 — Configure CloudWatch Logging

The backend ECS task definition is configured to send container logs to Amazon CloudWatch Logs.

Open:

    ECS
    → Task Definitions
    → three-tier-backend

Create a new revision.

Inside the backend container configuration, enable:

    Use log collection

Configure:

    Destination:
    Amazon CloudWatch

    awslogs-group:
    /ecs/three-tier-backend

    awslogs-region:
    us-east-1

    awslogs-stream-prefix:
    ecs

Create the new task definition revision.

---

## 🚀 Step 6 — Deploy the Logging Configuration

After creating the new task definition revision, update the backend ECS service.

Open:

    ECS
    → Clusters
    → three-tier-cluster
    → Services
    → three-tier-backend-service

Choose:

    Update service

Select the new task definition revision.

For this project, the monitoring configuration was deployed using:

    three-tier-backend:5

Wait for the deployment to complete.

Verify:

    Desired tasks: 1
    Running tasks: 1
    Pending tasks: 0
    Deployment: Successful

---

## 📋 Step 7 — Verify CloudWatch Logs

Open:

    CloudWatch
    → Logs
    → Log Management

Locate:

    /ecs/three-tier-backend

Open the log group and select a recent log stream.

Verify that application logs are being received.

Example messages include:

    Backend API running on port 5000

and:

    Connected to MongoDB successfully!

---

## 🔎 Step 8 — Configure Log Analytics

CloudWatch Logs can also be analyzed using the log analytics interface.

Select:

    /ecs/three-tier-backend

Choose an appropriate time range and run the query.

Verify that backend application events are returned.

The monitoring project successfully verified backend log events through CloudWatch.

---

## 📊 Step 9 — Add Alarm Status to Dashboard

Return to:

    CloudWatch
    → Dashboards
    → Three-Tier-App-Monitoring

Choose:

    Add widget

Select the alarm data source.

Add:

    Backend-CPU-High
    Frontend-CPU-High
    MongoDB-CPU-High

Use the widget title:

    Alarm Status Overview

This provides a centralized view of alarm states.

---

## 📋 Step 10 — Add Backend Logs to Dashboard

Inside:

    Three-Tier-App-Monitoring

Choose:

    Add widget
    → Logs

Use the backend log group:

    /ecs/three-tier-backend

Create the log widget with the backend log query.

Use the widget title:

    Backend Application Logs

This allows backend application events to be viewed directly from the monitoring dashboard.

---

## 💾 Step 11 — Save the Dashboard

After adding all widgets, save the dashboard.

The final dashboard contains:

    ECS Overall Resource Monitoring
    Alarm Status Overview
    Backend Application Logs

The dashboard provides a centralized monitoring view of the three-tier application.

---

## 🧪 Step 12 — Verify the Application

Open the live application.

Perform basic operations such as:

1. Add a new task.
2. Complete the task.
3. Refresh the page.
4. Verify that the application continues to work.

Then return to CloudWatch and verify:

    ECS Metrics
    CloudWatch Logs
    Alarm Status

---

## ✅ Final Verification

The following components should be available after setup:

    ✓ Three-Tier-App-Monitoring dashboard
    ✓ ECS CPU metrics
    ✓ ECS memory metrics
    ✓ ECS live task metrics
    ✓ Backend CloudWatch Logs
    ✓ Backend-CPU-High alarm
    ✓ Frontend-CPU-High alarm
    ✓ MongoDB-CPU-High alarm
    ✓ ThreeTierMonitoringAlerts SNS topic
    ✓ Confirmed email subscription
    ✓ Successful backend deployment with logging
    ✓ Application monitoring verification

---

## 📌 Important Note

This project reuses the existing three-tier application deployment.

The monitoring configuration should therefore be added without unnecessarily modifying or deleting the existing application infrastructure.

CloudWatch monitoring resources can be removed later using the instructions provided in `CLEANUP.md`.
