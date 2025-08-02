
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

