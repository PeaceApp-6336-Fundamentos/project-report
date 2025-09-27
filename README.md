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

### 4.1.5. Relational/Non Relational Database Diagram

### 4.1.6. Design Patterns

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
