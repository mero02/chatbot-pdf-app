# Proyecto: Entrenamiento de Chatbot con GPT (Versión 2)

Este proyecto se enfoca en entrenar un chatbot especializado en desarrollo web usando un modelo de lenguaje pequeño llamado TinyLlama. El proceso se divide en tres etapas principales: entrenamiento del modelo, pruebas del modelo entrenado y análisis de los resultados. Todo se hace en Google Colab con datos almacenados en Google Drive.

## ¿Qué hace este proyecto?
El objetivo es crear un chatbot que pueda responder preguntas sobre desarrollo web de manera precisa, considerando temas y subtemas específicos. Usamos técnicas de aprendizaje automático para "enseñar" al modelo con preguntas y respuestas de un dataset, y luego lo probamos para ver qué tan bien funciona.

---

## Etapa 1: Entrenamiento del Modelo
**¿Qué hace?**  
En esta etapa, preparamos y entrenamos el modelo de lenguaje. Primero instalamos las librerías necesarias (como transformers y torch). Luego cargamos un archivo CSV con preguntas y respuestas sobre desarrollo web, incluyendo temas y subtemas, lo convertimos en un dataset y lo dividimos en partes para entrenamiento y prueba.

**Pasos principales:**
- Cargamos el modelo base TinyLlama y su tokenizer (herramienta para procesar texto).
- Aplicamos una técnica llamada LoRA para hacer el entrenamiento más eficiente (no cambia todo el modelo, solo partes clave).
- Tokenizamos los datos: convertimos las preguntas y respuestas en un formato que el modelo entiende, agregando instrucciones como "[INST] Eres un asistente experto en desarrollo web. Tema: {tema}. Subtema: {subtema}. Responde claramente: {pregunta} [/INST]".
- Entrenamos el modelo usando un Trainer de Hugging Face, con configuraciones como tasa de aprendizaje y número de épocas.
- Guardamos el modelo entrenado en local y en Google Drive.

**Resultado:** Un modelo listo para generar respuestas contextualizadas por tema y subtema.

---

## Etapa 2: Pruebas del Modelo Entrenado
**¿Qué hace?**  
Aquí probamos el modelo entrenado para ver si responde bien a preguntas, ahora incluyendo temas y subtemas en el prompt. Cargamos el modelo guardado y definimos una función para generar respuestas. Usamos interfaces como Gradio para pruebas interactivas y también generamos pruebas automáticas y manuales con diferentes tipos de preguntas.

**Pasos principales:**
- Cargamos el modelo y tokenizer desde Google Drive.
- Definimos la función `responder()` que toma una pregunta, tema y subtema, los procesa y genera una respuesta usando el modelo (con parámetros como temperatura para variar las respuestas).
- Pruebas con Gradio: Creamos interfaces web donde puedes escribir una pregunta, tema y subtema, y ver la respuesta.
- Generamos pruebas: Tomamos preguntas del dataset original y creamos un CSV con pruebas. Incluimos preguntas vistas en entrenamiento, complejas dentro del dominio (desarrollo web), fuera de dominio y complejas fuera.
- Ejecutamos pruebas manuales (donde evalúas tú mismo) y automáticas (usando similitud de texto para comparar respuestas esperadas vs generadas).
- Guardamos los resultados en CSV para análisis.

**Resultado:** Archivos con resultados de pruebas, mostrando qué tan bien responde el modelo a diferentes tipos de preguntas en el contexto de desarrollo web.

---

## Etapa 3: Análisis de Resultados del Chatbot
**¿Qué hace?**  
En esta etapa, analizamos los resultados de las pruebas para entender el rendimiento del modelo. Usamos gráficos y estadísticas para ver dónde funciona bien y dónde falla.

**Pasos principales:**
- Instalamos librerías para gráficos (matplotlib, seaborn).
- Cargamos el CSV de resultados de las pruebas.
- Calculamos precisión por tipo de prueba (por ejemplo, preguntas vistas vs fuera de dominio).
- Creamos gráficos: barras para precisión, histogramas para similitud entre respuestas esperadas y generadas, y conteo de correctas/incorrectas.
- Mostramos ejemplos de errores (respuestas incorrectas con baja similitud) para análisis cualitativo.

**Resultado:** Gráficos y estadísticas que muestran el rendimiento del chatbot, ayudando a identificar fortalezas y debilidades en el dominio de desarrollo web.

---

## Requisitos y Cómo Ejecutar
- **Entorno:** Google Colab (con acceso a Google Drive).
- **Datos:** Necesitas un CSV llamado "desarrollo_preguntas_respuestas.csv" en tu Drive con columnas "Tema", "Subtema", "Pregunta" y "Respuesta".
- **Ejecución:** Ejecuta los notebooks en orden (Etapa 1, luego 2, luego 3). Asegúrate de montar Drive y ajustar rutas si es necesario.
- **Dependencias:** Se instalan automáticamente en cada notebook.

Este proyecto es una introducción básica al fine-tuning de modelos de lenguaje para chatbots especializados en desarrollo web.