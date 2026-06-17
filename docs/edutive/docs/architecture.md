# Architecture Overview

## Edutive School Management System

Edutive is a microservices-based school management platform designed to manage student information, teachers, assessments, report cards, and notifications.

## High-Level Architecture

```text
+----------------------+
| Angular Frontend     |
| (edutive-frontend)   |
+----------+-----------+
           |
           |
           +------------------------------------+
           |            |            |          |
           v            v            v          v
+----------------+ +----------------+ +----------------+ +------------------+
| Student Service| | Teacher Service| | Assessment Svc| | Report Card Svc |
+----------------+ +----------------+ +----------------+ +------------------+
         |                 |                  |                  |
         +-----------------+------------------+------------------+
                                   |
                                   v
                           +---------------+
                           |   MongoDB     |
                           +---------------+

Report Card Service
          |
          v
+------------------+
| Kafka Service    |
+------------------+

Email Service consumes Kafka events and sends notifications.
```

## Components

### Angular Frontend

* Technology: Angular
* Type: Web Application
* Responsibilities:

  * User Interface
  * API Consumption
  * Authentication and Authorization

### Student Service

* Technology: Spring Boot
* Database: MongoDB
* Responsibilities:

  * Student Profile Management
  * Student Enrollment
  * Student Records

### Teacher Service

* Technology: Spring Boot
* Database: MongoDB
* Responsibilities:

  * Teacher Management
  * Class Assignment

### Assessment Service

* Technology: Spring Boot
* Database: MongoDB
* Responsibilities:

  * Exams
  * Assessments
  * Grading

### Report Card Service

* Technology: Spring Boot
* Database: MongoDB
* Messaging: Kafka
* Responsibilities:

  * Report Generation
  * Grade Aggregation
  * Publish Events

### Email Service

* Technology: Node.js
* Responsibilities:

  * Email Notifications
  * Event Processing

## Shared Resources

### MongoDB

Shared database used by:

* Student Service
* Teacher Service
* Assessment Service
* Report Card Service
* Email Service

### Kafka

Messaging platform used for asynchronous communication between services.

## API Communication

* Angular Frontend consumes REST APIs exposed by all backend services.
* Services communicate asynchronously using Kafka events.

## Deployment Architecture

* Frontend: Angular
* Backend: Spring Boot Microservices
* Database: MongoDB
* Messaging: Kafka
* CI/CD: GitHub Actions
* Deployment Platform: Kubernetes / Cloud Run

## Ownership

**Owner:** Whitefield Global School
