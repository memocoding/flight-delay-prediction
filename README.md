# ✈️ Predicción de Atraso de Vuelos — SCL 2017

**Autor:** Guillermo Linco
**Herramientas:** Python · Scikit-learn · XGBoost · Pandas · Seaborn

---

## 📌 Descripción del proyecto

Este proyecto aplica técnicas de Machine Learning para predecir si un vuelo saldrá con **más de 15 minutos de atraso** desde el Aeropuerto Internacional Arturo Merino Benítez (Santiago, Chile), usando datos operacionales del año 2017.

La variable objetivo es binaria:
- `A tiempo` → diferencia entre hora operada y programada ≤ 15 minutos
- `Atrasado` → diferencia > 15 minutos

---

## 📁 Estructura del repositorio

```
.
├── prediccion-atrasos-vuelos_ml.ipynb   # Notebook principal con todo el pipeline
├── dataset_SCL_EXCEL.xlsx               # Dataset de vuelos (requerido, no incluido en el repo)
├── datos3.csv                           # Dataset procesado y listo para modelado (generado)
├── data_balanceada.csv                  # Dataset de entrenamiento balanceado (generado)
└── README.md                            # Este archivo
```

> ⚠️ El archivo `dataset_SCL_EXCEL.xlsx` **no está incluido** en el repositorio por su tamaño. Debe colocarse en la misma carpeta que el notebook antes de ejecutarlo.

---

## 🔄 Pipeline del proyecto

```
Carga del dataset
      ↓
Limpieza y renombrado de columnas
      ↓
Análisis de valores nulos
      ↓
Feature Engineering
  (ATRASO_15, TEMPORADA_ALTA, PERIODO_DIA, HORA)
      ↓
EDA (Análisis Exploratorio)
      ↓
División train/test (80/20, estratificada)
      ↓
Entrenamiento de modelos
  ├─ Árbol de Decisión (tuneado con GridSearchCV)
  ├─ Bagging
  ├─ Random Forest (tuneado con GridSearchCV)
  └─ XGBoost
      ↓
Balanceo de clases (RandomOverSampler)
      ↓
Re-entrenamiento con datos balanceados
      ↓
Comparación final de modelos
      ↓
Exportación de datasets transformados
```

---

## 📊 Features utilizadas

| Feature | Tipo | Descripción |
|---|---|---|
| `DIA_OPER` | Categórica | Día del mes de operación |
| `MES_OPER` | Categórica | Mes de operación |
| `DIA_NOM` | Categórica | Nombre del día (Lunes, Martes, …) |
| `HORA` | Categórica | Hora de operación |
| `TIPO_VUELO` | Categórica | Nacional o Internacional |
| `OPERA` | Categórica | Aerolínea operadora |
| `CIUDAD_DESTINO` | Categórica | Ciudad de destino |
| `TEMPORADA_ALTA` | Categórica | Si el vuelo opera en temporada alta |
| `PERIODO_DIA` | Categórica | Mañana / Tarde / Noche |
| `CANTIDAD_PASAJEROS_DESTINO` | Numérica | Cantidad de pasajeros al destino |
| `TIEMPO_VUELO_DESTINO` | Numérica | Duración del vuelo (horas) |

---

## 🤖 Modelos entrenados

| Modelo | Estrategia |
|---|---|
| Árbol de Decisión | Tuneado con GridSearchCV (5-fold CV) |
| Bagging | 100 estimadores base (árbol de decisión) |
| Random Forest | Tuneado con GridSearchCV (5-fold CV) |
| XGBoost | Gradient Boosting con parámetros fijos |
| Random Forest + Oversampling | Mismo modelo, datos balanceados |
| Árbol + Oversampling | Mismo modelo, datos balanceados |
| XGBoost + Oversampling | Mismo modelo, datos balanceados |

Las métricas reportadas son **Accuracy** y **ROC AUC**, complementadas con el *classification report* y la matriz de confusión para cada modelo.

---

## 🛠️ Instalación y uso

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/prediccion-atrasos-vuelos.git
cd prediccion-atrasos-vuelos
```

### 2. Instalar dependencias

```bash
pip install numpy pandas matplotlib seaborn scikit-learn imbalanced-learn xgboost openpyxl
```

### 3. Colocar el dataset

Copia `dataset_SCL_EXCEL.xlsx` en la raíz del repositorio.

### 4. Ejecutar el notebook

```bash
jupyter notebook prediccion-atrasos-vuelos_ml.ipynb
```

O usando JupyterLab:

```bash
jupyter lab prediccion-atrasos-vuelos_ml.ipynb
```

---

## 📦 Dependencias principales

| Librería | Versión recomendada |
|---|---|
| Python | ≥ 3.9 |
| pandas | ≥ 1.5 |
| scikit-learn | ≥ 1.2 |
| xgboost | ≥ 1.7 |
| imbalanced-learn | ≥ 0.10 |
| seaborn | ≥ 0.12 |

---

## 💡 Conclusiones clave

- El dataset tiene un **fuerte desbalance** de clases (~81% a tiempo, ~18% atrasado), lo que requiere estrategias específicas de evaluación y balanceo.
- El **período del día** y la **aerolínea** son variables con poder predictivo sobre el atraso.
- Los modelos de ensamble (**Random Forest**, **XGBoost**) superan al árbol individual en ROC AUC.
- El **oversampling** mejora el recall de la clase `Atrasado` pero puede reducir la accuracy general.
- Se recomienda priorizar **ROC AUC** sobre Accuracy en este problema por el desbalance existente.

---

## 📄 Licencia

Este proyecto fue desarrollado con fines académicos en el contexto del programa de Data Science UC.
