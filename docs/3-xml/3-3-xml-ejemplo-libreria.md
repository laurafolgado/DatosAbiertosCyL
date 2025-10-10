# 3.3. Ejemplo con librería (fast-xml-parser + Tabulator)

En este apartado vamos a ver cómo procesar un **XML** utilizando **librerías externas** para simplificar el **parseo** (XML → JSON) y la **visualización** (tabla interactiva).  
Trabajaremos con el dataset de **agenda cultural** (`eventos.xml`) del portal de Datos Abiertos de la Junta de Castilla y León.

---

## 📦 Cómo incluir las librerías

En este curso usaremos **CDN** (no hace falta `npm`). Inserta los scripts/estilos en tu HTML antes de tu código:

```html
<!-- fast-xml-parser: convierte XML → JSON -->
<script src="https://cdn.jsdelivr.net/npm/fast-xml-parser@4.4.0/dist/fast-xml-parser.min.js"></script>

<!-- Tabulator: tabla interactiva -->
<link
  rel="stylesheet"
  href="https://unpkg.com/tabulator-tables@6.2.5/dist/css/tabulator.min.css">
<script src="https://unpkg.com/tabulator-tables@6.2.5/dist/js/tabulator.min.js"></script>

<!-- Tu script -->
<script type="module" src="main.js"></script>
```

!!! tip "Consejo"
    Si más adelante empaquetas con Vite/Webpack, podrás:
    ```js
    import { XMLParser } from "fast-xml-parser";
    import Tabulator from "tabulator-tables";
    ```

---

## 📌 Flujo de trabajo con librería

1. **Incluir** `fast-xml-parser` y **Tabulator** desde CDN.
2. **Descargar** el XML con `fetch(url).then(res => res.text())`.
3. **Parsear** el XML con `XMLParser` → obtendrás un **objeto JS**.
4. **Localizar la lista** de elementos (p. ej. `json.root.element` o similar).
5. **Mapear** a un modelo de columnas útil para la interfaz.
6. **Renderizar** una **tabla interactiva** con Tabulator (columnas, paginación, ordenación).
7. **Gestionar errores** de red/parseo y mostrar un estado accesible.

---

## 🧩 Código paso a paso

### 1) Cargar y convertir el XML a JSON con fast-xml-parser

```js
// Ruta del XML (ajusta a tu estructura)
const URL_XML = "datasets/xml/eventos.xml";

async function cargarXMLcomoJSON() {
  // 1) Descargar XML como texto
  const res = await fetch(URL_XML, { cache: "no-cache" });
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  const xmlText = await res.text();

  // 2) Configurar el parser
  const options = {
    ignoreAttributes: false,   // conserva atributos
    attributeNamePrefix: "",   // nombres de atributo sin prefijo
    parseAttributeValue: true, // convierte "123" a número si procede
    // Fuerza arrays para nodos donde esperas lista (opcional):
    isArray: (name, jpath) => ["element", "evento", "monumento"].includes(name)
  };

  // 3) Parsear XML → JSON
  const parser = new fxp.XMLParser(options);
  const json = parser.parse(xmlText);
  return json;
}
```

---

### 2) Detectar la forma del dataset y quedarnos con la lista

El nodo que agrupa cada evento puede llamarse distinto según el XML (por ejemplo, `element`, `evento`, etc.).
Ajusta este helper a tu fichero real:

```js
function obtenerListaEventos(json) {
  // Explora con console.log(json) para confirmar la ruta
  const root = json.root ?? json.Eventos ?? json.eventos ?? json;
  const lista = root.element ?? root.evento ?? root.Evento ?? [];
  return Array.isArray(lista) ? lista : [lista]; // normaliza a array
}
```

---

### 3) Mapear a un modelo de columnas útil

Escogemos las claves que queremos mostrar (ajusta a tus nombres reales de etiqueta):

```js
function aFilasTabla(eventos) {
  return eventos.map(e => ({
    Titulo_es: e.Titulo_es ?? e.titulo ?? "",
    FechaInicio: e.FechaInicio ?? e.inicio ?? "",
    FechaFin: e.FechaFin ?? e.fin ?? "",
    LugarCelebracion: e.LugarCelebracion ?? e.lugar ?? "",
    Municipio: e.Municipio ?? "",
    Provincia: e.Provincia ?? ""
  }));
}
```

---

### 4) Generar columnas automáticamente para Tabulator

```js
function columnasDesdeDatos(data) {
  const ejemplo = data[0] ?? {};
  return Object.keys(ejemplo).map(k => ({
    title: k,
    field: k,
    sorter: "string",
    headerHozAlign: "left",
    hozAlign: "left"
  }));
}
```

---

### 5) Pintar la tabla con Tabulator

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

### 6) Búsqueda global (opcional)

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

### 7) Ponerlo todo junto

```js
(async () => {
  try {
    const json = await cargarXMLcomoJSON();
    const eventos = obtenerListaEventos(json);
    const filas = aFilasTabla(eventos);

    const tabla = renderTabla("#tabla", filas);

    // Si tienes un <input id="q"> en tu HTML:
    const q = document.getElementById("q");
    if (q) activarBusquedaGlobal(q, tabla);

  } catch (err) {
    console.error("Error al cargar/mostrar XML:", err);
    const status = document.getElementById("status");
    if (status) status.textContent = "❌ Error al cargar el XML. Comprueba la ruta y usa un servidor local.";
  }
})();
```

---

!!! tip "Consejo"
    *Explora siempre el JSON* que devuelve el parser con `console.log(json)`.
    En XML reales es habitual encontrar **namespaces** y **nodos opcionales**. Con `ignoreAttributes: false` conservarás atributos útiles, y con `isArray` puedes **forzar arrays** para listas aunque solo haya un elemento.

---

## ⚖️ Comparación con el enfoque *vanilla*

| Aspecto                       | Vanilla (DOMParser + `querySelectorAll`)      | Con librerías (fast-xml-parser + Tabulator)                                  |
| ----------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------- |
| Parseo de XML                 | Manual (DOM, nodos, textos, atributos)        | **Automático** XML → JSON con opciones (`ignoreAttributes`, `isArray`, etc.) |
| Namespaces                    | Hay que manejarlos en selectores              | Serializa claves con prefijo; puedes normalizarlas después                   |
| Estructuras y listas          | Recorres nodos y construyes tus objetos       | Obtienes **objetos/arrays** listos para usar                                 |
| Tabla y UI                    | Manual (crear `<table>`, filtros, paginación) | **Tabulator** ofrece paginación, ordenación, filtros                         |
| Robustez ante formatos reales | Depende de tu lógica de recorrido             | Parser probado, reporta inconsistencias y soporta múltiples opciones         |
| Dependencias                  | Ninguna                                       | 2 dependencias (ligeras)                                                     |
| Curva de aprendizaje          | Muy baja (si conoces DOM)                     | Muy baja (configurar parser y columnas)                                      |

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué ventajas aporta convertir el XML a JSON antes de trabajar con él?
    2. ¿Para qué sirven `ignoreAttributes` y `isArray` en `fast-xml-parser`?
    3. ¿Qué pasos das para localizar la **lista de eventos** dentro del JSON resultante?
    4. ¿Qué ventajas ofrece Tabulator frente a construir la tabla a mano?
    5. ¿Cómo implementarías una **búsqueda global** sobre todas las columnas?
