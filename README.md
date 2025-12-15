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

print(response)
```

---

## Diferencias con Ollama

### ¿Qué tipo de herramienta es Ollama?

Ollama es un runtime de modelos LLM local, fácil, estable y orientado a desarrolladores.

Está orientado a:
- Correr modelos localmente de forma simple.
- Servir modelos vía API HTTP como si fuera OpenAI.
- Prototipar apps rápidamente.
- Integrar LLMs en backend/frontend sin complicación.
- Uso en equipos (on-premises) donde la privacidad es prioridad.
- Trabajar con modelos en formato GGUF altamente optimizados.

Tipo de herramienta:
- Runtime práctico + servidor de inferencia LLM
- Muy parecido conceptualmente a:
  - OpenAI API (pero local)
  - LM Studio (pero sin UI)

O sea, es una herramienta para usar modelos rápidamente, no para entrenarlos ni modificarlos a bajo nivel.

### ¿Qué tipo de herramienta es Cortex?

Cortex es una herramienta más técnica para gestionar modelos y ejecutarlos, pero con menos estabilidad.

Está orientado a:
- Descargar, gestionar e instalar modelos compatibles.
- Ejecutar modelos LLM con opciones avanzadas.
- Controlar ciertos aspectos de rendimiento y hardware.
- Usuarios más técnicos que quieren otra alternativa al runtime clásico.

Tipo de herramienta:
- CLI de gestión + motor de ejecución de modelos
- Más parecido a:
  - `huggingface-cli` + un pequeño runtime
  - Herramientas experimentales tipo `mlc-llm` o `koboldcpp`

No es tan “API-first” ni tan pulido como Ollama.

### Resumen comparativo

| Característica | Ollama | Cortex |
|----------------|--------|--------|
| **Naturaleza** | Runtime + servidor API | CLI para modelos + runtime |
| **Enfoque** | Usarla como una API local tipo OpenAI | Gestionar modelos y ejecutarlos manualmente |
| **Nivel de usuario** | Intermedio / desarrolladores comunes | Más técnico |
| **Experiencia** | Rápida, estable, fluida | Variable, más experimental |
| **Objetivo** | Consumir modelos fácilmente | Manipular e instalar modelos específicos |
| **Optimización** | Pre-armada y automatizada | Depende del modelo elegido |

### Conclusión

**Ollama** es una herramienta práctica para usar modelos y hacer apps. Mientras que **Cortex** está más orientada orientada a gestionar y ejecutar modelos manualmente.

**Ollama** y **Cortex** NO son herramientas de entrenamiento, son herramientas de inferencia (ejecutar modelos).

El fine-tuning se hace antes, con frameworks de ML, y después el modelo resultante se usa en **Ollama** o **Cortex**.
