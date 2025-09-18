# 4.2. Ejemplo con JavaScript puro (vanilla)

En este apartado vamos a ver cómo procesar un JSON utilizando **solo JavaScript puro**, sin librerías externas.  
Trabajaremos con el dataset de **instituciones bibliotecarias de Castilla y León**, publicado en formato JSON.

---

## 📌 Flujo de trabajo con vanilla JS

1. **Cargar el archivo** con `fetch`.  
2. **Obtener el contenido como objeto** usando `.json()`.  
3. **Explorar la estructura** para identificar las claves principales.  
4. **Mapear los datos** a objetos JavaScript manejables.  
5. **Mostrar los datos** en la consola o en una tabla HTML sencilla.

---

## 📌 Código paso a paso

Primero, cargamos el JSON con `fetch` y lo convertimos directamente en objeto:

```js
async function cargarJSON() {
  let respuesta = await fetch("datasets/json/registro-instituciones-bibliotecarias-de-castilla-y-leon.json");
  let datos = await respuesta.json();
  console.log(datos);
}
```

Esto nos muestra todo el contenido del JSON como objeto JavaScript.

---

Supongamos que la estructura tiene una lista de instituciones bajo la clave `instituciones`.
Podemos recorrerla así:

```js
let instituciones = datos.instituciones;

instituciones.forEach(inst => {
  console.log(inst.nombre, inst.provincia);
});
```

Esto imprimiría en la consola el nombre y la provincia de cada institución.

---

A continuación, generamos una **tabla HTML** con los datos clave:

```js
function renderTabla(datos, columnas) {
  let tabla = document.createElement("table");

  // cabecera
  let thead = document.createElement("thead");
  let tr = document.createElement("tr");
  columnas.forEach(col => {
    let th = document.createElement("th");
    th.scope = "col";
    th.textContent = col;
    tr.appendChild(th);
  });
  thead.appendChild(tr);
  tabla.appendChild(thead);

  // filas
  let tbody = document.createElement("tbody");
  datos.forEach(fila => {
    let tr = document.createElement("tr");
    columnas.forEach(col => {
      let td = document.createElement("td");
      td.textContent = fila[col] ?? "";
      tr.appendChild(td);
    });
    tbody.appendChild(tr);
  });
  tabla.appendChild(tbody);

  document.body.appendChild(tabla);
}
```

Y lo usamos así:

```js
renderTabla(instituciones, ["nombre", "provincia", "municipio"]);
```

---

!!! tip "Consejo"
Este enfoque es ideal para **empezar a trabajar con JSON** en la web.
Ten en cuenta que la estructura del JSON puede variar de un dataset a otro:
\* A veces el objeto raíz contiene la lista bajo una clave (`instituciones`, `monumentos`).
\* Otras veces, directamente es un **array**.
Revisa siempre con `console.log` antes de procesar.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
  1. ¿Qué método usamos para convertir la respuesta en objeto JavaScript?
  2. ¿Qué diferencia hay entre usar `.text()` y `.json()` en `fetch`?
  3. ¿Cómo recorremos el array de instituciones en el ejemplo?
  4. ¿Por qué añadimos `?? ""` al mostrar los valores en la tabla?
  5. ¿Qué pasos previos harías para comprobar la estructura de un JSON nuevo antes de procesarlo?