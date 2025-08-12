
# Asistente Contextual desde PDF

Este proyecto permite cargar un archivo PDF y hacerle preguntas directamente, como si fuera un chatbot entrenado con ese contenido. Todo funciona desde una única plantilla en Google Colab.

---

## ¿Qué permite?

- Cargar cualquier PDF con contenido técnico o teórico (ej: resúmenes, apuntes).
- Hacerle preguntas en lenguaje natural.
- Responder con base en el contenido del PDF usando búsqueda semántica y un modelo generador.
- Si no tiene la respuesta en el PDF, intenta no inventar (configurable).
- Funciona con modelos ligeros como TinyLlama.
- Permite ejecutar pruebas masivas cargando un CSV con preguntas y respuestas esperadas para evaluar precisión.

---

##  Requisitos

- Cuenta en Google (para usar Google Colab).
- No requiere GPU paga (funciona con la gratuita de Colab).
- No requiere clave de API.
- PDF legible (no escaneado, texto plano).

---

##  Etapas del sistema

1. **Carga de PDF**  
   Se extrae el contenido del archivo, se segmenta y se filtran fragmentos útiles (mínimo 20 palabras por bloque).

2. **Vectorización**  
   Se generan representaciones numéricas de los fragmentos usando paraphrase-multilingual-mpnet-base-v2 (modelo de embeddings).

3. **Indexación FAISS**  
   Se crea una base de datos de vectores para búsqueda rápida y eficiente.

4. **Generación de respuestas**
   Los fragmentos más relevantes se envían como contexto a un modelo de lenguaje (TinyLlama por defecto), que redacta la respuesta final.

5. **Interfaz Gradio**  
   Se despliega una app donde:
   - Cargás el PDF.
   - Escribís una pregunta.
   - Elegís cuántos fragmentos relevantes buscar.
   - El modelo responde basándose en esos fragmentos.

6. **Modo de pruebas masivas (opcional)**
   - Cargá un PDF como siempre.
   - Usá el script de evaluación para leer un CSV con preguntas y respuestas esperadas.
   - Obtendrás un nuevo CSV con las respuestas generadas, puntuaciones de similitud y métricas de precisión.

---

##  ¿Cómo usarlo?

1. **Abrí el Colab.**
2. **Ejecutá todas las celdas** desde el inicio (instalan las dependencias y modelos).
3. **Cargá un PDF** desde la interfaz.
4. **Escribí una pregunta.**
5. **Obtené una respuesta basada en tu PDF!**

---

##  Personalizaciones posibles

- Cambiar el modelo de lenguaje (TinyLlama) por otro más grande si tenés recursos.
- Ajustar la cantidad de fragmentos que se usan como contexto (TOP_K).
- Ajustar el umbral de similitud (SIMILARITY_THRESHOLD) para ser más estricto o más permisivo.
- Agregar botones para guardar el índice, respuestas, o historial.
- Incorporar memoria conversacional si querés extenderlo.
- Automatizar más métricas de evaluación en el modo CSV.

---

##  Observaciones técnicas

- Los embeddings se calculan solo una vez por PDF.
- El modelo solo responde en base a lo que se recupera (no está entrenado).
- Si no hay información relevante, puede dar una respuesta genérica o indicar que no sabe.
- Todo se mantiene en RAM durante la sesión del Colab.
- El modo de pruebas CSV permite estudiar el comportamiento del sistema de forma controlada y reproducible.

---

##  Limitaciones

- No memoriza interacciones anteriores (no tiene estado de conversación).
- No detecta automáticamente si inventó una respuesta (eso depende del usuario).
- No funciona bien con PDFs escaneados o imágenes.

---

##  Pruebas

El sistema ahora incluye la ejecución de pruebas automatizadas dentro del mismo archivo principal que carga el PDF y responde preguntas. Esto permite:
- Probar el chatbot de forma masiva con un conjunto de preguntas definidas en un CSV.
- Comparar cada respuesta obtenida con la respuesta esperada.
- Guardar automáticamente los resultados en un nuevo CSV, con la columna precision estimada y espacio para observaciones.

### ¿Cómo funciona?
- Se carga un PDF base (igual que en el modo normal).
- Se carga un CSV con las pruebas (columnas: tipo_pregunta, pregunta, respuesta_esperada).
- El sistema ejecuta todas las preguntas, guarda la respuesta generada y calcula una precisión inicial.
- Se genera un CSV de resultados con:
   - Tipo de pregunta
   - Pregunta
   - Respuesta esperada
   - Respuesta obtenida
   - Puntaje estimado
   - Observaciones (vacías inicialmente, para revisión manual)

- 📄 Archivo de ejecución: incluido en el notebook principal.

---

## Revisión manual de pruebas
Para mejorar la evaluación, se incluye un segundo notebook:
- 📄 revision_pruebas_chatbot_vector_pdf.ipynb

### ¿Qué hace?
- Carga el CSV de resultados generado por las pruebas.
- Permite revisar cada respuesta de forma manual.
- Asignar una puntuación ajustada según el criterio del evaluador.
- Guardar un nuevo CSV con las puntuaciones corregidas.

### ¿Para qué sirve?
- Corregir casos donde la precisión automática no refleja la calidad real.
- Documentar observaciones detalladas de errores o aciertos.
- Hacer un análisis más profundo antes de pasar al análisis gráfico.

---

##  Análisis de las pruebas
Para facilitar la interpretación de los resultados obtenidos en las pruebas automatizadas, se incluye un segundo notebook dedicado exclusivamente al análisis visual de los datos.

### ¿Qué hace el análisis?
El archivo analisis_pruebas_chatbot_vector_pdf.ipynb toma como entrada el archivo CSV generado tras las pruebas (con las columnas tipo_pregunta, pregunta, respuesta_esperada, respuesta_obtenida, precision, observaciones) y realiza lo siguiente:
- Distribución de puntajes: Muestra cómo se comportó el modelo globalmente (desde respuestas incorrectas hasta totalmente correctas).
- Porcentaje de aciertos: Compara el rendimiento según tipo de pregunta (simple, compleja, fuera de dominio).
- Análisis desglosado por tipo: Evalúa los puntajes obtenidos en cada categoría de pregunta.
- Errores frecuentes: Detecta patrones de fallos comunes (respuestas genéricas, confusiones, omisiones).
- Matriz de confusión: Compara precisión esperada vs. precisión obtenida para visualizar los desvíos más frecuentes.

### ¿Para qué sirve?
- Identificar fortalezas y debilidades del modelo.
- Evaluar la consistencia de las respuestas.
- Guiar ajustes futuros en el índice, el modelo o los parámetros de recuperación.
- Facilitar la validación manual gracias a los gráficos generados automáticamente.

### ¿Dónde está el análisis?
El análisis se realiza desde el archivo:
- 📄 analisis_pruebas_chatbot_vector_pdf.ipynb

Podés cargar cualquier CSV de resultados generado por el sistema de pruebas y visualizar automáticamente los gráficos para obtener un diagnóstico más claro del desempeño del chatbot.