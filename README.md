# Memoria-LSTM-SSI

Repositorio complementario asociado a la memoria de título:

**Aplicación de algoritmos de redes neuronales para la modelación eficiente de problemas de interacción suelo–estructura no lineales**

Este repositorio contiene los notebooks utilizados para el procesamiento de datos, entrenamiento y evaluación de modelos LSTM aplicados a la aproximación de respuestas dinámicas obtenidas desde simulaciones numéricas en STKO/OpenSees. Su objetivo es entregar trazabilidad computacional del flujo metodológico desarrollado en la memoria y facilitar la revisión, reproducción y continuidad del trabajo.

## Notebooks incluidos

El flujo computacional se organiza en cuatro notebooks principales:

| Notebook | Descripción |
|---|---|
| Ordenamiento_INs_OUTs.ipynb | Procesamiento inicial de datos, lectura de registros y salidas de simulación, ordenamiento de variables de entrada y salida, y generación de archivos `.npz` por evento. |
| `LSTM_Teacher.ipynb` | Entrenamiento y evaluación del modelo `LSTM_Teacher`, utilizando variables completas del sistema suelo–estructura. |
| `LSTM_Student.ipynb` | Entrenamiento y evaluación de los modelos `LSTM_Student`, orientados a reducir la cantidad de variables de entrada respecto del modelo Teacher. |
| `Resultados.ipynb` | Consolidación de resultados, inferencia de modelos entrenados, cálculo de métricas y generación de figuras comparativas. |

## Flujo metodológico

El procedimiento implementado sigue el siguiente orden general:

1. Procesamiento de registros sísmicos y respuestas obtenidas desde STKO/OpenSees.
2. Construcción de archivos procesados por evento sísmico.
3. Entrenamiento del modelo `LSTM_Teacher`.
4. Entrenamiento de los modelos `LSTM_Student`.
5. Evaluación de resultados mediante métricas cuantitativas y comparación de historias temporales.

El modelo `LSTM_Teacher` utiliza como entradas principales:

- `vg`: velocidad del registro sísmico de entrada.
- `ag`: aceleración del registro sísmico de entrada.
- `d_mid_x`: desplazamiento horizontal en nodo de interfaz.
- `v_mid_x`: velocidad horizontal en nodo de interfaz.
- `a_mid_x`: aceleración horizontal en nodo de interfaz.

Y predice las siguientes variables de respuesta:

- `a_base_x`: aceleración basal de la estructura.
- `Fx`: fuerza horizontal basal.
- `Mz`: momento basal.

El modelo `LSTM_Student` corresponde a una versión reducida del framework, cuyo objetivo es aproximar la respuesta utilizando una menor cantidad de variables de entrada, principalmente asociadas al movimiento sísmico impuesto.

## Requisitos principales

Los notebooks fueron desarrollados en Python y ejecutados principalmente en Google Colab. Las librerías principales utilizadas son:

```text
numpy
pandas
matplotlib
scikit-learn
torch
pathlib
glob
```

## Estructura esperada de carpetas

Durante el desarrollo del trabajo, los notebooks fueron configurados para trabajar con una estructura base similar a la siguiente:

```text
/content/memoria/
│
├── Time Series Records_MACRO.xlsm
├── Modelos x Registro/
│
├── processed_step_full_v2/
│   ├── EQ001.npz
│   ├── EQ002.npz
│   ├── EQ003.npz
│   └── ...
│
└── runs_colab/
    ├── runs_notebook_B_teacher/
    └── runs_notebook_A_student/
```

## Consideraciones metodológicas

El flujo implementado considera separación de datos a nivel de evento sísmico, con el objetivo de evitar fuga de información entre entrenamiento y validación. Además, se utilizan ventanas temporales para entrenar los modelos, normalización robusta basada en media y percentil 99 absoluto, y evaluación por variable de salida.

Las métricas utilizadas en la evaluación incluyen principalmente:

- RMSE.
- MAE, cuando corresponde.
- Sesgo o bias.
- Coeficiente de correlación.
- Error relativo en peak.
- Tiempo de inferencia o duración de predicción, cuando corresponde.

## Alcance

Este repositorio no corresponde a un software final de uso general, sino a un conjunto de notebooks de investigación utilizados para respaldar el desarrollo de la memoria. Su propósito es documentar el flujo computacional, facilitar la revisión metodológica y servir como base para futuras extensiones del framework.

Cualquier aplicación a nuevos casos de estudio requiere regenerar el dataset con registros sísmicos, modelos numéricos, variables de entrada y variables de salida coherentes con el nuevo sistema suelo–estructura analizado.

## Dataset procesado

El dataset procesado requerido para ejecutar los notebooks se encuentra disponible como archivo complementario en la sección **Releases** del repositorio:

[Descargar dataset procesado] https://github.com/ricardoalvarezv-cpu/Memoria-LSTM-SSI/releases/download/v1.0-dataset/processed_step_full_v2.zip
[Descargar excel eventos] https://github.com/ricardoalvarezv-cpu/Memoria-LSTM-SSI/releases/download/v1.0-dataset/Time.Series.Records_MACRO.xlsm

Para ejecutar los notebooks en Google Colab, se recomienda clonar el repositorio en `/content/memoria` y descomprimir el dataset en esa misma ruta, de modo que la estructura final sea:

```text
/content/memoria/
├── LSTM_Teacher.ipynb
├── LSTM_Student.ipynb
├── Resultados.ipynb
├── Ordenamiento_INs_OUTs.ipynb
└── processed_step_full_v2/
    ├── EQ001.npz
    ├── EQ002.npz
    └── ...
```

## Celda para descomprimir en Colab


```python
from pathlib import Path
import zipfile

ROOT = Path("/content/memoria")
ZIP_PATH = ROOT / "processed_step_full_v2.zip"
PROCESSED_DIR = ROOT / "processed_step_full_v2"

if ZIP_PATH.exists() and not PROCESSED_DIR.exists():
    with zipfile.ZipFile(ZIP_PATH, "r") as zip_ref:
        zip_ref.extractall(ROOT)

print("ROOT:", ROOT)
print("PROCESSED_DIR:", PROCESSED_DIR)
print("Existe dataset:", PROCESSED_DIR.exists())
```

## Nota sobre rutas de ejecución

Los notebooks fueron desarrollados en Google Colab conectado a un entorno de ejecución local mediante contenedor Docker y GPU local. Por este motivo, algunas rutas internas fueron definidas originalmente respecto de una carpeta base `/content/memoria`.

Para reproducir el flujo sin modificar el código, se recomienda clonar el repositorio en Google Colab usando:

```bash
!git clone https://github.com/TU_USUARIO/Memoria-LSTM-SSI.git /content/memoria
```

Luego, el dataset procesado descargado desde la sección Releases debe descomprimirse de modo que quede disponible en:
/content/memoria/processed_step_full_v2/

## Autor

**Ricardo Álvarez**  
Memoria de título — Ingeniería Civil  
Universidad Técnica Federico Santa María
