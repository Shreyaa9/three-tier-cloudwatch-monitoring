# CloudWatch Monitoring Guide

## 📌 Overview

This document explains how the three-tier application is monitored using Amazon CloudWatch.

The monitoring setup provides centralized visibility into:

- ECS service metrics
- CPU utilization
- Memory utilization
- Running task count
- Application logs
- CloudWatch alarms
- SNS notifications
- Overall application health

The monitoring environment is built on top of the existing three-tier application deployed using Amazon ECS and AWS Fargate.

---

## 🏗️ Monitored Application

The application consists of three ECS services:

    Frontend:
    three-tier-frontend-service

    Backend:
    three-tier-backend-service

    Database:
    three-tier-mongodb-service-2

The services run on the ECS cluster:

    three-tier-cluster

---

## 📊 CloudWatch Dashboard

A centralized CloudWatch dashboard was created:

    Three-Tier-App-Monitoring

The dashboard provides a single view of the application's monitoring data.

It contains:

1. ECS Overall Resource Monitoring
2. Alarm Status Overview
3. Backend Application Logs

---

## 📈 ECS Metrics

CloudWatch monitors the following metrics for each ECS service.

### CPU Utilization

`CPUUtilization` shows the CPU usage of the ECS service.

It helps identify:

- High CPU consumption
- Potential resource pressure
- Increased application workload
- Services approaching their configured capacity

### Memory Utilization

`MemoryUtilization` shows memory usage of the ECS service.

It helps identify:

- Increasing memory consumption
- Potential memory pressure
- Unusual resource usage

### Live Task Count

`LiveTaskCount` provides visibility into the number of running ECS tasks.

It helps verify:

- Whether the desired task is running
- Whether tasks are available
- Changes in service availability

---

## 🔍 Services Being Monitored

### Frontend

    Service:
    three-tier-frontend-service

    Metrics:
    CPUUtilization
    MemoryUtilization
    LiveTaskCount

### Backend

    Service:
    three-tier-backend-service

    Metrics:
    CPUUtilization
    MemoryUtilization
    LiveTaskCount

### MongoDB

    Service:
    three-tier-mongodb-service-2

    Metrics:
    CPUUtilization
    MemoryUtilization
    LiveTaskCount

---

## 📋 CloudWatch Logs

The backend ECS container sends application logs to CloudWatch Logs.

### Log Group

    /ecs/three-tier-backend

### Region

    us-east-1

### Stream Prefix

    ecs

The logging configuration uses the AWS `awslogs` log driver.

The logs provide application-level information that can be used for monitoring and troubleshooting.

Example messages include:

    Backend API running on port 5000

and:

    Connected to MongoDB successfully!

---

## 🔎 Log Analytics

CloudWatch Logs Insights / Log Analytics can be used to search and analyze backend logs.

The backend log group:

    /ecs/three-tier-backend

can be selected for log analysis.

This allows recent log events to be reviewed from a centralized interface.

The monitoring project verified that backend logs were being successfully received by CloudWatch.

---

## 🚨 CPU Alarm Monitoring

CPU utilization alarms were configured for all three ECS services.

### Backend

    Backend-CPU-High

### Frontend

    Frontend-CPU-High

### MongoDB

    MongoDB-CPU-High

Each alarm uses:

    Metric:
    CPUUtilization

    Statistic:
    Average

    Threshold:
    Greater than 70%

    Evaluation Period:
    5 minutes

---

## 📧 SNS Notification Monitoring

The alarms are connected to the SNS topic:

    ThreeTierMonitoringAlerts

A confirmed email subscription is associated with the topic.

The notification workflow is:

    ECS CPU Metric
          ↓
    CloudWatch Alarm
          ↓
    Threshold Exceeded
          ↓
    Amazon SNS
          ↓
    Email Notification

---

## 🟢 Alarm Status

The CloudWatch dashboard provides an Alarm Status Overview.

During final project verification, the alarms were in the `OK` state:

    Backend-CPU-High    → OK
    Frontend-CPU-High   → OK
    MongoDB-CPU-High    → OK

The `OK` state indicates that the monitored CPU utilization was within the configured threshold at the time of verification.

---

## 🧪 Monitoring Verification

The monitoring system was verified using the live application.

The following application operations were performed:

1. Opened the deployed application.
2. Added a new task.
3. Completed the task.
4. Refreshed the application.
5. Verified that the application continued to work.

CloudWatch monitoring was then checked to verify that:

- ECS metrics were available.
- Backend logs were being generated.
- Backend logs were visible in CloudWatch.
- CloudWatch dashboard widgets were displaying monitoring information.
- All three CPU alarms were in the `OK` state.

---

## 🔄 Complete Monitoring Workflow

The complete monitoring workflow is:

    Three-Tier Application
            │
            ▼
    ECS Fargate Services
            │
       ┌────┴────┐
       │         │
       ▼         ▼
    Metrics     Logs
       │         │
       ▼         ▼
    CloudWatch CloudWatch
       │         │
       └────┬────┘
            │
            ▼
    CloudWatch Dashboard
            │
            ▼
      CPU Threshold
            │
            ▼
      CloudWatch Alarm
            │
            ▼
          SNS
            │
            ▼
      Email Notification

---

## 🎯 Monitoring Objectives

The CloudWatch monitoring implementation was designed to achieve the following:

- Monitor ECS resource utilization.
- Observe CPU consumption.
- Observe memory consumption.
- Track running ECS tasks.
- Collect backend application logs.
- Centralize monitoring information in a dashboard.
- Detect high CPU utilization.
- Send email notifications for alarm conditions.
- Support application troubleshooting.
- Provide visibility into application health.

---

## 🛠️ Monitoring Benefits

The implemented monitoring solution provides:

### Centralized Visibility

The CloudWatch dashboard provides a single location to view application metrics, alarms, and backend logs.

### Early Detection

CPU alarms can identify increased resource utilization before it becomes a larger operational issue.

### Log-Based Troubleshooting

CloudWatch Logs provides access to backend application events without requiring direct access to the running container.

### Automated Notifications

Amazon SNS allows alarm events to be delivered through email notifications.

### Operational Monitoring

ECS metrics and task counts provide visibility into the running state of the application services.

---

## 📸 Monitoring Evidence

The monitoring implementation is supported by screenshots stored in the `screenshots/` directory.

Relevant evidence includes:

    01-cloudwatch-dashboard.png
    02-backend-cloudwatch-logs.png
    03-cloudwatch-alarms.png
    04-backend-alarm-details.png
    05-frontend-alarm-details.png
    06-mongodb-alarm-details.png
    07-sns-notification.png
    08-backend-cloudwatch-deployment.png
    09-ecs-cloudwatch-logging-config.png
    10-live-application-test.png
    11-ecs-cluster-overview.png
    12-cloudwatch-log-group.png

---

## ✅ Final Monitoring Status

The three-tier application monitoring setup was successfully implemented using Amazon CloudWatch.

The completed monitoring components are:

    ✓ ECS Metrics
    ✓ CPU Monitoring
    ✓ Memory Monitoring
    ✓ Live Task Monitoring
    ✓ CloudWatch Dashboard
    ✓ CloudWatch Logs
    ✓ CloudWatch Alarms
    ✓ Amazon SNS Notifications
    ✓ Email Subscription
    ✓ Application Monitoring Verification

The monitoring system provides centralized observability for the deployed three-tier application and supports both proactive alerting and application troubleshooting.
