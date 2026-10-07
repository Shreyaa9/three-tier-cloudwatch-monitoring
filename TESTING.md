# Testing and Verification

## 📌 Overview

This document describes the testing performed to verify the monitoring setup for the three-tier application.

The testing covered:

- Application functionality
- ECS service health
- CloudWatch metrics
- CloudWatch Logs
- CloudWatch alarms
- Amazon SNS notifications
- Monitoring dashboard

---

## 🏗️ Test Environment

The application consists of three ECS services:

    three-tier-frontend-service
    three-tier-backend-service
    three-tier-mongodb-service-2

All services run on:

    ECS Cluster:
    three-tier-cluster

    Launch Type:
    AWS Fargate

AWS Region:

    us-east-1

---

## 🧪 Test 1 — Application Availability

### Objective

Verify that the deployed three-tier application is accessible through the frontend.

### Procedure

1. Open the live application in a web browser.
2. Verify that the frontend loads successfully.
3. Confirm that the application interface is displayed.

### Expected Result

The application should load successfully and display the To-Do application interface.

### Result

    PASS

The live application was successfully accessed during testing.

---

## 🧪 Test 2 — Add Task

### Objective

Verify that the application can create a new task.

### Procedure

1. Open the live application.
2. Enter a test task.
3. Click the Add Task button.
4. Verify that the task appears in the application.

### Expected Result

The new task should be added successfully.

### Result

    PASS

A test task was successfully added through the live application.

---

## 🧪 Test 3 — Complete Task

### Objective

Verify that an existing task can be completed.

### Procedure

1. Select an existing task.
2. Use the Complete Task option.
3. Verify that the task status changes.

### Expected Result

The selected task should be marked as completed.

### Result

    PASS

The task was successfully completed during testing.

---

## 🧪 Test 4 — Application Persistence

### Objective

Verify that application data remains available after a page refresh.

### Procedure

1. Add or complete a task.
2. Refresh the browser page.
3. Check the task list.

### Expected Result

The application should continue to display the existing task data.

### Result

    PASS

Application data remained available after page refresh during testing.

---

## 🧪 Test 5 — ECS Service Health

### Objective

Verify that all three ECS services are running correctly.

### Procedure

Open:

    AWS Console
    → ECS
    → Clusters
    → three-tier-cluster

Verify the services:

    three-tier-frontend-service
    three-tier-backend-service
    three-tier-mongodb-service-2

### Expected Result

All required services should be active with running tasks.

### Result

    PASS

The ECS cluster showed:

    3 Active Services
    3 Running Tasks

---

## 🧪 Test 6 — CloudWatch Metrics

### Objective

Verify that ECS monitoring metrics are available in CloudWatch.

### Metrics Tested

For the three ECS services:

    CPUUtilization
    MemoryUtilization
    LiveTaskCount

### Procedure

1. Open CloudWatch.
2. Open the `Three-Tier-App-Monitoring` dashboard.
3. Review the ECS Overall Resource Monitoring widget.
4. Verify that metric data is being displayed.

### Expected Result

CloudWatch should display metric data for the monitored ECS services.

### Result

    PASS

ECS monitoring metrics were successfully displayed on the CloudWatch dashboard.

---

## 🧪 Test 7 — Backend CloudWatch Logs

### Objective

Verify that backend container logs are being sent to CloudWatch.

### Log Group

    /ecs/three-tier-backend

### Procedure

1. Open CloudWatch.
2. Navigate to Logs.
3. Open `/ecs/three-tier-backend`.
4. Open a recent log stream.
5. Review the log events.

### Expected Result

Backend application log events should be available.

### Result

    PASS

The backend logs were successfully received by CloudWatch.

Verified messages included:

    Backend API running on port 5000

and:

    Connected to MongoDB successfully!

---

## 🧪 Test 8 — CloudWatch Log Analytics

### Objective

Verify that backend logs can be searched and analyzed.

### Procedure

1. Open the CloudWatch log analytics interface.
2. Select:

    /ecs/three-tier-backend

3. Select an appropriate time range.
4. Run the log query.
5. Review the returned events.

### Expected Result

Matching backend log events should be returned.

### Result

    PASS

The log analytics query successfully returned backend application events.

---

## 🧪 Test 9 — Backend CPU Alarm

### Objective

Verify that the backend CPU alarm is configured correctly.

### Alarm

    Backend-CPU-High

### Configuration

    Metric:
    CPUUtilization

    Statistic:
    Average

    Threshold:
    Greater than 70%

    Period:
    5 minutes

### Expected Result

The alarm should remain in the `OK` state while CPU utilization is within the configured threshold.

### Result

    PASS

The `Backend-CPU-High` alarm was successfully configured and was in the `OK` state during verification.

---

## 🧪 Test 10 — Frontend CPU Alarm

### Objective

Verify that the frontend CPU alarm is configured correctly.

### Alarm

    Frontend-CPU-High

### Configuration

    Metric:
    CPUUtilization

    Statistic:
    Average

    Threshold:
    Greater than 70%

    Period:
    5 minutes

### Expected Result

The alarm should remain in the `OK` state while CPU utilization is within the configured threshold.

### Result

    PASS

The `Frontend-CPU-High` alarm was successfully configured and was in the `OK` state during verification.

---

## 🧪 Test 11 — MongoDB CPU Alarm

### Objective

Verify that the MongoDB CPU alarm is configured correctly.

### Alarm

    MongoDB-CPU-High

### Configuration

    Metric:
    CPUUtilization

    Statistic:
    Average

    Threshold:
    Greater than 70%

    Period:
    5 minutes

### Expected Result

The alarm should remain in the `OK` state while CPU utilization is within the configured threshold.

### Result

    PASS

The `MongoDB-CPU-High` alarm was successfully configured and was in the `OK` state during verification.

---

## 🧪 Test 12 — SNS Notification

### Objective

Verify that the SNS notification system is configured correctly.

### SNS Topic

    ThreeTierMonitoringAlerts

### Procedure

1. Open Amazon SNS.
2. Open the monitoring topic.
3. Verify the email subscription status.
4. Confirm that the subscription is confirmed.

### Expected Result

The email subscription should show a confirmed status.

### Result

    PASS

The SNS email subscription was successfully confirmed.

---

## 🧪 Test 13 — CloudWatch Dashboard

### Objective

Verify that all monitoring components are available from the centralized dashboard.

### Dashboard

    Three-Tier-App-Monitoring

### Components Verified

    ECS Overall Resource Monitoring
    Alarm Status Overview
    Backend Application Logs

### Expected Result

The dashboard should display monitoring information from the application.

### Result

    PASS

The dashboard successfully displayed ECS metrics, alarm states, and backend CloudWatch logs.

---

## 🧪 Test 14 — Backend Logging Deployment

### Objective

Verify that the backend ECS service is running the task definition revision containing CloudWatch logging configuration.

### Configuration

    Service:
    three-tier-backend-service

    Task Definition:
    three-tier-backend:5

### Expected Result

The new task definition should deploy successfully with one running task.

### Result

    PASS

The backend service successfully deployed with the CloudWatch logging configuration.

---

## 📊 Final Test Summary

| Test | Result |
|---|---|
| Application Availability | PASS |
| Add Task | PASS |
| Complete Task | PASS |
| Application Persistence | PASS |
| ECS Service Health | PASS |
| CloudWatch Metrics | PASS |
| Backend CloudWatch Logs | PASS |
| Log Analytics | PASS |
| Backend CPU Alarm | PASS |
| Frontend CPU Alarm | PASS |
| MongoDB CPU Alarm | PASS |
| SNS Notification Configuration | PASS |
| CloudWatch Dashboard | PASS |
| Backend Logging Deployment | PASS |

---

## 📸 Testing Evidence

The following screenshots provide evidence of the monitoring implementation:

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

## ✅ Final Verification

The complete monitoring implementation was successfully tested.

The final verification confirmed:

    ✓ Three-tier application is accessible
    ✓ Application operations work correctly
    ✓ ECS services are running
    ✓ ECS metrics are available
    ✓ Backend logs are available in CloudWatch
    ✓ Log analytics returns application events
    ✓ Backend CPU alarm is configured
    ✓ Frontend CPU alarm is configured
    ✓ MongoDB CPU alarm is configured
    ✓ All three alarms are currently OK
    ✓ SNS email subscription is confirmed
    ✓ CloudWatch dashboard is operational
    ✓ Backend logging configuration is deployed

---

## 🎯 Conclusion

The testing confirms that the three-tier application can be monitored using AWS CloudWatch.

The implementation successfully provides:

    Metrics
    Logs
    Dashboard
    Alarms
    SNS Notifications
    Application Health Monitoring

The monitoring setup is ready for demonstration and documentation.
