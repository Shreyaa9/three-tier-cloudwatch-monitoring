# Troubleshooting

## 📌 Overview

This document describes common issues encountered while configuring CloudWatch monitoring for the three-tier application and the steps used to resolve or verify them.

The troubleshooting process focused on:

- ECS services
- CloudWatch metrics
- CloudWatch Logs
- ECS task definition logging
- CloudWatch alarms
- SNS notifications
- Dashboard monitoring

---

## 🔧 Issue 1 — Backend Logs Not Appearing in CloudWatch

### Problem

The backend application was running, but application logs were not initially available in CloudWatch Logs.

### Cause

CloudWatch logging had not yet been configured for the backend container.

### Solution

The backend ECS task definition was updated to use CloudWatch log collection.

The logging configuration used:

    Log Group:
    /ecs/three-tier-backend

    Region:
    us-east-1

    Stream Prefix:
    ecs

A new backend task definition revision was created and deployed.

### Verification

After deployment, new log streams appeared in:

    CloudWatch
    → Logs
    → /ecs/three-tier-backend

The logs contained messages such as:

    Backend API running on port 5000

    Connected to MongoDB successfully!

### Status

    RESOLVED

---

## 🔧 Issue 2 — CloudWatch Log Group or Streams Not Visible

### Problem

After enabling logging, the expected log streams were not immediately visible.

### Possible Causes

- Backend task had not restarted after the logging configuration change.
- The wrong log group was selected.
- The selected time range did not include recent logs.
- The ECS task was not running.

### Solution

The following checks were performed:

1. Verified the backend ECS service was running.
2. Verified the task definition contained the CloudWatch logging configuration.
3. Verified the log group:

    /ecs/three-tier-backend

4. Checked recent log streams.
5. Selected an appropriate CloudWatch time range.

### Status

    RESOLVED

---

## 🔧 Issue 3 — Backend Logging Task Definition Deployment

### Problem

The CloudWatch logging configuration required a new ECS task definition revision.

### Solution

A new task definition revision was created for the backend service.

The deployed revision was:

    three-tier-backend:5

The backend ECS service was then updated to use the new revision.

### Verification

The ECS service successfully deployed the updated task definition and the backend task started normally.

CloudWatch logs were subsequently generated.

### Status

    RESOLVED

---

## 🔧 Issue 4 — CloudWatch Dashboard Showing No Data

### Problem

A CloudWatch dashboard may initially show no metric data if the wrong service, metric, or time range is selected.

### Checks Performed

The following were verified:

    ECS Cluster:
    three-tier-cluster

    Frontend Service:
    three-tier-frontend-service

    Backend Service:
    three-tier-backend-service

    MongoDB Service:
    three-tier-mongodb-service-2

Metrics checked included:

    CPUUtilization
    MemoryUtilization
    LiveTaskCount

### Solution

The dashboard widgets were configured using the correct ECS cluster and service dimensions.

The time range was also adjusted to display recent monitoring data.

### Status

    RESOLVED

---

## 🔧 Issue 5 — Alarm Not Showing the Expected State

### Problem

A CloudWatch alarm may not immediately change state after creation.

### Cause

The alarm evaluates the configured metric over its specified evaluation period.

For this project, the CPU alarms use:

    Statistic:
    Average

    Threshold:
    Greater than 70%

    Period:
    5 minutes

### Solution

The alarm configuration was checked to ensure that:

- The correct ECS service was selected.
- CPUUtilization was selected.
- The threshold was set to 70%.
- The evaluation period was configured correctly.
- The SNS notification topic was selected.

### Verification

The alarms were successfully created and displayed an:

    OK

state during final verification.

### Status

    RESOLVED

---

## 🔧 Issue 6 — SNS Email Notification Not Received

### Problem

SNS email notifications require a confirmed email subscription.

### Checks Performed

The SNS topic was verified:

    ThreeTierMonitoringAlerts

The email subscription status was checked.

### Solution

The subscription confirmation email was opened and the subscription was confirmed.

### Verification

The SNS subscription showed a confirmed status and the monitoring notification email was received successfully.

### Status

    RESOLVED

---

## 🔧 Issue 7 — Application Working but Monitoring Data Delayed

### Problem

CloudWatch metrics, logs, or alarm states may not update immediately after an application action.

### Cause

CloudWatch monitoring data and alarm evaluations are not necessarily instantaneous.

### Solution

The dashboard and log views were refreshed after allowing sufficient time for new monitoring data to appear.

The relevant CloudWatch time range was also checked.

### Status

    VERIFIED

---

## 🔧 Issue 8 — Backend Logs Show MongoDB Connection Messages

### Observation

The backend CloudWatch logs contained:

    Connected to MongoDB successfully!

### Interpretation

This confirms that the backend was able to establish a connection with MongoDB during the tested deployment.

The backend also reported:

    Backend API running on port 5000

This confirms that the backend application started successfully.

### Status

    VERIFIED

---

## 🔧 Issue 9 — Application Works but CloudWatch Logs Are Missing

### Problem

The live application can function correctly even when application-level CloudWatch logging has not been configured.

### Explanation

Application functionality and log collection are separate concerns.

The application can communicate between:

    Frontend
        ↓
    Backend
        ↓
    MongoDB

while CloudWatch application logs require an additional ECS logging configuration.

### Solution

CloudWatch logging was explicitly configured for the backend container.

### Status

    RESOLVED

---

## 🔍 General Troubleshooting Checklist

When monitoring data is missing, check the following in order:

### 1. Check ECS

Verify:

    ECS Cluster
    → three-tier-cluster

Confirm that the required services and tasks are running.

### 2. Check Task Definition

Verify that the deployed task definition contains the required CloudWatch logging configuration.

For the backend:

    Log Group:
    /ecs/three-tier-backend

### 3. Check CloudWatch Logs

Open:

    CloudWatch
    → Logs
    → /ecs/three-tier-backend

Check recent log streams and log events.

### 4. Check Metrics

Verify:

    CPUUtilization
    MemoryUtilization
    LiveTaskCount

Make sure the correct ECS service and cluster dimensions are selected.

### 5. Check Alarms

Verify:

    Backend-CPU-High
    Frontend-CPU-High
    MongoDB-CPU-High

Check the current alarm state and configuration.

### 6. Check SNS

Verify:

    Topic:
    ThreeTierMonitoringAlerts

Confirm that the email subscription is confirmed.

### 7. Check Dashboard

Open:

    Three-Tier-App-Monitoring

Verify that the metric, alarm, and log widgets are displaying current information.

---

## 🛠️ Troubleshooting Flow

The recommended troubleshooting sequence is:

    Application
        ↓
    ECS Service
        ↓
    Task Definition
        ↓
    CloudWatch Metrics / Logs
        ↓
    CloudWatch Alarms
        ↓
    SNS Notification
        ↓
    CloudWatch Dashboard

This helps identify whether an issue is related to the application, ECS deployment, monitoring configuration, alarm evaluation, or notification system.

---

## ⚠️ Important Note

This monitoring project reuses the existing three-tier application.

Therefore, monitoring issues should be investigated without unnecessarily deleting or modifying the existing application infrastructure.

The ECS services, ECR repositories, Cloud Map configuration, and application networking should only be changed when required for troubleshooting.

---

## ✅ Final Status

All major monitoring components were successfully verified:

    ✓ ECS services running
    ✓ CloudWatch metrics available
    ✓ Backend CloudWatch logging working
    ✓ CloudWatch log streams available
    ✓ Log analytics working
    ✓ Backend CPU alarm configured
    ✓ Frontend CPU alarm configured
    ✓ MongoDB CPU alarm configured
    ✓ SNS email subscription confirmed
    ✓ CloudWatch dashboard operational
    ✓ Live application tested successfully

The monitoring system is operational and ready for demonstration.
