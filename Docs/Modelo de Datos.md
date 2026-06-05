# Capítulo V

# MODELO DE DATOS

## 1. ARQUITECTURA DE LA BASE DE DATOS: SABER ÌA

El modelo de datos de SaberIA está estructurado mediante una arquitectura jerárquica de cuatro capas principales. Esta estrategia top-down organiza las entidades según su dominio funcional, desde la gestión de identidad hasta la analítica del aprendizaje, minimizando la complejidad y garantizando que cada capa tenga una responsabilidad clara y acotada.

Cada capa tiene su propio esquema de base de datos (**core**, **pedagogy**, **performance**, **analytics/audit**), lo que facilita el mantenimiento, el control de acceso por módulo y la escalabilidad del sistema a versiones futuras.

| Capa | Nombre | Tablas principales | Propósito |
|--------|--------|--------|--------|
| Capa 1 | Autenticación y Perfiles | core.users · core.roles · core.user_role | Gestiona identidad, autenticación y control de acceso por rol |
| Capa 2 | Contenido Pedagógico | pedagogy.competencias · pedagogy.preguntas · pedagogy.simulacros · pedagogy.simulacro_preguntas | Organiza el banco de preguntas clasificado por MODESEC y los simulacros configurados |
| Capa 3 | Desempeño y Gamificación | performance.perfil_estudiante · performance.respuestas_xapi · performance.puntos · performance.niveles | Almacena el perfil de desempeño del estudiante, el registro XAPI y la lógica de gamificación |
| Capa 4 | Analítica y Auditoría | analytics.retroalimentacion · analytics.reporte_grupo · audit.log_acciones | Gestiona la retroalimentación personalizada, los reportes docentes y la trazabilidad del sistema |

---

# 2. CAPA 1: AUTENTICACIÓN Y PERFILES

Esta capa constituye la base de seguridad del sistema. Gestiona la identidad de todos los usuarios, sus roles y el control de acceso diferenciado. Garantiza que cada estudiante, docente o administrador esté correctamente identificado y que su acceso al sistema refleje únicamente las funciones autorizadas para su rol.

## 2.1 Descripción de tablas

| Tabla | Campos principales | Propósito |
|---------|---------|---------|
| core.users | id, nombre, correo, carrera, semestre, estado, creado_en | Almacena todos los usuarios del sistema con su información institucional básica. |
| core.roles | id, nombre, descripcion | Define los tres roles del sistema: administrador, docente y estudiante. |
| core.user_role | user_id (FK), role_id (FK), asignado_en | Tabla intermedia que asigna uno o más roles a cada usuario con marca de tiempo. |

## 2.2 Código DDL

```sql
-- =====================================================
-- CAPA 1: AUTENTICACIÓN Y PERFILES
-- =====================================================

Table core.users {
 id integer [pk]
 nombre varchar [not null]
 correo varchar [unique, not null]
 carrera varchar
 semestre integer
 estado varchar [not null]
 creado_en timestamp [default: `now()`]
}

Table core.roles {
 id integer [pk]
 nombre varchar [unique, not null]
 descripcion text
}

Table core.user_role {
 user_id integer [pk]
 role_id integer [pk]
 asignado_en timestamp [default: `now()`]
}

Ref: core.user_role.user_id > core.users.id
Ref: core.user_role.role_id > core.roles.id
```

---

# 3. CAPA 2: CONTENIDO PEDAGÓGICO

Esta capa es el motor de contenido del sistema. Define la jerarquía pedagógica completa: desde las competencias Saber Pro y sus evidencias hasta las preguntas del banco y la configuración de los simulacros. Toda entidad de esta capa está subordinada a la estructura MODESEC, garantizando que cada ítem evaluativo tenga un propósito pedagógico trazable.

## 3.1 Descripción de tablas

| Tabla | Campos principales | Propósito |
|---------|---------|---------|
| pedagogy.competencias | id, modulo, numero, nombre, afirmacion | Define las competencias Saber Pro (RQ y LC) con su afirmación oficial del ICFES. |
| pedagogy.evidencias | id, competencia_id, codigo, descripcion | Desglosa cada competencia en sus evidencias observables (E1.1, E2.2, etc.). |
| pedagogy.preguntas | id, evidencia_id, enunciado, tipo, dificultad, recurso_url, activa, version | Almacena cada ítem del banco clasificado por evidencia, con soporte multimedia y control de versiones. |
| pedagogy.opciones_pregunta | id, pregunta_id, texto, es_correcta | Registra las opciones de respuesta de cada pregunta de selección múltiple. |
| pedagogy.simulacros | id, nombre, creado_por, modulo, total_preguntas, estado | Define la configuración de cada simulacro: módulo, número de preguntas y estado. |
| pedagogy.simulacro_preguntas | simulacro_id, pregunta_id, orden | Tabla intermedia que asocia preguntas a simulacros con su orden de presentación. |

## 3.2 Código DDL

```sql
-- =====================================================
-- CAPA 2: CONTENIDO PEDAGÓGICO
-- =====================================================

Table pedagogy.competencias [headerColor: #1E6B2E] {
 id integer [primary key]
 modulo varchar [not null] -- RQ / LC
 numero integer [not null] -- 1, 2 o 3
 nombre varchar [not null]
 afirmacion text [not null]
}

Table pedagogy.evidencias [headerColor: #1E6B2E] {
 id integer [primary key]
 competencia_id integer [not null]
 codigo varchar [not null] -- E1.1, E2.3, etc.
 descripcion text [not null]
}

Table pedagogy.preguntas [headerColor: #1E6B2E] {
 id integer [primary key]
 evidencia_id integer [not null]
 enunciado text [not null]
 tipo varchar [not null] -- seleccion_multiple / analisis
 dificultad integer [not null] -- 1 a 4
 recurso_url varchar
 activa boolean [default: false]
 creado_por integer [not null]
 version integer [default: 1]
}

Table pedagogy.opciones_pregunta [headerColor: #1E6B2E] {
 id integer [primary key]
 pregunta_id integer [not null]
 texto text [not null]
 es_correcta boolean [default: false]
}

Table pedagogy.simulacros [headerColor: #1E6B2E] {
 id integer [primary key]
 nombre varchar [not null]
 creado_por integer [not null]
 modulo varchar [not null] -- RQ / LC / mixto
 total_preguntas integer [not null]
 estado varchar [not null] -- borrador / activo / cerrado
 creado_en timestamp [default: 'now()']
}

Table pedagogy.simulacro_preguntas [headerColor: #1E6B2E] {
 simulacro_id integer [primary key]
 pregunta_id integer [primary key]
 orden integer [not null]
}

-- REFERENCIAS INTERNAS CAPA 2

Ref: pedagogy.evidencias.competencia_id > pedagogy.competencias.id
Ref: pedagogy.preguntas.evidencia_id > pedagogy.evidencias.id
Ref: pedagogy.preguntas.creado_por > core.users.id
Ref: pedagogy.opciones_pregunta.pregunta_id > pedagogy.preguntas.id
Ref: pedagogy.simulacros.creado_por > core.users.id
Ref: pedagogy.simulacro_preguntas.simulacro_id > pedagogy.simulacros.id
Ref: pedagogy.simulacro_preguntas.pregunta_id > pedagogy.preguntas.id
```

---

# 4. CAPA 3: DESEMPEÑO Y GAMIFICACIÓN

Esta capa es el motor adaptativo del sistema. Almacena el perfil de desempeño de cada estudiante, las evidencias XAPI de cada interacción con los simulacros, el control de sesiones y toda la lógica de gamificación. Es la capa que permite a SaberIA conocer cómo aprende el estudiante y ajustar la experiencia en consecuencia.

## 4.1 Descripción de tablas

| Tabla | Campos principales | Propósito |
|---------|---------|---------|
| performance.perfil_estudiante | id, user_id, nivel_global, puntos_totales, racha_dias, ultima_sesion | Perfil de desempeño global del estudiante: nivel de gamificación, puntos acumulados y racha diaria. |
| performance.desempeno_competencia | id, user_id, competencia_id, tasa_acierto, nivel_dominio, simulacros_realizados | Registra el nivel de dominio del estudiante por cada competencia Saber Pro evaluada. |
| performance.respuestas_xapi | id, user_id, simulacro_id, pregunta_id, opcion_id, es_correcta, tiempo_segundos, intento, solicito_ayuda | Evidencias XAPI de cada respuesta: resultado, tiempo, intentos y uso de ayuda. |
| performance.sesion_simulacro | id, user_id, simulacro_id, estado, ultima_pregunta, iniciado_en, finalizado_en | Controla el estado de cada sesión de simulacro para permitir la reanudación ante desconexiones. |
| performance.transacciones_puntos | id, user_id, tipo, concepto, cantidad, registrado_en | Historial de puntos ganados y canjeados por el estudiante en la tienda virtual. |

## 4.2 Código DDL

```sql
-- =====================================================
-- CAPA 3: DESEMPEÑO Y GAMIFICACIÓN
-- =====================================================

Table core.users {
 id integer [pk]
}

Table pedagogy.competencias {
 id integer [pk]
}

Table pedagogy.preguntas {
 id integer [pk]
}

Table pedagogy.simulacros {
 id integer [pk]
}

Table performance.perfil_estudiante {
 id integer [pk]
 user_id integer [unique, not null]
 nivel_global integer [default: 1]
 puntos_totales integer [default: 0]
 racha_dias integer [default: 0]
 ultima_sesion timestamp
}

Table performance.desempeno_competencia {
 id integer [pk]
 user_id integer [not null]
 competencia_id integer [not null]
 tasa_acierto float [not null, default: 0]
 nivel_dominio integer [not null, default: 1]
 simulacros_realizados integer [default: 0]
 actualizado_en timestamp
}

Table performance.respuestas_xapi {
 id integer [pk]
 user_id integer [not null]
 simulacro_id integer [not null]
 pregunta_id integer [not null]
 opcion_id integer
 es_correcta boolean [not null]
 tiempo_segundos integer [not null]
 intento integer [default: 1]
 solicito_ayuda boolean [default: false]
 registrado_en timestamp [default: `now()`]
}

Table performance.sesion_simulacro {
 id integer [pk]
 user_id integer [not null]
 simulacro_id integer [not null]
 estado varchar [not null]
 ultima_pregunta integer [default: 0]
 iniciado_en timestamp
 finalizado_en timestamp
}

Table performance.transacciones_puntos {
 id integer [pk]
 user_id integer [not null]
 tipo varchar [not null]
 concepto varchar [not null]
 cantidad integer [not null]
 registrado_en timestamp [default: `now()`]
}

Ref: performance.perfil_estudiante.user_id > core.users.id
Ref: performance.desempeno_competencia.user_id > core.users.id
Ref: performance.desempeno_competencia.competencia_id > pedagogy.competencias.id
Ref: performance.respuestas_xapi.user_id > core.users.id
Ref: performance.respuestas_xapi.simulacro_id > pedagogy.simulacros.id
Ref: performance.respuestas_xapi.pregunta_id > pedagogy.preguntas.id
Ref: performance.sesion_simulacro.user_id > core.users.id
Ref: performance.sesion_simulacro.simulacro_id > pedagogy.simulacros.id
Ref: performance.transacciones_puntos.user_id > core.users.id
```

---

# 5. CAPA 4: ANALÍTICA Y AUDITORÍA

Esta capa cierra el ciclo pedagógico del sistema. Almacena la retroalimentación personalizada generada al finalizar cada simulacro, los reportes analíticos del docente sobre su grupo y el log de auditoría de todas las acciones críticas. Garantiza la trazabilidad institucional y la transparencia del sistema.

## 5.1 Descripción de tablas

| Tabla | Campos principales | Propósito |
|---------|---------|---------|
| analytics.retroalimentacion | id, user_id, simulacro_id, puntaje_total, diagnostico, competencia_debil_id, evidencia_debil_id | Almacena el diagnóstico personalizado generado al finalizar cada simulacro, identificando la competencia y evidencia más débil. |
| analytics.reporte_grupo | id, docente_id, simulacro_id, promedio_grupo, competencia_critica_id, total_estudiantes | Registra el reporte analítico del grupo generado para el docente, con promedio y competencia crítica. |
| audit.log_acciones | id, user_id, accion, entidad, entidad_id, valor_antes, valor_despues, ip, fecha | Log de auditoría de todas las acciones críticas del sistema con trazabilidad completa. |

## 5.2 Código DDL

```sql
-- =====================================================
-- CAPA 4: ANALÍTICA Y AUDITORÍA
-- =====================================================

Table core.users {
 id integer [pk]
}

Table pedagogy.simulacros {
 id integer [pk]
}

Table pedagogy.competencias {
 id integer [pk]
}

Table pedagogy.evidencias {
 id integer [pk]
}

Table analytics.retroalimentacion {
 id integer [pk]
 user_id integer [not null]
 simulacro_id integer [not null]
 puntaje_total float [not null]
 diagnostico text [not null]
 competencia_debil_id integer
 evidencia_debil_id integer
 generado_en timestamp [default: `now()`]
}

Table analytics.reporte_grupo {
 id integer [pk]
 docente_id integer [not null]
 simulacro_id integer [not null]
 promedio_grupo float
 competencia_critica_id integer
 total_estudiantes integer
 generado_en timestamp [default: `now()`]
}

Table audit.log_acciones {
 id integer [pk]
 user_id integer [not null]
 accion varchar [not null]
 entidad varchar [not null]
 entidad_id integer
 valor_antes text
 valor_despues text
 ip varchar
 fecha timestamp [default: `now()`]
}

Ref: analytics.retroalimentacion.user_id > core.users.id
Ref: analytics.retroalimentacion.simulacro_id > pedagogy.simulacros.id
Ref: analytics.retroalimentacion.competencia_debil_id > pedagogy.competencias.id
Ref: analytics.retroalimentacion.evidencia_debil_id > pedagogy.evidencias.id
Ref: analytics.reporte_grupo.docente_id > core.users.id
Ref: analytics.reporte_grupo.simulacro_id > pedagogy.simulacros.id
Ref: analytics.reporte_grupo.competencia_critica_id > pedagogy.competencias.id
Ref: audit.log_acciones.user_id > core.users.id
```

---

# 6. CRITERIO METODOLÓGICO DE CALIDAD DEL MODELO

La calidad del modelo de datos de SaberIA se verifica cuando cada tabla puede vincularse con un componente de la metodología MODESEC.

Las tablas de **pedagogy** reflejan los componentes de competencia, afirmaciones y evidencias.

Las tablas de **performance** materializan el registro de evidencias observables y los criterios de evaluación.

Las tablas de **analytics** operacionalizan la mediación pedagógica y la retroalimentación.

El esquema **audit** garantiza la trazabilidad y validación de todas las decisiones del sistema.

Cualquier tabla que no pueda vincularse con alguno de estos componentes debe revisarse para determinar si responde a una necesidad pedagógica real o si puede eliminarse del modelo.
