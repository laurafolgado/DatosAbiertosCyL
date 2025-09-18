# 2.4. Demo: Hostelería a domicilio (CSV)

Ya hemos visto cómo procesar un CSV con **JavaScript puro** y cómo simplificar la tarea usando **librerías externas** como Papa Parse y Tabulator.  
Es momento de ver el resultado final funcionando en una página real.

En esta demo utilizamos el dataset de **hostelería a domicilio** del portal de Datos Abiertos de la Junta de Castilla y León.  
El sistema carga el archivo CSV, lo parsea automáticamente y lo muestra en una **tabla interactiva** con búsqueda, ordenación y paginación.

---

## 📌 Qué encontrarás en la demo

* **Carga real del dataset** de hostelería en formato CSV.  
* **Parseo con Papa Parse**, que convierte el archivo en un array de objetos.  
* **Visualización con Tabulator**, que genera una tabla accesible y dinámica.  
* Controles de **búsqueda global** y **cambio de tamaño de página**.  
* Manejo de **errores de red** y mensajes accesibles para el usuario.

---

!!! tip "Consejo"
    Recuerda abrir el proyecto con un **servidor local** (por ejemplo, la extensión *Live Server* en VS Code).  
    Si intentas abrirlo directamente con `file://`, el navegador puede bloquear la carga del CSV por motivos de seguridad.

---

## 📌 Acceso a la demo

[▶️ Abrir demo en vivo](demo/index.html){ target=_blank }

También puedes verla incrustada aquí:

<iframe src="./demo/index.html" width="100%" height="640" loading="lazy"></iframe>

---

## 📝 Preguntas de reflexión

!!! question "Repaso"
    1. ¿Qué dos librerías se han utilizado en la demo y para qué sirve cada una?  
    2. ¿Qué diferencia principal observas entre esta tabla y la que generamos con JavaScript puro?  
    3. ¿Por qué es importante incluir mensajes de error y estado en una aplicación que consume datos externos?  
    4. ¿Qué mejoras añadirías a esta demo para hacerla más útil al usuario final?
```