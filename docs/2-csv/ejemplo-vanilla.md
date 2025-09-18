# 2.2. Ejemplo con JavaScript puro (vanilla)

En este apartado vamos a ver cómo procesar un CSV utilizando **solo JavaScript puro**, sin librerías externas.  
Trabajaremos con el dataset de **hostelería a domicilio** de la Junta de Castilla y León.

---

## 📌 Flujo de trabajo con vanilla JS

1. **Cargar el archivo** con `fetch`.  
2. **Obtener el contenido como texto** usando `.text()`.  
3. **Separar en líneas**: cada línea será un registro.  
4. **Detectar la primera línea como cabecera** (nombres de columnas).  
5. **Mapear las líneas restantes** a objetos JavaScript usando las cabeceras como claves.  
6. **Mostrar los datos** en la consola o en una tabla HTML sencilla.

---

## 📌 Código paso a paso

Primero, cargamos el CSV con `fetch` y lo convertimos en texto:

```js
async function cargarCSV() {
  let respuesta = await fetch("datasets/csv/hosteleria-a-domicilio.csv");
  let texto = await respuesta.text();
  console.log(texto);
}
````

Esto nos muestra todo el contenido del CSV en bruto.

---

A continuación, separamos en líneas y extraemos las cabeceras:

```js
let lineas = texto.trim().split("\n");
let cabeceras = lineas.shift().split(",");
```

* `lineas` es un array con todas las filas de datos.
* `cabeceras` es un array con los nombres de las columnas.

---

Después, convertimos cada línea en un objeto:

```js
let datos = lineas.map(linea => {
  let valores = linea.split(",");
  return Object.fromEntries(cabeceras.map((c, i) => [c, valores[i]]));
});
```

De esta manera obtenemos un array de objetos como este:

```js
[
  { "Nombre": "Bar La Plaza", "Localidad": "Salamanca", "Telefono": "923123456" },
  { "Nombre": "Café Sol", "Localidad": "Valladolid", "Telefono": "983654321" }
]
```

---

Finalmente, podemos mostrar una **tabla en HTML** con los datos:

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
      td.textContent = fila[col];
      tr.appendChild(td);
    });
    tbody.appendChild(tr);
  });
  tabla.appendChild(tbody);

  document.body.appendChild(tabla);
}
```

---

!!! tip "Consejo"
    Este enfoque es ideal para **entender cómo funciona el CSV por dentro** y para datasets sencillos.
    Sin embargo, con archivos muy grandes o con formatos más complejos (celdas con comillas, saltos de línea dentro de un campo…) puede resultar limitado.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué función cumple `fetch` en este ejemplo?
    2. ¿Qué guardamos en la variable `cabeceras`?
    3. ¿Cómo transformamos cada línea de texto en un objeto JavaScript?
    4. ¿Por qué se usa `Object.fromEntries` en el código?
    5. ¿Qué ventajas tiene mostrar los datos en una tabla HTML en lugar de solo en consola?


