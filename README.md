# Airbnb Clone Project

## Project Overview

This project is a comprehensive, full-stack application designed to simulate the development of a **robust booking platform, much like Airbnb**. It serves as a practical, real-world exercise focused on mastering modern software development practices, complex system architectures, and collaborative team dynamics.

The application will feature core booking functionalities, user authentication, property listing management, and a secure API for all transactions. It emphasizes **scalability, security, and maintainable code** throughout the entire development lifecycle.

---

## Project Goals

The primary objectives of this project are to:

- **Implement a scalable backend architecture** capable of handling numerous user requests and data transactions.
- **Design a normalized and efficient relational database** structure for managing users, properties, bookings, and reviews.
- **Develop a secure and well-documented RESTful API** for seamless client-server communication.
- **Master collaborative development workflows** using GitHub for version control and team coordination.
- **Establish a robust CI/CD pipeline** to automate testing and deployment processes.

---

## Technology Stack

The following technologies will be utilized to build and deploy the Airbnb Clone:

| Component             | Technology              | Rationale                                                                                                                      |
| :-------------------- | :---------------------- | :----------------------------------------------------------------------------------------------------------------------------- |
| **Backend Framework** | **Django** (Python)     | Chosen for its "batteries-included" approach, robust security features, and built-in ORM, accelerating secure API development. |
| **Database**          | **MySQL**               | Selected for its reliability, performance, and maturity as a transactional relational database management system.              |
| **API**               | **GraphQL** (over REST) | To provide clients with the ability to request exactly the data they need, enhancing performance and reducing over-fetching.   |
| **Containerization**  | **Docker**              | For consistent and portable development, testing, and production environments.                                                 |
| **CI/CD**             | **GitHub Actions**      | To automate the build, test, and deployment phases, ensuring code quality and rapid iteration.                                 |
| **Documentation**     | **Markdown**            | For clear and comprehensive project documentation.                                                                             |

---

## Team Roles

Below are the key roles, their responsibilities, and their contribution to the project's success.

| Role                         | Primary Responsibility                                                                                                      | Contribution to the Project                                                                                                                                                                    |
| :--------------------------- | :-------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Backend Developer            | Building and maintaining the server-side application logic using Django and GraphQL.                                        | Develops the core business logic (user authentication, booking workflows, property management), ensuring the application is functional, efficient, and scalable.                               |
| Database Administrator (DBA) | Designing, implementing, and maintaining the MySQL database architecture.                                                   | Guarantees data integrity, security, and high performance of the database. Responsible for schema design, query optimization, backups, and disaster recovery planning.                         |
| DevOps Engineer              | Establishing and managing the automated development and deployment pipeline (CI/CD).                                        | Ensures efficient, repeatable, and secure deployment by configuring tools like Docker and GitHub Actions. Focuses on infrastructure, monitoring, and environment consistency.                  |
| Security Engineer            | Implementing and auditing security measures across the entire application and API.                                          | Protects sensitive data by implementing best practices for authentication, authorization, data encryption, input validation, and vulnerability management.                                     |
| Technical Writer / Analyst   | Creating and maintaining all project documentation, including the README.md, feature specifications, and API documentation. | Ensures clear communication, maintainability, and knowledge transfer for all project artifacts, fulfilling key assessment requirements.                                                        |
| Project Lead / Architect     | Defining the overall technical vision, architecture, and standards for the system.                                          | Makes high-level design choices, oversees technical risk mitigation, and ensures that all components (Django, MySQL, GraphQL) are integrated cohesively to meet the project's long-term goals. |

## Database Design Overview (MySQL)

The core functionality of the Airbnb Clone relies on a robust relational database structure. The design will focus on separating key business concepts into distinct entities to ensure data integrity and efficient querying. The primary database system used is MySQL.

### Key Entities and Fields

| Entity     | Description                                                        | Important Fields                                                                                                                               |
| :--------- | :----------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- |
| Users      | Represents all registered platform users (hosts and guests).       | user_id (Primary Key), email (Unique), password_hash, first_name, is_host (Boolean)                                                            |
| Properties | Represents a bookable lodging space listed by a host.              | property_id (Primary Key), host_id (Foreign Key to Users), title, description, price_per_night, location                                       |
| Bookings   | Represents a confirmed reservation for a specific property.        | booking_id (Primary Key), guest_id (Foreign Key to Users), property_id (Foreign Key to Properties), check_in_date, check_out_date, total_price |
| Reviews    | Represents feedback given by a guest on a property they stayed at. | review_id (Primary Key), booking_id (Foreign Key to Bookings), guest_id (Foreign Key to Users), rating (1-5), comment                          |
| Payments   | Represents a transactional record for a booking.                   | payment_id (Primary Key), booking_id (Foreign Key to Bookings), amount, payment_method, transaction_status                                     |

### Entity Relationships

The entities are connected through Foreign Keys to establish the core business logic:

1. Users and Properties (One-to-Many):

a. A single User who is a host can list multiple Properties.

b. A Property belongs to exactly one host (User).

2. Users and Bookings (One-to-Many):

a. A single User (guest) can make multiple Bookings.

b. A Booking belongs to exactly one guest (User).

3. Properties and Bookings (One-to-Many):

a. A single Property can have multiple Bookings over time.

b. A Booking is always associated with one Property.

4. Bookings and Reviews (One-to-One / One-to-Zero-or-One):

a. A Review is made after a stay, and is uniquely tied to one completed Booking.

b. A Booking may have at most one Review.

5. Bookings and Payments (One-to-Many / One-to-One):

a. A single Booking can be associated with one or more Payments (e.g., an initial deposit and a final payment).

b. A Payment is always associated with a single Booking.

This structure ensures that every booking has a host, a guest, a property, and a record of payment, reflecting the real-world complexity of a platform like Airbnb.

## Feature Breakdown

The application is structured around several core modules that mirror the essential functionalities of a real-world booking platform. These features are driven by the backend logic and database design to ensure a seamless user experience.

| Feature                          | Description and Contribution                                                                                                                                                                                                                                                                         |
| :------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| User Management & Authentication | This feature handles user registration, secure login/logout, and profile management for both guests and hosts. It ensures the security and identity of users by implementing token-based authentication (e.g., using Django Rest Framework features) and managing different roles (is_host boolean). |
| Property Listing Management      | This module allows registered hosts to Create, Read, Update, and Delete (CRUD) their property listings. It includes capturing critical data like location, pricing, availability, and descriptions, which is essential for inventory and search functionality.                                       |
| Booking & Reservation System     | This is the central transaction feature where guests can select dates and reserve a property. It includes availability checks, applying pricing rules, and locking in the dates, forming the primary business workflow of the platform.                                                              |
| Review and Rating System         | Guests can leave detailed feedback and star ratings on properties after their stay is complete. This system builds trust and transparency on the platform by allowing future guests to make informed decisions and encouraging hosts to maintain high standards.                                     |
| Secure Payment Processing        | This feature integrates with a third-party payment gateway to securely handle transactions for bookings. It ensures that payment details are protected and that the booking process can be finalized reliably with transaction status tracking and records kept in the Payments entity.              |
| API Development (Django/GraphQL) | The feature defines the structure and endpoints for how the frontend and backend communicate. Utilizing GraphQL allows for efficient data fetching, minimizing over-fetching and ensuring the platform is fast and scalable.                                                                         |
| API Security                     | Securing the backend API is paramount for an application that handles sensitive user information and financial transactions. Given that our API serves as the gateway to the database, robust security measures are implemented to protect all data and business logic.                              |

## Key Security Measures

| Security Measure                  | Implementation Focus                                                                                                                                                                                                                                       |
| :-------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authentication (Token-Based)      | Users must prove their identity before accessing restricted endpoints. We will use JSON Web Tokens (JWTs) or similar session tokens issued upon successful login, which must be included in subsequent requests.                                           |
| Authorization (Permissions)       | After authentication, the API will verify that the authenticated user has the necessary permissions for the requested action. For example, a non-host user must be prevented from creating a Property, and a guest can only Review their own bookings.     |
| Input Validation and Sanitization | All data received from clients (e.g., through form submissions) must be rigorously checked before processing or storing in the MySQL database. This prevents common attacks like SQL Injection and Cross-Site Scripting (XSS) by cleaning malicious input. |
| Rate Limiting                     | This restricts the number of requests a single user or IP address can make to the API within a specific timeframe. It is crucial for defending against Denial-of-Service (DoS) attacks and brute-force password attempts.                                  |
| Data Encryption                   | Sensitive data, both in transit and at rest, must be encrypted. Passwords will be securely hashed before storage, and all API traffic will be forced over HTTPS/SSL to encrypt communication.                                                              |

## CI/CD Pipeline

Continuous Integration/Continuous Delivery (CI/CD) is a fundamental practice for modern, scalable software development, ensuring that code changes are reliably and efficiently released.

### What is CI/CD

A CI/CD Pipeline is an automated series of steps that takes code changes from the developer's repository, tests them, and prepares them for deployment (CI - Continuous Integration) or automatically deploys them to production (CD - Continuous Delivery).

### Why CI/CD is Important

- Rapid Feedback: It automatically runs tests (unit, integration, security) every time a change is pushed, allowing developers to catch and fix bugs early, reducing technical debt.

- Reliable Releases: Automation eliminates manual errors during the deployment process, ensuring that the application environment (built with Django, MySQL, etc.) is consistently configured and stable.

- Collaboration: It streamlines the team workflow, making merging code from multiple contributors safe and predictable, which is essential for a large, collaborative project.

### Tools for the Pipeline

The following tools will be utilized to build and manage the automated CI/CD pipeline:

- GitHub Actions: Serves as the primary automation engine. It will be configured to listen for events (e.g., pushes to the main branch) and trigger the entire pipeline (build, test, deploy).

- Docker: Used to containerize the application and the MySQL database. This ensures that the environment is identical across development, testing, and production stages, eliminating "it works on my machine" issues.

- Python Testing Frameworks (e.g., unittest/pytest): These tools, integrated into the CI process, ensure that the Django backend logic and API endpoints function correctly before deployment.
