# Capítulo VI

# MODELO ENTIDAD-RELACIÓN

## 1. BLOQUES DEL MODELO

El modelo entidad-relación de SaberIA está organizado en cuatro bloques funcionales, alineados con las cuatro capas del modelo de datos definido en el Capítulo V. Cada bloque describe las entidades de su dominio, sus atributos clave y las relaciones entre ellas, con su cardinalidad y significado pedagógico o técnico.

Para cada bloque se presentan dos tablas: la primera describe las entidades con sus atributos y propósito; la segunda detalla las relaciones entre entidades con su cardinalidad y la razón de esa asociación.

---

# 2. BLOQUE 1: AUTENTICACIÓN Y PERFILES

Este bloque representa la base de identidad del sistema. Define la relación entre los usuarios y sus roles, garantizando que cada actor del sistema tenga acceso diferenciado según sus responsabilidades. La entidad intermedia USUARIO_ROL permite asignar múltiples roles a un mismo usuario, lo que da flexibilidad para casos como un docente que también actúa como administrador.

## 2.1 Entidades

| Entidad | Atributos clave | Descripción |
|----------|----------|----------|
| USUARIO | id (PK)<br>nombre<br>correo<br>carrera<br>semestre<br>estado | Representa a cualquier persona que interactúa con el sistema: estudiante, docente o administrador. |
| ROL | id (PK)<br>nombre<br>descripcion | Define el conjunto de permisos asignados a un tipo de usuario dentro del sistema. |
| USUARIO_ROL | user_id (FK)<br>role_id (FK)<br>asignado_en | Entidad intermedia que vincula usuarios con sus roles. Permite que un usuario tenga múltiples roles. |

## 2.2 Relaciones

| Entidad A | Cardinalidad | Entidad B | Descripción de la relación |
|------------|------------|------------|------------|
| USUARIO | 1 : N | USUARIO_ROL | Un usuario puede tener uno o más roles asignados en el sistema. |
| ROL | 1 : N | USUARIO_ROL | Un rol puede estar asignado a múltiples usuarios simultáneamente. |

---

# 3. BLOQUE 2: CONTENIDO PEDAGÓGICO

Este bloque es el motor de conocimiento del sistema. Define la jerarquía pedagógica completa que rige el banco de preguntas: desde la competencia Saber Pro hasta la opción de respuesta individual. Toda esta estructura responde directamente al componente de competencia objetivo y evidencias de aprendizaje de la metodología MODESEC.

## 3.1 Entidades

| Entidad | Atributos clave | Descripción |
|----------|----------|----------|
| COMPETENCIA | id (PK)<br>modulo<br>numero<br>nombre<br>afirmacion | Competencia genérica Saber Pro (RQ o LC). Es el punto de partida pedagógico del sistema. |
| EVIDENCIA | id (PK)<br>competencia_id (FK)<br>codigo<br>descripcion | Aspecto observable que permite inferir el nivel de dominio de una afirmación. Eje de clasificación del banco. |
| PREGUNTA | id (PK)<br>evidencia_id (FK)<br>enunciado<br>dificultad<br>activa<br>version | Ítem evaluativo del banco clasificado por evidencia. Incluye control de versiones y validación docente. |
| OPCION | id (PK)<br>pregunta_id (FK)<br>texto<br>es_correcta | Opción de respuesta de una pregunta de selección múltiple. Solo una opción puede ser correcta. |
| SIMULACRO | id (PK)<br>nombre<br>creado_por (FK)<br>modulo<br>total_preguntas<br>estado | Configuración de un examen: módulo, número de preguntas y estado (borrador/activo/cerrado). |
| SIM_PREGUNTA | simulacro_id (FK)<br>pregunta_id (FK)<br>orden | Entidad intermedia que asocia preguntas a simulacros con su orden de presentación. |

## 3.2 Relaciones

| Entidad A | Cardinalidad | Entidad B | Descripción de la relación |
|------------|------------|------------|------------|
| COMPETENCIA | 1 : N | EVIDENCIA | Una competencia se desglosa en múltiples evidencias observables. |
| EVIDENCIA | 1 : N | PREGUNTA | Una evidencia agrupa múltiples preguntas del banco que la evalúan. |
| PREGUNTA | 1 : N | OPCION | Una pregunta tiene entre 2 y 5 opciones de respuesta. |
| SIMULACRO | N : M | PREGUNTA | Un simulacro contiene múltiples preguntas; una pregunta puede aparecer en varios simulacros. Relación resuelta por SIM_PREGUNTA. |
| USUARIO | 1 : N | SIMULACRO | Un docente o administrador puede crear múltiples simulacros. |

---

# 4. BLOQUE 3: DESEMPEÑO Y GAMIFICACIÓN

Este bloque es el motor adaptativo del sistema. Almacena el historial completo de interacciones del estudiante con los simulacros mediante el estándar XAPI, construye su perfil de desempeño por competencia y gestiona toda la lógica de gamificación. Es el bloque que permite a SaberIA conocer cómo aprende el estudiante y ajustar la experiencia pedagógica en consecuencia.

## 4.1 Entidades

| Entidad | Atributos clave | Descripción |
|----------|----------|----------|
| PERFIL_EST | id (PK)<br>user_id (FK)<br>nivel_global<br>puntos_totales<br>racha_dias | Perfil de desempeño global del estudiante. Concentra su nivel de gamificación, puntos y racha diaria. |
| DESEMPEÑO_COMP | id (PK)<br>user_id (FK)<br>competencia_id (FK)<br>tasa_acierto<br>nivel_dominio | Nivel de dominio del estudiante por cada competencia Saber Pro evaluada. Se actualiza con cada simulacro. |
| RESP_XAPI | id (PK)<br>user_id (FK)<br>simulacro_id (FK)<br>pregunta_id (FK)<br>es_correcta<br>tiempo_seg<br>intento | Evidencia XAPI de cada respuesta: resultado, tiempo invertido, número de intentos y uso de ayuda. |
| SESION_SIM | id (PK)<br>user_id (FK)<br>simulacro_id (FK)<br>estado<br>ultima_pregunta | Controla el estado de cada sesión de simulacro para permitir reanudación ante desconexiones. |
| TRANSAC_PUNTOS | id (PK)<br>user_id (FK)<br>tipo<br>concepto<br>cantidad | Historial de puntos ganados y canjeados. Permite rastrear cada movimiento de la economía de gamificación. |

## 4.2 Relaciones

| Entidad A | Cardinalidad | Entidad B | Descripción de la relación |
|------------|------------|------------|------------|
| USUARIO | 1 : 1 | PERFIL_EST | Cada estudiante tiene exactamente un perfil de desempeño global. |
| USUARIO | 1 : N | DESEMPEÑO_COMP | Un estudiante tiene un registro de desempeño por cada competencia evaluada. |
| COMPETENCIA | 1 : N | DESEMPEÑO_COMP | Una competencia agrupa los registros de desempeño de todos los estudiantes que la han evaluado. |
| USUARIO | 1 : N | RESP_XAPI | Un estudiante genera múltiples registros XAPI a lo largo de sus simulacros. |
| SIMULACRO | 1 : N | RESP_XAPI | Un simulacro genera tantos registros XAPI como preguntas respondidas por cada estudiante. |
| USUARIO | 1 : N | SESION_SIM | Un estudiante puede tener múltiples sesiones de simulacro (activas, completadas o abandonadas). |
| USUARIO | 1 : N | TRANSAC_PUNTOS | Un estudiante acumula múltiples transacciones de puntos a lo largo del tiempo. |

---

# 5. BLOQUE 4: ANALÍTICA Y AUDITORÍA

Este bloque cierra el ciclo pedagógico del sistema. La retroalimentación personalizada conecta el desempeño del estudiante con las competencias donde debe mejorar. El reporte de grupo le da al docente visibilidad analítica sobre sus estudiantes. El log de auditoría garantiza la trazabilidad institucional de todas las acciones críticas, respondiendo al principio de trazabilidad y validación de MODESEC.

## 5.1 Entidades

| Entidad | Atributos clave | Descripción |
|----------|----------|----------|
| RETROALIM | id (PK)<br>user_id (FK)<br>simulacro_id (FK)<br>puntaje_total<br>diagnostico<br>competencia_debil_id (FK) | Diagnóstico personalizado generado al finalizar cada simulacro. Identifica la competencia y evidencia más débil del estudiante. |
| REPORTE_GRUPO | id (PK)<br>docente_id (FK)<br>simulacro_id (FK)<br>promedio_grupo<br>competencia_critica_id (FK) | Reporte analítico del grupo para el docente. Muestra promedio, competencia crítica y total de estudiantes evaluados. |
| LOG_ACCIONES | id (PK)<br>user_id (FK)<br>accion<br>entidad<br>valor_antes<br>valor_despues<br>fecha | Log de auditoría de todas las acciones críticas del sistema con trazabilidad completa de cambios. |

## 5.2 Relaciones

| Entidad A | Cardinalidad | Entidad B | Descripción de la relación |
|------------|------------|------------|------------|
| USUARIO | 1 : N | RETROALIM | Un estudiante recibe una retroalimentación por cada simulacro completado. |
| SIMULACRO | 1 : N | RETROALIM | Un simulacro genera una retroalimentación por cada estudiante que lo completa. |
| COMPETENCIA | 1 : N | RETROALIM | Una competencia puede ser identificada como débil en múltiples retroalimentaciones. |
| USUARIO | 1 : N | REPORTE_GRUPO | Un docente puede generar múltiples reportes de grupo para distintos simulacros. |
| SIMULACRO | 1 : N | REPORTE_GRUPO | Un simulacro puede tener un reporte de grupo por cada docente que lo asignó. |
| USUARIO | 1 : N | LOG_ACCIONES | Cada usuario genera entradas en el log cada vez que ejecuta una acción crítica. |

---

# 6. COHERENCIA DEL MODELO CON MODESEC

El modelo entidad-relación de SaberIA no es una estructura técnica aislada: cada bloque puede vincularse directamente con uno o más componentes de la metodología MODESEC.

- **Bloque 1 (Autenticación y Perfiles)** — garantiza el control docente: solo usuarios con rol validado pueden crear preguntas, configurar simulacros o acceder a reportes.

- **Bloque 2 (Contenido Pedagógico)** — operacionaliza la competencia objetivo y las evidencias: la jerarquía Competencia → Evidencia → Pregunta es la columna vertebral pedagógica del sistema.

- **Bloque 3 (Desempeño y Gamificación)** — materializa el registro de evidencias observables (XAPI), los criterios de evaluación (nivel de dominio por competencia) y la mediación pedagógica (ajuste de dificultad basado en desempeño).

- **Bloque 4 (Analítica y Auditoría)** — implementa la retroalimentación al estudiante y al docente, cerrando el ciclo pedagógico completo y garantizando la trazabilidad y validación de todas las decisiones del sistema.

Esta correspondencia confirma que SaberIA está construido sobre una base metodológica coherente: cada decisión de diseño del modelo de datos responde a una intención pedagógica previa, y no a una acumulación arbitraria de funcionalidades.
