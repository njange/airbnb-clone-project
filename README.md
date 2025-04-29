# Airbnb Clone Project

## Overview
The Airbnb Clone Project is a comprehensive, real-world application designed to simulate the development of a robust booking platform like Airbnb. This project focuses on backend systems, database design, API development, and application security, enabling developers to build a scalable and secure web application.

## Project Goals
- Develop a scalable backend architecture.
- Implement secure and efficient APIs.
- Design a relational database structure.
- Integrate modern tools and technologies for CI/CD and deployment.

## Tech Stack
- **Django**: A web framework for building RESTful APIs and backend logic, enabling rapid development and clean, pragmatic design.
- **MySQL**: A relational database for storing and managing application data, ensuring data integrity and efficient querying.
- **GraphQL**: A query language for APIs that allows clients to request only the data they need, improving performance and flexibility.
- **Docker**: A tool for containerization, providing consistent development and deployment environments across different systems.
- **GitHub Actions**: A CI/CD platform for automating testing, building, and deployment processes, ensuring efficient and error-free development workflows.

## Team Roles

- **Backend Developer**: Responsible for designing and implementing the server-side logic, creating APIs, and ensuring the backend architecture is scalable and efficient.
- **Database Administrator**: Manages the database design, ensures data integrity, and optimizes database performance for the application.
- **DevOps Engineer**: Sets up and maintains CI/CD pipelines, manages containerization using Docker, and ensures smooth deployment processes.
- **API Security Specialist**: Focuses on implementing authentication, authorization, and other security measures to protect user data and secure transactions.
- **Project Manager**: Oversees the project timeline, coordinates between team members, and ensures that the project goals are met on schedule.

---
## Database Design

### Key Entities and Fields
1. **Users**
   - Fields: `id`, `name`, `email`, `password`, `role`
   - Description: Represents the users of the platform, including guests and hosts.

2. **Properties**
   - Fields: `id`, `name`, `location`, `price`, `host_id`
   - Description: Represents the properties listed by hosts for booking.

3. **Bookings**
   - Fields: `id`, `user_id`, `property_id`, `start_date`, `end_date`
   - Description: Represents reservations made by users for specific properties.

4. **Reviews**
   - Fields: `id`, `user_id`, `property_id`, `rating`, `comment`
   - Description: Represents feedback provided by users for properties they have booked.

5. **Payments**
   - Fields: `id`, `booking_id`, `amount`, `payment_date`, `status`
   - Description: Represents payment transactions for bookings.

### Relationships
- A **User** can have multiple **Bookings**.
- A **Property** belongs to a **User** (host).
- A **Booking** is associated with one **Property** and one **User**.
- A **Review** is linked to a **User** and a **Property**.
- A **Payment** is tied to a **Booking**.

---

---
