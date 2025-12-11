# Uso de Cortex

## Qué es Cortex?
Cortex es una plataforma dinámica de API de IA local diseñada para ejecutar y personalizar de forma fácil y eficiente Modelos de Lenguaje Grandes (LLM). Cuenta con una interfaz de línea de comandos (CLI) sencilla, inspirada en Ollama, y ​​está desarrollada íntegramente en C++. Puede descargar el paquete de instalación para Windows, macOS y Linux.

## Qué nos permite?
Cortex nos permite desplegar LLMs (Large Language Models) en local de manera fácil y rápida y además nos ofrece una API totalmente compatible con OpenIA.

---

## Ejemplos de uso

### Interactuar con el modelo vía API Rest y Curl

Al arrancar el modelo Cortex nos habilita una API Rest totalmente compatible con OpenIA para hacer peticiones vía Curl:

```curl
curl  http://127.0.0.1:39281/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
     "model": "llama3.2:3b-gguf-q4-km",
     "messages": [{"role": "user", "content": "Devuelve el analisis de sentimiento del siguiente comentario \"El tutorial no me funcionó en absoluto.\". Devuelve  el valor '-1' en caso de sentimiento de insatisfacción, '0' en caso de sentimiento neutro o '1' en caso de sentimiento de satisfacción. Usa la siguiente plantilla para la respuesta: ```valor```"}],
     "temperature": 0.7
   }'
```

### Interactuar con el modelo vía aplicaciones Python

1. Para empezar a utilizar el modelo vía Python lo primero que hace falta es crear un entorno virtual `env` e instalar como única dependencia la librería de OpenAI:

```python
# Crear y activar entorno virtual
virtualenv env
source env/bin/activate

# Instalar dependencia
pip install openai===1.55.3
```

2. Y después crear nuestra aplicación:

```python
from openai import OpenAI

ENDPOINT = "http://localhost:39281/v1"
MODEL = "llama3.2:3b-gguf-q4-km"

client = OpenAI(
    base_url=ENDPOINT,
    api_key="not-needed"
)

completion_payload = {
    "messages": [
        {"role": "user", "content": "Devuelve el analisis de sentimiento del siguiente comentario \"El tutorial no me funcionó en absoluto.\". Devuelve  el valor '-1' en caso de sentimiento de insatisfacción, '0' en caso de sentimiento neutro o '1' en caso de sentimiento de satisfacción. Usa la siguiente plantilla para la respuesta: ```valor```"}
    ]
}

response = client.chat.completions.create(
    top_p=0.9,
    temperature=0.6,
    model=MODEL,
    messages=completion_payload["messages"]
)

print(response)```
