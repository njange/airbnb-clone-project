# Airbnb Clone Project

<p align="center">
  <img src="https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white" />
  <img src="https://img.shields.io/badge/DRF-red?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white" />
  <img src="https://img.shields.io/badge/Celery-darkgreen?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/GraphQL-E10098?style=for-the-badge&logo=graphql&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" />
</p>


## Overview
The Airbnb Clone Project is a comprehensive, real-world application designed to simulate the development of a robust booking platform like Airbnb. This project focuses on backend systems, database design, API development, and application security, enabling developers to build a scalable and secure web application.

---

## 📋 Table of Contents
1. [Project Goals](#project-goals)
2. [Tech Stack](#tech-stack)
3. [Team Roles](#team-roles)
4. [Database Design](#database-design)
5. [Feature Breakdown](#feature-breakdown)
6. [API Security](#api-security)
7. [CI/CD Pipeline](#cicd-pipeline)

---

## Project Goals
- Develop a scalable backend architecture.
- Implement secure and efficient APIs.
- Design a relational database structure.
- Integrate modern tools and technologies for CI/CD and deployment.

## Tech Stack

- **Django**: A high-level Python web framework for building robust and scalable backend systems.
- **Django REST Framework (DRF)**: A powerful toolkit for building RESTful APIs, enabling seamless communication between the frontend and backend.
- **PostgreSQL**: A relational database system used for storing and managing structured data efficiently.
- **Redis**: An in-memory data structure store used for caching and message brokering to improve application performance.
- **Celery**: A task queue library for handling asynchronous tasks and background jobs, such as sending emails or processing large datasets.
- **GraphQL**: A query language for APIs that allows clients to request only the data they need, improving flexibility and performance.
- **Docker**: A containerization platform that ensures consistent development and deployment environments across different systems.
- **GitHub Actions**: A CI/CD platform for automating workflows, including testing, building, and deploying the application.

---

## Team Roles

- **Backend Developer**: Responsible for designing and implementing the server-side logic, creating APIs, and ensuring the backend architecture is scalable and efficient.
- **Database Administrator**: Manages the database design, ensures data integrity, and optimizes database performance for the application.
- **DevOps Engineer**: Sets up and maintains CI/CD pipelines, manages containerization using Docker, and ensures smooth deployment processes.
- **API Security Specialist**: Focuses on implementing authentication, authorization, and other security measures to protect user data and secure transactions.
- **Project Manager**: Oversees the project timeline, coordinates between team members, and ensures that the project goals are met on schedule.

---
## Database Design

### Database Specification - Airbnb

#### Entities and Attributes

1. **User**
   - `user_id`: Primary Key, UUID, Indexed
   - `first_name`: VARCHAR, NOT NULL
   - `last_name`: VARCHAR, NOT NULL
   - `email`: VARCHAR, UNIQUE, NOT NULL
   - `password_hash`: VARCHAR, NOT NULL
   - `phone_number`: VARCHAR, NULL
   - `role`: ENUM (guest, host, admin), NOT NULL
   - `created_at`: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

2. **Property**
   - `property_id`: Primary Key, UUID, Indexed
   - `host_id`: Foreign Key, references `User(user_id)`
   - `name`: VARCHAR, NOT NULL
   - `description`: TEXT, NOT NULL
   - `location`: VARCHAR, NOT NULL
   - `pricepernight`: DECIMAL, NOT NULL
   - `created_at`: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
   - `updated_at`: TIMESTAMP, ON UPDATE CURRENT_TIMESTAMP

3. **Booking**
   - `booking_id`: Primary Key, UUID, Indexed
   - `property_id`: Foreign Key, references `Property(property_id)`
   - `user_id`: Foreign Key, references `User(user_id)`
   - `start_date`: DATE, NOT NULL
   - `end_date`: DATE, NOT NULL
   - `total_price`: DECIMAL, NOT NULL
   - `status`: ENUM (pending, confirmed, canceled), NOT NULL
   - `created_at`: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

4. **Payment**
   - `payment_id`: Primary Key, UUID, Indexed
   - `booking_id`: Foreign Key, references `Booking(booking_id)`
   - `amount`: DECIMAL, NOT NULL
   - `payment_date`: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
   - `payment_method`: ENUM (credit_card, paypal, stripe), NOT NULL

5. **Review**
   - `review_id`: Primary Key, UUID, Indexed
   - `property_id`: Foreign Key, references `Property(property_id)`
   - `user_id`: Foreign Key, references `User(user_id)`
   - `rating`: INTEGER, CHECK: `rating >= 1 AND rating <= 5`, NOT NULL
   - `comment`: TEXT, NOT NULL
   - `created_at`: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

6. **Message**
   - `message_id`: Primary Key, UUID, Indexed
   - `sender_id`: Foreign Key, references `User(user_id)`
   - `recipient_id`: Foreign Key, references `User(user_id)`
   - `message_body`: TEXT, NOT NULL
   - `sent_at`: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

---

#### Constraints

1. **User Table**
   - Unique constraint on `email`.
   - Non-null constraints on required fields.

2. **Property Table**
   - Foreign key constraint on `host_id`.
   - Non-null constraints on essential attributes.

3. **Booking Table**
   - Foreign key constraints on `property_id` and `user_id`.
   - `status` must be one of `pending`, `confirmed`, or `canceled`.

4. **Payment Table**
   - Foreign key constraint on `booking_id`, ensuring payment is linked to valid bookings.

5. **Review Table**
   - Constraints on `rating` values (1-5).
   - Foreign key constraints on `property_id` and `user_id`.

6. **Message Table**
   - Foreign key constraints on `sender_id` and `recipient_id`.

---

#### Indexing
- **Primary Keys**: Indexed automatically.
- **Additional Indexes**:
  - `email` in the **User** table.
  - `property_id` in the **Property** and **Booking** tables.
  - `booking_id` in the **Booking** and **Payment** tables.

---

### Relationships Summary
- A **User** can have multiple **Bookings** and **Reviews**, and can own multiple **Properties** (1-to-many).
- A **Property** can have multiple **Bookings** and **Reviews**, but belongs to one **User** (host) (1-to-many, many-to-1).
- A **Booking** is associated with one **User** and one **Property**, and can have one **Payment** (many-to-1, 1-to-1).
- A **Review** is linked to one **User** and one **Property** (many-to-1).
- A **Payment** is tied to one **Booking** (1-to-1).

---
## Feature Breakdown

1. **User Management**  
   This feature allows users to register, log in, and manage their profiles. It ensures secure authentication and authorization, enabling both guests and hosts to access the platform's functionalities.

2. **Property Management**  
   Hosts can list their properties, update details, and manage availability. This feature provides a seamless way for hosts to showcase their properties to potential guests.

3. **Booking System**  
   Guests can search for properties, check availability, and make reservations. This feature ensures a smooth booking process, including date selection and confirmation.

4. **Review System**  
   Users can leave reviews and ratings for properties they have booked. This feature helps maintain transparency and provides valuable feedback for hosts and future guests.

5. **Payment Processing**  
   Secure payment gateways are integrated to handle transactions for bookings. This feature ensures that payments are processed safely and efficiently, supporting multiple payment methods.

---
## API Security

To ensure the security and integrity of the Airbnb Clone platform, the following key security measures will be implemented:

1. **Authentication**  
   Secure authentication mechanisms, such as token-based authentication (e.g., JWT), will be used to verify the identity of users. This ensures that only authorized users can access the platform.

2. **Authorization**  
   Role-based access control (RBAC) will be implemented to restrict access to specific resources based on user roles (e.g., guest, host, admin). This prevents unauthorized actions and protects sensitive data.

3. **Rate Limiting**  
   Rate limiting will be applied to APIs to prevent abuse, such as brute force attacks or excessive requests, ensuring the platform remains stable and responsive.

4. **Data Encryption**  
   Sensitive data, such as passwords and payment information, will be encrypted both in transit (using HTTPS) and at rest. This protects user data from being intercepted or accessed by unauthorized parties.

5. **Input Validation and Sanitization**  
   All user inputs will be validated and sanitized to prevent common vulnerabilities like SQL injection and cross-site scripting (XSS). This ensures the integrity of the application.

6. **Secure Payment Processing**  
   Payment transactions will be handled through trusted third-party payment gateways, ensuring compliance with industry standards like PCI DSS to protect financial data.

### Importance of Security
- **Protecting User Data**: Ensures that personal and sensitive information remains confidential and secure.
- **Securing Payments**: Safeguards financial transactions to prevent fraud and unauthorized access.
- **Maintaining Platform Integrity**: Prevents malicious activities that could disrupt the platform or compromise its functionality.
- **Building User Trust**: A secure platform fosters trust among users, encouraging them to use the service confidently.

---
## CI/CD Pipeline

Continuous Integration and Continuous Deployment (CI/CD) pipelines are automated workflows that streamline the process of building, testing, and deploying code. They ensure that code changes are integrated frequently and deployed efficiently, reducing errors and improving development speed.

### Importance of CI/CD Pipelines
- **Automation**: Automates repetitive tasks like testing and deployment, saving time and reducing human error.
- **Consistency**: Ensures that the application behaves the same across different environments (development, staging, production).
- **Faster Delivery**: Speeds up the release cycle by enabling frequent and reliable deployments.
- **Improved Collaboration**: Encourages developers to integrate their code changes frequently, reducing integration conflicts.

### Tools for CI/CD
- **GitHub Actions**: Automates workflows for testing, building, and deploying the application directly from the GitHub repository.
- **Docker**: Provides containerization to ensure consistent environments across development and production.
- **Jenkins**: An open-source automation server for building and deploying applications.
- **CircleCI**: A CI/CD platform that integrates with GitHub for seamless automation.

---

---
