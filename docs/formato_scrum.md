# FORMATOS SCRUM — PROYECTO FINAL

**Administración de Servicios de Internet** · Mtro. Juan Ángel Calvillo Pérez
**Práctica 7** · Infraestructura de servicios para una escuela — Equipo 10

## 1. Ficha de inicio y visión del producto

| Campo | Detalle |
|---|---|
| Nombre del proyecto | Infraestructura de servicios de red para una institución educativa |
| Equipo | Equipo 10 — Enzo Valdés Zavala, Jesús Alejandro García Vázquez, Rafael Ortega de la Paz. |
| Product Owner | Jesús Alejandro García Vázquez — representa las necesidades de la institución educativa (cliente) y valida qué funcionalidades tienen prioridad. |
| Scrum Master | Enzo Valdés Zavala — da seguimiento a los sprints, modera las dailies y elimina impedimentos del equipo. |
| Fecha de inicio | 14/09/2026 |
| Problema que se desea resolver | Una institución educativa no cuenta con una infraestructura de red integrada: los equipos no reciben una configuración de red automática, no existe un dominio interno para identificar los servicios, no hay un sitio institucional accesible desde la red local, los documentos de cada área se manejan de forma dispersa y no hay un mecanismo seguro para administrar el servidor a distancia. |
| Usuarios / beneficiarios | Alumnos, profesores y personal administrativo de la institución que utilizan equipos Windows y Linux conectados a la red escolar, así como el personal de sistemas encargado de administrar el servidor. |
| Visión del producto | Contar con una red escolar centralizada y confiable donde los servicios básicos de conectividad (asignación de IPs, resolución de nombres, sitio institucional, archivos compartidos y administración remota) funcionen de manera integrada, con una configuración clara y fácil de mantener. |
| Objetivo general | Diseñar, implementar, probar y documentar una infraestructura de servicios de Internet (DHCP, DNS, servidor web, Samba y SSH) sobre máquinas virtuales, que dé soporte a las necesidades de conectividad y de compartición de recursos de una institución educativa. |
| Alcance inicial | Configuración de un servidor DHCP para asignación automática de direcciones IP; configuración de DNS con dominio interno (escuela.local); publicación de un sitio web institucional accesible desde los clientes; configuración de Samba para compartir archivos entre departamentos; habilitación de acceso remoto vía SSH para el personal de sistemas; documentación técnica y de usuario, y video de demostración de la instalación. |
| Fuera de alcance | Integración con sistemas de gestión escolar (control de calificaciones, inscripciones, etc.); balanceo de carga o alta disponibilidad entre varios servidores web; servicios de correo electrónico; administración de más de un plantel o sede. |

## 2. Product Backlog

Historias de usuario priorizadas para cubrir los cinco servicios requeridos por la práctica: DHCP, DNS, sitio web institucional, archivos compartidos (Samba) y acceso remoto (SSH).

| ID | Historia de usuario | Prioridad | Valor | Estimación | Criterios de aceptación | Estado |
|---|---|---|---|---|---|---|
| HU00 | Como desarrolladores del proyecto, quiero desarrollar la documentación alineada a la metodología SCRUM para implementar la respectiva planeación. Asignar los roles de Scrum Master, Product Owner y las tareas de desarrollo. Organizar los sprints y horarios de reuniones. Crear un repositorio de Git para el proyecto. | Alta | 5 pts | 1 semana | 1. Se desarrolló el formato Scrum. 2. Se delegaron los roles. 3. Se propusieron los sprints. 4. Se organizaron los horarios. 5. Se creó el repositorio en Git. | Finalizado |
| HU01 | Como administrador de red, quiero que los equipos cliente obtengan automáticamente su dirección IP, puerta de enlace y DNS al conectarse a la red, para no tener que configurar cada equipo manualmente. | Alta | 3 pts | 2 semanas (con HU02) | 1. El cliente recibe IP, gateway y DNS del servidor DHCP. 2. La IP está dentro del rango del pool. 3. Se verifica con ipconfig/ifconfig. | Pendiente |
| HU02 | Como usuario de la red escolar, quiero acceder a los servicios mediante nombres de dominio (www.escuela.local) en lugar de IPs. | Alta | 3 pts | 2 semanas (con HU01) | 1. escuela.local resuelve desde el cliente. 2. Funciona con nslookup/dig. 3. El cliente usa el DNS entregado por DHCP. | Pendiente |
| HU03 | Como miembro de la comunidad escolar, quiero consultar un sitio web institucional con avisos, horarios y contacto. | Alta | 5 pts | 2 semanas | 1. El sitio carga con el dominio interno. 2. El contenido identifica a la institución. 3. Apache responde sin errores. | Pendiente |
| HU04 | Como profesor/administrativo, quiero acceso a una carpeta compartida para guardar documentos de mi departamento. | Media | 5 pts | 2 semanas (con HU05 y HU06) | 1. Carpeta visible vía Samba. 2. Solo usuarios autorizados leen/escriben. 3. Se prueba desde Windows y Linux. | Pendiente |
| HU05 | Como responsable de sistemas, quiero conectarme remotamente vía SSH para administrar el servidor. | Media | 3 pts | 2 semanas (con HU04 y HU06) | 1. SSH se establece correctamente. 2. Requiere autenticación válida. 3. Se documenta comando y resultado. | Pendiente |
| HU06 | Como administrador de red, quiero usuarios y permisos diferenciados por departamento en el servidor de archivos. | Media | 5 pts | 2 semanas (con HU04 y HU05) | 1. Existen grupos por área. 2. Un grupo no accede a carpetas de otro. 3. Se demuestra un acceso permitido y uno rechazado. | Pendiente |
| HU07 | Como coordinador, quiero documentación clara de IPs, dominios, usuarios y configuraciones. | Alta | 2 pts | 2 semanas | 1. Existe documento con topología e IPs. 2. Se listan archivos de configuración. 3. Permite reproducir la instalación. | Pendiente |

## 3. Historia de usuario detallada

**HU01 — Asignación automática de direcciones IP**
- Como: administrador de red
- Quiero: que los equipos cliente obtengan automáticamente su IP, gateway y DNS al conectarse
- Para: no tener que configurar cada equipo manualmente
- Prioridad: Alta · Estimación: 3 pts (2 semanas, con HU02)
- Dependencias: Ninguna (requiere VMs base creadas)
- Criterios de aceptación: 1. El cliente recibe IP, gateway y DNS del DHCP. 2. La IP está en el rango del pool. 3. Se verifica con ipconfig/ifconfig.
- Notas: Se desarrolla junto con HU02 en el Sprint 1.

**HU02 — Resolución de nombres de dominio interno**
- Como: usuario de la red escolar
- Quiero: acceder a los servicios por nombre de dominio (www.escuela.local) en vez de IP
- Para: que sea más fácil identificarlos y recordarlos
- Prioridad: Alta · Estimación: 3 pts (2 semanas, con HU01)
- Dependencias: HU01 (el cliente recibe el DNS vía DHCP)
- Criterios de aceptación: 1. escuela.local resuelve desde el cliente. 2. Funciona con nslookup/dig. 3. El cliente usa el DNS de DHCP.
- Notas: Se desarrolla junto con HU01 en el Sprint 1.

**HU03 — Sitio web institucional**
- Como: miembro de la comunidad escolar
- Quiero: consultar un sitio web institucional
- Para: conocer avisos, horarios y datos de contacto
- Prioridad: Alta · Estimación: 5 pts (2 semanas)
- Dependencias: HU02 (se publica bajo el dominio interno)
- Criterios de aceptación: 1. El sitio carga con el dominio interno. 2. Identifica a la institución. 3. Apache responde sin errores.
- Notas: Corresponde al Sprint 2.

**HU04 — Acceso a carpetas compartidas**
- Como: profesor o personal administrativo
- Quiero: tener acceso a una carpeta compartida en la red
- Para: guardar y consultar documentos de mi departamento
- Prioridad: Media · Estimación: 5 pts (2 semanas, con HU05 y HU06)
- Dependencias: Ninguna
- Criterios de aceptación: 1. Carpeta visible vía Samba. 2. Solo autorizados leen/escriben. 3. Se prueba en Windows y Linux.
- Notas: Corresponde al Sprint 3.

**HU05 — Conexión remota segura por SSH**
- Como: responsable de sistemas
- Quiero: conectarme remota y seguramente vía SSH
- Para: administrar el servidor sin estar presente
- Prioridad: Media · Estimación: 3 pts (2 semanas, con HU04 y HU06)
- Dependencias: Ninguna
- Criterios de aceptación: 1. SSH se establece correctamente. 2. Requiere autenticación válida. 3. Se documenta comando y resultado.
- Notas: Corresponde al Sprint 3.

**HU06 — Permisos diferenciados por departamento**
- Como: administrador de red
- Quiero: usuarios y permisos diferenciados por área en el servidor de archivos
- Para: evitar accesos no autorizados entre áreas
- Prioridad: Media · Estimación: 5 pts (2 semanas, con HU04 y HU05)
- Dependencias: HU04 (Samba configurado)
- Criterios de aceptación: 1. Existen grupos por área. 2. Un grupo no accede a carpetas de otro. 3. Se demuestra acceso permitido y rechazado.
- Notas: Corresponde al Sprint 3.

**HU07 — Documentación técnica del proyecto**
- Como: coordinador del proyecto
- Quiero: documentación clara de IPs, dominios, usuarios y configuraciones
- Para: dar mantenimiento o continuidad al proyecto
- Prioridad: Alta · Estimación: 2 pts (2 semanas)
- Dependencias: HU01–HU06
- Criterios de aceptación: 1. Existe documento con topología e IPs. 2. Se listan archivos de configuración. 3. Permite reproducir la instalación.
- Notas: Corresponde al Sprint 4; incluye el video de 8–12 minutos.

## 4. Sprint Planning / Sprint Backlog — Sprint 0

| Campo | Detalle |
|---|---|
| Sprint | Sprint 0 |
| Objetivo del Sprint | Preparar el entorno de trabajo, definir responsabilidades y afinar el plan de acción. |
| Fecha inicio / fin | 14/09/2026 – 20/09/2026 |
| Integrantes | Equipo 10 — Enzo Valdés Zavala, Jesús Alejandro García Vázquez, Rafael Ortega de la Paz. |

**Tareas del Sprint 0**

| ID | Tarea | Responsable | Estimación | Estado |
|---|---|---|---|---|
| HU001 | Organizar el formato de entrega Scrum. | Rafael Ortega de la Paz | 4 h | Finalizado |
| HU002 | Delegar los roles de Scrum Master y Product Owner. | Enzo Valdés Zavala | 1 h | Finalizado |
| HU003 | Proponer los sprints de desarrollo. | Enzo Valdés Zavala | 2 h | Finalizado |
| HU004 | Crear el repositorio en GitHub. | Rafael Ortega de la Paz | 1 h | Finalizado |
| HU005 | Organizar los horarios de reuniones semanales. | Jesús Alejandro García Vázquez | < 1h | Finalizado |
| HU006 | Primera reunión Scrum. | Enzo Valdés Zavala | 0.5 h | Finalizado |

## 5. Daily Scrum (bitácora)

Se llenará día a día durante el Sprint 0 con los avances reales del equipo. (Ver también `docs/bitacora.md` para la versión que se actualiza sprint a sprint.)

| Fecha | ¿Qué hice ayer? | ¿Qué haré hoy? | Impedimentos |
|---|---|---|---|
| 14/09/2026 | Se analizan los requerimientos del proyecto, se contacta a los integrantes. | Se propone la planificación de sprints. | Ninguno significativo. |
| 17/09/2026 | Días festivos, sin avances significativos. | Se delegan los roles de Scrum Master y Product Owner. | Ninguno significativo. |
| 18/09/2026 | Se delegan los roles del proyecto. | Reunión Scrum para presentar sprints y cronograma. Se crea el repositorio de Git. | Dificultad para organizar horarios (trabajo/servicio social). |
| 19/09/2026 | Reunión Scrum y planificación general. | Segunda reunión Scrum para dar formato al entregable. | Ninguno significativo. |

## 6. Registro de impedimentos

| ID | Fecha | Impedimento | Impacto | Responsable | Acción | Estado |
|---|---|---|---|---|---|---|
| IMP01 | 18/09/2026 | Incompatibilidad de horarios de los integrantes para las reuniones. | Alto | Enzo Valdés Zavala | Ubicar espacios libres y determinar un horario fijo. | Cerrado |

## 7. Sprint Review

| Campo | Detalle |
|---|---|
| Sprint | 0 |
| Incremento presentado | Planeación inicial: roles de PO y SM, Product Backlog priorizado, propuesta de sprints, reuniones organizadas y repositorio en GitHub creado. |
| Participantes / interesados | Integrantes del equipo 10 |
| Comentarios recibidos | Revisar fechas de próximos sprints, mantener actualizado el repositorio y definir responsables con anticipación. |
| Historias aceptadas | 1 (HU00) |
| Historias no aceptadas y motivo | Ninguna |
| Nuevos elementos para Product Backlog | Preparar las VMs y verificar conectividad básica antes de DHCP/DNS en el Sprint 1. |

## 8. Sprint Retrospective

| Campo | Detalle |
|---|---|
| Sprint | 0 |
| Fecha | 14/09/2026 – 20/09/2026 |
| ¿Qué funcionó bien? | Organización y planificación de los sprints. |
| ¿Qué no funcionó? | Búsqueda de horarios compatibles para las reuniones. |
| ¿Qué debemos mejorar? | Comunicación anticipada de cambios y sugerencias. |
| Acciones de mejora para el siguiente Sprint | Seguir el cronograma acordado. |

## 9. Definition of Done (DoD)

| Criterio | Sí/No | Evidencia |
|---|---|---|
| Código implementado | NO | NO |
| Pruebas realizadas | NO | NO |
| Criterios de aceptación cumplidos | SI | BACKLOG |
| Revisión del equipo | SI | Reunión en Scrum realizada |
| Documentación actualizada | SI | Formato actual, entregable |

## Propuesta de Sprints (dos semanas cada uno)

- **Sprint 0** (14–20 sep): Organización y delegación de tareas.
- **Sprint 1** (23 sep–6 oct): Infraestructura base — DHCP y DNS (HU01, HU02).
- **Sprint 2** (7–20 oct): Servicio web institucional (HU03).
- **Sprint 3** (21 oct–3 nov): Archivos compartidos y acceso remoto (HU04, HU05, HU06).
- **Sprint 4** (4–17 nov): Pruebas integrales, documentación y video (HU07).
