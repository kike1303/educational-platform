# Plan para Escalar la Arquitectura de la Plataforma Educativa

La plataforma educativa actual se basa en un monolito de backend desarrollado en Python, que interactúa con un frontend React, una base de datos, un Data Warehouse, un proveedor de notificaciones y un proveedor de tracking. Con el crecimiento esperado de la plataforma, es esencial escalar la arquitectura para mantener el rendimiento, la mantenibilidad del código y la experiencia del usuario.


## Desglose del problema

Para lograr la escalabilidad, performance y mantenibilidad deseada, es necesario desacoplar el monolito en microservicios que permitan desarrollar, probar, desplegar y escalar componentes de manera independiente. Esto implica:

- **Identificar los Dominios**: Dividir el monolito en áreas de funcionalidad que puedan ser manejadas como servicios independientes.

- **Desacoplamiento de Datos**: Cada microservicio debería tener su propia base de datos para evitar dependencias entre ellos.

- **Comunicación entre Servicios**: Definir cómo se comunicarán los microservicios entre sí de forma eficiente y segura.

- **Escalado Independiente**: Los microservicios deben ser capaces de escalar independientemente de acuerdo a la demanda de sus respectivas áreas.

- **Monitoreo y Mantenimiento**: Implementar herramientas y prácticas que permitan monitorear la salud de los servicios y facilitar su mantenimiento.


## Dominios y sus responsabilidades

### Autenticación y Autorización:
Manejo de diferentes tipos de autenticación y autorización para los usuarios.

Este dominio es crítico ya que controla el acceso a la plataforma y define qué acciones pueden realizar los usuarios.

**Responsabilidades del Servicio de Autenticación y Autorización:**

1. **Gestionar Diferentes Métodos de Autenticación**: Permitir que los usuarios se autentiquen utilizando diferentes métodos como Basic Auth, Facebook, Google, SSO, y OAuth.

1. **Gestionar Permisos y Roles**: Definir y administrar roles de usuario (por ejemplo, profesor, alumno, padre) y asignar permisos específicos a estos roles.

1. **Validación de Tokens**: Validar tokens de acceso para asegurar que las solicitudes a la API estén autorizadas.

1. **Revocación de Acceso**: Proveer mecanismos para revocar el acceso cuando sea necesario, por ejemplo, en caso de comportamiento sospechoso.


### Gestión de Cursos:

Manejo de cursos, clases, profesores, alumnos y padres, y sus relaciones.

Este dominio es el núcleo de la plataforma educativa y abarca la creación y gestión de cursos, clases, profesores, alumnos y padres.

**Responsabilidades del Servicio de Gestión de Cursos:**

1. **CRUD de Cursos y Clases**: Proporcionar operaciones de creación, lectura, actualización y eliminación para cursos y clases.

1. **Asociación de Profesores, Alumnos y Padres**: Permitir la asociación de profesores a cursos, inscripción de alumnos a clases y asociación de alumnos con sus padres.

1. **Gestión de Calendarios**: Manejar fechas y horarios de clases.

1. **Proveer APIs para Frontend**: Exponer APIs para que el frontend de React pueda consumir y mostrar la información relacionada con los cursos.


### Notificaciones:

Enviar emails transaccionales y notificaciones de marketing.

Este dominio es responsable de mantener a los usuarios informados sobre eventos importantes y acciones de marketing.

**Responsabilidades del Servicio de Notificaciones:**

1. **Envío de Emails Transaccionales**: Enviar emails en respuesta a eventos específicos, como la inscripción en un curso.

1. **Integración con Proveedores de Marketing**: Integrarse con servicios externos para enviar campañas de marketing.

1. **Gestión de Preferencias de Notificación**: Permitir a los usuarios gestionar sus preferencias de notificación.


### Tracking de Comportamiento de Usuarios:

Seguimiento del comportamiento de los usuarios y sincronización con proveedores externos.

Este dominio se encarga de registrar y analizar cómo los usuarios interactúan con la plataforma. Es importante para entender el comportamiento del usuario y para la optimización de la experiencia del usuario.

**Responsabilidades del Servicio de Tracking de Comportamiento de Usuarios:**

1. **Registrar Eventos de Usuario**: Capturar eventos importantes como compras, finalización de cursos, y otras interacciones del usuario.

1. **Integración con Proveedores de Tracking**: Sincronizar la información de comportamiento del usuario con proveedores externos para análisis más profundos.

1. **Análisis de Comportamiento**: Realizar análisis básicos del comportamiento del usuario que pueda ser útil para mejorar la plataforma.

### Integración de Data Warehouse:

Consumir APIs para completar la información en un Data Warehouse.

Este dominio se centra en la recopilación, almacenamiento y análisis de datos para ayudar en la toma de decisiones basada en datos y la identificación de oportunidades.

**Responsabilidades del Servicio de Integración de Data Warehouse:**

1. **Recopilación de Datos**: Recoger datos de diferentes partes de la aplicación, como el comportamiento del usuario, datos de cursos, y más.

1. **Transformación y Almacenamiento**: Transformar los datos recogidos en un formato que sea óptimo para el análisis y almacenarlo en un Data Warehouse.

1. **Análisis de Datos**: Proporcionar herramientas y capacidades para que los analistas de datos puedan analizar los datos almacenados y generar insights.

1. **Exposición de Datos a Otros Servicios**: Si es necesario, exponer ciertos conjuntos de datos a otros servicios a través de APIs.

## Enfoque y plan inicial

- **Servicio de Autenticación y Autorización**. Es fundamental garantizar que los usuarios puedan acceder de manera segura a la plataforma. Además, como es la base de la interacción del usuario, es crucial tener esto en su lugar.

- **Servicio de Gestión de Cursos**. Esto permitirá que la plataforma funcione como una solución educativa, que el core de todo.

- **Servicio de Notificaciones**. Esto permitirá mantener a los usuarios informados, garantizar transparencia en la compra de los cursos. Crear campañas de marketing de manera eficiente para hacer despegar el negocio, etc.

## Estándares de Microservicios y Elecciones Tecnológicas

- **Stack tecnológico**: Teniendo en cuenta que el monolito fue desarrollado en python, y que lo más probable es que el personal de backend en la compañía tenga conocimientos en este lenguaje, haría los microservicios en python y el frontend lo mantendría en React. Aunque sea posible usar distintos lenguajes por microservicio, la idea sería mantener un stack fijo con sus estandares bien definido. Quizá se podría agregar un segundo lenguaje en el backend, como Golang, para microservicios que necesitemos que sean de alto rendimiento y velocidad.

- **Comunicación**: Utilizar REST para la comunicación entre servicios. REST es simple y ampliamente adoptado.

- **Seguridad**: Implementar OAuth 2.0 o JWT para asegurar la comunicación entre servicios.

- **Contenedorización**: Utilizar Docker para contenedorizar y orquestación de los microservicios.

- **Monitoreo y Logging**: Integrar soluciones de monitoreo y registro como DataDog.

***

Es importante destacar que en el proceso de desglose, es necesario garantizar que la separación en microservicios no afecte la consistencia de los datos ni la experiencia del usuario. Esto se puede lograr a través de patrones de microservicios como API Gateway o Event Sourcing.

Además, en la transición hacia microservicios, es fundamental garantizar la capacitación del equipo en las nuevas tecnologías y prácticas adoptadas, así como establecer procesos claros de desarrollo y despliegue.

En conjunto, estos cinco dominios y sus microservicios asociados forman una arquitectura más modular y escalable en comparación con el monolito original. Al separar responsabilidades en microservicios específicos, se facilita el mantenimiento, se mejora la capacidad de escalar de forma independiente y se acelera el tiempo de entrega de nuevas características. Además, esto permite que los equipos de desarrollo trabajen en paralelo en diferentes dominios sin interferir entre sí, lo que aumenta la eficiencia general del proceso de desarrollo.

***

### Diagrama de Plataforma Educacional



***

## API de Servicio de Gestión de Cursos

```
openapi: 3.0.3
info:
  title: Servicio de Gestión de Cursos
  description: API para la gestión de cursos, clases, profesores, alumnos y padres. Actualmente solo está el CRUD de cursos y algunos esquemas :)
  version: 1.0.0
servers:
  - url: 'https://api.example.com/v1'
paths:
  /courses:
    get:
      summary: Obtiene una lista de todos los cursos
      responses:
        '200':
          description: Lista de cursos
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Course'
    post:
      summary: Crea un nuevo curso
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Course'
      responses:
        '201':
          description: Curso creado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Course'
  /courses/{courseId}:
    get:
      summary: Obtiene un curso específico
      parameters:
        - name: courseId
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Detalles del curso
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Course'
    put:
      summary: Actualiza un curso específico
      parameters:
        - name: courseId
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Course'
      responses:
        '200':
          description: Curso actualizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Course'
    delete:
      summary: Elimina un curso específico
      parameters:
        - name: courseId
          in: path
          required: true
          schema:
            type: integer
      responses:
        '204':
          description: Curso eliminado
components:
  schemas:
    Course:
      type: object
      properties:
        id:
          type: integer
          readOnly: true
        name:
          type: string
          maxLength: 255
        description:
          type: string
        teacherId:
          type: integer
        students:
          type: array
          items:
            $ref: '#/components/schemas/Student'
    Student:
      type: object
      properties:
        id:
          type: integer
          readOnly: true
        name:
          type: string
          maxLength: 255
        parent:
          $ref: '#/components/schemas/Parent'
    Parent:
      type: object
      properties:
        id:
          type: integer
          readOnly: true
        name:
          type: string
          maxLength: 255


```

