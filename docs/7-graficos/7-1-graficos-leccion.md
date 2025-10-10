# 7.1. Ejemplo con librería – Tablas interactivas (Tabulator)

En este apartado vamos a mostrar el dataset de **estadísticas de uso de redes sociales** en una **tabla interactiva** usando la librería **Tabulator**.
Con esta librería añadimos de forma sencilla funciones como **ordenación, filtros, búsqueda y paginación** que en vanilla JS tendríamos que programar a mano.

---

## 📦 Cómo incluir la librería

Con Tabulator podemos trabajar directamente desde un **CDN**.
Incluimos el CSS y el JS en el `<head>` y después nuestro propio script:

```html
<link href="https://cdn.jsdelivr.net/npm/tabulator-tables@5.6.2/dist/css/tabulator.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/tabulator-tables@5.6.2/dist/js/tabulator.min.js"></script>
<script type="module" src="main.js"></script>
```

---

## 📌 Flujo de trabajo con librería

1. **Incluir Tabulator** desde CDN.
2. **Crear un contenedor** `<div>` para la tabla.
3. **Definir las columnas** que queremos mostrar (nombre, año, porcentaje…).
4. **Cargar el CSV** con `fetch` y convertirlo en array de objetos.
5. **Pasar los datos a Tabulator** para que renderice la tabla.
6. Añadir **funciones extra**: paginación, búsqueda, ordenación automática.

---

## 🧩 Código paso a paso

### 1) HTML base

```html
<div id="tabla-redes"></div>
```

---

### 2) Cargar y parsear el CSV

```js
async function cargarCSV() {
  const res = await fetch("datasets/csv/estadisticas-de-uso-de-redes-sociales.csv");
  const texto = await res.text();

  let lineas = texto.trim().split("\n");
  let cabeceras = lineas.shift().split(",");

  return lineas.map(linea => {
    let valores = linea.split(",");
    return Object.fromEntries(cabeceras.map((c, i) => [c, valores[i]]));
  });
}
```

---

### 3) Configurar Tabulator

```js
async function initTabla() {
  const datos = await cargarCSV();

  new Tabulator("#tabla-redes", {
    data: datos,
    layout: "fitColumns", // ajustar ancho al contenedor
    pagination: "local",
    paginationSize: 10,
    columns: [
      { title: "Año", field: "Año", sorter: "number" },
      { title: "Red social", field: "Red", sorter: "string" },
      { title: "Porcentaje de uso", field: "Porcentaje", sorter: "number" }
    ],
  });
}

initTabla();
```

---

!!! tip "Consejo"
Con Tabulator puedes añadir fácilmente:
\* **Filtros avanzados** (`headerFilter`).
\* **Exportación** a CSV, XLSX o PDF.
\* **Temas visuales** para adaptar el estilo a tu web.

---

## ⚖️ Comparación con el enfoque *vanilla*

| Aspecto              | Vanilla JS                  | Con Tabulator                            |
| -------------------- | --------------------------- | ---------------------------------------- |
| Renderizado          | Manual con `createElement`  | Automático con configuración de columnas |
| Ordenación           | Programar a mano            | Opción `sorter` incluida                 |
| Paginación           | Programar a mano            | Opción `pagination` incluida             |
| Búsqueda/Filtros     | Programar a mano            | `headerFilter` en cada columna           |
| Exportar datos       | A mano (Blob, CSV)          | Métodos `download()` integrados          |
| Curva de aprendizaje | Muy baja (pero más trabajo) | Baja: declarativo y rápido               |

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué ventajas aporta usar Tabulator frente a una tabla creada a mano con JavaScript?
    2. ¿Qué parámetro permite definir el número de filas por página?
    3. ¿Cómo harías para añadir un filtro de búsqueda en la columna “Red social”?
    4. ¿Qué opción de Tabulator usarías si quisieras exportar la tabla a CSV?
    5. ¿Por qué es útil `layout: "fitColumns"` en una tabla responsive?