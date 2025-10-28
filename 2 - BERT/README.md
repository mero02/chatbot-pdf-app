# Proyecto: Chatbot con BERT (Question Answering Extractiva)

Este proyecto implementa un chatbot que responde preguntas sobre documentos PDF usando un modelo BERT especializado en Question Answering (QA) extractiva. El sistema recupera fragmentos relevantes del PDF y extrae respuestas directas del texto, sin generar nuevo contenido. Se divide en tres etapas principales: interfaz con carga dinámica de PDF, revisión de resultados y análisis de resultados. Todo se hace en Google Colab con datos almacenados en Google Drive.

## ¿Qué hace este proyecto?
El objetivo es crear un chatbot que responda preguntas cerradas o factuales sobre el contenido de un PDF, extrayendo respuestas literales del documento. Usa embeddings para buscar fragmentos similares y BERT QA para encontrar la respuesta exacta dentro de esos fragmentos.

---

## Etapa 1: Interfaz con carga dinámica de PDF + preguntas (BERT puro)
**¿Qué hace?**  
En esta etapa, se crea una interfaz para cargar PDFs dinámicamente, procesarlos en segmentos, vectorizarlos y responder preguntas usando BERT QA extractiva.

**Pasos principales:**
- Instalar librerías como Gradio, PyMuPDF, sentence-transformers y transformers.
- Configurar modelos: embeddings con all-mpnet-base-v2 y BERT QA pre-entrenado (bert-large-uncased-whole-word-masking-finetuned-squad).
- Extraer texto del PDF, limpiarlo y dividirlo en segmentos.
- Construir índice de embeddings para búsqueda rápida.
- Recuperar contexto relevante basado en similitud coseno con umbral.
- Usar BERT QA para extraer respuestas directas del contexto (sin generación, solo extracción).
- Interfaz Gradio con carga de PDF, sliders para parámetros y ejemplos.
- Incluir evaluación automática de respuestas con puntuación 1-5 y pruebas masivas desde CSV.

**Resultado:** Interfaz funcional para cargar PDFs y hacer preguntas, con sistema de evaluación integrado.

---

## Etapa 2: Revisión de los resultados
**¿Qué hace?**  
Revisa los resultados de pruebas automáticas, permitiendo evaluación manual final para refinar las calificaciones.

**Pasos principales:**
- Cargar CSV de resultados automáticos.
- Agrupar por pregunta para comparar respuestas.
- Evaluación manual: revisar cada pregunta, comparar respuesta obtenida vs esperada y asignar precisión final con observaciones.
- Guardar CSV unificado con evaluaciones manuales.

**Resultado:** CSV con evaluaciones refinadas para análisis más preciso.

---

## Etapa 3: Análisis de los resultados
**¿Qué hace?**  
Analiza los resultados evaluados manualmente mediante gráficos y estadísticas para evaluar el rendimiento del sistema BERT QA.

**Pasos principales:**
- Cargar CSV de resultados manuales.
- Crear gráficos: distribución de puntajes por tipo de pregunta, porcentaje de éxito, desglose detallado, análisis de errores y matriz de calor.
- Función para guardar todos los gráficos en alta resolución.

**Resultado:** Conjunto de gráficos que muestran rendimiento por tipo de pregunta y áreas de mejora.

---

## Requisitos y Cómo Ejecutar
- **Entorno:** Google Colab (con acceso a Google Drive).
- **Datos:** Necesitas un PDF y CSVs de preguntas/respuestas en tu Drive.
- **Ejecución:** Ejecuta los notebooks en orden (1, luego 2, luego 3). Asegúrate de montar Drive y ajustar rutas.
- **Dependencias:** Se instalan automáticamente en cada notebook.

### Requisitos para Ejecutar en PC Local
Si deseas ejecutar este proyecto en una PC local en lugar de Google Colab, considera los siguientes requisitos mínimos (basados en los modelos utilizados):

- **Procesador (CPU):** Intel i5 o AMD Ryzen 5 (o superior). Compatible con AVX2 para optimizaciones de transformers.
- **Memoria RAM:** Mínimo 16 GB, recomendado 32 GB. BERT QA grande requiere ~4-6 GB de RAM solo para el modelo, más embeddings y procesamiento.
- **Almacenamiento (Disco):** Al menos 20 GB libres. Los modelos (embeddings + BERT QA) ocupan ~5-7 GB en disco, más espacio para datos y resultados.
- **Tarjeta Gráfica (GPU):** Recomendada NVIDIA con al menos 8 GB VRAM (ej. GTX 1080 Ti, RTX 2060 o superior). Soporte para CUDA 11.0+. Sin GPU, el procesamiento será mucho más lento (CPU-only).
- **Sistema Operativo:** Windows 10/11, macOS 10.15+, o Linux (Ubuntu recomendado).
- **Software:** Python 3.8+, pip, y librerías como PyTorch con CUDA, transformers, sentence-transformers, etc. Instala vía `pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118` para GPU.
- **Notas:** En PC local, ajusta DEVICE a "cpu" si no tienes GPU. El procesamiento de PDFs grandes puede ser lento sin GPU. Para desarrollo, usa entornos virtuales como conda.

Este proyecto demuestra QA extractiva con BERT para respuestas precisas y literales de documentos. Ideal para preguntas factuales donde la respuesta está directamente en el texto.