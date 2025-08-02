
# Asistente Contextual desde PDF

Este proyecto permite cargar un archivo PDF y hacerle preguntas directamente, como si fuera un chatbot entrenado con ese contenido. Todo funciona desde una única plantilla en Google Colab.

---

## ¿Qué permite?

- Cargar cualquier PDF con contenido técnico o teórico (ej: resúmenes, apuntes).
- Hacerle preguntas en lenguaje natural.
- Responder con base en el contenido del PDF.
- Si no tiene la respuesta en el PDF, intenta no inventar (se puede ajustar).
- Funciona con modelos ligeros como TinyLlama.

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
   Se generan representaciones numéricas de los fragmentos usando `all-MiniLM-L6-v2` (modelo de embeddings).

3. **Indexación FAISS**  
   Se crea una base de datos de vectores para búsqueda rápida y eficiente.

4. **Interfaz Gradio**  
   Se despliega una app donde:
   - Cargás el PDF.
   - Escribís una pregunta.
   - Elegís cuántos fragmentos relevantes buscar.
   - El modelo responde basándose en esos fragmentos.

---

##  ¿Cómo usarlo?

1. **Abrí el Colab.**
2. **Ejecutá todas las celdas** desde el inicio (instalan las dependencias y modelos).
3. **Cargá un PDF** desde la interfaz.
4. **Escribí una pregunta.**
5. **Obtené una respuesta basada en tu PDF!**

---

##  Personalizaciones posibles

- Cambiar el modelo de lenguaje (`TinyLlama`) por otro más grande si tenés recursos.
- Ajustar la cantidad de fragmentos que se usan como contexto (`top_k`).
- Agregar botones para guardar el índice, respuestas, o historial.
- Incorporar memoria conversacional si querés extenderlo.

---

##  Observaciones técnicas

- Los embeddings son precomputados solo una vez por PDF.
- El modelo solo responde en base a lo que se recupera (no está entrenado).
- Si no hay información en el PDF, puede dar una respuesta genérica o indicar que no sabe.
- Todo se mantiene en RAM durante la sesión del Colab.

---

##  Limitaciones

- No memoriza interacciones anteriores (no tiene estado de conversación).
- No detecta automáticamente si inventó una respuesta (eso depende del usuario).
- No funciona bien con PDFs escaneados o imágenes.

---

##  Pruebas

Este proyecto incluye un archivo adicional de pruebas automatizadas para evaluar la calidad de las respuestas del chatbot.

### ¿Qué evalúan las pruebas?
- Precisión de las respuestas: Se compara la respuesta generada con una respuesta esperada.
- Capacidad de recuperación: Verifica si el chatbot encuentra la información relevante en el PDF.
- Resistencia al alucinamiento: Evalúa si el modelo evita inventar contenido cuando la respuesta no está en el PDF.

### ¿Cómo se hacen?
- Se utiliza un PDF base fijo para todas las pruebas.
- Un archivo CSV contiene varias preguntas, junto con las respuestas esperadas.
- Se ejecuta automáticamente una comparación entre:
   - Pregunta
   - Respuesta esperada
   - Respuesta obtenida por el chatbot
   - Se genera un nuevo archivo CSV con los resultados y una columna de precisión estimada.

### ¿Dónde están las pruebas?
Las pruebas están en el archivo:
- 📄 pruebas_chatbot_vector_pdf.ipynb

Podés abrirlo en Google Colab, cargar tu propio PDF y CSV de preguntas, y ejecutar el análisis automático para ver cómo responde el modelo frente a preguntas conocidas.

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