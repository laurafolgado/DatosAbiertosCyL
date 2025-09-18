# 2.3. Ejemplo con librería (Papa Parse + Tabulator)

En este apartado vamos a ver cómo procesar un CSV utilizando **librerías externas** para simplificar el parseo y la visualización.  
Trabajaremos con el dataset de **hostelería a domicilio** de la Junta de Castilla y León en formato CSV.

---

## 📦 Cómo incluir las librerías

En este curso usaremos **CDN** (no hace falta `npm`). Inserta los scripts/estilos en tu HTML antes de tu código:

```html
<!-- Papa Parse: parseo CSV robusto -->
<script src="https://unpkg.com/papaparse@5.4.1/papaparse.min.js"></script>

<!-- Tabulator: tabla interactiva -->
<link
  rel="stylesheet"
  href="https://unpkg.com/tabulator-tables@6.2.5/dist/css/tabulator.min.css">
<script src="https://unpkg.com/tabulator-tables@6.2.5/dist/js/tabulator.min.js"></script>

<!-- Tu script -->
<script type="module" src="main.js"></script>
```

!!! tip "Consejo"
  Si luego empaquetas con Vite/Webpack, podrás `import Papa from "papaparse"` y `import { Tabulator } from "tabulator-tables"`.

---

## 📌 Flujo de trabajo con librería

1. **Incluir** Papa Parse y Tabulator desde CDN.
2. **Descargar** el CSV con `fetch(url).then(res => res.text())`.
3. **Parsear** el texto con `Papa.parse` (cabeceras, líneas vacías, etc.).
4. **Crear** una **tabla interactiva** con Tabulator (columnas, paginación, ordenación).
5. **Gestionar errores** y mostrar un estado accesible.

---

## 🧩 Código paso a paso

### 1) Cargar y parsear el CSV con Papa Parse

```js
// Ruta del CSV (ajusta a tu estructura)
const URL_CSV = "datasets/csv/hosteleria-a-domicilio.csv";

async function cargarCSV() {
  const res = await fetch(URL_CSV, { cache: "no-cache" });
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  const texto = await res.text();

  // Parseo robusto
  const parsed = Papa.parse(texto, {
    header: true,         // primera línea = cabeceras
    skipEmptyLines: true, // ignora filas vacías
    dynamicTyping: false, // valores como texto (puedes ajustar a true si lo necesitas)
    delimiter: ""         // autodetecta (,, ;, etc.)
  });

  if (parsed.errors?.length) {
    console.warn("Avisos Papa Parse:", parsed.errors.slice(0, 5));
  }

  return parsed.data; // array de objetos
}
```

---

### 2) Generar columnas automáticamente para Tabulator

```js
function columnasDesdeDatos(data) {
  const ejemplo = data[0] ?? {};
  return Object.keys(ejemplo).map(k => ({
    title: k,   // etiqueta visible
    field: k,   // clave del objeto
    sorter: "string",
    headerHozAlign: "left",
    hozAlign: "left"
  }));
}
```

---

### 3) Pintar la tabla con Tabulator

```js
function renderTabla(selector, data) {
  const columns = columnasDesdeDatos(data);

  return new Tabulator(selector, {
    data,
    columns,
    layout: "fitDataStretch",
    pagination: true,
    paginationSize: 10,
    paginationSizeSelector: [10, 25, 50, 100],
    placeholder: "No hay datos que mostrar",
    progressiveRender: true,
    progressiveRenderSize: 200,
    columnDefaults: { headerHozAlign: "left", hozAlign: "left" }
  });
}
```

---

### 4) Búsqueda global (opcional)

```js
function activarBusquedaGlobal(input, table) {
  input.addEventListener("input", () => {
    const term = input.value.trim().toLowerCase();
    if (!term) return table.clearFilter(true);

    table.setFilter((row) => {
      const obj = row.getData();
      return Object.values(obj).some(v =>
        String(v ?? "").toLowerCase().includes(term));
    });
  });
}
```

---

### 5) Ponerlo todo junto

```js
(async () => {
  try {
    const datos = await cargarCSV();
    const tabla = renderTabla("#tabla", datos);

    // Si tienes un <input id="q"> en tu HTML
    const q = document.getElementById("q");
    if (q) activarBusquedaGlobal(q, tabla);

  } catch (err) {
    console.error("Error al cargar/mostrar CSV:", err);
    // Muestra un mensaje accesible en la página si quieres
    const status = document.getElementById("status");
    if (status) status.textContent = "❌ Error al cargar el CSV. Comprueba la ruta y usa un servidor local.";
  }
})();
```

---

!!! tip "Consejo"
    **Rutas relativas**: si tu HTML está en `2-csv/demo/index.html` y los datos en `datasets/csv/…`, la ruta típica será `../../datasets/csv/hosteleria-a-domicilio.csv`.
    Verifica siempre la ruta final en DevTools → pestaña **Network**.

---

## ⚖️ Comparación con el enfoque *vanilla*

| Aspecto              | Vanilla (split + parseo propio)                                | Con librerías (Papa Parse + Tabulator)                                                    |
| -------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Parseo de CSV        | Manual (`split`, comillas, saltos de línea, separador)         | **Automático y robusto** (cabeceras, comillas, autodetección de separador, líneas vacías) |
| Tipado de valores    | Manual (hay que convertir números/booleanos si se desea)       | `dynamicTyping` opcional (convierte números/booleanos)                                    |
| Errores de formato   | A tu cargo (tienes que manejarlos)                             | Reporte de errores/advertencias en `parsed.errors`                                        |
| Tabla y UI           | Manual (construcción de `<table>`, ordenación/filtrado a mano) | **Tabulator** ofrece paginación, ordenación, filtros, responsive                          |
| Código repetitivo    | Más líneas (plantillas de tabla, eventos, filtros)             | Menos repetición: configuración y listo                                                   |
| Dependencias         | Ninguna                                                        | 2 dependencias (ligeras)                                                                  |
| Curva de aprendizaje | Muy baja                                                       | Muy baja (configurar opciones y columnas)                                                 |

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué parámetros principales has usado en `Papa.parse` y para qué sirven?
    2. ¿Cómo generas dinámicamente las columnas de Tabulator a partir de los datos?
    3. ¿Qué ventaja ofrece Tabulator frente a crear una tabla HTML “a mano”?
    4. ¿Cómo implementarías una **búsqueda global** sobre todas las columnas?
    5. Si el CSV es muy grande, ¿qué opciones de Tabulator ayudan a que sea fluido?