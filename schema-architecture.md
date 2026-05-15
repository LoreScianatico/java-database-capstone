#Architecture Summary

This document depicts a Spring Boot application with a dual-frontend, dual-database architecture. On the frontend side, there are two entry points: server-side rendered Dashboards (AdminDashboard and DoctorDashboard) that communicate via Thymeleaf Controllers, and REST Modules (Appointments, PatientDashboard, PatientRecord) that interact through REST Controllers using a JSON API. Both controller types funnel into a shared Service Layer, which acts as the core business logic hub — a classic Spring layered architecture pattern.
Below the service layer, data persistence is split across two databases. MySQL handles structured relational data (Patient, Doctor, Appointment, Admin) as JPA entities via Spring Data repositories. MongoDB handles document-based data (Prescription) through its own repository. This polyglot persistence approach makes sense here — relational data like patient-doctor relationships fits MySQL's schema model, while prescriptions (which may have variable or nested structure) are stored as flexible MongoDB documents.

Data control

1. User accesses either a Dashboard (AdminDashboard or DoctorDashboard) or a REST Module (Appointments, PatientDashboard, PatientRecord).
2. The request is routed to the appropriate controller inside the Spring Boot App — Thymeleaf Controllers for dashboard views, or REST Controllers for JSON API calls.
3. Both controller types delegate business logic to the shared Service Layer, which acts as the single point of orchestration for the application.
4. The Service Layer determines which repository to use — MySQL Repositories for relational data, or MongoDB Repository for document-based data.
5. Each repository accesses its respective database — MySQL Database or MongoDB Database — to perform read/write operations.
6. Data is mapped through the appropriate models — MySQL Models (JPA Entities: Patient, Doctor, Appointment, Admin) or the MongoDB Model (Prescription Document).
7. The models are defined within their respective model packages — MySQLModels grouping the four JPA entities, and MongoDBModels grouping the Prescription document — providing the structured schema used throughout the application.
