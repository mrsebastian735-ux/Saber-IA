# Capítulo IV

# HISTORIAS DE USUARIO

## 1. PROPÓSITO

Este capítulo presenta el conjunto inicial de historias de usuario de SaberIA, organizadas por módulo funcional. Cada historia sigue el formato estándar **Como [tipo de usuario] / Quiero [acción u objetivo] / Para [beneficio o valor]**, e incluye criterios de aceptación en notación Gherkin (**Dado / Cuando / Entonces**) que permiten verificar su cumplimiento durante el desarrollo y las pruebas.

Las historias se derivan de la fundamentación del proyecto (Capítulo I), las competencias Saber Pro operacionalizadas (Capítulo II) y la metodología MODESEC (Capítulo III). Su propósito es orientar el análisis funcional, la priorización del MVP y el modelado de datos del sistema.

Los módulos cubiertos en este capítulo son:

- BAN (Banco de Preguntas)
- SIM (Ejecución de Simulacros)
- GAME (Gamificación)
- RET (Resultados y Retroalimentación)
- PER (Acceso y Perfiles)
- ADM (Administración y Seguridad)

---

# 2. BANCO DE PREGUNTAS (BAN)

Módulo que gestiona el repositorio de ítems evaluativos. Cada pregunta debe estar clasificada por módulo Saber Pro, competencia, afirmación y evidencia, garantizando que el diagnóstico de dificultades sea preciso y pedagógicamente significativo.

| ID | Historia (Como / Quiero / Para) | Valor | Prioridad | Depende de | Criterios de aceptación |
|------|------|------|------|------|------|
| BAN-01 | Como administrador, quiero clasificar preguntas por competencia, componente, afirmación y evidencia para medir habilidades específicas alineadas con el Saber Pro. | Alta | Must | — | **Dado** que registro una pregunta con enunciado y opciones.<br>**Cuando** asigno módulo, competencia, afirmación y evidencia.<br>**Entonces** se guarda la estructura completa y se impide asociarla a múltiples competencias simultáneamente. |
| BAN-02 | Como administrador, quiero incluir recursos multimedia (imágenes, tablas, código) en las preguntas para simular la complejidad visual de las pruebas reales. | Alta | Must | BAN-01 | **Dado** que la pregunta requiere análisis gráfico.<br>**Cuando** adjunto una imagen o tabla al ítem.<br>**Entonces** el recurso se visualiza correctamente en la interfaz del estudiante durante el simulacro. |
| BAN-03 | Como sistema, quiero mantener un historial de versiones de cada pregunta para rastrear cambios y mejorar la calidad del banco con el tiempo. | Media | Should | BAN-01 | **Dado** un cambio en el enunciado o en las opciones de respuesta.<br>**Cuando** el administrador guarda la edición.<br>**Entonces** se genera una nueva versión y se preserva el registro anterior para auditoría. |

---

# 3. EJECUCIÓN DE SIMULACROS (SIM)

Módulo que gestiona la generación y ejecución de los simulacros. Incluye la parametrización por competencia, el guardado automático del progreso y el registro de evidencias mediante el estándar XAPI.

| ID | Historia (Como / Quiero / Para) | Valor | Prioridad | Depende de | Criterios de aceptación |
|------|------|------|------|------|------|
| SIM-01 | Como administrador, quiero parametrizar el número de preguntas por competencia al generar un simulacro para adaptar la duración y el enfoque del examen. | Alta | Must | BAN-01 | **Dado** el generador de simulacros.<br>**Cuando** ingreso la cantidad de preguntas por competencia.<br>**Entonces** el sistema selecciona aleatoriamente esa cantidad del banco activo respetando la distribución definida. |
| SIM-02 | Como estudiante, quiero que el sistema guarde mi progreso automáticamente ante fallos de conexión para no perder mi avance en el simulacro. | Alta | Must | PER-01 | **Dado** un simulacro en curso.<br>**Cuando** se detecta desconexión o cierre inesperado de sesión.<br>**Entonces** los datos se guardan localmente y el estudiante puede retomar desde la última pregunta respondida. |
| SIM-03 | Como sistema, quiero registrar el tiempo invertido por pregunta y el tiempo total del examen mediante XAPI para detectar patrones de dificultad de lectura o conocimiento. | Media | Should | SIM-01 | **Dado** que el estudiante navega entre preguntas.<br>**Cuando** pasa a la siguiente.<br>**Entonces** el sistema registra los segundos transcurridos de forma interna como evidencia XAPI, sin mostrarlo al estudiante. |

---

# 4. GAMIFICACIÓN Y MOTIVACIÓN (GAME)

Módulo central de motivación del sistema. La gamificación en SaberIA no es decorativa: está alineada con el desempeño competencial real del estudiante, de modo que los puntos y niveles reflejan el progreso de aprendizaje.

| ID | Historia (Como / Quiero / Para) | Valor | Prioridad | Depende de | Criterios de aceptación |
|------|------|------|------|------|------|
| GAME-01 | Como estudiante, quiero ganar puntos por acciones de estudio (rachas diarias, completar simulacros) para incentivar mi práctica constante. | Alta | Must | PER-01 | **Dado** el acceso diario al sistema.<br>**Cuando** entro 5 días seguidos o finalizo una prueba sin errores.<br>**Entonces** mi puntaje total aumenta según la tabla de logros definida por el administrador. |
| GAME-02 | Como sistema, quiero asignar niveles automáticos (Novato, Avanzado, Pro, Experto) según la experiencia acumulada para mostrar el progreso del estudiante en el ranking. | Media | Should | GAME-01 | **Dado** que un estudiante acumula puntos.<br>**Cuando** alcanza el umbral definido para un nuevo nivel.<br>**Entonces** su perfil se actualiza automáticamente con el nuevo rango y se muestra en el ranking. |
| GAME-03 | Como estudiante, quiero acceder a una tienda virtual para gastar mis puntos en ventajas dentro de la plataforma y mantener mi motivación. | Media | Could | GAME-01 | **Dado** que tengo puntos acumulados.<br>**Cuando** entro al módulo de tienda.<br>**Entonces** puedo ver los artículos disponibles, su costo en puntos y canjear los que puedo pagar. |

---

# 5. RESULTADOS Y RETROALIMENTACIÓN (RET)

Módulo que cierra el ciclo pedagógico de cada simulacro. Genera retroalimentación personalizada al estudiante por competencia y evidencia, y proporciona al docente reportes analíticos del grupo para la toma de decisiones pedagógicas.

| ID | Historia (Como / Quiero / Para) | Valor | Prioridad | Depende de | Criterios de aceptación |
|------|------|------|------|------|------|
| RET-01 | Como estudiante, quiero recibir retroalimentación dinámica por competencia al finalizar cada simulacro para saber exactamente en qué áreas debo mejorar. | Alta | Must | SIM-01 | **Dado** el puntaje y el perfil de errores obtenidos.<br>**Cuando** finalizo el simulacro.<br>**Entonces** el sistema muestra un diagnóstico indicando la competencia y evidencia específica con mayor índice de fallos (ej. "Reforzar E2.2 – Ejecución de planes"). |
| RET-02 | Como estudiante, quiero ver un histórico comparativo de mis simulacros para analizar mi evolución por competencia a lo largo del tiempo. | Media | Should | SIM-01 | **Dado** mi historial de simulacros.<br>**Cuando** consulto la sección de progreso.<br>**Entonces** veo una gráfica comparativa de mi desempeño por competencia y evidencia en el tiempo. |
| RET-03 | Como docente, quiero ver el reporte de desempeño de mis grupos para identificar las competencias con mayor índice de fallos y planificar refuerzo. | Media | Should | PER-01 | **Dado** un grupo de estudiantes asignado.<br>**Cuando** consulto el reporte del grupo.<br>**Entonces** veo el promedio por competencia, la evidencia con mayor tasa de error y la evolución del grupo en el tiempo. |

---

# 6. ACCESO Y PERFILES (PER)

Módulo de autenticación y gestión de perfiles. Garantiza que cada usuario acceda con su identidad institucional y que su perfil (carrera, semestre, preferencias) quede correctamente asociado para personalizar la experiencia.

| ID | Historia (Como / Quiero / Para) | Valor | Prioridad | Depende de | Criterios de aceptación |
|------|------|------|------|------|------|
| PER-01 | Como estudiante, quiero acceder al sistema mediante mi correo institucional para autenticarme sin crear nuevas contraseñas y tener mi perfil asociado automáticamente. | Alta | Must | — | **Dado** el portal de inicio.<br>**Cuando** ingreso con mi correo @institucional y contraseña.<br>**Entonces** el sistema me autentica, asocia mi carrera y semestre, y redirige a mi panel principal. |
| PER-02 | Como docente, quiero acceder con mi correo institucional para gestionar mis grupos y consultar reportes de desempeño desde mi panel. | Alta | Must | — | **Dado** el portal de inicio.<br>**Cuando** ingreso con credenciales de docente.<br>**Entonces** accedo a mi panel con visibilidad de grupos asignados, reportes y banco de preguntas. |
| PER-03 | Como estudiante, quiero ver y editar mi perfil (semestre, carrera, preferencias de notificación) para mantener mi información actualizada. | Media | Should | PER-01 | **Dado** mi perfil activo.<br>**Cuando** actualizo semestre, carrera o preferencias.<br>**Entonces** los cambios se guardan y se reflejan en las estadísticas y simulacros personalizados. |

---

# 7. ADMINISTRACIÓN Y SEGURIDAD (ADM)

Módulo que controla los roles, permisos y trazabilidad del sistema. Garantiza que cada actor acceda únicamente a las funciones autorizadas y que toda acción crítica quede registrada para auditoría institucional.

| ID | Historia (Como / Quiero / Para) | Valor | Prioridad | Depende de | Criterios de aceptación |
|------|------|------|------|------|------|
| ADM-01 | Como administrador, quiero gestionar roles globales (administrador, docente, estudiante) para controlar qué puede ver y hacer cada actor en el sistema. | Alta | Must | PER-01 | **Dado** un usuario registrado.<br>**Cuando** cambio su rol desde el panel administrativo.<br>**Entonces** la interfaz y los permisos de acceso reflejan inmediatamente el nuevo rol asignado. |
| ADM-02 | Como administrador, quiero aplicar control de acceso por rol para evitar que usuarios vean información o realicen acciones no autorizadas. | Alta | Must | ADM-01 | **Dado** un usuario autenticado con rol estudiante.<br>**Cuando** intenta acceder a funciones del administrador.<br>**Entonces** el sistema bloquea el acceso y registra el intento en el log de auditoría. |
| ADM-03 | Como sistema, quiero auditar acciones críticas (crear, editar, eliminar preguntas; cambios de rol; configuración de simulacros) para mantener trazabilidad institucional. | Alta | Must | — | **Dado** que ocurre una acción crítica en el sistema.<br>**Cuando** se ejecuta.<br>**Entonces** se registra automáticamente: usuario, acción, entidad afectada, valor anterior, valor nuevo y marca de tiempo. |

---

# 8. REGLAS DE NEGOCIO TRANSVERSALES

Las siguientes reglas aplican de manera transversal a todos los módulos del sistema y deben ser respetadas en cada decisión de diseño, desarrollo y prueba.

| ID | Regla | Descripción |
|------|------|------|
| RN-01 | Unicidad de competencia | Una pregunta no puede pertenecer a varias competencias simultáneamente. Esto garantiza que el diagnóstico de fallos sea preciso y no ambiguo. |
| RN-02 | Escalado de dificultad | El sistema escala la complejidad de las preguntas presentadas de forma proporcional al nivel de desempeño acumulado del estudiante, evitando clasificaciones rígidas. |
| RN-03 | Escala de evaluación | Los niveles de dominio siguen la escala estándar del ICFES (1 al 4) para garantizar coherencia con los reportes oficiales de las pruebas Saber Pro. |
| RN-04 | Gamificación alineada | Los puntos y niveles de gamificación se calculan con base en el desempeño real por competencia, no solo por completar simulacros, para mantener el vínculo pedagógico. |
| RN-05 | Privacidad de datos | Los datos personales de los estudiantes (correo, carrera, semestre) no se exponen en el ranking público ni en reportes agregados; solo se usan internamente para personalización. |
| RN-06 | Registro XAPI obligatorio | Toda interacción del estudiante con un simulacro activo (respuesta, tiempo, intento, ayuda) debe quedar registrada bajo el estándar XAPI antes de avanzar a la siguiente pregunta. |
| RN-07 | Control docente sobre banco | Ninguna pregunta generada o cargada al banco queda activa en simulacros sin al menos una revisión y aprobación por parte de un docente o administrador. |
| RN-08 | Auditoría de acciones críticas | Toda acción de crear, editar o eliminar sobre entidades clave (preguntas, simulacros, roles, usuarios) genera un registro de auditoría con usuario, fecha y detalle del cambio. |

---
