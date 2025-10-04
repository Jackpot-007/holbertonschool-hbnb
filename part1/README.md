## Proyecto de diagramas UML

## Parte 0 – Diagrama de paquete de alto nivel

<img width="250" height="600" alt="Diagrama_de_Paquetes(Parte 0)" src="https://github.com/user-attachments/assets/45c8266c-8e46-4259-89a4-07766223a3ca" />

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

## Parte 1 – Diagrama de clases de lógica empresarial

<img width="250" height="600" alt="Diagrama_de_Clases(Parte 1)" src="https://github.com/user-attachments/assets/2e14af07-91f3-414d-a93b-f5af36c59cca" />
