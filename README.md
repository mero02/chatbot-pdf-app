# Proyecto de Investigación: Chatbots para Procesamiento de PDFs

Este proyecto de investigación explora diferentes enfoques para crear chatbots capaces de responder preguntas basadas en el contenido de documentos PDF. Se han implementado cinco casos principales, cada uno utilizando técnicas de inteligencia artificial variadas: fine-tuning de modelos de lenguaje, Retrieval-Augmented Generation (RAG), Question Answering extractiva con BERT, y un enfoque híbrido. Todos los experimentos se ejecutan en Google Colab con datos en Google Drive.

## Casos Implementados

### 1. GPT - Entrenamiento V1
**Descripción:** Fine-tuning del modelo TinyLlama para un chatbot especializado en redes de computadoras. Se entrena con un dataset de preguntas y respuestas, dividiendo en etapas de entrenamiento, pruebas y análisis.

**Limitaciones:** Limitado al dominio de redes de computadoras; requiere dataset específico; riesgo de overfitting; genera respuestas basadas en entrenamiento, no en documentos externos.

**Cómo continuar:** Expandir a múltiples dominios con datasets más grandes; integrar RAG para combinar con documentos; probar modelos más grandes como GPT-2 o Llama 2; agregar evaluación automática más robusta.

### 2. GPT - Entrenamiento V2
**Descripción:** Similar al V1, pero enfocado en desarrollo web con inclusión de temas y subtemas en el prompt. Mejora el contexto al especificar tema y subtema para respuestas más precisas.

**Limitaciones:** Aún limitado a un dominio (desarrollo web); depende de calidad del dataset con temas/subtemas; no maneja documentos PDF directamente, solo Q&A predefinido; generación puede ser inconsistente sin contexto externo.

**Cómo continuar:** Combinar con RAG para procesar PDFs en tiempo real; usar modelos multilingües para dominios variados; implementar evaluación con métricas como BLEU o ROUGE; agregar memoria conversacional.

### 3. GPT - Segmentando RAG V3
**Descripción:** Implementa RAG completo con TinyLlama: segmenta PDFs, crea embeddings, recupera contexto relevante y genera respuestas. Incluye carga estática/dinámica, pruebas automáticas y análisis detallado.

**Limitaciones:** Depende de calidad de embeddings y segmentación; respuestas pueden ser genéricas si el contexto es insuficiente; requiere GPU para rendimiento; evaluación manual necesaria para precisión.

**Cómo continuar:** Optimizar segmentación con técnicas como sliding windows; integrar modelos más avanzados (e.g., GPT-4 via API); agregar multi-documento support; implementar caching para PDFs procesados; usar evaluación automática con LLMs.

### 4. BERT (Question Answering Extractiva)
**Descripción:** Usa BERT QA para extraer respuestas directas de PDFs sin generación. Recupera fragmentos relevantes via embeddings y extrae respuestas literales del texto.

**Limitaciones:** Solo respuestas extractivas, no genera explicaciones o síntesis; limitado a preguntas factuales; no maneja preguntas abiertas o complejas; requiere contexto exacto en el documento.

**Cómo continuar:** Combinar con modelo generativo para respuestas híbridas; mejorar recuperación con re-ranking; expandir a multi-hop QA; usar BERT multilingüe para documentos no ingleses; integrar con RAG para contexto más amplio.

### 5. Híbrido (BERT + GPT)
**Descripción:** Enfoque híbrido que usa embeddings (basados en BERT) para recuperación de contexto de PDFs, y TinyLlama para generar respuestas basadas en ese contexto. Incluye pruebas masivas y análisis gráfico.

**Limitaciones:** Depende de embeddings para recuperación; generación puede inventar si contexto es pobre; requiere ajuste manual de umbrales; limitado por recursos de TinyLlama.

**Cómo continuar:** Reemplazar TinyLlama con modelos más capaces como Llama 3; integrar QA extractiva como fallback; agregar fine-tuning del generador; implementar multi-modal para PDFs con imágenes; optimizar para CPU-only.

## Ranking de Performance y Condiciones de Progreso

Basado en flexibilidad, precisión reportada, escalabilidad y potencial de mejora:

1. **Híbrido (BERT + GPT)**: Mejor overall. Combina recuperación precisa con generación flexible, maneja PDFs dinámicos, incluye evaluación integrada. Alto potencial para progresar con modelos más grandes y optimizaciones.

2. **GPT - Segmentando RAG V3**: Muy fuerte en RAG puro. Excelente para PDFs, con análisis detallado. Fácil de extender a dominios variados; limitación principal es el modelo generativo pequeño.

3. **BERT (Question Answering Extractiva)**: Excelente para preguntas factuales precisas. Bajo riesgo de alucinaciones, pero limitado en alcance. Bueno para casos específicos; progreso vía integración con generativos.

4. **GPT - Entrenamiento V2**: Mejor que V1 por contexto temático. Bueno para dominios especializados, pero menos flexible que RAG. Progreso limitado sin combinar con recuperación externa.

5. **GPT - Entrenamiento V1**: Básico y limitado. Útil como baseline, pero obsoleto comparado con RAG/híbridos. Menor potencial sin modificaciones mayores.

## Requisitos Generales
- Entorno: Google Colab con GPU gratuita.
- Datos: PDFs legibles y CSVs de preguntas/respuestas.
- Dependencias: Instaladas automáticamente en notebooks.

Para ejecutar, clona el repo y sigue los READMEs individuales en cada carpeta.