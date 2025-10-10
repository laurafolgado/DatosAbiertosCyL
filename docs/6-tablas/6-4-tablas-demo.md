# 6.4. Demo: Estadísticas de redes sociales en tabla

Ya hemos visto cómo crear tablas accesibles con **JavaScript puro** y cómo mejorarlas con una **librería como Tabulator**.
En esta demo mostramos el dataset de **estadísticas de uso de redes sociales** en formato CSV como una **tabla dinámica**.

---

## 📌 Qué encontrarás en las demos

* **Carga real del dataset** `estadisticas-de-uso-de-redes-sociales.csv`.
* Dos enfoques:

  * **Vanilla**: tabla HTML básica generada con `createElement`.
  * **Librería (Tabulator)**: tabla con búsqueda, ordenación y paginación.
* Manejo de errores y rutas relativas para trabajar en servidor local.

---

!!! tip "Consejo"
Con Tabulator puedes añadir filtros avanzados y exportación de datos.
Revisa la documentación si quieres ir más allá de lo mostrado aquí.

---

## 📌 Acceso a las demos

### ▶️ Demo con JavaScript puro (Vanilla)
<a href="../demo/6-tablas-js/index.html" target="_blank">Abrir demo en vivo (Vanilla Javascript)</a>

También puedes verla incrustada aquí:

<iframe src="../demo/6-tablas-js/index.html" width="100%" height="640" loading="lazy"></iframe>

---

### ▶️ Demo con librería (`Tabulatorr`)
<a href="../demo/6-tablas-librerias/index.html" target="_blank">Abrir demo en vivo (Tabulator)</a>

También puedes verla incrustada aquí:

<iframe src="../demo/6-tablas-librerias/index.html" width="100%" height="640" loading="lazy"></iframe>

---

## 📝 Preguntas de reflexión

!!! question "Repaso"
    1. ¿Qué ventajas aporta Tabulator frente a una tabla en HTML básico?
    2. ¿Qué problemas puedes tener al mostrar un CSV grande directamente en una tabla?
    3. ¿Qué significa `layout: "fitColumns"` en Tabulator?
    4. ¿Qué mejoras podrías añadir para hacer la tabla más accesible?
    5. ¿Cómo cambiarías el código para mostrar solo las filas de un año concreto?
