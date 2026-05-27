# Explicación de los Módulos

## 1. Instructor

El módulo `Instructor` administra toda la información relacionada con los instructores del SENA.  
Permite registrar instructores de planta o contratistas, almacenar sus datos personales y controlar su disponibilidad para asignación de horarios.

También valida restricciones como:
- Email único
- Documento único
- Estado activo/inactivo
- Áreas de conocimiento

### Relación
- Un instructor puede tener muchos bloques de horario.
- Un instructor puede recibir muchas notificaciones.
- Un instructor puede generar múltiples registros de seguridad.

Relaciones:
- `Instructor 1 : N Schedule`
- `Instructor 1 : N Notification`
- `Instructor 1 : N Security`

---

## 2. Classroom (Room)

El módulo `Classroom` representa las aulas físicas o virtuales donde se desarrollan las clases.

Permite:
- Registrar aulas
- Controlar capacidad
- Registrar ubicación
- Registrar equipos disponibles
- Desactivar aulas sin eliminarlas

### Relación
- Un aula puede contener muchos horarios.
- Un aula puede participar en múltiples reportes.

Relaciones:
- `Classroom 1 : N Schedule`
- `Classroom 1 : N Report`

---

## 3. TimeSlot

El módulo `TimeSlot` administra las franjas horarias del sistema.

Define:
- Día de la semana
- Hora de inicio
- Hora de finalización

Este módulo ayuda a evitar conflictos de horario y permite reutilizar bloques horarios.

### Relación
- Una franja horaria puede estar asociada a muchos horarios.

Relaciones:
- `TimeSlot 1 : N Schedule`

---

## 4. TrainingGroup

El módulo `TrainingGroup` representa las fichas o grupos académicos del SENA.

Permite:
- Registrar grupos de formación
- Asociar estudiantes
- Asociar horarios
- Gestionar cantidad de aprendices

### Relación
- Un grupo puede tener muchos horarios.
- Un grupo puede tener muchas matrículas.
- Un grupo puede aparecer en múltiples reportes.

Relaciones:
- `TrainingGroup 1 : N Schedule`
- `TrainingGroup 1 : N Enrollment`
- `TrainingGroup 1 : N Report`

---

## 5. Schedule

El módulo `Schedule` es el núcleo principal del sistema.

Se encarga de:
- Asignar instructor
- Asignar aula
- Asignar grupo
- Asignar franja horaria
- Validar conflictos
- Gestionar estados de horario

Aquí se conectan la mayoría de módulos del sistema.

### Relaciones
- Muchos horarios pertenecen a un instructor
- Muchos horarios pertenecen a un aula
- Muchos horarios pertenecen a una franja horaria
- Muchos horarios pertenecen a un grupo
- Un horario puede tener observaciones
- Un horario puede aparecer en reportes
- Un horario puede generar auditoría
- Un horario usa configuraciones del sistema

Relaciones:
- `Schedule N : 1 Instructor`
- `Schedule N : 1 Classroom`
- `Schedule N : 1 TimeSlot`
- `Schedule N : 1 TrainingGroup`
- `Schedule 1 : N Observation`
- `Schedule 1 : N AuditLog`
- `Schedule 1 : N Report`
- `Configuration 1 : N Schedule`

---

## 6. Observation

El módulo `Observation` permite registrar observaciones o notas sobre un horario específico.

Se utiliza para:
- Registrar novedades
- Registrar problemas
- Registrar comentarios administrativos
- Clasificar severidad

### Relación
- Un horario puede tener muchas observaciones.
- Una observación puede generar registros de seguridad.

Relaciones:
- `Schedule 1 : N Observation`
- `Observation 1 : N Security`

---

## 7. AuditLog (Auditoría)

El módulo `AuditLog` registra todas las acciones realizadas dentro del sistema.

Permite almacenar:
- Quién hizo la acción
- Qué acción realizó
- Cuándo ocurrió
- Estado antes y después

Es fundamental para:
- Trazabilidad
- Seguridad
- Auditoría
- Cumplimiento normativo

### Relación
- Un horario puede generar múltiples registros de auditoría.

Relaciones:
- `Schedule 1 : N AuditLog`

---

## 8. Report (Reporting)

El módulo `Report` almacena reportes generados por el sistema.

Sirve para:
- Consultar estadísticas
- Generar dashboards
- Revisar carga académica
- Revisar conflictos
- Analizar ocupación de aulas

### Relación
Puede usar información de:
- Instructor
- Classroom
- Schedule
- TrainingGroup

Relaciones:
- `Instructor 1 : N Report`
- `Classroom 1 : N Report`
- `Schedule 1 : N Report`
- `TrainingGroup 1 : N Report`

---

## 9. Security (Seguridad)

El módulo `Security` controla el acceso a información sensible (PII).

Permite registrar:
- Quién accedió
- Qué campos consultó
- Desde qué endpoint
- Fecha y hora del acceso

Se utiliza para cumplir la Ley 1581 de protección de datos.

### Relación
- Puede relacionarse con Instructor y Observation.

Relaciones:
- `Instructor 1 : N Security`
- `Observation 1 : N Security`

---

## 10. Student (Estudiantes)

El módulo `Student` administra los aprendices del sistema.

Permite:
- Registrar estudiantes
- Guardar información personal
- Asociar estudiantes a fichas
- Gestionar estados activos
- Enviar notificaciones académicas

### Relación
- Un estudiante puede tener muchas matrículas.
- Un estudiante puede recibir muchas notificaciones.

Relaciones:
- `Student 1 : N Enrollment`
- `Student 1 : N Notification`

---

## 11. Enrollment (Matrículas)

El módulo `Enrollment` conecta estudiantes con grupos de formación.

Permite:
- Registrar matrículas
- Controlar estado de matrícula
- Relacionar estudiantes con fichas

### Relaciones
- Muchas matrículas pertenecen a un estudiante
- Muchas matrículas pertenecen a un grupo

Relaciones:
- `Enrollment N : 1 Student`
- `Enrollment N : 1 TrainingGroup`

---

## 12. Configuration

El módulo `Configuration` almacena configuraciones generales del sistema.

Permite:
- Guardar parámetros globales
- Configurar reglas
- Gestionar opciones administrativas
- Centralizar configuraciones del sistema

### Relación
- Las configuraciones pueden afectar múltiples horarios.

Relaciones:
- `Configuration 1 : N Schedule`

---

## 13. Notification

El módulo `Notification` administra las notificaciones del sistema.

Permite:
- Enviar alertas académicas
- Informar cambios de horario
- Enviar recordatorios
- Notificar conflictos o cancelaciones

### Relación
- Un estudiante puede recibir muchas notificaciones.
- Un instructor puede recibir muchas notificaciones.

Relaciones:
- `Student 1 : N Notification`
- `Instructor 1 : N Notification`

---

# Flujo Principal del Sistema

1. Se registran instructores, aulas y estudiantes.
2. Se crean grupos de formación.
3. Se matriculan estudiantes en grupos.
4. Se crean franjas horarias.
5. El sistema genera horarios.
6. Se validan conflictos.
7. Se registran observaciones.
8. Se generan reportes.
9. Se envían notificaciones.
10. Todas las acciones quedan auditadas.
11. Seguridad registra accesos a datos sensibles.