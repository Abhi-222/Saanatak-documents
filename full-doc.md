# OT-Microservices | Full Stack | Documentation

<div align="center">
<br/>

<p>
  <img src="https://img.shields.io/badge/Go-EmployeeAPI-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Python-AttendanceAPI-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Java-SalaryAPI-orange?style=for-the-badge" />
</p>

<p>
  <img src="https://img.shields.io/badge/Flask-NotificationAPI-lightgrey?style=for-the-badge" />
  <img src="https://img.shields.io/badge/ScyllaDB-Database-blueviolet?style=for-the-badge" />
  <img src="https://img.shields.io/badge/PostgreSQL-AttendanceDB-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/NGINX-Frontend-red?style=for-the-badge" />
</p>

</div>

---

# Author Table

| Author | Created On | Version | Last Updated  | L0 Reviewer | L1 Reviewer    | L2 Reviewer |
|--------|------------|---------|---------------|-------------|----------------|-------------|
| Sahil  | 16-09-26   | 1.0     |  16-09-26     | `Vishal ? Divya M`   | `Aayush Verma` | `Mahesh Kumar / Varun` |


---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Application Overview](#2-application-overview)
3. [Application Components](#3-application-components)
4. [Overall Architecture](#4-overall-architecture)  
5. [Application Flow](#5-application-flow)
6. [Component Communication](#6-component-communication)
7. [Conclusion](#7-conclusion)
8. [Contact Information](#8-contact-information)
9. [References](#9-references)

---

# 1. Introduction

OT-Microservices is a microservices-based application consisting of a frontend, backend services, and supporting databases.
This document explains the **overall component architecture** and the **various flow diagrams** of the application — how it is structured, and how a request moves through it from user to database and back. Setup, run, and troubleshooting steps are covered separately under the "Run OT MS Application" documentation.

---

# 2. Application Overview

OT-Microservices follows a **microservices architecture**, where different business functionalities are handled by separate services.

The application contains:-
* React-based frontend
* NGINX reverse proxy
* Employee API (port 8080)
* Salary API (port 8082)
* Attendance API (port 8081)
* Notification Service (port 5000)
* ScyllaDB
* PostgreSQL
* Redis

Each component has a specific responsibility. The frontend provides the user interface, NGINX handles request routing, backend services process business operations, and databases store application data.

### High-Level Flow

<img width="878" height="636" alt="Screenshot 2026-09-14 223028" src="https://github.com/user-attachments/assets/99f7eaa8-c2bc-496d-b91b-1fe9ddc01ac2" />


---

# 3. Application Components

## 3.1 React Frontend

### What is it?

React is used to build the user-facing part of the application.

### Why is it used?

It provides the interface through which users can perform employee, salary, and attendance-related operations.

### How does it work?

The frontend accepts user actions and sends API requests to the application through NGINX.

```text
User
 ↓
React Frontend
 ↓
NGINX
 ↓
Backend Service
```

---

## 3.2 NGINX

### What is it?

NGINX is used as a **reverse proxy** between the frontend and backend services.

### Why is it used?

Instead of the frontend directly communicating with multiple backend services, NGINX provides a central routing layer.

### How does it work?

NGINX receives an API request, identifies the required service, forwards the request, and returns the service response to the frontend.

```text
Frontend
    |
    v
  NGINX
    |
    +----> Employee API
    |
    +----> Salary API
    |
    +----> Attendance API
```

---

## 3.3 Employee API

### What is it?

Employee API is a Go-based backend service using the Gin framework, running on **port 8080**.

### Why is it used?

It handles employee-related business operations.

### How does it work?

Employee requests are routed to the Employee API. The service processes the request and communicates with ScyllaDB when employee data is required.

```text
Frontend
   ↓
NGINX
   ↓
Employee API
   ↓
ScyllaDB
```

### Main Operations

* Create employee record
* Search employee records
* Retrieve employee information
* Employee health checks

---

## 3.4 Salary API

### What is it?

Salary API is a Java-based backend service built using Spring Boot, running on **port 8082**.

### Why is it used?

It manages salary-related operations independently from other business services.

### How does it work?

A salary request reaches the Salary API through NGINX. The API processes the request and communicates with ScyllaDB for salary data.

```text
Frontend
   ↓
NGINX
   ↓
Salary API
   ↓
ScyllaDB
```

### Main Operations

* Create salary records
* Search salary records
* Retrieve salary information

---

## 3.5 Attendance API

### What is it?

Attendance API is a Python-based backend service using Flask, running on **port 8081**.

### Why is it used?

It handles attendance-related operations.

### How does it work?

Attendance requests are routed through NGINX to the Attendance API. The API communicates with PostgreSQL for attendance data.

```text
Frontend
   ↓
NGINX
   ↓
Attendance API
   ↓
PostgreSQL
```

### Main Operations

* Create attendance records
* Search attendance records

---

## 3.6 Notification Service

### What is it?

Notification Service is a Python-based service, running on **port 5000**, responsible for sending email notifications to employees.

### Why is it used?

Notification functionality is kept separate from the main business APIs so it can run independently, without adding load to the request path used by end users.

### How does it work?

Unlike Employee, Salary and Attendance, the Notification Service is **not** invoked by the Frontend/NGINX. It is triggered independently (on a schedule or on demand), reads employee/salary/attendance data from the other services, and sends emails.

```text
Scheduler / Manual Trigger
   ↓
Notification Service
   ↓
Reads data from Employee API / Salary API / Attendance API
   ↓
Sends Email
```

---

## 3.7 ScyllaDB

### What is it?

ScyllaDB is the database used for employee and salary-related application data.

### Data handled

```text
Employee API ──┐
               ├──> ScyllaDB
Salary API ────┘
```

The application uses the `employee_db` keyspace for these services.

---

## 3.8 PostgreSQL

PostgreSQL is used by the Attendance API for attendance-related data.

```text
Attendance API
      |
      v
 PostgreSQL
```

---

## 3.9 Redis

Redis is used as a supporting in-memory data service where required by the application components.

```text
Application Service
       |
       v
     Redis
```

Redis is separate from the primary persistent databases used for employee, salary, and attendance data.

---

# 4. Overall Architecture

## 4.1 Component Architecture Diagram

><img width="1536" height="1024" alt="ChatGPT Image Apr 26, 2026, 10_41_56 AM" src="https://github.com/user-attachments/assets/0c394672-3e22-47f2-8b84-a87742fc62fe" />


---

# 5. Application Flow

## 5.1 Overall Request Flow

<img width="1092" height="642" alt="Screenshot 2026-09-14 014132" src="https://github.com/user-attachments/assets/744ef058-6243-4805-8fe7-60d40ad9dc72" />


### Step-by-Step

1. User performs an operation in the frontend.
2. React frontend creates the API request.
3. Request is sent to NGINX.
4. NGINX identifies the required backend service.
5. Request is forwarded to that service.
6. Backend service processes the request.
7. Backend communicates with its own database if data access is needed.
8. Database returns the result.
9. Backend service prepares the API response.
10. NGINX forwards the response to the frontend.
11. Frontend displays the result to the user.

---

## 5.2 Employee Flow

<img width="1042" height="126" alt="Screenshot 2026-09-14 014250" src="https://github.com/user-attachments/assets/25c0786b-70ef-4e40-b6ed-1b6476293268" />

---

## 5.3 Salary Flow

<img width="1027" height="138" alt="Screenshot 2026-09-14 014416" src="https://github.com/user-attachments/assets/2d35e1d0-cd39-48bc-a252-3f6b2dede7d3" />

---

## 5.4 Attendance Flow

<img width="1078" height="123" alt="Screenshot 2026-09-14 014454" src="https://github.com/user-attachments/assets/69b472f1-bc55-4d31-8f18-12d6520866ac" />

---

## 5.5 Notification Flow

<img width="1207" height="480" alt="Screenshot 2026-09-14 014550" src="https://github.com/user-attachments/assets/e4d58460-33e0-4e82-9a3a-e312584f1c11" />

> Unlike the Employee, Salary, and Attendance flows, this flow does **not** pass through the Frontend or NGINX — the Notification Service is triggered independently and only reads data from the other three APIs.

---

# 6. Component Communication

| **Source**            | **Destination**       | **Purpose**                |
| ---------------------- | ---------------------- | --------------------------- |
| User                   | React Frontend          | User interaction            |
| React Frontend         | NGINX                   | API requests                |
| NGINX                  | Employee API (:8080)    | Employee operations         |
| NGINX                  | Salary API (:8082)      | Salary operations           |
| NGINX                  | Attendance API (:8081)  | Attendance operations       |
| Employee API           | ScyllaDB                | Employee data                |
| Salary API             | ScyllaDB                | Salary data                  |
| Attendance API         | PostgreSQL              | Attendance data              |
| Application services   | Redis                   | Supporting in-memory operations |
| Notification Service (:5000) | Employee/Salary/Attendance API | Reads data for notifications (independent of NGINX) |

---

# 7. Conclusion

This document explains the OT-Microservices components, overall component architecture, and application flow.
It shows how a user request moves through the frontend, NGINX, backend services, and databases, and clarifies that the Notification Service runs independently of this request path, reading data from the other services on its own trigger.

---

# 8. Contact Information

| **Name**      | **Email**                             |
| ------------- | --------------------------------------- |
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)   |

---

# 9. References

| **Reference**                                                    | **Description**                                 |
| ---------------------------------------------------------------- | ----------------------------------------------- |
| [OT-Microservices GitHub](https://github.com/OT-MICROSERVICES)   | OT-Microservices organization (all repositories) |
| [frontend](https://github.com/OT-MICROSERVICES/frontend)         | Frontend repository                             |
| [employee-api](https://github.com/OT-MICROSERVICES/employee-api) | Employee API repository                         |
| [salary-api](https://github.com/OT-MICROSERVICES/salary-api)     | Salary API repository                           |
| [attendance-api](https://github.com/OT-MICROSERVICES/attendance-api) | Attendance API repository                   |
| [notification-worker](https://github.com/OT-MICROSERVICES/notification-worker) | Notification Service repository   |
