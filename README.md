# 🏭 Utility Twin 2026
### Optimización de Suministro Industrial mediante Inteligencia Artificial

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![XGBoost](https://img.shields.io/badge/XGBoost-1.7+-orange)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.3+-red?logo=scikit-learn)
![Status](https://img.shields.io/badge/Status-Completado-green)

---

## 📌 Resumen

En manufactura, el vapor es el corazón de la producción. Su gestión suele ser **reactiva**: las calderas se ajustan *después* de que la presión ya cayó o el combustible ya se disparó. Cada reacción tardía es una oportunidad de pérdida.

Este proyecto construye un **Gemelo Digital (Utility Twin)**: un sistema de inteligencia artificial que anticipa la demanda de vapor, protege la operación ante paros no planificados y detecta fallas mecánicas antes de que ocurran.

> 📁 **Dataset:** Simulación sintética de 8,758 registros horarios diseñada para representar condiciones operativas reales de una planta de manufactura industrial.

---

## 🎯 El Problema

| Problema | Impacto |
|:---|:---|
| Gestión reactiva de calderas | Paros forzados por baja presión |
| Exceso de presión "por si acaso" | Sobrecosto en combustible y emisiones |
| Variables invisibles (clima, inercia térmica) | Imposible calcular manualmente en tiempo real |

---

## 🛠️ Las Tres Herramientas que Entrega el Sistema

🛡️ **Techo de Seguridad P95** — Nivel de operación que garantiza disponibilidad el 95% del tiempo, derivado matemáticamente de los costos reales de la empresa.

🔍 **Monitor de Performance** — Alerta cuando el modelo detecta que la planta se comporta diferente a su historia: posible fuga, falla de trampa térmica o sensor descalibrado.

📊 **Simulador de Escenarios** — Responde preguntas como *"¿cuánto nos cuesta una ola de calor?"* con números concretos, no intuición.

---

## 📊 Resultados

### Comparación de Modelos

| Modelo | R² | MAE | Infraestimación (Media) |
|:---|---:|---:|---:|
| Baseline Naive (Lag-1) | 0.488 | 0.0424 | 0.0442 |
| Regresión Lineal | 0.842 | 0.0294 | 0.0271 |
| XGBoost Estándar | 0.893 | 0.0158 | 0.0173 |
| Random Forest | 0.923 | 0.0154 | 0.0163 |
| **XGBoost Asimétrico (Final)** | **0.892** | **0.0211** | **0.0188** |

Random Forest alcanzó R² = 0.923 (superior a XGBoost), pero carece de flexibilidad para funciones de pérdida custom — crítico para operación industrial asimétrica.   El modelo final fue elegido sobre Random Forest no por precisión, sino por su función de pérdida asimétrica: **penaliza 50 veces más quedarse sin vapor que tener exceso**.


---

## 💰 Evaluación Económica — Decisión de Negocio
Aunque Random Forest obtuvo mejor R² y MAE, la selección final se basó en **Costo Operacional Esperado**, no solo en precisión estadística.

En esta aplicación, subestimar la demanda (falta de vapor) es significativamente más costoso que sobreestimar.

### Costo Monetario Esperado (USD/año)

| Modelo | Costo Anual |
|:---|---:|
| Random Forest | $ 11,002.00 |
| XGBoost Estándar | $ 12,084.09 |
| **XGBoost Asimétrico (Final)** | **$ 5,508.82** |

El modelo asimétrico reduce el riesgo económico a la **mitad** frente a enfoques tradicionales.

> La decisión se tomó alineando el modelo con el riesgo financiero real de la operación.



### Impacto Económico

| Escenario | Impacto Anual |
|:---|---:|
| +1°C en temperatura promedio | +$522 USD/año |
| -10% volumen de producción | -$7,348 USD/año |
| 1 parada no planificada de 4 horas | -$8,000 USD/evento |

> **El argumento de negocio:** Una sola parada no planificada destruye 
> el ahorro de un año de optimización. El ROI se mide en continuidad operativa.

---

## 🔑 Decisiones Técnicas Clave

**Eliminación del Hindsight Bias**
Un modelo inicial alcanzó R² = 0.99 usando variables como `demanda_p50` que no existirían en el momento de predicción real. Al eliminarlas, el R² bajó a 0.892 — ese es el límite físico real de predictibilidad de la planta.

**Autocorrelación (Lag-1)**
La inclusión de `consumo_lag_1` permite al modelo capturar la inercia térmica del sistema: el estado de la caldera ahora depende de su estado en la hora anterior.

**Regresión Quantílica (P10/P50/P95)**
En lugar de una predicción puntual, el sistema entrega bandas de confianza. El P95 no es un número arbitrario — se deriva de la fórmula:

$$\tau = \frac{C_{falta}}{C_{falta} + C_{exceso}}$$

**División Temporal (80/20)**
El set de prueba usa datos cronológicamente posteriores al entrenamiento, simulando condiciones reales de despliegue.

---

## 💻 Stack Tecnológico

```
Python 3.10+
├── pandas / numpy          → Manipulación de datos
├── scikit-learn            → Preprocesamiento y modelos base
├── xgboost                 → Modelo final + regresión quantílica
├── matplotlib / seaborn    → Visualización
```

---

## 🚀 Cómo Ejecutar

1. Abrí el notebook en Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

2. El dataset se descarga automáticamente desde Google Drive al ejecutar la segunda celda.

3. Ejecutá todas las celdas en orden. El notebook está diseñado para correr de principio a fin sin intervención manual.

---

## 📁 Estructura del Notebook

```
1. 📦 Importación de librerías y carga de datos
2. 🔍 Exploración y análisis (EDA)
3. ⚙️ Ingeniería de características y eliminación de Hindsight Bias
4. 🤖 Entrenamiento y comparación de modelos
5. 📊 Modelo quantílico: gestión probabilística del riesgo
6. 🚨 Sistema de Monitoreo de Performance
7. 📈 Dinámica Térmica: Economía de Escala Energética
8. 💰 Análisis de impacto económico
9. 🏁 Conclusiones y recomendaciones
```

---

## 🔭 Próximos Pasos

1. **Ingesta de datos reales** — Integración con SCADA
2. **API de inferencia en tiempo real** — Endpoint para sistema de control
3. **Pipeline de retraining automatizado** — Degradación detectada → modelo actualizado
4. **Expansión multi-utility** — Agua, aire comprimido, electricidad

---

## 🧠 Limitaciones y Consideraciones

| Limitación | Implicación | Mitigación Actual |
|:---|:---|:---|
| Dataset sintético | Valida arquitectura, no calibración real de planta | Estructura diseñada para ingestión de datos reales |
| Suposición de lag estable | El consumo previo puede no ser representativo en paradas muy prolongadas | Mecanismo de fallback implementado |
| Quantil P95 estático | No se adapta automáticamente a cambios de costos de combustible | Recalibración manual semestral sugerida |
| Sin inferencia en tiempo real | Latencia no testeada en producción | Arquitectura preparada para endpoint API |


---

> **"El Utility Twin 2026 transforma la operación de reactiva a predictiva. 
> En una industria donde una parada cuesta más que un año de optimización, 
> anticipar no es una ventaja es una necesidad."**
