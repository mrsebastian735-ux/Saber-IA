# Capítulo I

# FUNDAMENTACIÓN DEL PROYECTO

## 1. NOMBRE DEL PROYECTO

**SaberIA**
Simulador de Pruebas SaberPRO bajo Metodología MODESEC

## 2. DESCRIPCIÓN

SaberIA es una plataforma de software educativo orientada a transformar la manera en que los estudiantes universitarios se preparan para las pruebas Saber Pro. El sistema no se limita a ofrecer un banco de preguntas: integra evaluación por competencias, analítica de aprendizaje basada en estándares XAPI y gamificación como eje motivacional central, para construir un entorno inteligente que interpreta el desempeño del estudiante, identifica sus fortalezas y dificultades, y genera retroalimentación personalizada orientada al desarrollo real de competencias.

## 3. PROBLEMA QUE BUSCA RESOLVER

Los simuladores de pruebas estandarizadas disponibles actualmente operan bajo una lógica instrumental que privilegia la respuesta correcta sobre la comprensión del proceso cognitivo que la genera. Esta situación produce estudiantes que memorizan formatos sin desarrollar las competencias que el examen realmente evalúa.

El problema central puede sintetizarse en tres dimensiones:

* **Desconexión entre preparación y enfoque competencial:** los métodos de estudio tradicionales no están alineados con la estructura evaluativa de las pruebas Saber Pro, que mide la capacidad de aplicar conocimientos en contextos diversos.
* **Invisibilidad del proceso de aprendizaje:** los simuladores existentes solo registran aciertos y errores, ignorando variables como tiempo de respuesta, patrones de dificultad y trayectorias cognitivas.
* **Ausencia de retroalimentación pedagógica significativa:** el estudiante recibe un puntaje, pero no orientaciones concretas sobre qué competencias debe fortalecer ni cómo hacerlo.

## 4. IDEA DE SOLUCIÓN

SaberIA propone un sistema que parte de la competencia como unidad fundamental de diseño. El docente o administrador configura el banco de preguntas clasificadas por competencia, componente, afirmación y evidencia; el sistema ejecuta simulacros adaptativos, registra cada interacción mediante XAPI y genera retroalimentación inteligente.

La gamificación —como elemento central, no decorativo— incentiva la práctica constante mediante puntos, niveles y recompensas alineadas con el desempeño real.

Específicamente, el sistema permite:

* Clasificar preguntas según la estructura oficial de competencias Saber Pro (componente, afirmación, evidencia).
* Ejecutar simulacros con registro invisible de tiempo por pregunta y patrones de respuesta.
* Construir y actualizar un perfil de desempeño del estudiante basado en evidencias observables.
* Generar retroalimentación dinámica al finalizar cada prueba, indicando las áreas específicas que requieren refuerzo.
* Motivar la práctica continua mediante un sistema de gamificación con puntos, niveles, rachas y tienda virtual.
* Ofrecer al docente reportes analíticos del grupo con tendencias y competencias críticas.

## 5. PROPUESTA DE VALOR INICIAL

SaberIA no es un simulador de preguntas: es un entorno de aprendizaje inteligente que convierte cada simulacro en una fuente de información pedagógica.

Su propuesta de valor se distingue en tres aspectos:

* **Para el estudiante:** recibe retroalimentación personalizada por competencia, ve su evolución en el tiempo y encuentra motivación genuina para practicar gracias a la gamificación integrada.
* **Para el docente:** accede a analítica real del grupo, identifica con precisión qué competencias requieren intervención y toma decisiones pedagógicas con mayor información y menor esfuerzo.
* **Para la institución:** obtiene métricas de preparación que permiten proyectar el desempeño institucional en las pruebas Saber Pro y ajustar estrategias formativas a tiempo.

A diferencia de otros sistemas, SaberIA construye conocimiento pedagógico acumulado del estudiante que no se pierde entre sesiones: el perfil de desempeño evoluciona con cada interacción y alimenta decisiones más precisas con el paso del tiempo.

## 6. USUARIOS PRINCIPALES

### USUARIOS DEL SISTEMA

| Tipo Usuario        | Rol en el sistema                                                                                |
| ------------------- | ------------------------------------------------------------------------------------------------ |
| Usuario principal   | Docente / Administrador: Gestiona el banco de preguntas, configura simulacros y revisa analítica |
| Usuario activo      | Estudiante: Practica simulacros, recibe retroalimentación y acumula puntos                       |
| Usuario beneficiado | Institución educativa: Obtiene métricas de desempeño grupal y tendencias de aprendizaje          |
| Usuario secundario  | Sistema (IA/analítica): Genera retroalimentación automática y adapta la dificultad               |

## 7. OBJETIVO GENERAL

Diseñar un simulador inteligente de pruebas Saber Pro fundamentado en competencias, metodología MODESEC, analítica de aprendizaje XAPI y gamificación, que permita a los estudiantes universitarios prepararse de manera significativa, alineada con el enfoque evaluativo oficial, y que proporcione al docente información pedagógica útil para la toma de decisiones.

## 8. OBJETIVOS ESPECÍFICOS

* Construir un banco de preguntas estructurado por competencias, componentes, afirmaciones y evidencias, alineado con el modelo evaluativo de las pruebas Saber Pro.
* Implementar un motor de simulacros que registre interacciones detalladas del estudiante mediante el estándar XAPI, capturando tiempo, intentos y patrones de respuesta.
* Desarrollar un sistema de gamificación como eje motivacional central, con puntos, niveles, rachas y tienda virtual alineados con el desempeño competencial real.
* Generar retroalimentación dinámica y personalizada al estudiante, identificando con precisión las competencias y áreas que requieren fortalecimiento.
* Ofrecer al docente reportes y métricas analíticas del grupo que faciliten la identificación de tendencias y la toma de decisiones pedagógicas.
* Garantizar la escalabilidad, trazabilidad y seguridad del sistema mediante buenas prácticas de desarrollo, auditoría y control de acceso por roles.

## 9. SUPUESTOS INICIALES

* Los estudiantes universitarios cuentan con acceso a internet y dispositivos para usar la plataforma en cualquier semestre.
* Los docentes están dispuestos a participar en la configuración y validación del banco de preguntas.
* La estructura de competencias del ICFES para las pruebas Saber Pro puede operacionalizarse dentro del sistema como base de diseño.
* La gamificación, cuando está alineada con el desempeño real, aumenta la motivación y la frecuencia de uso de la plataforma.
* El estándar XAPI es suficiente para capturar las interacciones relevantes del estudiante en el contexto del simulacro.

## 10. RIESGOS INICIALES

| Riesgo                                                | Impacto | Mitigación sugerida                                                              |
| ----------------------------------------------------- | ------- | -------------------------------------------------------------------------------- |
| Banco de preguntas insuficiente o mal clasificado     | Alto    | Validar la estructura de competencias con docentes expertos antes del desarrollo |
| Dependencia excesiva de la IA sin revisión pedagógica | Alto    | Establecer revisión docente obligatoria de los ítems generados                   |
| Gamificación que desvía el foco del aprendizaje       | Medio   | Alinear mecánicas de juego con evidencias de desempeño real                      |
| Baja adopción por parte de los estudiantes            | Medio   | Diseño UX atractivo y onboarding guiado desde el primer acceso                   |
| Datos de analítica mal interpretados                  | Alto    | Proveer visualizaciones claras y guías de lectura para docentes                  |

## 11. PREGUNTAS PENDIENTES

* ¿Qué competencias genéricas y específicas de las pruebas Saber Pro se incluirán en la versión inicial del sistema?
* ¿Cómo se validará pedagógicamente la calidad de las preguntas generadas o cargadas al banco?
* ¿Qué variables del perfil de desempeño del estudiante se registrarán y cuáles no?
* ¿Cómo se actualizará el nivel de dificultad de los simulacros en función del desempeño acumulado?
* ¿Qué mecánicas de gamificación tendrán mayor impacto motivacional en estudiantes universitarios?
* ¿Qué métricas demostrarán que el sistema realmente aporta valor pedagógico medible?

## 12. CRITERIO DE ÉXITO INICIAL

El proyecto tendrá una base sólida cuando pueda demostrar con claridad:

* Qué competencias Saber Pro evalúa y cómo las operacionaliza en ítems concretos.
* Cómo construye y actualiza el perfil de desempeño del estudiante a partir de evidencias observables.
* Cómo transforma ese perfil en retroalimentación pedagógica útil y accionable.
* Cómo la gamificación contribuye al aprendizaje y no solo al entretenimiento.
* Cómo valida que el sistema mejora la preparación real del estudiante para la prueba.
* Qué información entrega al docente y cómo este puede usarla para intervenir pedagógicamente.

## 13. NÚCLEO DEL PRODUCTO

El núcleo de SaberIA no es una colección de preguntas ni un juego: es un sistema capaz de:

* Evaluar competencias de manera estructurada y alineada con el enfoque oficial Saber Pro.
* Registrar evidencias detalladas del proceso de aprendizaje, no solo del resultado.
* Retroalimentar al estudiante de manera inteligente y personalizada por competencia.
* Motivar la práctica constante mediante gamificación alineada con el desempeño real.
* Proveer al docente información analítica útil para la toma de decisiones pedagógicas.
* Conservar un perfil de desempeño transversal del estudiante que evoluciona con cada sesión.

## 14. PERSONALIZACIÓN EN LA PRIMERA VERSIÓN CONCEPTUAL

En su lógica central, SaberIA personaliza principalmente:

* El nivel de dificultad de las preguntas presentadas, ajustado al desempeño acumulado del estudiante.
* La retroalimentación entregada al finalizar cada simulacro, específica por competencia y área de dificultad.
* Las recomendaciones de estudio y refuerzo generadas según el perfil de desempeño individual.
* La información que recibe el docente sobre su grupo, filtrada por relevancia pedagógica.

## 15. DECISIONES DEL SISTEMA Y DEL DOCENTE

### DECISIONES DEL SISTEMA vs. DECISIONES DEL DOCENTE

| El sistema (IA) se encarga de                                   | El docente conserva control sobre                                |
| --------------------------------------------------------------- | ---------------------------------------------------------------- |
| Generar y seleccionar preguntas según competencia y nivel       | Definir qué competencias se evalúan en cada simulacro            |
| Registrar tiempos, intentos y patrones de respuesta (XAPI)      | Revisar y aprobar preguntas del banco antes de publicarlas       |
| Asignar puntos y calcular niveles de gamificación               | Interpretar los reportes de analítica de su grupo                |
| Retroalimentar automáticamente al estudiante tras cada pregunta | Ajustar la configuración del simulacro según el contexto         |
| Detectar áreas de dificultad y sugerir rutas de refuerzo        | Decidir cómo usar la información para intervenir pedagógicamente |
