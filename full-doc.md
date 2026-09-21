# OT-Microservices | Full Stack | Detailed Documentation

---

## Author Table

| **Author**    | **Created On** | **Version** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ------------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Sahil Butola  | 16-09-2026     | 1.0         | 17-09-2026         | Vishal / Divya  | Aayush Verma    | Mahesh  / Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Application Overview](#2-application-overview)
3. [Overall Architecture](#3-overall-architecture)
4. [Application Flows](#4-application-flows)
5. [Component Communication](#5-component-communication)
6. [Conclusion](#6-conclusion)
7. [Contact Information](#7-contact-information)
8. [References](#8-references)

---

# 1. Introduction

This document provides the **overall component architecture** and **various application flow diagrams** of the OT-Microservices application.

---

# 2. Application Overview

OT-Microservices follows a microservices architecture where different business functionalities are handled by independent services.

The application consists of the following major components:

* React Frontend
* NGINX
* Employee API
* Salary API
* Attendance API
* Notification Service
* ScyllaDB
* PostgreSQL
* Redis

-The React Frontend communicates with the Employee, Salary, and Attendance APIs through NGINX.
-The Employee API and Salary API use ScyllaDB, while the Attendance API uses PostgreSQL.
-The Notification Service operates independently from the normal Frontend-to-Backend request path.

---

# 3. Overall Architecture

## 3.1 Component Architecture Diagram

The following diagram represents the overall component architecture of the OT-Microservices application, including the frontend, reverse proxy, backend services, databases, Redis, and Notification Service.

<img width="1536" height="1024" alt="OT-Microservices Component Architecture" src="https://github.com/user-attachments/assets/0c394672-3e22-47f2-8b84-a87742fc62fe" />

---

# 4. Application Flows

## 4.1 Overall Application Flow

The following diagram represents the overall application request flow.

A user interacts with the React Frontend. The request is forwarded to NGINX, which routes it to the appropriate backend service. The backend service communicates with its respective database when data access is required.

<img width="1092" height="642" alt="image" src="https://github.com/user-attachments/assets/6fb3ca3d-cad6-43ea-af16-2fc636052bd9" />

---

## 4.2 Employee Flow

The Employee API handles employee-related operations. Employee requests originate from the frontend and are routed through NGINX to the Employee API.

<img width="1042" height="126" alt="image" src="https://github.com/user-attachments/assets/8763ef30-8198-4e57-8cf7-2e68f3955854" />

---

## 4.3 Salary Flow

The Salary API handles salary-related operations. Salary requests are routed through NGINX to the Salary API, which communicates with ScyllaDB.

<img width="1027" height="138" alt="image" src="https://github.com/user-attachments/assets/da4f4a0c-d258-4008-b7cb-7b051a279452" />


---

## 4.4 Attendance Flow

The Attendance API handles attendance-related operations. Attendance requests are routed through NGINX to the Attendance API, which communicates with PostgreSQL.

<img width="1078" height="123" alt="image" src="https://github.com/user-attachments/assets/040098b7-9ca3-4513-bea4-8710d3b43007" />


---

## 4.5 Notification Flow

The Notification Service works independently from the normal frontend and NGINX request path.
It is triggered separately and communicates with the Employee, Salary, and Attendance APIs to retrieve the required application data before sending notifications.

<img width="1207" height="480" alt="image" src="https://github.com/user-attachments/assets/ec932c2d-d7a0-4fe0-9ab3-6574650450ee" />

---

# 5. Component Communication

The following table summarizes the communication between the major application components.

| **Source**           | **Destination**                     | **Purpose**                        |
| -------------------- | ----------------------------------- | ---------------------------------- |
| User                 | React Frontend                      | User interaction                   |
| React Frontend       | NGINX                               | API requests                       |
| NGINX                | Employee API                        | Employee operations                |
| NGINX                | Salary API                          | Salary operations                  |
| NGINX                | Attendance API                      | Attendance operations              |
| Employee API         | ScyllaDB                            | Employee data                      |
| Salary API           | ScyllaDB                            | Salary data                        |
| Attendance API       | PostgreSQL                          | Attendance data                    |
| Application Services | Redis                               | Supporting in-memory operations    |
| Notification Service | Employee / Salary / Attendance APIs | Retrieve required application data |
| Notification Service | Email                               | Send notifications                 |

---

# 6. Conclusion

This document provides the overall architecture and application flow of the OT-Microservices application, showing how its major components communicate and interact.

---

# 7. Contact Information

| **Name**      | **Email**                                                                           |
| ------------- | ----------------------------------------------------------------------------------- |
| Sahil Butola  | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)   |

---

# 8. References

| **Reference**                                                                  | **Description**                 |
| ------------------------------------------------------------------------------ | ------------------------------- |
| [OT-Microservices GitHub](https://github.com/OT-MICROSERVICES)                 | OT-Microservices organization   |
| [Frontend](https://github.com/OT-MICROSERVICES/frontend)                       | Frontend repository             |
| [Employee API](https://github.com/OT-MICROSERVICES/employee-api)               | Employee API repository         |
| [Salary API](https://github.com/OT-MICROSERVICES/salary-api)                   | Salary API repository           |
| [Attendance API](https://github.com/OT-MICROSERVICES/attendance-api)           | Attendance API repository       |
| [Notification Worker](https://github.com/OT-MICROSERVICES/notification-worker) | Notification Service repository |
