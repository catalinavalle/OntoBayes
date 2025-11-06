# 🧠 Generación y Evaluación de Ontologías Clínicas con Redes Bayesianas

## 📋 Descripción General
Este proyecto tiene como objetivo **extraer conocimiento clínico desde documentos médicos en PDF**, generar automáticamente una **ontología en formato OWL**, construir una **red bayesiana causal** a partir de las relaciones encontradas y, finalmente, **evaluar el estado clínico de un paciente** a partir de un texto libre mediante inferencia probabilística.

---

## 🔄 Flujo de Trabajo

1. **Carga y procesamiento de documentos PDF**  
   Los archivos médicos son segmentados e indexados en un vectorstore FAISS utilizando embeddings de OpenAI.

2. **Generación automática de ontología OWL**  
   Se identifican factores de riesgo, condiciones y síntomas, construyendo una ontología RDF/XML con relaciones:
   - `Factor → causa → Condición`
   - `Condición → provoca → Síntoma`

3. **Construcción de la red bayesiana**  
   Se generan las dependencias probabilísticas entre factores, condiciones y síntomas, con probabilidades estimadas mediante el modelo de lenguaje.

4. **Evaluación de pacientes**  
   A partir de un texto clínico libre, se extraen los factores y síntomas presentes y se calcula la probabilidad de la condición objetivo junto a una explicación médica automatizada.

---

## ⚙️ Instrucciones de Uso

1. **Instalar dependencias**
   ```bash
   !apt-get -qq install -y graphviz libgraphviz-dev
   !pip install pygraphviz pypdf owlready2 rdflib
   !pip install -r requirements.txt
   ```

2. **Configurar la API de OpenAI**
   ```python
   import os
   os.environ["OPENAI_API_KEY"] = "TU_API_KEY_DE_OPENAI"
   ```

3. **Ejecutar el flujo principal**
   ```python
   carpeta_pdfs = "pdfs"
   documentos = cargar_pdfs(carpeta_pdfs)
   chunks = dividir_texto(documentos)
   vectorstore = crear_vectorstore(chunks)

   extraer_ontologia(vectorstore, "Cáncer Oral")
   relaciones_AC, relaciones_CS = leer_ontologia_owl("ontologia.owl")

   red_bayesiana, relaciones_prob = crear_red_bayesiana(relaciones_AC, relaciones_CS, vectorstore)
   visualizar_red_bayesiana(red_bayesiana, relaciones_prob)

   evaluar_paciente_desde_texto(red_bayesiana, vectorstore, nodos_antecedentes, nodos_sintomas, nodos_condiciones)
   ```

---

## 🎯 Objetivo Final
Ofrecer una herramienta de **razonamiento clínico asistido por IA**, capaz de **construir conocimiento médico estructurado y realizar inferencias probabilísticas explicables** a partir de textos clínicos no estructurados.
