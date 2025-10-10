# 3.4. Demo: Agenda cultural (XML)

Ya hemos visto cómo procesar un XML con **JavaScript puro** y cómo simplificar la tarea usando una **librería externa** como `fast-xml-parser`.  
Es momento de ver el resultado final funcionando en páginas reales.

En estas demos utilizamos el dataset de **agenda cultural** del portal de Datos Abiertos de la Junta de Castilla y León (`eventos.xml`).  
El sistema carga el archivo XML, lo convierte a una estructura manejable y muestra los eventos en **tarjetas/tabla accesible** con ordenación y búsqueda.

---

## 📌 Qué encontrarás en las demos

* **Carga real del dataset** `eventos.xml`.  
* Dos enfoques:
  - **Vanilla (DOMParser)**: convierte el XML en DOM y recorre nodos con `querySelectorAll`.
  - **Librería (`fast-xml-parser`)**: transforma XML → JSON para operar con objetos JS.
* Controles de **búsqueda** y **ordenación por fecha** (según la demo).  
* Manejo de **errores de red** y mensajes accesibles para el usuario.

---

!!! tip "Consejo"
    Recuerda abrir el proyecto con un **servidor local** (por ejemplo, la extensión *Live Server* en VS Code, o `mkdocs serve`).  
    Si intentas abrirlo directamente con `file://`, el navegador puede bloquear la carga del XML por motivos de seguridad.

---

## 📌 Acceso a las demos

### ▶️ Demo con JavaScript puro (vanilla)
<a href="../demo/3-xml-js/index.html" target="_blank">Abrir demo en vivo (Vanilla Javascript)</a>

También puedes verla incrustada aquí:

<iframe src="../demo/3-xml-js/index.html" width="100%" height="640" loading="lazy"></iframe>

---

### ▶️ Demo con librería (`fast-xml-parser`)

<a href="../demo/3-xml-librerias/index.html" target="_blank">Abrir demo en vivo (Librería)</a>

También puedes verla incrustada aquí:

<iframe src="../demo/3-xml-librerias/index.html" width="100%" height="640" loading="lazy"></iframe>


---

## 📝 Preguntas de reflexión

!!! question "Repaso"
    1. ¿Qué diferencia clave hay entre el enfoque **vanilla** (DOMParser) y la demo con **librería** (XML → JSON)?  
    2. ¿Qué pasos realiza el código antes de renderizar los eventos en pantalla?  
    3. ¿Qué harías si el XML incluyera **namespaces** (p. ej., `dc:title`)?  
    4. ¿Cómo manejarías eventos sin fecha de inicio o con una fecha mal formateada?  
    5. ¿Qué ventajas e inconvenientes observas al convertir XML a JSON para su consumo en la web?
