# Prospect Form Generator API
Welcome to the documentation of **Prospect Form Generator Api**, an application built with **C#** and **.NET 8.0** whose main purpose is to generate a personalized form for different clients and store the responses that said form receives.

## 📖 Overview

**Prospect Form Generator API** is a modular and scalable solution designed to dynamically generate custom forms for different clients and store their responses securely.

This API follows the principles of **Clean Architecture**, ensuring a clean separation of concerns and maintainability over time. It supports a **multi-database architecture**, where each client has its own isolated database. Form generation is dynamic and customizable per client, and it is easily embeddable into third-party websites through a generated JavaScript script.

The project includes:
- Custom field rendering based on form design stored per client.
- Dynamic loading of `select` options using stored procedures.
- A secure and decoupled database access strategy using Azure Key Vault for credential management.
- A robust error handling mechanism via custom middleware.
- Unit testing to ensure business logic correctness.
- Docker support for easy deployment and environment setup.

## 🧱 Project Structure
This project follows the principles of **Clean Architecture**, ensuring separation of concerns, scalability, and testability.

### Layer Responsibilities

- **[Domain](domain.md)**  
  Contains the enterprise-wide business rules. This layer is the most stable and contains entities, value objects, and business logic.

- **[Application](application.md)** 
  Contains the use cases and interfaces. It orchestrates domain logic without knowing how things are persisted or presented.

- **[Infrastructure](infrastructure.md)** 
  It contains the contexts that handle the database access and the repositories to handle database operations.

- **[Presentation](presentation.md)**
  The entry point of the application, exposing the API. It communicates with the Application layer through use cases.