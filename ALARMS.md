# CloudWatch Alarms and Notifications

## 📌 Overview

This document describes the Amazon CloudWatch alarms and Amazon SNS notification system configured for the three-tier application.

The monitoring setup uses CPU utilization thresholds to detect increased resource usage across the Frontend, Backend, and MongoDB ECS services.

When CPU utilization exceeds the configured threshold, CloudWatch changes the alarm state and sends a notification through Amazon SNS.

---

## 🚨 Alarm Configuration

Three CloudWatch CPU utilization alarms were configured.

| Service | Alarm Name | Metric | Threshold | Period |
|---|---|---|---|---|
| Frontend | `Frontend-CPU-High` | CPUUtilization | > 70% | 5 minutes |
| Backend | `Backend-CPU-High` | CPUUtilization | > 70% | 5 minutes |
| MongoDB | `MongoDB-CPU-High` | CPUUtilization | > 70% | 5 minutes |

All alarms use the **Average** statistic for CPU utilization.

---

## 1. Backend CPU Alarm

### Alarm Name

    Backend-CPU-High

### Configuration

    Service:
    three-tier-backend-service

    Metric:
    CPUUtilization

    Statistic:
    Average

    Threshold:
    Greater than 70%

    Evaluation Period:
    5 minutes

The alarm monitors CPU utilization of the backend ECS service.

If backend CPU utilization exceeds 70% for the configured evaluation period, the alarm can transition to the `In alarm` state.

---

## 2. Frontend CPU Alarm

### Alarm Name

    Frontend-CPU-High

### Configuration

    Service:
    three-tier-frontend-service

    Metric:
    CPUUtilization

    Statistic:
    Average

    Threshold:
    Greater than 70%

    Evaluation Period:
    5 minutes

The alarm monitors CPU utilization of the frontend ECS service.

If frontend CPU utilization exceeds 70% for the configured evaluation period, the alarm can transition to the `In alarm` state.

---

## 3. MongoDB CPU Alarm

### Alarm Name

    MongoDB-CPU-High

### Configuration

    Service:
    three-tier-mongodb-service-2

    Metric:
    CPUUtilization

    Statistic:
    Average

    Threshold:
    Greater than 70%

    Evaluation Period:
    5 minutes

The alarm monitors CPU utilization of the MongoDB ECS service.

If MongoDB CPU utilization exceeds 70% for the configured evaluation period, the alarm can transition to the `In alarm` state.

---

## 📧 SNS Notification Configuration

An Amazon SNS topic was created to handle CloudWatch alarm notifications.

### SNS Topic

    ThreeTierMonitoringAlerts

A confirmed email subscription was configured for the SNS topic.

The same SNS topic is used by all three CloudWatch alarms.

---

## 🔄 Alert Flow

The monitoring alert flow is:

    ECS Service
         ↓
    CPUUtilization Metric
         ↓
    CloudWatch Alarm
         ↓
    CPU > 70%
         ↓
    In Alarm State
         ↓
    ThreeTierMonitoringAlerts
         ↓
    Email Notification

---

## 🟢 Alarm States

CloudWatch alarms can have different states.

### OK

The monitored metric is within the configured threshold.

Current project state:

    Backend-CPU-High   → OK
    Frontend-CPU-High  → OK
    MongoDB-CPU-High   → OK

### In Alarm

The monitored metric has exceeded the configured threshold according to the alarm evaluation settings.

In this state, the configured SNS notification action can send an email alert.

### Insufficient Data

CloudWatch does not have enough metric data to determine the alarm state.

---

## 📊 Alarm Monitoring Through Dashboard

The alarm states are displayed in the CloudWatch dashboard:

    Three-Tier-App-Monitoring

The dashboard contains an:

    Alarm Status Overview

widget showing the current state of all three alarms.

This provides a centralized view of the health of the monitored ECS services.

---

## 🧪 Alarm Verification

The configured alarms were verified through the CloudWatch Alarms console.

The following were confirmed:

- Backend CPU alarm exists.
- Frontend CPU alarm exists.
- MongoDB CPU alarm exists.
- All alarms use CPUUtilization.
- Threshold is configured at 70%.
- Evaluation period is 5 minutes.
- All alarms are connected to the SNS notification topic.
- SNS email subscription is confirmed.
- All three alarms are currently in the `OK` state.

---

## 📸 Evidence

The following screenshots provide evidence of the alarm configuration:

    screenshots/03-cloudwatch-alarms.png
    screenshots/04-backend-alarm-details.png
    screenshots/05-frontend-alarm-details.png
    screenshots/06-mongodb-alarm-details.png
    screenshots/07-sns-notification.png

---

## 🎯 Purpose of the Alarm System

The alarm system helps identify abnormal CPU utilization in the three-tier application.

It provides:

- Proactive monitoring
- Threshold-based detection
- Centralized alarm visibility
- Email notifications
- Faster response to resource utilization issues
- Better operational awareness of ECS services

---

## ✅ Final Status

The CloudWatch alarm and notification system was successfully configured.

    Frontend-CPU-High   → OK
    Backend-CPU-High    → OK
    MongoDB-CPU-High    → OK

The alarms are integrated with:

    CloudWatch
         ↓
    Amazon SNS
         ↓
    Confirmed Email Subscription

This completes the CPU-based alerting component of the three-tier application monitoring project.
