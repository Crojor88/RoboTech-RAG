# Asistente Industrial por voz - RAG

Sistema de Retrieval Augmented Generation (RAG) con interacción por voz para consulta de manuales técnicos de robótica e industrial.

## ¿Qué es?

**Asistente Industrial por voz** es un sistema de inteligencia artificial local que permite ingestar, indexar y consultar manuales técnicos de equipos industriales de forma inteligente mediante voz. Utiliza:

- **STT (Speech-to-Text)**: Faster-Whisper para reconocer voz en español
- **RAG**: Modelos de lenguaje locales (Ollama) para procesar documentos PDF y responder preguntas
- **TTS (Text-to-Speech)**: pyttsx3 para responder con voz sintetizada

## ¿Para qué sirve?

- **Consulta por voz**: Habla directamente con el asistente sobre tus manuales técnicos
- **Ingestión de manuales**: Extrae contenido de PDFs incluyendo texto, tablas y diagramas técnicos
- **Indexación semántica**: Almacena el contenido en ChromaDB para búsqueda rápida
- **Respuesta por voz**: El asistente responde hablando automáticamente
- **Interfaz web**: Interfaz Gradio accesible desde el navegador

## Arquitectura

```
Asistente Industrial
├── STT (Faster-Whisper)  → Reconocimiento de voz en español
├── RAG System            → Consulta inteligente de manuales
│   ├── IngestionAgent   → Extracción de PDF
│   ├── IndexerAgent     → Indexación en ChromaDB
│   ├── ConsultantAgent  → Búsqueda semántica
│   └── GeneratorAgent   → Generación con Ollama
└── TTS (pyttsx3)        → Síntesis de voz en español
```

### Tecnologías utilizadas

- **Faster-Whisper**: Reconocimiento de voz (modelo tiny, CPU)
- **Ollama**: Modelos LLM locales (Llama 3.2)
- **ChromaDB**: Base de datos vectorial
- **Sentence-Transformers**: Embeddings (all-MiniLM-L6-v2)
- **Gradio**: Interfaz web
- **pyttsx3**: Síntesis de voz
- **PyMuPDF**: Extracción de PDF

## Requisitos previos

1. **Ollama instalado y ejecutándose**
   - Descarga: https://ollama.com
   - Modelo necesario: `llama3.2`

2. **Python 3.11+**

3. **Dependencias**:
   ```
   pip install -r requirements.txt
   ```

## Instalación

1. Clona el repositorio:
   ```bash
   git clone https://github.com/Crojor88/Asistente-Industrial-por-voz-RAG.git
   cd Asistente-Industrial-por-voz-RAG
   ```

2. Crea un entorno virtual:
   ```bash
   python -m venv venv
   venv\Scripts\activate  # Windows
   ```

3. Instala las dependencias:
   ```bash
   pip install -r requirements.txt
   ```

4. Asegúrate de tener Ollama ejecutándose:
   ```bash
   ollama serve
   ```

## Uso

### Iniciar la interfaz web

```bash
python app.py
```

Accede a `http://127.0.0.1:7860` en tu navegador.

### Ingestar manuales

```python
from src.main import RoboTechRAG

rag = RoboTechRAG()

# Ingestar un manual PDF
rag.ingest_manual(
    pdf_path="data/manuals/mi_manual.pdf",
    manufacturer="Siemens",
    model="S7-1200"
)
```

O usa el script de carga masiva:
```bash
python bulk_ingest.py
```

## Estructura del proyecto

```
Asistente Industrial/
├── app.py                    # Interfaz web Gradio con voz
├── bulk_ingest.py            # Carga masiva de PDFs
├── test_tts.py              # Pruebas de síntesis de voz
├── test_rag.py              # Pruebas del sistema RAG
├── src/
│   ├── main.py              # Clase principal RoboTechRAG
│   └── agents/
│       ├── ingestion.py     # Extracción de PDF
│       ├── indexer.py       # Indexación ChromaDB
│       ├── consultant.py    # Búsqueda semántica
│       └── generator.py     # Generación de respuestas
├── data/
│   ├── db/                  # ChromaDB
│   └── manuals/             # PDFs de manuales
└── requirements.txt
```

## Manuales incluidos

- Siemens S7-1200
- Schneider GV2
- Schneider iID
- Schneider IC60N

## Limitaciones

- Requiere Ollama ejecutándose localmente
- Los primeros arranques pueden tardar en descargar modelos
- TTS usa voces del sistema (Windows)
- Solo soporta PDFs por ahora
