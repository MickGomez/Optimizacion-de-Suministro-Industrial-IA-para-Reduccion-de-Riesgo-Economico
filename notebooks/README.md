Este directorio contiene el núcleo analítico del proyecto. El desarrollo sigue un enfoque de **Analítica Prescriptiva** orientado a la mitigación de riesgos financieros.

 Implementación de extremo a extremo que incluye:

    * **Ingeniería de Características**: Tratamiento de autocorrelación y lags.
    * **Eliminación de Hindsight Bias**: Remoción de variables predictoras no disponibles en tiempo real
    * **Loss Function Custom**: Optimización de hiperparámetros de XGBoost utilizando una función de pérdida asimétrica (Costo Falta vs. Costo Exceso).

##  Requisitos de Ejecución
El notebook está configurado para ejecutarse de forma determinista. Se recomienda el uso de **Google Colab** para asegurar la disponibilidad de las dependencias (`XGBoost 1.7+`, `Scikit-Learn 1.3+`).
