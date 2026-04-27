# Fase 3: Diseño Computacional

## 🎯 La Meta Principal

Diseñar la estructura computacional del simulador Saber Pro de manera que la arquitectura tecnológica permita operacionalizar el modelo pedagógico basado en competencias, registrar evidencias de desempeño y procesar analítica de aprendizaje para generar retroalimentación inteligente.

Esta fase traduce la estructura pedagógica definida previamente en componentes lógicos, flujos de procesamiento y mecanismos de captura de datos que harán posible la interpretación automatizada del desempeño estudiantil.

---

## ⚠️ El Problema

En muchos desarrollos de software educativo, la arquitectura computacional se diseña únicamente para administrar preguntas, usuarios y resultados, sin contemplar la complejidad de los procesos pedagógicos que se desean modelar.

Esto produce sistemas que:

- Evalúan respuestas pero no interpretan competencias.
- Registran puntajes pero no evidencias de aprendizaje.
- Carecen de trazabilidad sobre cómo el estudiante resolvió una prueba.
- No pueden generar diagnósticos formativos automáticos.
- No soportan procesos de adaptación o personalización.

Para este proyecto, una arquitectura computacional tradicional sería insuficiente, ya que el sistema debe funcionar como un entorno inteligente de evaluación y aprendizaje, no como un simple cuestionario digital.

---

## 🧠 Modelado Computacional de la Estructura Pedagógica

La arquitectura del sistema debe representar computacionalmente la jerarquía pedagógica del modelo Saber Pro:

### Competencia
Entidad principal que organiza el proceso evaluativo y sirve como eje de análisis del desempeño.

### Afirmación
Subcomponente asociado a una competencia que describe capacidades específicas evaluables.

### Evidencia
Indicador observable que permite inferir el dominio de una afirmación.

### Ítem Evaluativo
Unidad de interacción diseñada para producir evidencias medibles sobre el desempeño del estudiante.

---

## 📊 Diseño del Sistema de Captura de Evidencias

Para soportar el análisis competencial, el sistema debe registrar más que respuestas correctas o incorrectas.

Se modelan computacionalmente eventos de interacción como:

- Tiempo invertido por pregunta.
- Orden de navegación entre preguntas.
- Número de cambios de respuesta.
- Frecuencia de revisión de preguntas.
- Secuencia de resolución del simulacro.
- Finalización o abandono de prueba.

Estos datos serán estructurados mediante eventos XAPI para alimentar la analítica de aprendizaje.

---

## 🔍 Diseño del Motor de Diagnóstico Competencial

El sistema debe incorporar reglas computacionales para transformar respuestas e interacciones en inferencias pedagógicas.

### Funciones del motor diagnóstico:

- Calcular dominio por competencia.
- Inferir nivel de desempeño por evidencia.
- Detectar patrones de error recurrentes.
- Identificar dificultades de lectura o interpretación.
- Priorizar competencias de refuerzo.

---

## 🧮 Diseño del Motor de Analítica de Aprendizaje

Se define la lógica para procesar datos históricos y conductuales del estudiante.

### Variables analizadas:

- Rendimiento por competencia.
- Evolución temporal del desempeño.
- Tendencias de mejora o deterioro.
- Patrones de tiempo/respuesta.
- Comparación entre simulacros.

---

## 🔄 Diseño del Flujo Inteligente de Retroalimentación

El sistema computacional debe transformar resultados en intervención pedagógica automatizada.

### El flujo contempla:

1. Recolección de respuestas e interacciones.
2. Procesamiento de evidencias registradas.
3. Inferencia de nivel competencial.
4. Generación de diagnóstico individual.
5. Producción de recomendaciones personalizadas.

---

## 🏗️ Componentes Tecnológicos Derivados

La implementación requerirá los siguientes subsistemas:

### Subsistema de Evaluación

Gestiona simulacros, preguntas y respuestas.

### Subsistema de Registro XAPI

Captura eventos de interacción estructurados.

### Subsistema Analítico

Procesa datos y ejecuta inferencias diagnósticas.

### Subsistema de Retroalimentación

Presenta resultados interpretados pedagógicamente.

---

## 📌 Resultado Esperado de la Fase

Al finalizar esta fase, el proyecto contará con un diseño computacional capaz de:

- Representar formalmente la estructura pedagógica del sistema.
- Registrar evidencias completas del desempeño.
- Procesar analítica de aprendizaje.
- Generar diagnósticos automáticos por competencia.
- Servir como base para la implementación técnica del simulador inteligente.

---

## 🏛️ Principio MODESEC Aplicado

> "El diseño computacional debe modelar explícitamente la lógica pedagógica que sustenta el sistema."

Este principio garantiza que la tecnología no simplifique el proceso educativo, sino que lo represente fielmente dentro de la arquitectura del software.
