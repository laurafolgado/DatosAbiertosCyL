# 4.4. Demo: Instituciones bibliotecarias (JSON)

Ya hemos visto cómo procesar un JSON con **JavaScript puro** y cómo simplificar la tarea usando una **librería externa** como `Axios`.  
Es momento de ver el resultado final funcionando en páginas reales.

En estas demos utilizamos el dataset de **instituciones bibliotecarias de Castilla y León** (`registro-instituciones-bibliotecarias-de-castilla-y-leon.json`).  
El sistema carga el archivo JSON, obtiene la lista de instituciones y las muestra en una **tabla accesible** con columnas clave como nombre, municipio y provincia.

---

## 📌 Qué encontrarás en las demos

* **Carga real del dataset** `registro-instituciones-bibliotecarias-de-castilla-y-leon.json`.  
* Dos enfoques:
  - **Vanilla (`fetch` + `.json()`)**: carga y procesa el JSON sin dependencias externas.  
  - **Librería (`Axios`)**: añade simplificación en el manejo de errores, cabeceras y timeouts.  
* Renderizado de una **tabla HTML accesible** con los datos principales.  
* Gestión de **errores de red** y mensajes en consola o pantalla para depuración.

---

!!! tip "Consejo"
    Recuerda abrir el proyecto con un **servidor local** (por ejemplo, la extensión *Live Server* en VS Code, o `mkdocs serve`).  
    Si intentas abrirlo directamente con `file://`, el navegador puede bloquear la carga del JSON por motivos de seguridad.

---

## 📌 Acceso a las demos

### ▶️ Demo con JavaScript puro (vanilla)
<a href="../demo/4-json-js/index.html" target="_blank">Abrir demo en vivo (Vanilla Javascript)</a>

También puedes verla incrustada aquí:

<iframe src="../demo/4-json-js/index.html" width="100%" height="640" loading="lazy"></iframe>

---

### ▶️ Demo con librería (`Axios`)
<a href="../demo/4-json-librerias/index.html" target="_blank">Abrir demo en vivo (Axios)</a>

También puedes verla incrustada aquí:

<iframe src="../demo/4-json-librerias/index.html" width="100%" height="640" loading="lazy"></iframe>

---

## 📝 Preguntas de reflexión

!!! question "Repaso"
    1. ¿Qué diferencia clave hay entre el enfoque **vanilla** (fetch) y el de **librería** (Axios)?  
    2. ¿Qué ventajas aporta Axios en cuanto a manejo de errores y timeouts?  
    3. ¿Qué pasos realiza el código antes de renderizar la tabla en pantalla?  
    4. ¿Cómo manejarías un JSON cuya estructura cambiara (por ejemplo, si la lista no estuviera bajo `instituciones`)?  
    5. ¿Qué mejoras añadirías a la demo para facilitar la exploración de los datos (ej. filtros, paginación, búsqueda)?