# Detección temprana de estrés estudiantil con IA

**Proyecto final · Sistemas Inteligentes (MAC, 2027-1)** · *nombre de la startup pendiente*

Muchos estudiantes llegan al área de orientación cuando el estrés ya afectó sus calificaciones o su salud, o nunca llegan. Proponemos una **autoevaluación de 6 preguntas** en la que un modelo de IA estima si el nivel de estrés del alumno es **bajo, medio o alto** y le muestra los factores que más pesan. El modelo se ejecuta incluso en el navegador, sin enviar las respuestas a un servidor.

> Herramienta de apoyo y orientación. **No es un diagnóstico clínico.**

## Datos

- **Fuente:** [Student Early Stress Detection](https://www.kaggle.com/datasets/rishavraj1232/student-early-stress-detection) (Rishav Raj, Kaggle, v2). Licencia CC BY-SA 4.0 (en Kaggle aparece también como CC BY 4.0; tomamos la más restrictiva).
- **Auditoría:** el dataset une 6 fuentes. Solo `stress_survey` (**1,100 alumnos**) tiene una señal de estrés real:

| Fuente | Filas | Problema |
| --- | --- | --- |
| `mental_health` | 150,000 | Ruido: ninguna columna predice el estrés |
| `dropout_risk_2026` | 100,000 | Fuga del objetivo: `anxiety_score` es idéntica al estrés |
| `oulad`, `uci_dropout`, `uci_performance` | 38,061 | Estrés deducido de la reprobación (`clase = at_risk × 2`) |
| `stress_survey` | **1,100** | Señal real: se usa para entrenar |

- **Entradas del modelo:** horas de sueño, horas de estudio a la semana, apoyo social, presión académica, actividades extracurriculares y promedio.
- **No se piden** datos demográficos, socioeconómicos ni escalas clínicas (ansiedad, depresión, burnout). Quitarlos **no cuesta precisión** (ver la sección 10 del notebook).

## Resultados

| Modelo | F1 macro, validación cruzada de 5 particiones | F1 macro en prueba (165 alumnos) | Recall de estrés alto en prueba |
| --- | --- | --- | --- |
| Clase mayoritaria | — | 0.17 | 0.00 |
| Regresión logística | 0.88 ± 0.02 | 0.88 | 0.95 |
| Gradient boosting | 0.90 ± 0.01 | 0.88 | 0.93 |
| **Red Keras (producción)** | **0.89 ± 0.01** | **0.86** | **0.91** |

Los tres modelos están empatados. Se elige la red porque es la única que se exporta a TFLite y a la web sin servidor.

**Limitaciones:**
- Los datos no son de México.
- El dataset es demasiado separable (una sola pregunta ya predice mucho), así que estas métricas son un techo optimista.
- El notebook propone un **estudio local** con alumnos de la facultad para validar el modelo y ajustarlo (*fine-tuning*).

## Modelos exportados (`modelos/`)

| Archivo | Uso |
| --- | --- |
| `contrato.json` | **Interfaz común**: orden, unidades, rangos y conversión de las 6 entradas, clases de salida, mensajes y métricas |
| `modelo_estres.keras` | API: `keras.models.load_model(...)` |
| `modelo_estres.tflite` | App móvil, o TFLite en el navegador |
| `pesos_web.json` | Web sin servidor: normalización y pesos para calcular la red en JavaScript |

El modelo recibe los valores **tal como los captura el formulario**, porque la normalización va dentro de la red. Los tres formatos dan las mismas probabilidades (diferencia < 1e-6), y el notebook lo verifica.

## Cómo ejecutar

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
# Descarga el dataset de Kaggle y copia final_student_dataset.csv en data/
jupyter nbconvert --to notebook --execute --inplace modelo_estres.ipynb   # ~1 min en CPU
```

## Equipo

| Integrante | Parte |
| --- | --- |
| *pendiente* | Modelo (este notebook) |
| *pendiente* | API |
| *pendiente* | Web |
| *pendiente* | *pendiente* |
