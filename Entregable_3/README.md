# Entregable 3 — Módulo 3: Prototipo e Implementación
**Proyecto Aplicado en Analítica de Datos — Universidad de los Andes**  
Grupo 23: Danilo Suárez Vargas · Valeria Iglesias Miranda · Sergio Andrés Perdomo Murcia

---

## Descripción

En este módulo se construyó el prototipo del tablero predictivo para la zona internacional del aeropuerto. La aplicación consume la API del modelo Random Forest entrenado en el Módulo 2 y le permite al coordinador operativo ingresar los datos de la franja 
actual, obtener el pronóstico de los próximos 15 minutos y decidir si necesita habilitar filtros adicionales. El tablero también 
incluye una vista histórica para revisar el comportamiento del turno y explorar el flujo por filtro individual.

> 🔗 **URL del tablero:** `https://proyecto-final-dsa.streamlit.app/`

---

## Estructura de la carpeta

```
Entregable_3/
├── app.py                                  ← código del tablero Streamlit
├── requirements.txt                        ← dependencias necesarias para Streamlit
├── manual_de_usuario.md                    ← manual de usuario
├── tabla_requerimientos_diligenciada.md    ← rúbrica de evaluación diligenciada
├── API_documentacion.md                    ← documentación de la API
├── Reporte_de_seleccion_y_parametrizacion_de_modelos.md  ← reporte técnico de experimentos
├── diagrama_arquitectura.png               ← diagrama esquemático
├── Banner_PAAD_01.jpg                      ← imagen de encabezado
└── data/
    └── sensores_filtro_15m.csv             ← flujo histórico por filtro (13 filtros · 15 min)
```

---

## Componentes del prototipo

| Componente | Descripción |
|---|---|
| **API** | Servicio FastAPI desplegado en `http://137.184.102.248` que ejecuta el modelo `rf_iter_2` (Random Forest · 600 árboles · WMAPE 17.9%). Documentación completa en `API_documentacion.md`. |
| **Tablero Streamlit** | Interfaz web con Vista Ahora (predicción en tiempo real) y Vista Histórico (análisis por sesión y por filtro). Desplegado en Streamlit. |

---

## Cómo ejecutar el tablero localmente

```bash
# 1. Clonar el repositorio
git clone https://github.com/danilosuarez/Proyecto_analitica_final.git
cd Proyecto_analitica_final/Entregable_3

# 2. Crear y activar entorno virtual
python -m venv venv

# Windows
venv\Scripts\activate
# Mac / Linux
source venv/bin/activate

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Ejecutar la aplicación
streamlit run app.py
```

La aplicación se abre automáticamente en `http://localhost:8501`.

---

## Funcionalidades implementadas

| Funcionalidad | Descripción |
|---|---|
| Predicción en tiempo real | Ingreso manual de 25 variables y consulta al modelo vía API |
| Semáforo operativo | Normal / Alerta / Crítico según umbral paramétrico |
| Filtros activos ajustables | Slider que recalcula umbrales según dotación real del turno |
| Indicador de carga | Velocímetro visual con zonas de color por nivel de riesgo |
| Distribución VeriPax | Gráfico de barras por ventana de anticipación |
| Filtro de mayor carga | Referencia histórica del filtro con mayor flujo reciente |
| Evolución de sesión | Gráfica de predicciones acumuladas vs promedio móvil |
| Histórico por filtro | Selector de fecha y visualización de los 13 filtros individuales |
| Exportar CSV | Descarga del historial completo del turno |

---

## Umbrales operativos

Los umbrales se calculan dinámicamente según el número de filtros activos configurado en el tablero:

| Filtros activos | Capacidad máx. (pax/15 min) | Alerta (70%) | Crítico (85%) |
|---|---|---|---|
| 8 | 313 | 219 | 266 |
| 9 | 352 | 247 | 299 |
| 10 | 391 | 274 | 333 |
| 13 (referencia) | 509 | 356 | 432 |

> Capacidad por filtro: 60 seg/min × 15 min / 23 seg/pax = 39.1 pax/filtro/franja.

---

## Artefactos por criterio de rúbrica

| Criterio | Artefacto |
|---|---|
| Prototipo ejecutable | `app.py` · URL Streamlit Cloud |
| Manual de usuario | `manual_de_usuario.md` |
| Diagrama esquemático| `diagrama_arquitectura.drawio.png` |
| Reporte técnico de experimentos | `Entregable_2_v2/Reporte_de_seleccion_y_parametrizacion_de_modelos.md` |
| Rúbrica de evaluación diligenciada | `tabla_requerimientos_diligenciada.md` |
| Código del prototipo | `app.py` · `https://github.com/danilosuarez/Proyecto_analitica_final` |

---

## Relación con el Módulo 2

El tablero consume directamente el modelo entrenado en el Módulo 2 a través de la API.

| Recurso | Ubicación |
|---|---|
| Modelo entrenado | `Entregable_2_v2/resultados_modelado/artifacts/best_model.joblib` |
| Notebook de modelado | `Entregable_2_v2/notebooks/Notebook_Modelado_Modulo2_PAAD2026.ipynb` |
| Dataset maestro | `Entregable_2_v2/bases_limpias/dataset_zona_15m.csv` |
| Reporte técnico | `Entregable_2_v2/Reporte_de_seleccion_y_parametrizacion_de_modelos.md` |

---
