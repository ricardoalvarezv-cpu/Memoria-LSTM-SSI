# Memoria-LSTM-SSI

Repositorio complementario asociado a la memoria:

**Aplicación de algoritmos de redes neuronales para la modelación eficiente de problemas de interacción suelo–estructura no lineales**

Este repositorio contiene los notebooks utilizados para el procesamiento de datos, entrenamiento y evaluación de modelos LSTM aplicados a la aproximación de respuestas dinámicas en un problema de interacción suelo–estructura.

El objetivo principal del repositorio es entregar trazabilidad computacional del flujo metodológico desarrollado en la memoria, permitiendo revisar la construcción del dataset, el entrenamiento de los modelos neuronales y la evaluación de resultados.

---

## 1. Descripción general

El trabajo desarrolla un framework basado en redes neuronales recurrentes tipo LSTM para aproximar variables de respuesta dinámica obtenidas desde simulaciones numéricas en STKO/OpenSees.

El flujo considera dos niveles principales de modelación neuronal:

- **LSTM_Teacher:** modelo de referencia entrenado con variables completas del sistema.
- **LSTM_Student:** modelo reducido que busca aproximar la respuesta utilizando una menor cantidad de variables de entrada.

El enfoque se orienta a evaluar la factibilidad de utilizar modelos neuronales como aproximadores eficientes de respuestas asociadas a la interacción suelo–estructura.

---

## 2. Estructura del repositorio

```text
Memoria-LSTM-SSI/
│
├── Ordenamiento_IN`s_&_OUT`s.ipynb
├── LSTM_Teacher.ipynb
├── LSTM_Student.ipynb
├── Resultados.ipynb
└── README.md
