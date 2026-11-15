# Extracción de información de artículos científicos con un LLM local

Convierte una colección de artículos científicos en PDF en una tabla estructurada: una fila por artículo y una columna por variable de interés, con nulos cuando el dato no aparece. La lectura la hace un modelo de lenguaje de Hugging Face que se ejecuta en local, sin APIs de pago.

| documento | condicion | n_participantes | intervencion |
|---|---|---|---|
| fpsyg-08-00308 | acute pain | 46 | placebo and nocebo |
| fpsyg-09-01240 | depression | NULL | NULL |
| fpsyg-11-01354 | anxiety | NULL | NULL |
| fpsyg-12-639236 | stress | 152 | placebo cream |
| fpsyt-14-1301143 | depression | NULL | NULL |
| fpubh-10-1045777 | depression | NULL | NULL |

Los nulos corresponden a revisiones, artículos teóricos o estudios con datos de redes sociales, donde no hay participantes reclutados ni tratamiento.

## Cómo funciona

1. **Descarga** cada artículo por su DOI desde Frontiers (acceso abierto), con la API de Europe PMC como respaldo.
2. **Extrae el texto** con PyMuPDF, quita la bibliografía y lo recorta para que quepa en el contexto del modelo.
3. **Pregunta al LLM** (`Qwen3-4B-Instruct-2507` con GPU, `Qwen2.5-1.5B-Instruct` sin ella) por un JSON con tres variables y `null` si no aparecen.
4. **Construye la tabla** con pandas y la exporta a CSV con los nulos como `NULL`.

## Uso

La forma más sencilla es abrir el notebook en Google Colab con GPU (*Entorno de ejecución → T4 GPU*) y ejecutarlo entero: descarga los artículos y el modelo automáticamente.

En local:

```bash
pip install -r requirements.txt
jupyter notebook extraccion_articulos_llm.ipynb
```

Para analizar otros artículos basta con cambiar el diccionario `REFERENCIAS` (nombre → DOI); para extraer otras variables, el diccionario `VARIABLES`.

## Tecnologías

Python · Hugging Face Transformers · PyTorch · PyMuPDF · pandas · Europe PMC API
