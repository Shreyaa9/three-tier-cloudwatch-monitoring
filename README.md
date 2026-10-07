# Three-Tier Application Monitoring Using AWS CloudWatch

## 📌 Project Overview

This project focuses on monitoring a Dockerized three-tier application deployed on Amazon ECS using AWS CloudWatch.

The application consists of three services:

- Frontend – Nginx
- Backend – Node.js / Express
- Database – MongoDB

The monitoring layer uses Amazon CloudWatch to collect and visualize ECS metrics, monitor application logs, configure CPU utilization alarms, and send notifications through Amazon SNS.

The objective of this project is to provide centralized monitoring, logging, visualization, and alerting for the deployed three-tier application.

---

## 🏗️ Application Architecture

The application follows a three-tier architecture consisting of a frontend, backend, and database layer.

    ┌──────────────────────┐
    │        User          │
    │     Web Browser      │
    └──────────┬───────────┘
               │
               │ HTTP Request
               ▼
    ┌──────────────────────┐
    │    Frontend Tier     │
    │    Nginx Container   │
    │      Port 80         │
    └──────────┬───────────┘
               │
               │ /api/tasks
               ▼
    ┌──────────────────────┐
    │     Backend Tier     │
    │   Node.js / Express  │
    │      Port 5000       │
    └──────────┬───────────┘
               │
               │ MongoDB Connection
               ▼
    ┌──────────────────────┐
    │    Database Tier     │
    │   MongoDB Container  │
    │      Port 27017      │
    └──────────────────────┘

### Frontend Tier

- Nginx serves the web application.
- Runs on port `80`.
- Handles user requests and forwards API requests to the backend.

### Backend Tier

- Built using Node.js and Express.
- Runs on port `5000`.
- Provides REST API endpoints for task operations.
- Communicates with MongoDB for data storage.

### Database Tier

- Uses MongoDB for storing application data.
- Runs on port `27017`.
- Stores and retrieves To-Do task information.

### Communication Flow

    User → Frontend (Nginx) → Backend (Node.js/Express) → MongoDB

The three tiers are deployed as separate containers using Amazon ECS Fargate.

---

## ☁️ AWS Monitoring Architecture

The deployed application is monitored using Amazon CloudWatch.

    ┌──────────────────────────────┐
    │        ECS Fargate           │
    │       Three-Tier App         │
    └──────────────┬───────────────┘
                   │
         ┌─────────┼─────────┐
         │         │         │
         ▼         ▼         ▼
    ECS Metrics  CloudWatch  CloudWatch
                 Logs        Alarms
                              │
                              ▼
                         Amazon SNS
                              │
                              ▼
                        Email Alert

         CloudWatch Dashboard
                  ▲
                  │
        Metrics + Alarms + Logs

### Monitoring Flow

    ECS Services
         ↓
    CloudWatch Metrics
         ↓
    CloudWatch Dashboard
         ↓
    CloudWatch Alarms
         ↓
    Amazon SNS
         ↓
    Email Notification

Backend application logs follow a separate path:

    ECS Backend Container
         ↓
    CloudWatch Logs
         ↓
    /ecs/three-tier-backend
         ↓
    Dashboard Log Widget

---

## ☁️ AWS Services Used

| Service | Purpose |
|---|---|
| Amazon ECS | Container orchestration |
| AWS Fargate | Serverless container execution |
| Amazon ECR | Container image storage |
| Amazon CloudWatch | Metrics, logs, dashboard and alarms |
| Amazon SNS | Monitoring notifications |
| AWS Cloud Map | Service discovery |
| IAM | Access control and permissions |

---

## 🐳 Application Stack

### Frontend

- Nginx
- HTML
- JavaScript
- Docker
- Amazon ECS / Fargate

### Backend

- Node.js
- Express.js
- Mongoose
- Docker
- Amazon ECS / Fargate

### Database

- MongoDB
- Docker
- Amazon ECS / Fargate

---

## 📊 CloudWatch Monitoring

A dedicated CloudWatch dashboard named **Three-Tier-App-Monitoring** was created to provide a centralized view of the application.

The dashboard monitors the following ECS services:

- `three-tier-frontend-service`
- `three-tier-backend-service`
- `three-tier-mongodb-service-2`

### Metrics Monitored

For each ECS service, the dashboard monitors:

- CPUUtilization
- MemoryUtilization
- LiveTaskCount

These metrics provide visibility into:

- CPU consumption
- Memory usage
- Running task count
- Overall ECS service health

---

## 📈 CloudWatch Dashboard

The monitoring dashboard contains the following components.

### ECS Overall Resource Monitoring

A combined graph displaying:

- Frontend CPU utilization
- Frontend memory utilization
- Frontend live task count
- Backend CPU utilization
- Backend memory utilization
- Backend live task count
- MongoDB CPU utilization
- MongoDB memory utilization
- MongoDB live task count

### Alarm Status Overview

The dashboard displays the current state of all configured CPU utilization alarms.

### Backend Log Monitoring

The dashboard also includes a CloudWatch Logs widget for:

    /ecs/three-tier-backend

This provides quick visibility into backend application logs.

---

## 🚨 CloudWatch Alarms

Three CPU utilization alarms were configured.

### Backend CPU Alarm

    Alarm Name: Backend-CPU-High
    Metric: CPUUtilization
    Condition: CPUUtilization > 70%
    Evaluation Period: 5 minutes

### Frontend CPU Alarm

    Alarm Name: Frontend-CPU-High
    Metric: CPUUtilization
    Condition: CPUUtilization > 70%
    Evaluation Period: 5 minutes

### MongoDB CPU Alarm

    Alarm Name: MongoDB-CPU-High
    Metric: CPUUtilization
    Condition: CPUUtilization > 70%
    Evaluation Period: 5 minutes

All three alarms are connected to the monitoring notification system.

---

## 📧 Amazon SNS Notifications

An Amazon SNS topic was created for monitoring alerts:

    ThreeTierMonitoringAlerts

The topic is used by CloudWatch alarms to send notifications when an alarm enters the `In alarm` state.

An email subscription was configured and confirmed successfully.

### Alert Flow

    ECS CPU Metric
         ↓
    CloudWatch Alarm
         ↓
      CPU > 70%
         ↓
      Amazon SNS
         ↓
      Email Alert

---

## 📋 CloudWatch Logs

Backend container logs are configured using the AWS `awslogs` log driver.

### Log Group

    /ecs/three-tier-backend

### Region

    us-east-1

### Stream Prefix

    ecs

The logs include application startup and database connection information.

### Example Log Messages

    Backend API running on port 5000
    Connected to MongoDB successfully!

---

## 🔧 ECS Configuration

The backend ECS service is deployed using:

    Cluster:
    three-tier-cluster

    Service:
    three-tier-backend-service

    Task Definition:
    three-tier-backend

    Revision:
    5

    Launch Type:
    AWS Fargate

The backend service was successfully deployed with:

    1 Desired Task
    1 Running Task
    0 Pending Tasks
    Deployment Status: Success

---

## 🔍 Monitoring Workflow

The monitoring workflow implemented in this project is:

    Application
         ↓
    ECS Fargate Services
         ↓
    CloudWatch Metrics
         ↓
    CloudWatch Dashboard
         ↓
    CPU Utilization Monitoring
         ↓
    CloudWatch Alarms
         ↓
    Amazon SNS
         ↓
    Email Notification

Application logs follow a separate monitoring path:

    ECS Backend Container
         ↓
    CloudWatch Logs
         ↓
    /ecs/three-tier-backend
         ↓
    Log Streams
         ↓
    Dashboard Log Widget

---

## 🧪 Testing and Verification

The deployed application was tested by performing application operations through the live frontend.

The following were verified:

- Application loads successfully
- New task can be added
- Task can be completed
- Data persists after page refresh
- Backend API is running
- Backend successfully connects to MongoDB
- CloudWatch metrics are receiving data
- CloudWatch logs are being generated
- CPU alarms are active
- Backend CPU alarm is in the `OK` state
- Frontend CPU alarm is in the `OK` state
- MongoDB CPU alarm is in the `OK` state
- SNS email subscription is confirmed

---

## 📸 Project Screenshots

All monitoring evidence is available in the `screenshots/` directory.

### CloudWatch Dashboard

[CloudWatch Monitoring Dashboard](screenshots/01-cloudwatch-dashboard.png)

### Logs and Alarms

[Backend CloudWatch Logs](screenshots/02-backend-cloudwatch-logs.png)

[CloudWatch Alarms](screenshots/03-cloudwatch-alarms.png)

[Backend CPU Alarm](screenshots/04-backend-alarm-details.png)

[Frontend CPU Alarm](screenshots/05-frontend-alarm-details.png)

[MongoDB CPU Alarm](screenshots/06-mongodb-alarm-details.png)

### Notifications

[SNS Monitoring Notification](screenshots/07-sns-notification.png)

### ECS Configuration

[Backend ECS Deployment](screenshots/08-backend-cloudwatch-deployment.png)

[ECS CloudWatch Logging Configuration](screenshots/09-ecs-cloudwatch-logging-config.png)

[ECS Cluster Overview](screenshots/11-ecs-cluster-overview.png)

### Application

[Live Application Test](screenshots/10-live-application-test.png)

### CloudWatch Log Group

[CloudWatch Log Group](screenshots/12-cloudwatch-log-group.png)

---

## 📁 Repository Structure

    three-tier-cloudwatch-monitoring/
    │
    ├── screenshots/
    │   ├── 01-cloudwatch-dashboard.png
    │   ├── 02-backend-cloudwatch-logs.png
    │   ├── 03-cloudwatch-alarms.png
    │   ├── 04-backend-alarm-details.png
    │   ├── 05-frontend-alarm-details.png
    │   ├── 06-mongodb-alarm-details.png
    │   ├── 07-sns-notification.png
    │   ├── 08-backend-cloudwatch-deployment.png
    │   ├── 09-ecs-cloudwatch-logging-config.png
    │   ├── 10-live-application-test.png
    │   ├── 11-ecs-cluster-overview.png
    │   └── 12-cloudwatch-log-group.png
    │
    ├── README.md
    ├── ARCHITECTURE.md
    ├── MONITORING.md
    ├── ALARMS.md
    ├── SETUP.md
    ├── TESTING.md
    ├── TROUBLESHOOTING.md
    └── CLEANUP.md

---

## 🎯 Project Objectives

The main objectives of this project are:

1. Monitor a deployed three-tier application using AWS CloudWatch.
2. Visualize ECS resource utilization through a centralized dashboard.
3. Collect and analyze backend application logs.
4. Configure CPU utilization alarms for application services.
5. Configure SNS-based email notifications.
6. Verify application health through monitoring data.
7. Understand practical AWS observability and monitoring workflows.

---

## ✅ Project Outcome

The three-tier application was successfully monitored using AWS CloudWatch.

The project demonstrates:

- ECS service monitoring
- Fargate resource monitoring
- CloudWatch metrics
- CloudWatch dashboards
- CloudWatch Logs
- CloudWatch alarms
- Amazon SNS notifications
- Application health verification

The final monitoring setup provides a centralized way to observe the health and resource utilization of the three-tier application and receive alerts when CPU utilization exceeds the configured threshold.

---

## 👩‍💻 Author

**Shreyaa9**

GitHub:  
https://github.com/Shreyaa9

Repository:  
https://github.com/Shreyaa9/three-tier-cloudwatch-monitoring
