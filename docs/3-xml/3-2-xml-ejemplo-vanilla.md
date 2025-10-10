# 3.2. Ejemplo con JavaScript puro (vanilla) – XML

En este ejemplo aprenderás a **leer y procesar un XML** usando **solo JavaScript** (sin librerías).  
Trabajaremos con el dataset real **`eventos.xml`** (agenda cultural) y mostraremos cómo convertir sus nodos en **objetos JavaScript** para poder listarlos o construir tarjetas.

---

## 📌 Flujo de trabajo con vanilla JS

1. **Obtener el archivo** con `fetch`.  
2. **Convertir la respuesta a texto** (`.text()`).  
3. **Parsear el texto a DOM** con `DOMParser`.  
4. **Seleccionar nodos** con `querySelectorAll`.  
5. **Mapear cada nodo** a un objeto (extrayendo subelementos y atributos).  
6. **Normalizar campos** como fechas o coordenadas.  
7. **Renderizar** en tarjetas accesibles.  

---

## 🧩 Código paso a paso

### 1) Cargar y parsear el XML

```js
async function cargarXML(url) {
  const res = await fetch(url);
  const xmlText = await res.text();

  const parser = new DOMParser();
  const xmlDoc = parser.parseFromString(xmlText, "application/xml");

  const parseError = xmlDoc.querySelector("parsererror");
  if (parseError) {
    throw new Error("Error al parsear el XML: " + parseError.textContent);
  }

  return xmlDoc;
}

// Uso
cargarXML("datasets/xml/eventos.xml").then(xmlDoc => {
  console.log("Documento XML:", xmlDoc);
});
```

---

### 2) Seleccionar nodos de evento y convertirlos a objetos

```js
function leerEventos(xmlDoc) {
  const nodosEvento = xmlDoc.querySelectorAll("element");

  return Array.from(nodosEvento).map(nodo => {
    const get = (sel) => nodo.querySelector(sel)?.textContent?.trim() ?? "";

    return {
      titulo: get("Titulo_es"),
      inicio: get("FechaInicio"),
      fin: get("FechaFin"),
      descripcion: get("Descripcion_es"),
      lugar: get("LugarCelebracion"),
      municipio: get("Municipio"),
      provincia: get("Provincia"),
      coords: get("LugarCelebracionDirectorio_Posicion"),
      imagen: get("ImagenEvento")
    };
  });
}
```

---

### 3) Normalizar campos (fechas y coordenadas)

```js
const toDate = (s) => {
  const d = new Date(s?.trim());
  return isNaN(d) ? null : d;
};

const parseCoords = (s) => {
  if (!s) return null;
  const [lat, lon, alt] = s.split("#").map(Number);
  return Number.isFinite(lat) && Number.isFinite(lon)
    ? { lat, lon, alt: Number.isFinite(alt) ? alt : null }
    : null;
};

function normalizarEventos(eventos) {
  return eventos.map(ev => ({
    ...ev,
    inicioDate: toDate(ev.inicio),
    finDate: toDate(ev.fin),
    geo: parseCoords(ev.coords)
  }));
}
```

---

### 4) Renderizar en tarjetas accesibles

HTML mínimo:

```html
<section id="lista" aria-live="polite"></section>
```

JS:

```js
function renderEventos(eventos, contenedor) {
  contenedor.innerHTML = "";
  const fragment = document.createDocumentFragment();

  eventos.forEach(ev => {
    const art = document.createElement("article");
    art.className = "card";
    art.innerHTML = `
      <h2>${ev.titulo || "Evento sin título"}</h2>
      <p><strong>Inicio:</strong> ${ev.inicio || "—"} · <strong>Fin:</strong> ${ev.fin || "—"}</p>
      ${ev.lugar ? `<p><strong>Lugar:</strong> ${ev.lugar} (${ev.municipio || ""}${ev.provincia ? ", " + ev.provincia : ""})</p>` : ""}
      ${ev.imagen ? `<img src="${ev.imagen}" alt="${ev.titulo ? `Imagen del evento “${ev.titulo}”` : "Imagen del evento"}">` : ""}
    `;
    fragment.appendChild(art);
  });

  contenedor.appendChild(fragment);
}
```

---

### 5) Ponerlo todo junto

```js
(async () => {
  const xmlDoc = await cargarXML("datasets/xml/eventos.xml");
  const eventos = leerEventos(xmlDoc);
  const normalizados = normalizarEventos(eventos);

  normalizados.sort((a, b) => (a.inicioDate || 0) - (b.inicioDate || 0));

  renderEventos(normalizados.slice(0, 10), document.getElementById("lista"));
})();
```

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué devuelve `DOMParser.parseFromString` y por qué es útil?
    2. ¿Cómo seleccionamos todos los nodos de evento en el XML?
    3. ¿Qué ventaja tiene usar `?.textContent ?? ""` al leer campos del XML?
    4. ¿Cómo transformarías `lat#lon#alt` en un objeto `{ lat, lon, alt }`?
    5. ¿Por qué es recomendable normalizar las fechas antes de ordenarlas o filtrarlas?