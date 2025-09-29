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

En el marco del método ADD v3, los tactics representan las decisiones arquitectónicas específicas que permiten alcanzar los atributos de calidad definidos para el sistema. Mientras que los principles statements establecen lineamientos generales, los tactics se enfocan en acciones concretas que orientan el diseño técnico y la implementación de la solución. Para PeaceApp, se han definido tácticas orientadas a garantizar seguridad, disponibilidad, escalabilidad, mantenibilidad y usabilidad, respondiendo a las necesidades críticas del sistema y alineándose con la visión de negocio.

#### Seguridad

| Táctica | Descripción | Justificación |
|---------|-------------|---------------|
| Autenticación con JWT | Uso de tokens seguros para validar identidad de los usuarios. | Garantiza que solo usuarios autorizados accedan a la aplicación. |
| Cifrado de datos en tránsito (HTTPS/TLS) | Encriptación de la comunicación entre cliente y servidor. | Protege la información sensible como reportes y ubicaciones. |
| Control de accesos basado en roles | Definir permisos diferenciados para usuarios y autoridades. | Evita accesos indebidos y asegura un uso confiable de la plataforma. |

#### Disponibilidad

| Táctica | Descripción | Justificación |
|---------|-------------|---------------|
| Balanceo de carga | Distribuir peticiones entre múltiples servidores. | Asegura que la aplicación esté disponible incluso en picos de uso. |
| Replicación de base de datos | Mantener copias sincronizadas en diferentes nodos. | Minimiza el impacto de fallos y mejora la resiliencia. |
| Monitoreo proactivo | Uso de alertas y métricas en tiempo real. | Permite detectar y resolver problemas antes de que afecten a los usuarios. |

#### Escalabilidad

| Táctica | Descripción | Justificación |
|---------|-------------|---------------|
| Arquitectura basada en microservicios | Dividir el sistema en servicios independientes. | Facilita crecer por módulos sin afectar al resto de la aplicación. |
| Auto-escalado en la nube | Ajustar dinámicamente la capacidad de servidores según demanda. | Reduce costos y soporta incrementos en la carga de usuarios. |
| Cacheo de información | Uso de Redis u otra capa de caché. | Optimiza consultas frecuentes como reportes recientes o mapas de calor. |

#### Mantenibilidad

| Táctica | Descripción | Justificación |
|---------|-------------|---------------|
| Código modular y documentado | Separar componentes por responsabilidad. | Facilita la localización y corrección de errores. |
| Pruebas automatizadas | Integrar pruebas unitarias y de integración. | Reduce riesgos de fallos al introducir cambios. |
| Uso de patrones de diseño | Aplicar MVC o MVVM en la app móvil. | Mejora la organización y facilita la incorporación de nuevas funcionalidades. |

#### Usabilidad

| Táctica | Descripción | Justificación |
|---------|-------------|---------------|
| Diseño centrado en el usuario (UX) | Interfaces simples, consistentes y accesibles. | Incrementa la adopción de la aplicación y la confianza de los usuarios. |
| Retroalimentación inmediata | Confirmaciones y notificaciones visuales/sonoras al interactuar. | Genera confianza y sensación de control en el usuario. |
| Internacionalización y accesibilidad | Soporte multilenguaje y compatibilidad con accesibilidad móvil. | Permite llegar a un público más amplio, incluyendo personas con discapacidad. |


## 4.2. Architectural Drivers

### 4.2.1. Design Purpose

El propósito del diseño arquitectónico de PeaceApp es proporcionar una base estructurada, segura, escalable y mantenible que soporte de manera eficiente la gestión de reportes ciudadanos de incidentes en tiempo real. A través del uso de principios de diseño como la separación de responsabilidades, el uso de patrones arquitectónicos adecuados y la definición de capas claras, se busca facilitar la implementación coherente de funcionalidades esenciales orientadas a la seguridad ciudadana, garantizando un desarrollo modular y alineado con las necesidades reales del dominio urbano. Esta arquitectura permitirá al equipo de desarrollo incorporar nuevas características, escalar servicios críticos como notificaciones en tiempo real o geolocalización, integrar sistemas externos (por ejemplo, autoridades locales o servicios de emergencia), y mantener una alta calidad en la experiencia de usuario, incluso ante cambios en el entorno tecnológico o en los requerimientos sociales.

Para este proceso de diseño, nos basaremos en los siguientes objetivos y principios:

- Escalabilidad y Flexibilidad: La arquitectura debe adaptarse a cambios futuros, permitiendo la integración de nuevas funcionalidades y la expansión del sistema sin comprometer rendimiento o estabilidad.
- Modularidad y Reusabilidad: Fomentar un diseño modular que permita la reutilización de componentes y la incorporación de nuevas funcionalidades sin afectar el sistema existente.
- Desacoplamiento: Reducir dependencias entre componentes, facilitando actualizaciones, mantenimiento y resolución de problemas.
- Seguridad y Protección de Datos: Garantizar la protección de la información sensible de los usuarios mediante mecanismos de seguridad robustos y políticas de privacidad estrictas.
- Interoperabilidad: Asegurar que PeaceApp pueda interactuar con plataformas externas, dispositivos móviles y servicios de terceros, especialmente con autoridades y sistemas de emergencia.
- Mantenibilidad y Actualización Continua: Diseñar con visión a largo plazo para facilitar el mantenimiento y la evolución constante del sistema ante nuevas necesidades sociales o tecnológicas.
- Alta Disponibilidad y Confiabilidad: Garantizar que el sistema esté operativo de manera continua, incluso en contextos de alta demanda o emergencias.
- Experiencia de Usuario Óptima: Proporcionar una interacción fluida, intuitiva y confiable que incentive la participación ciudadana y eleve los niveles de confianza en la aplicación.

### 4.2.2. Primary Functionality (Primary User Stories)

A continuación, se detallan las funciones esenciales (user stories) que influyen de manera directa en la organización del sistema y orientan las decisiones del diseño arquitectónico de PeaceApp.

| User Story ID | Título                                             | Descripción                                                                                                                                                                                                              |
|---------------|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| US04          | Registro de Usuarios | Como usuario, quiero poder registrarme en la aplicación, para acceder a las funcionalidades de PeaceApp.                                                                                                                |
| US05          | Iniciar Sesión       | Como usuario registrado, quiero poder iniciar sesión con mi correo y contraseña, para acceder a mi cuenta.                                                                                                              |
| US06          | Generar Reporte de Incidentes  | Como usuario, quiero poder generar reportes de incidentes de seguridad, para contribuir a la actualización del mapa de calor.                                                                                 |
| US07          | Adjuntar Evidencia al Reporte  | Como usuario, quiero poder adjuntar fotos o videos al reporte, para dar mayor credibilidad y detalle al incidente reportado.                                                                                  |
| US08          | Visualización de Reportes        | Como ciudadano, quiero poder ver los reportes de otros usuarios sobre incidentes ocurridos en la zona, para estar al tanto de los eventos de seguridad.                                                     |
| US09          | Recibir Alertas de Zonas de Riesgo | Como ciudadano, quiero recibir alertas si me acerco a una zona de alto riesgo, para tomar las precauciones necesarias.                                                                                    |
| US10          | Compartir Ubicación con Contactos en la Aplicación Móvil | Como usuario de la aplicación móvil, quiero poder compartir mi ubicación con mis contactos cercanos, para que puedan monitorear mi trayecto y estar alertas ante cualquier peligro. |
| US13          | Acceder a Mapa con Reportes          | Como usuario, quiero poder ver un mapa interactivo con los reportes de incidentes en mi área, para tomar decisiones informadas sobre mi seguridad.                                                      |
| US15          | Filtrar Reportes         | Como usuario, quiero poder filtrar los reportes para ver todos los reportes o solo los que yo he creado, para gestionar mejor la información relevante según mis intereses.                                         |
| US16          | Buscar Ubicación en el Mapa         | Como usuario, quiero poder explorar reportes de seguridad en diferentes zonas del mapa, para tomar decisiones informadas sobre mis desplazamientos.                                                      |

### 4.2.3. Quality Attribute Scenarios

Los atributos de calidad determinados para PeaceApp son los que se detallan a continuación:

**a. Disponibilidad:**

Este atributo se trata de la capacidad del sistema para estar disponible y operativo cuando se necesita.
En nuestra solución, esto significa que los usuarios podrán acceder a la aplicación en cualquier momento para reportar incidentes de seguridad o recibir alertas en tiempo real.
La facilidad con la que se puede garantizar esta disponibilidad depende de una infraestructura confiable, redundante y con monitoreo constante.
Una alta disponibilidad transmite confianza al ciudadano, asegurando que la plataforma siempre esté lista para su uso sin interrupciones inesperadas, especialmente en situaciones críticas.

**b. Rendimiento:**

Este atributo se trata de la capacidad del sistema para responder con rapidez y eficiencia bajo distintas cargas de trabajo.
En nuestra solución, buscamos que los reportes de incidentes, el envío de alertas y las consultas de mapas se procesen en tiempo mínimo, evitando retrasos que puedan afectar la seguridad del usuario.
La facilidad con la que se mantiene un buen rendimiento se logra mediante una arquitectura optimizada, balanceo de carga y pruebas de estrés que validen la eficiencia del sistema.
Un alto rendimiento mejora la experiencia de los usuarios, reduciendo la frustración y asegurando que la información crítica llegue de forma oportuna.

**c. Escalabilidad:**

Este atributo se trata de la capacidad del sistema para crecer y adaptarse a un aumento de usuarios, datos o transacciones.
En nuestra solución, esto significa que la aplicación puede ampliarse fácilmente para atender a más distritos o ciudades, soportando un mayor volumen de alertas y usuarios sin perder eficiencia.
La facilidad con la que se logra esta escalabilidad proviene de una arquitectura modular y distribuida, diseñada desde el inicio para soportar crecimiento.
Un sistema escalable asegura la continuidad del servicio y la adopción masiva sin necesidad de rediseños costosos.

**d. Mantenibilidad:**

Este atributo se trata de la capacidad del sistema para ser modificado fácilmente cuando se necesitan cambios o mejoras.
En nuestra solución, el código será limpio, modular y bien documentado, lo cual facilita la detección y corrección de errores, así como la integración de nuevas funcionalidades (ejemplo: chat en tiempo real con autoridades).
La facilidad con la que se mantiene y mejora el sistema reduce los tiempos de desarrollo, evita errores nuevos y permite adaptarse a las necesidades cambiantes de la comunidad.
Una alta mantenibilidad asegura la evolución constante de PeaceApp con bajo riesgo y esfuerzo.

**e. Usabilidad:**

Este atributo se trata de la capacidad del sistema para ser entendido y utilizado con facilidad por los usuarios finales.
En nuestra solución, nos enfocamos en una interfaz clara, intuitiva y accesible, que permita a los ciudadanos reportar incidentes o consultar alertas de manera rápida incluso en momentos de emergencia.
La facilidad con la que un usuario interactúa con la aplicación depende de un diseño UX/UI bien estructurado, retroalimentación visual clara y soporte para accesibilidad.
Una alta usabilidad fomenta la adopción de la plataforma, reduce la curva de aprendizaje y genera confianza en los ciudadanos para seguir utilizándola.

**f. Seguridad:**

Este atributo se trata de la capacidad del sistema para proteger la confidencialidad, integridad y disponibilidad de la información.
En nuestra solución, esto implica implementar autenticación robusta, encriptación de datos y control de acceso según roles (usuario, autoridad, administrador).
La facilidad con la que se garantiza la seguridad depende de políticas claras de protección de datos y del uso de estándares internacionales (ejemplo: OWASP).
Una alta seguridad incrementa la confianza de los usuarios y asegura que la información sensible no sea vulnerada ni utilizada de forma indebida.

**a. Disponibilidad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | Un ciudadano necesita reportar un incidente en la calle a través de la app. |
| **Fuente de Estímulo**  | Ciudadano que usa la app                                                    |
| **Estímulo**            | Intenta acceder a la app para reportar un incidente en la calle             |
| **Medioambiente**       | El sistema se encuentra bajo condiciones normales de operación              |
| **Artefacto**           | Servidor y frontend de PeaceApp                                             |
| **Respuesta**           | El sistema responde y permite el registro del incidente sin caída del servicio |
| **Medida de Respuesta** | Tiempo de disponibilidad ≥ 99.5% mensual                                    |

**b. Rendimiento:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | Un usuario envía un reporte con foto y geolocalización en horario concurrido. |
| **Fuente de Estímulo**  | Usuario enviando un reporte                                                 |
| **Estímulo**            | Envío de reporte de incidente con foto y geolocalización                    |
| **Medioambiente**       | Sistema con 1000 usuarios concurrentes                                      |
| **Artefacto**           | API de backend y base de datos                                              |
| **Respuesta**           | Procesa y guarda el reporte en la base de datos                             |
| **Medida de Respuesta** | Tiempo de respuesta ≤ 3 segundos                                            |

**c. Escalabilidad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | El servicio se expande a nuevos distritos y la cantidad de usuarios se duplica. |
| **Fuente de Estímulo**  | Municipalidad que amplía la cobertura                                       |
| **Estímulo**            | Aumento de usuarios al extender el servicio a nuevos distritos              |
| **Medioambiente**       | Sistema en crecimiento exponencial de usuarios y transacciones              |
| **Artefacto**           | Arquitectura distribuida en la nube                                         |
| **Respuesta**           | Se despliegan nuevas instancias y balanceadores automáticamente             |
| **Medida de Respuesta** | El sistema soporta +10,000 usuarios sin degradación notable del rendimiento |

**d. Mantenibilidad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | El equipo de desarrollo necesita agregar un módulo de comunicación con autoridades. |
| **Fuente de Estímulo**  | Equipo de desarrollo                                                        |
| **Estímulo**            | Necesidad de agregar un nuevo módulo de comunicación directa con autoridades |
| **Medioambiente**       | Sistema en operación con versiones previas estables                         |
| **Artefacto**           | Código fuente modular de PeaceApp                                           |
| **Respuesta**           | Se implementa el nuevo módulo sin afectar funcionalidades existentes        |
| **Medida de Respuesta** | Cambios implementados en ≤ 2 sprints sin errores críticos en producción     |

**e. Usabilidad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | Un ciudadano bajo estrés necesita presionar el botón de emergencia.          |
| **Fuente de Estímulo**  | Ciudadano que reporta un incidente                                          |
| **Estímulo**            | Dificultad para ubicar el botón de “Emergencia”                             |
| **Medioambiente**       | App utilizada en situación de estrés (robo, accidente, etc.)                |
| **Artefacto**           | Interfaz gráfica del usuario (UI/UX)                                        |
| **Respuesta**           | Se muestra un botón visible y accesible en la pantalla principal            |
| **Medida de Respuesta** | 90% de usuarios logra reportar en ≤ 5 segundos durante pruebas de usabilidad |

**f. Seguridad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | Un atacante intenta ingresar al sistema usando credenciales robadas.        |
| **Fuente de Estímulo**  | Atacante externo                                                            |
| **Estímulo**            | Intenta acceder a la base de datos con credenciales robadas                 |
| **Medioambiente**       | Bajo ataque de fuerza bruta en horario crítico                              |
| **Artefacto**           | Sistema de autenticación y base de datos                                    |
| **Respuesta**           | Bloquea el intento, activa alertas y mantiene la integridad de los datos    |
| **Medida de Respuesta** | 100% de accesos no autorizados bloqueados; logs generados y auditados en < 1min |


### 4.2.4. Constraints

La tabla a continuación muestra las limitaciones que deben considerarse dentro del desarrollo de PeaceApp.

| **ID**   | **Constraint** | **Restricción** | **Impacto** |
|----------|----------------|-----------------|-------------|
| **CON-01** | Conectividad Urbana Variable | En muchas zonas urbanas la conectividad móvil puede ser inestable o intermitente. | La aplicación debe permitir funcionamiento básico offline (ej. registro temporal de incidentes) y sincronización automática cuando haya conexión. |
| **CON-02** | Presupuesto y Recursos | El proyecto cuenta con recursos financieros y humanos limitados. | Se priorizará el uso de tecnologías open source y librerías con soporte comunitario/comercial, enfocándose en un MVP funcional antes de ampliar funcionalidades. |
| **CON-03** | Cumplimiento Normativo en Protección de Datos | PeaceApp debe cumplir con la Ley de Protección de Datos Personales en Perú (Ley N.° 29733) y normativas internacionales de privacidad. | Requiere incorporar cifrado de datos, autenticación segura, consentimiento explícito de usuarios y auditoría de accesos. |
| **CON-04** | Integración con Infraestructura Municipal y Policial | La aplicación busca colaborar con entidades municipales y policiales que tienen sus propios sistemas de información. | Es necesario desarrollar APIs estandarizadas y protocolos de intercambio de datos, garantizando compatibilidad e interoperabilidad. |
| **CON-05** | Diversidad de Dispositivos | Los usuarios utilizarán teléfonos móviles de diferentes gamas (alta, media, baja). | La aplicación debe ser ligera, optimizada en rendimiento y compatible con Android/iOS en versiones aún usadas en el mercado. |
| **CON-06** | Tiempo de Entrega | El proyecto debe contar con un prototipo funcional para ser validado en pruebas piloto en un plazo corto. | Se debe adoptar un enfoque ágil (Scrum/Lean UX), entregando un MVP con las funcionalidades críticas: alertas, geolocalización y mapa de incidentes. |
| **CON-07** | Sostenibilidad a Largo Plazo | La continuidad del sistema depende de su mantenimiento y financiamiento posterior. | Influirá en la elección de arquitecturas escalables y de bajo mantenimiento, así como en la búsqueda de convenios con municipalidades y ONGs. |


### 4.2.5. Architectural Concerns

Las consideraciones arquitectónicas importantes para nuestro proyecto que aseguran el cumplimiento de los requisitos del negocio y la satisfacción de las expectativas de los interesados son las siguientes:

| **ID**   | **Architectural Concern** | **Descripción** | **Impacto en la Arquitectura** |
|----------|----------------------------|-----------------|--------------------------------|
| **ARC-01** | Protección de Datos Personales | Los usuarios comparten información sensible (ubicación en tiempo real, reportes de incidentes). | Requiere aplicar cifrado en tránsito y en reposo, políticas de anonimización y control de accesos estrictos. |
| **ARC-02** | Escalabilidad en Altas Cargas | En situaciones de emergencia pueden generarse picos de tráfico por múltiples reportes simultáneos. | La arquitectura debe escalar horizontalmente (ej. balanceadores de carga, microservicios) para mantener tiempos de respuesta aceptables. |
| **ARC-03** | Disponibilidad y Resiliencia | La aplicación debe estar disponible 24/7, incluso frente a fallas en servidores o cortes de red. | Se deben diseñar mecanismos de redundancia, replicación de datos y recuperación ante desastres (DR). |
| **ARC-04** | Integración con Entidades Externas | PeaceApp debe interoperar con sistemas de municipalidades, serenazgo y policía. | Implica diseñar APIs seguras y estandarizadas, además de considerar latencia y confiabilidad en el intercambio de datos. |
| **ARC-05** | Experiencia de Usuario (UX) | La interfaz debe ser clara y rápida de usar, especialmente en contextos de estrés. | Obliga a un diseño minimalista, accesible y validado con pruebas de usabilidad. |
| **ARC-06** | Gestión de Incidentes Falsos o Maliciosos | Los usuarios podrían enviar reportes falsos que perjudiquen la confianza en la plataforma. | Se requieren mecanismos de validación (ej. reputación de usuario, moderación). |
| **ARC-07** | Sostenibilidad y Mantenibilidad | La aplicación debe evolucionar a largo plazo sin costos excesivos de mantenimiento. | Se deben usar arquitecturas modulares, componentes desacoplados y tecnologías ampliamente soportadas. |

## 4.3. ADD Iterations

### 4.3.1 Iteration 1: Optimización de Procesos Clave para la Seguridad Ciudadana
### 4.3.1.1 Architectural Design Backlog 1

Se definen los impulsores (drivers) que conforman la primera iteración del método ADD, centrada en la optimización del proceso de reporte, visualización y comunicación de incidentes en tiempo real.

| ID   | Título                          | Scenario                                                                                                    | Quality Attribute | Driver                                                                 |
|------|---------------------------------|------------------------------------------------------------------------------------------------------------|-------------------|------------------------------------------------------------------------|
| US04 | Registro de Usuarios            | Como usuario, quiero poder registrarme en la aplicación, para acceder a las funcionalidades de PeaceApp.   | Seguridad         | Garantizar registro seguro y almacenamiento cifrado de datos personales. |
| US06 | Generar Reporte de Incidentes   | Como usuario, quiero poder generar reportes de incidentes de seguridad, para contribuir al mapa de calor.  | Usabilidad        | Ofrecer una interfaz rápida e intuitiva para reportar incidentes en tiempo real. |
| US08 | Visualización de Reportes       | Como ciudadano, quiero poder ver los reportes de otros usuarios, para estar al tanto de los eventos.       | Rendimiento       | Asegurar tiempos de carga menores a 2 segundos para mostrar listados de reportes. |
| US09 | Recibir Alertas de Zonas de Riesgo | Como ciudadano, quiero recibir alertas si me acerco a una zona de alto riesgo.                            | Disponibilidad    | Implementar notificaciones push en tiempo real con alta tasa de entrega. |

### 4.3.1.2 Establish Iteration Goal by Selecting Drivers

Se establecen las metas de iteración para cada atributo de calidad elegido.

| ID   | Quality Attribute | Scenario                                                                                                                                     |
|------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| US06 | Usabilidad       | Proporcionar a los ciudadanos una experiencia intuitiva para registrar incidentes en tiempo real. Esto permitirá la adopción masiva de la plataforma y la actualización constante de datos de seguridad. |
| US08 | Rendimiento      | Garantizar que la visualización del mapa de calor interactivo sea rápida, con tiempos de respuesta inferiores a 2 segundos, asegurando la confiabilidad de la información al momento de desplazarse. |
| US09 | Disponibilidad   | Asegurar que las notificaciones push se entreguen de forma inmediata y confiable, reduciendo riesgos al mantener a los usuarios informados en situaciones críticas. |
| US04 | Seguridad        | Implementar mecanismos de autenticación y encriptación de datos para proteger la información sensible de los usuarios, fortaleciendo la confianza en la aplicación. |

### 4.3.1.3 Choose One or More Elements of the System to Refine

En esta iteración, el equipo se enfocará en refinar los siguientes elementos clave del sistema, alineados a los drivers identificados (usabilidad, rendimiento, disponibilidad y seguridad):

| Elemento                                           | Mejoras contempladas                                                                                                      |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| **Interfaz de Usuario – Módulo de Reportes y Mapas Interactivos** | - Inputs claros y validados para reportar incidentes (tipo, ubicación, hora).<br>- Mapas interactivos con filtros de incidentes y rutas seguras.<br>- Navegación simple y soporte de accesibilidad (colores contrastados y textos legibles). |
| **Backend – Procesamiento de Reportes y Generación del Mapa de Calor** | - Almacenamiento estructurado de incidentes con validación automática.<br>- Algoritmos de agregación para mapas de calor dinámicos.<br>- Optimización de consultas para tiempos de respuesta menores a 2 segundos. |
| **Módulo de Notificaciones y Alertas en Tiempo Real** | - Gestión automática de alertas en el backend.<br>- Envío de notificaciones push inmediatas y confiables.<br>- Configuración de alertas personalizadas por usuario. |
| **Módulo de Seguridad y Autenticación**           | - Implementación de autenticación robusta (OAuth 2.0, JWT).<br>- Encriptación de datos sensibles en tránsito y en reposo.<br>- Control de acceso basado en roles (usuario, autoridad, administrador). |


### 4.3.1.4 Choose One or More Design Concepts That Satisfy the Selected Drivers

Para satisfacer los atributos de calidad seleccionados en esta iteración (**usabilidad, rendimiento, disponibilidad y seguridad**), se han elegido los siguientes conceptos de diseño:

| Decisiones | Justificación y Supuestos |
|------------|---------------------------|
| **Diseño de Formularios Simples e Intuitivos para Reportar Incidentes** | Se usarán los formularios de registro de incidentes (tipo, ubicación, hora) con validaciones dinámicas y feedback visual inmediato. Esto facilita el uso para cualquier ciudadano, incluso con poca experiencia tecnológica, mejorando la **usabilidad**. |
| **Mapas Interactivos para Visualización de Calor** | Se utilizarán mapas dinámicos con filtros de incidentes y rutas seguras, optimizados para tiempos de respuesta menores a 2 segundos. Esto garantiza una experiencia fluida en la consulta de información crítica, mejorando el **rendimiento**. |
| **Sistema de Notificaciones Push en Tiempo Real** | Se empleará un módulo de notificaciones automáticas, con entrega inmediata y confiable de alertas sobre incidentes cercanos. Esto asegura que los ciudadanos reciban información en situaciones críticas, cumpliendo con la **disponibilidad**. |
| **Mecanismos de Autenticación y Encriptación de Datos** | Se implementará autenticación robusta (OAuth 2.0, JWT) y encriptación de datos sensibles en tránsito y en reposo. Además, se gestionará el acceso según roles (usuario, autoridad, administrador). Esto protege la información sensible y fortalece la **seguridad**. |


### 4.3.1.5 Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

En esta iteración, se definen e instancian los siguientes elementos arquitectónicos principales, asignando responsabilidades específicas y estableciendo sus interfaces de comunicación:

| Apartado                                      | Decisión                                                                 | Justificación                                                                 |
|-----------------------------------------------|--------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Frontend Web/Móvil (PeaceApp UI)**          | Interfaz de usuario para ciudadanos y autoridades. Maneja el registro de incidentes, visualización de mapas de calor y gestión de notificaciones. | Brinda una experiencia amigable e inclusiva, conectando directamente a los usuarios con las funcionalidades principales. |
| **Backend API Gateway**                       | Orquesta las solicitudes de los módulos, maneja la autenticación, enrutamiento y validaciones básicas. | Permite centralizar la seguridad, simplificar la comunicación y escalar los microservicios.. |
| **Microservicio de Reportes de Incidentes**   | CRUD de reportes (crear, editar, eliminar, listar incidentes). Validación y almacenamiento de datos. | API REST expuesta al API Gateway. Procesa la información en tiempo real. |
| **Microservicio de Mapas Interactivos**       | Generación de mapas de calor dinámicos con filtros y rutas seguras. | API REST para consultas rápidas de incidentes. Algoritmos de agregación para visualización eficiente. |
| **Microservicio de Notificaciones y Alertas** | Gestión y envío de notificaciones push en tiempo real. Configuración de alertas personalizadas. | Mantiene a los usuarios informados y comprometidos con las actividades. |
| **Microservicio de Seguridad y Autenticación**| Manejo de autenticación de usuarios y cifrado de datos sensibles. Control de acceso basado en roles. | Garantiza almacenamiento seguro, integridad y disponibilidad de la información. | 

#### 4.3.X.6. Sketch Views (C4 & UML) and Record Design Decisions

#### 4.3.X.7. Analysis of Current Design and Review Iteration Goal (Kanban Board)
