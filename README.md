# Checador virtual con IA

**Proyecto final · Sistemas Inteligentes (MAC, semestre 2027-1)** · Startup basada en Inteligencia Artificial

Las rutas de transporte concesionado en México (combis, microbuses y camiones urbanos) se regulan a mano: un checador parado en un punto anota a qué hora pasa cada unidad y avisa cuánto le lleva la de adelante. Solo hay información donde hay checador, y las unidades terminan **amontonadas**: varias pasan juntas y luego dejan huecos largos.

Proponemos un **checador virtual con IA** que rastrea las unidades de la ruta, **predice cuándo llegarán a cada parada** y **avisa al conductor** cuando se está pegando a la unidad de adelante.

| Usuario | Qué obtiene |
| --- | --- |
| Pasajero | Tiempo estimado de llegada de la siguiente unidad a su parada |
| Conductor | Avisos cortos: cuánto le lleva la unidad de adelante y si debe bajar o mantener el ritmo |
| Checador | Vista de toda la ruta, alertas de amontonamiento y reporte del día |

## Contenido

| Archivo | Qué es |
| --- | --- |
| `checador_virtual_ia.ipynb` | Notebook principal: simulador de la ruta, modelos de IA, regulación, demostración y conclusiones |
| `requirements.txt` | Dependencias con versiones fijas |
| `modelos/` | Se genera al ejecutar el notebook: modelos entrenados y configuración |

### Qué hace el notebook

1. **Simulador** de una ruta ficticia de 14 paradas y 32 unidades durante 42 días. Los tiempos son supuestos del equipo. El amontonamiento surge solo, igual que en la calle.
2. **Predicción de llegada (ETA)** con *gradient boosting* y con una **red neuronal (Keras)**. Ambos se comparan contra el tiempo por experiencia y el promedio histórico.
3. **Detección temprana de amontonamiento** a 3 paradas, comparada contra la regla del checador.
4. **Regulación**: se resimulan los días de prueba con 4 formas de operar (sin control, checador, regla en todas las paradas e IA).
5. **Demostración** de lo que verían el pasajero, el conductor y el checador.

## Resultados (8 días de prueba, datos simulados)

| Indicador | Sin IA | Con la solución |
| --- | --- | --- |
| Error medio al predecir la llegada | 5.9 min (experiencia) / 2.5 min (promedio histórico) | **1.8 min** |
| Llegadas amontonadas | 12.9 % sin control / 6.2 % con checador | **3.3 %** |
| Espera promedio del pasajero | 4.75 / 4.46 min | **4.37 min** |

> **Sobre los datos.** No contamos con registros reales de una ruta: todo se genera con un simulador. Lo que se demuestra es que el sistema funciona y supera a métodos simples dentro de la simulación. La precisión real se validaría con una ruta piloto.

## Cómo ejecutarlo

Requiere **Python 3.14**.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab checador_virtual_ia.ipynb
```

Ejecuta todas las celdas en orden; tarda unos 4 minutos en CPU, la mayor parte en entrenar la red neuronal. Los resultados son reproducibles (semilla 42).

## Equipo

| Integrante | Módulo |
| --- | --- |
| _Pendiente_ | Datos y simulador |
| _Pendiente_ | Modelos de IA y evaluación |
| _Pendiente_ | Regulación y escenarios |
| _Pendiente_ | Demostración y conclusiones |
