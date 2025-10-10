# 5.4. Demo: Registro de actividades (API REST)

Ya hemos visto cómo consumir una **API REST** con **JavaScript puro** (`fetch`) y cómo simplificar la tarea usando **Axios**.
Es momento de ver el resultado final funcionando en una página real.

En esta demo utilizamos el **Registro de actividades de Castilla y León**, disponible en el portal de **Datos Abiertos** a través de su API REST.
El sistema hace la consulta a la API y muestra los resultados en una **tabla accesible** con búsqueda y paginación.

---

## 📌 Qué encontrarás en las demos

* **Consulta real a la API REST** del Registro de actividades.
* Dos enfoques:
    * **Vanilla (fetch)**: petición directa, parseo con `.json()` y renderizado manual.
    * **Librería (Axios)**: simplifica la carga, manejo de errores y cancelación.
* **Tabla accesible** con columnas clave (ejemplo: nombre, municipio, actividad…).
* Controles de **búsqueda** y **paginación** para explorar los datos.
* Manejo de **errores de red** con mensajes claros para el usuario.

---

!!! tip "Consejo"
    Recuerda abrir el proyecto con un **servidor local** (`mkdocs serve`, *Live Server* en VS Code, etc.).
    Si intentas abrirlo directamente con `file://`, el navegador puede bloquear las peticiones por motivos de seguridad.

---

## 📌 Acceso a las demos

### ▶️ Demo con JavaScript puro (vanilla)
<a href="../demo/5-api-js/index.html" target="_blank">Abrir demo en vivo (Vanilla Javascript)</a>

También puedes verla incrustada aquí:

<iframe src="../demo/5-api-js/index.html" width="100%" height="640" loading="lazy"></iframe>

---

### ▶️ Demo con librería (`Axios`)
<a href="../demo/5-api-librerias/index.html" target="_blank">Abrir demo en vivo (Axios)</a>

También puedes verla incrustada aquí:

<iframe src="../demo/5-api-librerias/index.html" width="100%" height="640" loading="lazy"></iframe>

---

## 📝 Preguntas de reflexión

!!! question "Repaso"
    1. ¿Qué diferencia observas entre la demo con `fetch` y la demo con Axios?
    2. ¿Qué ventajas ofrece Axios a la hora de manejar errores o cancelar peticiones?
    3. ¿Por qué es importante incluir búsqueda y paginación cuando los resultados de la API son muy numerosos?
    4. ¿Qué harías si la API devuelve campos o nombres de claves diferentes a los que esperas?
    5. ¿Qué problemas pueden surgir si consultas la API directamente desde el navegador sin servidor local?