# Capítulo IV: Product Architecture Design

## 4.1. Design Concepts, ViewPoints & ER Diagrams

### 4.1.1. Principles Statements

Con el fin de garantizar que nuestra solución evolucione de forma ordenada y sostenible, resulta fundamental definir principios y lineamientos que orienten el diseño y desarrollo del software. Estos principios proporcionan una base sólida para asegurar que el sistema sea seguro, flexible y de fácil mantenimiento, además de favorecer la escalabilidad y la calidad del código a lo largo del tiempo.

#### Principio de Privacidad y Seguridad de la Información:

Todas las funcionalidades deben incorporar mecanismos de protección de datos personales. Esto incluye una autenticación segura, control de acceso basado en roles y encriptación de información sensible, garantizando la confianza de los ciudadanos.

#### Principio de Usabilidad y Experiencia de Usuario:

El sistema debe ofrecer una interfaz clara, intuitiva y accesible. Los flujos de interacción deben estar centrados en el usuario, con validación constante mediante pruebas de usabilidad, para fomentar la adopción y el uso continuo de la aplicación.

#### Principio de Escalabilidad y Rendimiento:

La arquitectura de PeaceApp debe soportar el crecimiento en volumen de reportes y usuarios sin degradar la experiencia. Se privilegiarán patrones y tecnologías que permitan balancear cargas y mantener un tiempo de respuesta óptimo.

#### Modularidad y Desacoplamiento:

El diseño del sistema debe estructurarse en componentes independientes y reutilizables. Esto reduce el impacto de los cambios, facilita la evolución del sistema y permite escalar de forma diferenciada los módulos críticos como notificaciones, mapas o geolocalización.

#### Preferencia por asincronía:

Cuando sea posible, se priorizarán las llamadas asincrónicas frente a las sincrónicas, para mejorar el rendimiento, la tolerancia a fallos y la capacidad de respuesta en escenarios de alta concurrencia.

#### Uso de librerías con soporte comercial:

Se favorecerá la adopción de bibliotecas y frameworks que cuenten con respaldo empresarial o comunidades activas, garantizando soporte técnico, estabilidad y actualizaciones constantes.

#### Alineamiento con principios SOLID:

Se aplicarán los principios de Responsabilidad Única, Abierto-Cerrado, Segregación de Interfaces e Inversión de Dependencias para asegurar un diseño orientado a objetos claro, extensible y de bajo acoplamiento.

### 4.1.2. Approaches Statements Architectural Styles & Patterns

En esta sección describimos los enfoques fundamentales que tendremos en cuenta durante el desarrollo de PeaceApp. Estos enfoques nos brindan una base conceptual que guía nuestras decisiones frente a los retos de arquitectura y diseño que enfrentamos. Con ellos aseguramos que la aplicación evolucione de forma coherente con nuestra visión de seguridad, usabilidad y escalabilidad.

#### Approaches Statements:

##### Domain-Driven Design (DDD): 

La complejidad del dominio de seguridad ciudadana se abordará mediante la separación clara de contextos (reportes de incidentes, geolocalización, autenticación, alertas), lo que facilitará mantener un modelo de negocio alineado con las reglas y procesos reales.

##### Enfoque Centrado en el Usuario (UCD):

Dado que PeaceApp busca generar confianza en situaciones de seguridad ciudadana, se prioriza la investigación con usuarios, pruebas de usabilidad y diseño de interfaces accesibles para diferentes perfiles de ciudadanos.

##### Agile Software Development:

Se adoptará un marco ágil con iteraciones cortas que permitan lanzar versiones funcionales tempranas, validar hipótesis con usuarios y ajustar funcionalidades en base a feedback real.

##### Continuous Integration & Continuous Deployment (CI/CD):

Se automatizará la construcción, pruebas y despliegue de cada iteración para asegurar calidad, confiabilidad y tiempo de respuesta rápido frente a nuevas necesidades.

##### Unit Testing & Refactoring:

Realizaremos validación continua de componentes, junto con prácticas de refactorización, lo que reforzará la mantenibilidad del sistema y reducirá riesgos de regresión.

#### Architectural Styles & Patterns:

##### Arquitectura basada en Microservicios:

Cada módulo crítico (autenticación, geolocalización, gestión de reportes, notificaciones) se implementa como un servicio independiente. Esto facilita el escalamiento selectivo, mejora la resiliencia y permite desplegar nuevas funcionalidades sin afectar todo el sistema.

##### Patrón Cliente-Servidor:

Tanto la aplicación móvil como web actúan como cliente que interactúa con servicios backend mediante APIs RESTful seguras, asegurando separación de responsabilidades.

##### Patrón Repositorio:

El acceso a datos se implementará mediante este patrón para separar la lógica de negocio de la persistencia, facilitando cambios futuros en la base de datos sin afectar otros componentes del sistema.

### 4.1.3. Context Diagram

El diagrama de contexto representa a PeaceApp, una aplicación móvil y web orientada a la seguridad ciudadana que permite a los usuarios reportar incidentes, visualizar zonas de riesgo y compartir su ubicación en tiempo real. Los actores que interactúan con el sistema son el Citizen, quien utiliza la aplicación para mantenerse informado y enviar reportes, y el Admin, encargado de gestionar cuentas, reportes y alertas dentro de la plataforma. A su vez, PeaceApp consume los servicios externos del Map System, para obtener datos de geolocalización y mapas, así como del SMS Gateway y la WhatsApp API, utilizados para enviar alertas y compartir ubicaciones con contactos de confianza fuera de la aplicación.

![](assets/structurizr-83580-SystemContext.png)

### 4.1.4. Approach driven ViewPoints Diagrams

#### 4.1.4.1. Diagrama de Actividad:

El diagrama de actividades de PeaceApp describe el flujo de acciones del usuario desde el inicio de sesión o registro hasta la interacción con las funcionalidades principales de la aplicación, comenzando con el acceso al mapa que muestra los reportes realizados por la comunidad o vacío si no existen, y que además envía notificaciones cuando el usuario se encuentra cerca de una zona de riesgo; desde allí, el usuario puede acceder a la pestaña de reportes para visualizar todos los incidentes o solo los propios, crear nuevos reportes seleccionando tipo, título, descripción, ubicación y evidencia, con opción de eliminarlos en caso de error; también puede gestionar su información personal en la pestaña de perfil con la posibilidad de modificar datos o cerrar sesión, y finalmente compartir su ubicación en tiempo real con contactos de confianza mediante SMS o WhatsApp para reforzar su seguridad.

![](assets/DiagramadeActividad.png)

#### 4.1.4.2. Diagrama de Estado:

Para el diagrama de estado, diagramamos los procesos más importantes de PeaceApp

**Proceso del ciclo de vida de un reporte:** El diagrama de estados del reporte en PeaceApp representa las etapas que atraviesa un incidente desde su creación hasta su eliminación. El proceso inicia en un estado de “Sin reporte registrado”, desde donde el usuario puede crear un nuevo reporte seleccionando el tipo de incidente (robo, falta de iluminación, acoso, accidente u otro). Posteriormente, debe ingresar datos y evidencia, completando un formulario que valida la información. Si los campos no están completos, el sistema solicita correcciones; en caso contrario, el reporte pasa al estado de “Reporte creado”. Una vez creado, el reporte se vuelve “Reporte visible”, mostrándose en el mapa interactivo y en la pestaña de reportes. Si un usuario se encuentra cerca de la ubicación del reporte, este genera una “Alerta creada” que refuerza la seguridad preventiva. Finalmente, el reporte puede ser eliminado por el usuario, alcanzando el estado de “Reporte eliminado” y concluyendo su ciclo de vida dentro de la aplicación.

![](assets/CiclodevidadeunReporte.png)

Figura 2: Ciclo de vida de un Reporte con LucidChart

**Proceso del ciclo de compartir ubicación en PeaceApp:** El diagrama de estados de la funcionalidad de compartir ubicación en PeaceApp muestra el proceso que sigue un usuario desde el estado inicial de “Sin ubicación compartida”. El flujo comienza al acceder a la pestaña de ubicación, donde se verifica si el usuario otorgó permisos para acceder a su lista de contactos. En caso de no hacerlo, el sistema se mantiene en “Ubicación no compartida”; de lo contrario, el usuario puede seleccionar un contacto y elegir el canal de envío (SMS o WhatsApp). Posteriormente, se valida el acceso al servicio de mensajería: si no se autoriza, el estado vuelve a “Ubicación no compartida”; en caso afirmativo, la aplicación pasa al estado de “Ubicación enviada”, donde la información en tiempo real es compartida con el contacto seleccionado. El ciclo finaliza cuando el usuario detiene la acción o cierra sesión, retornando nuevamente al estado de “Ubicación no compartida”.

![](assets/CiclodevidadeunaUbicacion.png)

Figura 3: Ciclo de compartir ubicación en tiempo real con LucidChart

#### 4.1.4.3. Diagrama de Clase:

El diagrama de clases de PeaceApp modela la estructura principal del sistema, organizando los componentes en controladores, servicios y repositorios para mantener un diseño modular y desacoplado. Se representan clases clave como User, que gestiona la información de autenticación y las relaciones con los reportes y alertas; Report, que encapsula los datos de los incidentes creados por los usuarios; Alert, que administra las notificaciones generadas cuando un usuario se encuentra cerca de una zona de riesgo; y Location, que permite registrar y obtener coordenadas geográficas. Además, se incluyen los controladores y servicios de autenticación, notificaciones, reportes, pagos y localización, cada uno conectado a su respectivo repositorio para la persistencia de datos. Este diseño refleja la aplicación de principios SOLID y la arquitectura en capas, asegurando escalabilidad, seguridad y mantenibilidad en el sistema.

![](assets/ClassDiagram.png)

#### 4.1.4.4. Diagrama de Contenedores:

El diagrama de contenedores de PeaceApp ilustra cómo los usuarios del sistema, representados por los roles de Ciudadano y Administrador, interactúan con las diferentes interfaces y componentes de la solución. Los ciudadanos acceden a la Landing Page para obtener información general sobre la aplicación y utilizan tanto la Aplicación Web como la Aplicación Móvil y la Single Page Application (SPA) para gestionar reportes, recibir alertas y compartir su ubicación. Estas interfaces se comunican con un API Gateway RESTful, que centraliza las solicitudes y distribuye el tráfico hacia los distintos microservicios. En el backend se encuentran servicios especializados para la gestión de reportes, perfiles de usuario, autenticación, alertas y localización. La información se almacena en una Base de Datos Relacional, mientras que para funciones críticas como la mensajería y la geolocalización se integran servicios externos como el SMS Gateway, la API de WhatsApp y un Map System que provee datos cartográficos y de zonas de riesgo.
- API Gateway RESTful: Es el componente encargado de recibir todas las solicitudes provenientes de las aplicaciones cliente (Web, Móvil y SPA) y redirigirlas a los distintos microservicios. Implementado sobre JSON/HTTPS, el gateway administra rutas, seguridad y balance de peticiones, garantizando una capa de control centralizado.

##### Bounded Contexts (Microservicios):

- Authentication Service: Gestiona el inicio de sesión, registro de usuarios y autenticación mediante validación de credenciales.
- Profiles Service: Maneja los datos personales y las preferencias de los usuarios, permitiendo consultar y actualizar información de perfil.
- Reports Service: Administra la creación, edición, visualización y eliminación de reportes de incidentes, además de exponerlos en el mapa.
- Alerts Service: Genera y distribuye notificaciones a los usuarios cuando se encuentran en zonas cercanas a reportes activos.
- Location Service: Gestiona el registro y consulta de coordenadas de ubicación, permitiendo el envío de la ubicación en tiempo real y la integración con mapas.

Cada uno de estos microservicios expone su propia API y se comunica directamente con la base de datos relacional para almacenar y consultar información.

##### Servicios Externos:

- SMS Gateway: Servicio externo para enviar mensajes de texto con alertas y notificaciones a los contactos registrados.
- WhatsApp API: Servicio externo utilizado para compartir la ubicación en tiempo real mediante la plataforma de mensajería WhatsApp.
- Map System: Proveedor externo de información geográfica que permite mostrar mapas, ubicar incidentes y destacar zonas críticas en la aplicación.

![](assets/structurizr-83580-Containers.png)

### 4.1.5. Relational/Non Relational Database Diagram

El sistema de base de datos de PeaceApp está diseñado para dar soporte a la gestión integral de reportes de seguridad, alertas, usuarios, ubicaciones y pagos dentro de la aplicación. El modelo refleja una estructura relacional clara y modular, en la que los usuarios pueden registrar incidentes, recibir alertas de zonas de riesgo, compartir su ubicación en tiempo real, gestionar rutas seguras, así como realizar pagos y almacenar sus ubicaciones favoritas. Esta organización permite garantizar trazabilidad, consistencia y seguridad en los datos, facilitando el control de toda la información crítica del sistema.

![](assets/RelationalDatabaseDiagram.png)

- users: Almacena la información principal de los usuarios del sistema, incluyendo nombres, apellidos, teléfono, correo electrónico, credenciales y fechas de creación y actualización. Es la tabla central del modelo y se conecta con reportes, alertas, ubicaciones, favoritos y pagos.

- accounts: Contiene datos de las cuentas de usuario y sus credenciales de autenticación (nombre de usuario, contraseña cifrada, fechas de creación/actualización). Se vincula a la tabla users para habilitar el inicio de sesión y la gestión de credenciales.

- accounts_user: Representa la relación entre accounts y users, enlazando a cada usuario con su cuenta correspondiente mediante claves foráneas.

- reports: Registra los incidentes reportados por los usuarios, con atributos como fecha, título, descripción, tipo de reporte y estado. Se asocia con usuarios y ubicaciones para su trazabilidad.

- reports_user: Gestiona la relación entre reports y users, permitiendo identificar qué usuario creó cada reporte.

- alerts: Almacena las notificaciones generadas por los reportes, incluyendo fecha, detalle y estado. Se utilizan para avisar a los usuarios cercanos a zonas de riesgo.

- alerts_user: Define la relación entre alerts y users, permitiendo vincular qué usuarios reciben cada alerta generada.

- locations: Contiene las coordenadas geográficas (latitud, longitud) utilizadas para mapear reportes, rutas seguras y ubicaciones compartidas.

- location_reports: Tabla de relación entre reports y locations, que permite registrar la ubicación de cada reporte en el mapa.

- ubications_user: Permite gestionar las ubicaciones compartidas en tiempo real entre usuarios, incluyendo la fecha de inicio de la compartición y el usuario relacionado.

- favorites: Registra las ubicaciones o rutas marcadas como favoritas por los usuarios, vinculando a users y a locations para facilitar accesos rápidos.

- favorites_user: Relaciona a los usuarios con sus ubicaciones o rutas favoritas mediante claves foráneas.

- safe_routes: Contiene la información de rutas seguras definidas por los usuarios, asociadas a una ubicación registrada.

- payments: Almacena las transacciones realizadas en la aplicación, registrando el monto, método de pago, estado de la transacción y fechas de creación/actualización.

- payments_user: Relaciona las transacciones de la tabla payments con los usuarios que las realizaron, garantizando la trazabilidad de cada operación.

### 4.1.6. Design Patterns

El uso de patrones de diseño en PeaceApp permitirá desarrollar una solución extensible y confiable, donde cada módulo se mantenga desacoplado y fácilmente evolucionable. Estos patrones garantizan la reutilización de código, la reducción de complejidad y la alineación de la arquitectura con los principios de seguridad, escalabilidad y usabilidad definidos previamente.

#### Domain Driven Design (DDD)

- **Propósito:** Modelar el dominio de seguridad ciudadana (alertas, reportes, usuarios, ubicaciones) con un lenguaje cercano al negocio, usando tácticas como **Entidades, Objetos de Valor, Agregados y Repositorios**.  
- **Beneficio:** Permite que las entidades clave como *Usuario, Alerta, Ubicación* estén alineadas con la realidad del problema, facilitando la comunicación con stakeholders y la evolución del sistema frente a nuevos requerimientos (p. ej., integración con autoridades locales).

#### Strategy

- **Propósito:** Definir un conjunto de algoritmos o comportamientos intercambiables sin modificar el cliente.  
- **Beneficio:** Facilita implementar distintos **tipos de alertas** (robo, accidente, emergencia médica) con lógicas personalizadas sin alterar el flujo central de la aplicación.

#### Observer

- **Propósito:** Notificar automáticamente a múltiples suscriptores cuando ocurre un evento.  
- **Beneficio:** Cuando un usuario reporta una alerta, el sistema notifica en tiempo real a otros usuarios cercanos y, opcionalmente, a las autoridades, garantizando rapidez en la difusión de información.

#### Factory Method

- **Propósito:** Delegar la creación de objetos a subclases o métodos especializados para reducir el acoplamiento.  
- **Beneficio:** Permite crear objetos dinámicamente según el tipo de alerta o nivel de usuario (básico, premium, autoridad) sin modificar la lógica central.

#### Composite

- **Propósito:** Tratar de manera uniforme objetos individuales y composiciones.  
- **Beneficio:** Posibilita representar un **mapa de alertas** como una composición jerárquica (ciudad → distrito → barrio → alerta individual), simplificando la visualización y gestión de la información en distintos niveles de detalle.

#### Role-Based Access Control (RBAC)

- **Propósito:** Asignar permisos a roles y roles a usuarios para controlar accesos.  
- **Beneficio:** Diferencia los privilegios entre **usuarios comunes, moderadores y autoridades**, garantizando seguridad y personalización en las funcionalidades disponibles.


### 4.1.7. Tactics

## 4.2. Architectural Drivers

### 4.2.1. Design Purpose

### 4.2.2. Primary Functionality (Primary User Stories)

### 4.2.3. Quality Attribute Scenarios

### 4.2.4. Constraints

### 4.2.5. Architectural Concerns

## 4.3. ADD Iterations

### 4.3.X. Iteration N: <Iteration Name>

#### 4.3.X.1. Architectural Design Backlog N

#### 4.3.X.2. Establish Iteration Goal by Selecting Drivers

#### 4.3.X.3. Choose One or More Elements of the System to Refine

#### 4.3.X.4. Choose One or More Design Concepts That Satisfy the Selected Drivers

#### 4.3.X.5. Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

#### 4.3.X.6. Sketch Views (C4 & UML) and Record Design Decisions

#### 4.3.X.7. Analysis of Current Design and Review Iteration Goal (Kanban Board)
