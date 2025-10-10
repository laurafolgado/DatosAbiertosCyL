# 6.2. Ejemplo con JavaScript puro (vanilla) – Tabla accesible

En este ejemplo vamos a mostrar el dataset de **estadísticas de uso de redes sociales** en una **tabla HTML dinámica** utilizando **solo JavaScript puro**.

---

## 📌 Flujo de trabajo con vanilla JS

1. **Cargar el CSV** con `fetch`.
2. **Convertirlo a texto** con `.text()`.
3. **Separar cabecera y filas** (con `.split()` por líneas y columnas).
4. **Transformar las filas en objetos** para trabajar de forma más clara.
5. **Generar la tabla HTML** dinámicamente con `<table>`, `<thead>` y `<tbody>`.
6. **Insertarla en la página** con `appendChild`.

---

## 🧩 Código paso a paso

### 1) Cargar el CSV

```js
async function cargarCSV() {
  const res = await fetch("datasets/csv/estadisticas-de-uso-de-redes-sociales.csv");
  const texto = await res.text();
  return texto;
}
```

---

### 2) Separar cabecera y filas

```js
function parsearCSV(texto) {
  let lineas = texto.trim().split("\n");
  let cabeceras = lineas.shift().split(",");

  let datos = lineas.map(linea => {
    let valores = linea.split(",");
    return Object.fromEntries(cabeceras.map((c, i) => [c, valores[i]]));
  });

  return { cabeceras, datos };
}
```

---

### 3) Renderizar la tabla HTML

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

### 4) Ponerlo todo junto

```js
(async () => {
  const texto = await cargarCSV();
  const { cabeceras, datos } = parsearCSV(texto);

  // mostramos solo algunas columnas clave
  renderTabla(datos, ["Año", "Red", "Porcentaje"]);
})();
```

---

!!! tip "Consejo"
Con JavaScript puro es fácil montar una tabla básica.
Sin embargo, si quieres añadir **paginación, búsqueda o filtros**, pronto conviene usar una librería como **Tabulator** o **DataTables**, que lo simplifican mucho.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
1\. ¿Qué devuelve la función `cargarCSV()`?
2\. ¿Cómo obtenemos las cabeceras del CSV en este ejemplo?
3\. ¿Qué hace `Object.fromEntries` al transformar cada fila?
4\. ¿Qué etiquetas HTML usamos para diferenciar cabecera (`<thead>`) y cuerpo (`<tbody>`)?
5\. ¿Qué limitaciones tiene esta tabla si el dataset tiene miles de registros?

---

El siguiente paso será ver cómo mejorar esta tabla usando **librerías externas** como Tabulator, para añadir funcionalidades avanzadas.