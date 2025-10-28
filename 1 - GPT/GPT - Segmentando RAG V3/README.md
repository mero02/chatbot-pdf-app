# Proyecto: Chatbot con RAG (Retrieval-Augmented Generation) V3

Este proyecto implementa un chatbot inteligente que responde preguntas basadas en el contenido de documentos PDF. Utiliza técnicas de RAG (Retrieval-Augmented Generation) para recuperar información relevante del documento y generar respuestas precisas. El proceso se divide en varias etapas: carga y segmentación del PDF, vectorización, recuperación de contexto, generación de respuestas y análisis de resultados. Todo se hace en Google Colab con datos almacenados en Google Drive.

## ¿Qué hace este proyecto?
El objetivo es crear un chatbot que pueda responder preguntas sobre el contenido de un PDF de manera precisa, recuperando solo la información relevante del documento. Usa embeddings para buscar fragmentos similares a la pregunta y un modelo de lenguaje para generar respuestas basadas en ese contexto.

---

## Etapa 1: Interfaz con carga estática de PDF + preguntas
**¿Qué hace?**  
En esta etapa, se carga un PDF fijo, se extrae y segmenta su texto, se vectoriza para búsqueda rápida, y se crea una interfaz básica para hacer preguntas.

**Pasos principales:**
- Instalar librerías como PyMuPDF para leer PDFs.
- Extraer texto del PDF página por página, limpiarlo y dividirlo en segmentos de longitud limitada.
- Usar sentence-transformers para convertir segmentos en embeddings vectoriales.
- Crear un índice FAISS para búsqueda rápida de similitud.
- Definir función para recuperar contexto relevante basado en la pregunta.
- Cargar modelo TinyLlama para generar respuestas basadas en el contexto recuperado.
- Crear interfaz en Gradio para preguntas interactivas.

**Resultado:** Una interfaz funcional donde puedes hacer preguntas sobre el PDF y obtener respuestas basadas en su contenido.

---

## Etapa 2: Interfaz con carga dinámica de PDF + preguntas
**¿Qué hace?**  
Similar a la etapa 1, pero permite cargar PDFs dinámicamente en la interfaz, con mejoras en la limpieza de texto y configuración de parámetros.

**Pasos principales:**
- Instalar todas las dependencias necesarias.
- Configurar modelos de embeddings (all-MiniLM-L6-v2 o all-mpnet-base-v2) y LLM (TinyLlama).
- Funciones para extraer segmentos del PDF con mejor limpieza (eliminar espacios múltiples, dividir párrafos largos).
- Construir índice FAISS con normalización de embeddings para mejor similitud.
- Recuperar contexto con umbral de similitud para filtrar resultados irrelevantes.
- Generar respuestas con prompt estructurado, asegurando que solo use el contexto proporcionado.
- Interfaz Gradio avanzada con carga de archivos, sliders para parámetros y ejemplos.

**Resultado:** Una interfaz más robusta y flexible para cargar cualquier PDF y hacer preguntas sobre él.

---

## Etapa 2.1: Pruebas del modelo
**¿Qué hace?**  
Aquí se ejecutan pruebas automáticas del sistema RAG con un conjunto de preguntas predefinidas, evaluando la calidad de las respuestas.

**Pasos principales:**
- Configurar modelos y funciones similares a etapas anteriores.
- Definir función de evaluación que puntúa respuestas del 1 al 5 (1=incorrecta, 5=perfecta), considerando tipo de pregunta (simple, compleja, fuera de dominio).
- Ejecutar pruebas: procesar PDF, cargar CSV de preguntas, generar respuestas y evaluar automáticamente.
- Guardar resultados en CSV con estadísticas por tipo de pregunta.

**Resultado:** Archivo CSV con resultados de pruebas, mostrando precisión por categoría.

---

## Etapa 2.2: Análisis de los resultados
**¿Qué hace?**  
Analiza los resultados de las pruebas mediante gráficos y estadísticas para evaluar el rendimiento del chatbot.

**Pasos principales:**
- Cargar CSV de resultados.
- Crear gráficos: distribución de puntajes por tipo de pregunta, porcentaje de éxito, desglose detallado, análisis de errores y matriz de calor.
- Función para guardar todos los gráficos en alta resolución.

**Resultado:** Conjunto de gráficos que muestran fortalezas y debilidades del sistema RAG.

---

## Etapa 2.3: Revisión de los resultados
**¿Qué hace?**  
Revisa y unifica resultados de múltiples pruebas (diferentes umbrales o configuraciones), permitiendo evaluación manual final.

**Pasos principales:**
- Cargar múltiples CSVs de resultados (ej. con diferentes umbrales de similitud: 30%, 80%, 100%).
- Unificar por pregunta, mostrando todas las respuestas obtenidas.
- Evaluación manual: revisar cada pregunta, comparar respuestas y asignar precisión final con observaciones.
- Guardar CSV unificado con evaluaciones finales.

**Resultado:** CSV consolidado con evaluaciones manuales para análisis final.

---

## Requisitos y Cómo Ejecutar
- **Entorno:** Google Colab (con acceso a Google Drive).
- **Datos:** Necesitas un PDF (ej. "Redes_entrada.pdf") y CSVs de preguntas/respuestas en tu Drive.
- **Ejecución:** Ejecuta los notebooks en orden (1, luego 2, luego 2.1, 2.2, 2.3). Asegúrate de montar Drive y ajustar rutas.
- **Dependencias:** Se instalan automáticamente en cada notebook.

Este proyecto demuestra una implementación completa de RAG para chatbots basados en documentos, con énfasis en evaluación y mejora iterativa.