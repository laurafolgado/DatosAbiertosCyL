# 7.4. Demo: Estadísticas de redes sociales en gráficos

Ya hemos visto cómo generar gráficos con **Canvas (vanilla)** y cómo simplificarlos usando **Chart.js**.
En esta demo mostramos el dataset de **estadísticas de uso de redes sociales** como un **gráfico de barras**.

---

## 📌 Qué encontrarás en las demos

* **Carga real del dataset** `estadisticas-de-uso-de-redes-sociales.csv`.
* Dos enfoques:

  * **Vanilla (Canvas API)**: gráfico dibujado manualmente.
  * **Librería (Chart.js)**: gráfico de barras interactivo y responsive.
* Ejemplo filtrado para el año **2020** (puedes adaptarlo a otros años).

---

!!! tip "Consejo"
    Chart.js permite cambiar fácilmente el tipo de gráfico (`line`, `pie`, `doughnut`, etc.).
    Experimenta cambiando la configuración para visualizar los mismos datos de formas diferentes.

---

## 📌 Acceso a las demos

### ▶️ Demo con librería (Chart.js)

[**Abrir demo en vivo**](){ target=\_blank }

También puedes verla incrustada aquí:

<iframe src="./demo/chartjs.html" width="100%" height="400" loading="lazy"></iframe>

---

### ▶️ Demo con JavaScript puro (vanilla)

[**Abrir demo en vivo (vanilla)**](){ target=\_blank }

También puedes verla incrustada aquí:

<iframe src="" width="100%" height="400" loading="lazy"></iframe>

---

## 📝 Preguntas de reflexión

!!! question "Repaso"
    1. ¿Qué ventaja tiene Chart.js frente a dibujar con Canvas manualmente?
    2. ¿Qué arrays necesitamos preparar para pasar datos a Chart.js?
    3. ¿Qué propiedad controla el tipo de gráfico en Chart.js?
    4. ¿Cómo cambiarías el código para representar datos de 2021 en lugar de 2020?
    5. ¿Qué tipo de gráfico (además de barras) sería útil para comparar el uso de redes sociales?
