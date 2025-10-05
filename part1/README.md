## UML diagram project

HBnB Project — Technical Architecture Document

Author: Lautaro Ezequiel y Lucas Noble

Version: 1.0

Document Type: Comprehensive Technical Architecture & Design

## 1. Introduction

- This technical document describes the architecture, design, and main components of the HBnB project.
Its purpose is to serve as a reference guide for the development and implementation of the system, ensuring that each part of the application maintains a consistent, modular, and scalable structure.

- The document compiles the results of the following previous stages:

Part 0: High-Level Package Diagram (Layered Architecture and Facade Pattern).
Part 1: Detailed Class Diagram for Business Logic Layer (Business Model).
Part 2: Sequence Diagrams for API Calls (Interaction Flow Between Layers).

- Each section includes UML diagrams rendered in Mermaid.js, accompanied by detailed explanations of design decisions, interactions between components, and the responsibility of each element within the system.

## Part 0 – High-level package diagram

## General description:

- The HBnB system adopts a three-layer architecture to achieve separation of responsibilities, scalability, and efficient maintenance.
- The layers are:

Presentation Layer (Services/API): Manages interaction with the end user, handling HTTP requests and JSON responses through RESTful endpoints.

Business Logic Layer (Models): Implements business logic and defines the main entities of the system (User, Place, Review, Amenity).

Persistence Layer (Database): Responsible for storing and retrieving data using data access objects (DAOs or repositories).

- The Facade pattern acts as a unified interface between layers, allowing the presentation layer to communicate with the business logic without depending on internal implementation details.

<img width="400" height="800" alt="Diagrama_de_Paquetes(Parte 0)" src="https://github.com/user-attachments/assets/45c8266c-8e46-4259-89a4-07766223a3ca" />

## 1) Explanation of the layers of the Packet Diagram:

## 1- Presentation Layer (Services, API)
- Responsibility: Manages interaction between the user and the application.
- Components:
  - API: Exposes endpoints (example: GET /places, POST /users).
  - Services: Coordinate requests and call the Facade.

## 2- Business Logic Layer (Models + Facade)
- Responsibility: Contains the business rules and the core of the application.
- Components:
  - Main models: User, Place, Review, Amenity.
  - Facade: Unified interface that receives requests from the presentation layer and manages communication with the persistence layer.

## 3- Persistence Layer (Data storage)
- Responsibility: Directly manages the database.
- Components:
  - DatabaseRepository: CRUD of entities.
  - StorageAdapter: Abstracts database technology (MySQL, SQLite, etc.).

## Role of the Facade Pattern
The Facade acts as an intermediary between the Presentation Layer and the Business Logic Layer:
  - Simplifies interaction and reduces complexity.
  - Enables loose coupling: the presentation layer is unaware of internal details of models or persistence.
  - Makes it possible to change internal logic without affecting the public API.

Simplified flow:

[User Request] → API → Facade → Models → Repository → Database

## Part 1. Detailed class diagram for the business logic layer

<img width="400" height="800" alt="Diagrama_de_Clases(Parte 1)" src="https://github.com/user-attachments/assets/84e290b0-e4ba-4268-bfed-e5fda288c597" />

## 1) Explanation of the entities in the Class Diagram:

## General description:

- This layer contains the core logic of the system and the models representing the domain entities:

## 1- User
- Role: Represents the users of the application (owners, travelers).
- Key attributes:
  - id (UUID, unique identifier).
  - name, email, password.
  - created_at, updated_at for traceability.
- Methods:
  - create(), update(), delete().

## 2- Place
- Role: Represents available locations or accommodations.
- Key attributes:
  - id, name, description, location.
  - price_per_night
  - created_at, updated_at.
- Methods:
  - create(), update(), delete().

## 3- Review
- Role: Represents the reviews that users make about places.
- Key attributes:
  - id, content, rating.
  - created_at, updated_at.
- Methods:
  - create(), update(), delete().

## Amenity
- Role: Represents the amenities or services available at a location.
- Key attributes:
  - id, name, description.
  - created_at, updated_at.
- Methods:
  - create(), update(), delete().

## Relationships between Entities
- User – Place:
  - A User can own multiple Places.
- User – Review:
  - A User can write several Reviews.
- Place – Review:
  - Un Place puede tener múltiples Review.
- Place – Amenity:
  - Many-to-many relationship: a Place can have multiple Amenities, and an Amenity can be in multiple Places.

## Part 2. Sequence diagrams for API calls

## General description:

- Sequence diagrams show how layers communicate to process API requests, reflecting the flow of information from the user to the database and back.

## 1) User Registration (User Registration Diagram)

<img width="1920" height="1080" alt="Diagrama_de_Registro_de_Usuario(Parte 2)" src="https://github.com/user-attachments/assets/33eb3505-c91e-4152-8b2b-54abb78f4568" />

## Explanation
- The user sends their registration data to the /users/register endpoint.
The presentation layer (API) validates the request and passes it to the business logic, which validates the data and saves it to the database.
A confirmation is then returned to the user with a status code of 201 (Created).

## 2) Place Creation (Creation of Place)

<img width="1920" height="1080" alt="Diagrama_de_Creación_de_Lugar(Parte 2)" src="https://github.com/user-attachments/assets/045595a8-8b6a-488c-924c-73ccf6ec6244" />

## Explanation
- The user creates a new post by submitting their location data.
The API validates the information and passes it to the business layer, which performs additional validations and saves the location in the database.
The process ends by returning the ID of the created place to the user.

## 3) Review Submission (Submit Review)

<img width="1920" height="1080" alt="Diagrama_de_Envio_de_Reseña(Parte 2)" src="https://github.com/user-attachments/assets/40b8be64-fb61-4d11-a7b9-2fe558c08110" />

## Explanation
- The user submits a review associated with a place.
The API receives the request and delegates it to the business logic, which validates whether the user and the place exist and whether the review meets the rules.
The review is then stored and the user is responded to successfully.

## 4) Fetching a List of Places (List of Places)

<img width="1920" height="1080" alt="Diagrama_de_Listado de Lugares(Parte 2)" src="https://github.com/user-attachments/assets/3f213531-8f0d-405f-b720-e97677971528" />

## Explanation
- The user requests a list of places with certain filters (for example, city or price).
The API calls the business logic, which queries the database for the corresponding places.
Finally, the data is returned in JSON format with code 200 OK.

## 6. References:

- https://learn.microsoft.com/en-us/style-guide/ Microsoft Writing Style Guide

- https://developers.google.com/style Google Developer Documentation Style Guide

- https://www.uml-diagrams.org/ UML Diagrams Guide

- https://mermaid.js.org/ Mermaid.js Documentation
