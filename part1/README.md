## Proyecto de diagramas UML

## Parte 0 – Diagrama de paquetes de alto nivel

<img width="400" height="600" alt="Diagrama_de_Paquetes(Parte 0)" src="https://github.com/user-attachments/assets/45c8266c-8e46-4259-89a4-07766223a3ca" />

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

## Parte 1 –  Diagrama de clases detallado para la capa de lógica de negocio

<img width="250" height="600" alt="Diagrama_de_Clases(Parte 1)" src="https://github.com/user-attachments/assets/2e14af07-91f3-414d-a93b-f5af36c59cca" />

## 1) Explicación de las entidades del Diagrama de Clases:

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

## Parte 2 - Diagramas de secuencia para llamadas a la API

## 1) User Registration (Diagrama de Registro de Usuario)

<img width="250" height="600" alt="Diagrama_de_Registro_de_Usuario(Parte 2)" src="https://github.com/user-attachments/assets/33eb3505-c91e-4152-8b2b-54abb78f4568" />

## Explicación
- El usuario envía sus datos de registro al endpoint /users/register.
La capa de presentación (API) valida la solicitud y la pasa a la lógica de negocio, que valida los datos y los guarda en la base de datos.
Luego se devuelve una confirmación al usuario con un código de estado 201 (Created).

