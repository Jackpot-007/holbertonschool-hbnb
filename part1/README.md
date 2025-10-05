## Proyecto de diagramas UML

HBnB Project — Technical Architecture Document

Author: Lautaro Ezequiel y Lucas Noble

Version: 1.0

Document Type: Comprehensive Technical Architecture & Design

## 1. Introduction

- El presente documento técnico describe la arquitectura, el diseño y los principales componentes del proyecto HBnB.
Su propósito es servir como guía de referencia para el desarrollo e implementación del sistema, asegurando que cada parte de la aplicación mantenga una estructura coherente, modular y escalable.

- El documento compila los resultados de las siguientes etapas previas:

Parte 0: High-Level Package Diagram (Arquitectura por capas y patrón Facade).
Parte 1: Detailed Class Diagram for Business Logic Layer (Modelo de negocio).
Parte 2: Sequence Diagrams for API Calls (Flujo de interacción entre capas).

- Cada sección incluye diagramas UML representados en Mermaid.js, acompañados de explicaciones detalladas sobre las decisiones de diseño, las interacciones entre componentes y la responsabilidad de cada elemento dentro del sistema.

## Parte 0 – Diagrama de paquetes de alto nivel

## Descripción general:

- El sistema HBnB adopta una arquitectura en tres capas (Three-Layer Architecture) para lograr separación de responsabilidades, escalabilidad y mantenimiento eficiente.
- Las capas son:

Presentation Layer (Servicios / API): Gestiona la interacción con el usuario final, manejando peticiones HTTP y respuestas JSON a través de endpoints RESTful.

Business Logic Layer (Modelos): Implementa la lógica del negocio y define las entidades principales del sistema (User, Place, Review, Amenity).

Persistence Layer (Base de datos): Responsable del almacenamiento y recuperación de datos mediante objetos de acceso a datos (DAOs o repositorios).

- El patrón Facade actúa como interfaz unificada entre las capas, permitiendo que la capa de presentación se comunique con la lógica del negocio sin depender de los detalles internos de implementación.

<img width="400" height="800" alt="Diagrama_de_Paquetes(Parte 0)" src="https://github.com/user-attachments/assets/45c8266c-8e46-4259-89a4-07766223a3ca" />

## 1) Explicación de las capas del Diagrama de Paquetes:

## 1- Presentation Layer (Servicios, API)
- Responsabilidad: Gestiona la interacción entre el usuario y la aplicación.
- Componentes:
  - API: Expone endpoints (ejemplo: GET /places, POST /users).
  - Services: coordinan las peticiones y llaman al Facade.

## 2- Business Logic Layer (Modelos + Facade)
- Responsabilidad: Contiene las reglas de negocio y el núcleo de la aplicación.
- Componentes:
  - Modelos principales: User, Place, Review, Amenity.
  - Facade: interfaz unificada que recibe las peticiones desde la capa de presentación y gestiona la comunicación con la capa de persistencia.

## 3- Persistence Layer (Almacenamiento de datos)
- Responsabilidad: Maneja directamente la base de datos.
- Componentes:
  - DatabaseRepository: CRUD de las entidades.
  - StorageAdapter: abstrae la tecnología de la base de datos (MySQL, SQLite, etc.).

## Rol del Patrón Facade
El Facade actúa como intermediario entre la Presentation Layer y la Business Logic Layer:
  - Simplifica la interacción y reduce la complejidad.
  - Permite bajo acoplamiento: la capa de presentación no conoce detalles internos de los modelos ni de la persistencia.
  - Hace posible cambiar la lógica interna sin afectar la API pública.

Flujo simplificado:

[User Request] → API → Facade → Models → Repository → Database

## Parte 1. Diagrama de clases detallado para la capa de lógica de negocio

<img width="400" height="800" alt="Diagrama_de_Clases(Parte 1)" src="https://github.com/user-attachments/assets/84e290b0-e4ba-4268-bfed-e5fda288c597" />

## 1) Explicación de las entidades del Diagrama de Clases:

## Descripción general:

- Esta capa contiene la lógica central del sistema y los modelos que representan las entidades del dominio:

## 1- User (Usuario)
- Rol: Representa a los usuarios de la aplicación (propietarios, viajeros).
- Atributos principales:
  - id (UUID, identificador único).
  - name, email, password.
  - created_at, updated_at para trazabilidad.
- Métodos:
  - create(), update(), delete().

## 2- Place (Lugar)
- Rol: Representa los lugares o alojamientos disponibles.
- Atributos principales:
  - id, name, description, location.
  - price_per_night
  - created_at, updated_at.
- Métodos:
  - create(), update(), delete().

## 3- Review (Revisar)
- Rol: Representa las reseñas que los usuarios hacen sobre los lugares.
- Atributos principales:
  - id, content, rating.
  - created_at, updated_at.
- Métodos:
  - create(), update(), delete().

## Amenity (Amenidad)
- Rol: Representa las comodidades o servicios disponibles en un lugar.
- Atributos principales:
  - id, name, description.
  - created_at, updated_at.
- Métodos:
  - create(), update(), delete().

## Relaciones entre Entidades
- User – Place:
  - Un User puede poseer varios Place.
- User – Review:
  - Un User puede escribir varias Review.
- Place – Review:
  - Un Place puede tener múltiples Review.
- Place – Amenity:
  - Relación muchos a muchos: un Place puede tener varios Amenity, y un Amenity puede estar en varios Place.

## Parte 2. Diagramas de secuencia para llamadas a la API

## Descripción general:

- Los diagramas de secuencia muestran cómo se comunican las capas para procesar las peticiones de la API, reflejando el flujo de información desde el usuario hasta la base de datos y de regreso.

## 1) User Registration (Diagrama de Registro de Usuario)

<img width="1920" height="1080" alt="Diagrama_de_Registro_de_Usuario(Parte 2)" src="https://github.com/user-attachments/assets/33eb3505-c91e-4152-8b2b-54abb78f4568" />

## Explicación
- El usuario envía sus datos de registro al endpoint /users/register.
La capa de presentación (API) valida la solicitud y la pasa a la lógica de negocio, que valida los datos y los guarda en la base de datos.
Luego se devuelve una confirmación al usuario con un código de estado 201 (Created).

## 2) Place Creation (Creación de Lugar)

<img width="1920" height="1080" alt="Diagrama_de_Creación_de_Lugar(Parte 2)" src="https://github.com/user-attachments/assets/045595a8-8b6a-488c-924c-73ccf6ec6244" />

## Explicación
- El usuario crea una nueva publicación enviando los datos de su lugar.
La API valida la información y la pasa a la capa de negocio, que realiza las validaciones adicionales y guarda el lugar en la base de datos.
El proceso termina devolviendo el ID del lugar creado al usuario.

## 3) Review Submission (Envío de Reseña)

<img width="1920" height="1080" alt="Diagrama_de_Envio_de_Reseña(Parte 2)" src="https://github.com/user-attachments/assets/40b8be64-fb61-4d11-a7b9-2fe558c08110" />

## Explicación
- El usuario envía una reseña asociada a un lugar.
La API recibe la solicitud y la delega a la lógica de negocio, que valida si el usuario y el lugar existen y si la reseña cumple las reglas.
Luego se almacena la reseña y se responde al usuario con éxito.

## 4) Fetching a List of Places (Listado de Lugares)

<img width="1920" height="1080" alt="Diagrama_de_Listado de Lugares(Parte 2)" src="https://github.com/user-attachments/assets/3f213531-8f0d-405f-b720-e97677971528" />

## Explicación
- El usuario solicita una lista de lugares con ciertos filtros (por ejemplo, ciudad o precio).
La API llama a la lógica de negocio, que consulta a la base de datos los lugares correspondientes.
Finalmente, se devuelven los datos en formato JSON con código 200 OK.

## 6. Referencias:

- https://learn.microsoft.com/en-us/style-guide/ Microsoft Writing Style Guide

- https://developers.google.com/style Google Developer Documentation Style Guide

- https://www.uml-diagrams.org/ UML Diagrams Guide

- https://mermaid.js.org/ Mermaid.js Documentation
