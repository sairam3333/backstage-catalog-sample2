# Deployment Guide

## Overview

The Edutive School Management System consists of multiple microservices deployed in a cloud-native environment.

## Technology Stack

| Component        | Technology     |
| ---------------- | -------------- |
| Frontend         | Angular        |
| Backend          | Spring Boot    |
| Database         | MongoDB        |
| Messaging        | Kafka          |
| Notifications    | Node.js        |
| CI/CD            | GitHub Actions |
| Containerization | Docker         |
| Orchestration    | Kubernetes     |
| Cloud Platform   | Google Cloud   |

---

## Deployment Architecture

```text
                        +-------------------+
                        |    End Users      |
                        +---------+---------+
                                  |
                                  v
                        +-------------------+
                        |   Load Balancer   |
                        +---------+---------+
                                  |
                                  v
                    +---------------------------+
                    |   Angular Frontend        |
                    |   (edutive-frontend)      |
                    +---------------------------+
                                  |
         -------------------------------------------------
         |             |             |             |      |
         v             v             v             v      v

 +---------------+ +---------------+ +---------------+ +-----------------+ +---------------+
 | Student Svc   | | Teacher Svc   | | Assessment    | | Report Card Svc | | Email Svc     |
 | Spring Boot   | | Spring Boot   | | Spring Boot   | | Spring Boot     | | Node.js       |
 +-------+-------+ +-------+-------+ +-------+-------+ +--------+--------+ +-------+-------+
         |                 |                 |                  |                  |
         +-----------------+-----------------+------------------+------------------+
                                           |
                                           v
                                   +---------------+
                                   |   MongoDB     |
                                   +---------------+

                                   +---------------+
                                   |    Kafka      |
                                   +---------------+
```

---

## Prerequisites

Before deployment, ensure the following are available:

* Docker
* Kubernetes Cluster (GKE/EKS/AKS)
* MongoDB Instance
* Kafka Cluster
* GitHub Actions Secrets
* Container Registry

---

## Environment Variables

### Student Service

```bash
MONGO_URI=mongodb://mongodb:27017/edutive
SPRING_PROFILES_ACTIVE=prod
```

### Report Card Service

```bash
MONGO_URI=mongodb://mongodb:27017/edutive
KAFKA_BOOTSTRAP_SERVERS=kafka:9092
SPRING_PROFILES_ACTIVE=prod
```

### Email Service

```bash
MONGO_URI=mongodb://mongodb:27017/edutive
KAFKA_BOOTSTRAP_SERVERS=kafka:9092
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
```

---

## Docker Build

Build Docker images:

```bash
docker build -t student-service .
docker build -t teacher-service .
docker build -t assessment-service .
docker build -t report-card-service .
docker build -t email-service .
```

Push images to registry:

```bash
docker push gcr.io/project-id/student-service:latest
docker push gcr.io/project-id/teacher-service:latest
```

---

## Kubernetes Deployment

Deploy services:

```bash
kubectl apply -f k8s/
```

Verify deployments:

```bash
kubectl get pods
kubectl get services
```

---

## CI/CD Pipeline

Deployment flow:

1. Developer pushes code to GitHub.
2. GitHub Actions builds Docker images.
3. Images are pushed to Container Registry.
4. Kubernetes deployments are updated.
5. Application becomes available to users.

---

## Monitoring

Recommended tools:

* Prometheus
* Grafana
* Loki
* Backstage

---

## Rollback

Rollback to previous deployment:

```bash
kubectl rollout undo deployment/student-service
```

Check rollout status:

```bash
kubectl rollout status deployment/student-service
```
