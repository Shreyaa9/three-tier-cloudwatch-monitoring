# Three-Tier Application Monitoring Architecture

## 📌 Overview

This document describes the architecture of the three-tier application and the AWS CloudWatch monitoring layer implemented for the deployment.

The application follows a three-tier architecture consisting of:

1. Frontend
2. Backend
3. Database

The application is deployed using Amazon ECS with AWS Fargate, while AWS CloudWatch is used for monitoring metrics, logs, dashboards, and alarms.

---

## 🏗️ Application Architecture

    ┌──────────────────────────────┐
    │        User / Browser        │
    └──────────────┬───────────────┘
                   │
                   │ HTTP
                   ▼
    ┌──────────────────────────────┐
    │     Frontend - Nginx         │
    │                              │
    │     ECS Fargate              │
    │     Port: 80                 │
    └──────────────┬───────────────┘
                   │
                   │ API Requests
                   ▼
    ┌──────────────────────────────┐
    │     Backend - Node.js        │
    │                              │
    │     ECS Fargate              │
    │     Port: 5000               │
    └──────────────┬───────────────┘
                   │
                   │ MongoDB Connection
                   ▼
    ┌──────────────────────────────┐
    │        MongoDB               │
    │                              │
    │     ECS Fargate              │
    │     Port: 27017              │
    └──────────────────────────────┘

---

## ☁️ AWS Infrastructure

The application runs inside an Amazon ECS cluster:

    ECS Cluster
    └── three-tier-cluster
        │
        ├── three-tier-frontend-service
        │
        ├── three-tier-backend-service
        │
        └── three-tier-mongodb-service-2

All three services use AWS Fargate as the compute platform.

---

## 🔎 Service Discovery

AWS Cloud Map is used for service discovery between the ECS services.

The private namespace is:

    three-tier.local

The registered services include:

    backend.three-tier.local
    mongodb.three-tier.local

The backend uses the MongoDB service name to connect to the database.

The backend MongoDB connection is configured as:

    mongodb://mongodb.three-tier.local:27017/todoDB

This allows the backend service to communicate with MongoDB without depending on a fixed IP address.

---

## 📊 CloudWatch Monitoring Architecture

AWS CloudWatch provides the monitoring layer for the application.

    ┌─────────────────────────────────────────┐
    │          Three-Tier Application          │
    │                                         │
    │  Frontend    Backend       MongoDB      │
    └──────┬─────────┬─────────────┬──────────┘
           │         │             │
           │         │             │
           ▼         ▼             ▼
    ┌─────────────────────────────────────────┐
    │          Amazon CloudWatch              │
    │                                         │
    │  • ECS Metrics                          │
    │  • CloudWatch Logs                      │
    │  • CloudWatch Dashboard                 │
    │  • CloudWatch Alarms                    │
    └───────────────┬─────────────────────────┘
                    │
                    ▼
             ┌──────────────┐
             │ Amazon SNS   │
             └──────┬───────┘
                    │
                    ▼
              Email Alert

---

## 📈 Metrics Monitoring

CloudWatch collects ECS service-level metrics for the application.

The dashboard monitors:

### Frontend

    three-tier-frontend-service

Metrics:

- CPUUtilization
- MemoryUtilization
- LiveTaskCount

### Backend

    three-tier-backend-service

Metrics:

- CPUUtilization
- MemoryUtilization
- LiveTaskCount

### MongoDB

    three-tier-mongodb-service-2

Metrics:

- CPUUtilization
- MemoryUtilization
- LiveTaskCount

These metrics are combined into the CloudWatch dashboard:

    Three-Tier-App-Monitoring

---

## 📋 Log Monitoring Architecture

The backend container is configured to send logs to Amazon CloudWatch Logs using the `awslogs` log driver.

    Backend Container
          │
          │ awslogs
          ▼
    CloudWatch Log Group
          │
          ▼
    /ecs/three-tier-backend
          │
          ▼
    ECS Log Streams
          │
          ▼
    CloudWatch Dashboard

The backend logs include important application events such as:

    Backend API running on port 5000

and:

    Connected to MongoDB successfully!

---

## 🚨 Alarm Architecture

Three CPU utilization alarms were configured.

    ┌─────────────────────────────┐
    │ ECS CPUUtilization Metrics  │
    └──────────────┬──────────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ CloudWatch      │
          │ Alarm           │
          └────────┬────────┘
                   │
             CPU > 70%
                   │
                   ▼
          ┌─────────────────┐
          │ Amazon SNS      │
          │ Notification    │
          └────────┬────────┘
                   │
                   ▼
              Email Alert

Configured alarms:

    Backend-CPU-High
    Frontend-CPU-High
    MongoDB-CPU-High

Each alarm monitors CPU utilization above 70% for the configured evaluation period of 5 minutes.

---

## 📧 Notification Flow

The notification architecture is:

    ECS Service
         │
         ▼
    CPUUtilization Metric
         │
         ▼
    CloudWatch Alarm
         │
         │ Threshold exceeded
         ▼
    SNS Topic
         │
         ▼
    ThreeTierMonitoringAlerts
         │
         ▼
    Confirmed Email Subscription
         │
         ▼
    Email Notification

The SNS topic used for monitoring alerts is:

    ThreeTierMonitoringAlerts

---

## 📊 Monitoring Dashboard Architecture

The CloudWatch dashboard provides a centralized monitoring view.

Dashboard name:

    Three-Tier-App-Monitoring

The dashboard contains:

### ECS Overall Resource Monitoring

Displays:

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

Displays the status of:

- Backend-CPU-High
- Frontend-CPU-High
- MongoDB-CPU-High

### Backend Logs

Displays recent events from:

    /ecs/three-tier-backend

---

## 🔄 Complete Monitoring Flow

The complete architecture can be represented as:

    User
     │
     ▼
    Nginx Frontend
     │
     ▼
    Node.js Backend
     │
     ▼
    MongoDB
     
     │
     │ ECS Metrics
     ▼
    CloudWatch
     │
     ├──────────────► Dashboard
     │
     ├──────────────► Logs
     │
     └──────────────► Alarms
                         │
                         ▼
                    Amazon SNS
                         │
                         ▼
                    Email Alert

---

## 🔐 Security and Access

The application services run within the AWS networking environment using ECS task networking.

AWS IAM controls access to AWS resources.

CloudWatch receives metrics and logs from the ECS deployment through the configured ECS and task execution configuration.

The monitoring setup does not require direct access to the application containers for normal metric and log collection.

---

## 🎯 Architecture Benefits

This monitoring architecture provides:

- Centralized ECS monitoring
- Real-time resource visibility
- Application log collection
- CPU utilization monitoring
- Automated threshold-based alerts
- Email notifications
- Service health visibility
- Easier troubleshooting
- Centralized operational dashboard

---

## 📌 Final Architecture Summary

The project combines a Dockerized three-tier application with AWS observability services.

The application runs as three ECS Fargate services:

    Frontend → Backend → MongoDB

AWS CloudWatch provides:

    Metrics
    Logs
    Dashboard
    Alarms

Amazon SNS provides:

    Email Notifications

This architecture provides a centralized monitoring solution for observing the health and resource utilization of the deployed three-tier application.
