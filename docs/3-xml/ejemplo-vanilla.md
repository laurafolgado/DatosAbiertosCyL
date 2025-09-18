
# 3.2. Ejemplo con JavaScript puro (vanilla) – XML

En este ejemplo aprenderás a **leer y procesar un XML** usando **solo JavaScript** (sin librerías).  
Trabajaremos con el dataset real **`eventos.xml`** (agenda cultural) y mostraremos cómo convertir sus nodos en **objetos JavaScript** para poder listarlos o construir tarjetas.

---

## 📌 Flujo de trabajo (paso a paso)

1. **Obtener el archivo** con `fetch`.  
2. **Convertir la respuesta a texto** (`.text()`).  
3. **Parsear el texto a DOM** con `DOMParser`.  
4. **Seleccionar nodos** con `querySelectorAll`.  
5. **Mapear cada nodo** a un objeto (extrayendo subelementos y atributos).  
6. **Renderizar** (por ejemplo, en tarjetas accesibles).

---

## 📌 Cargar y parsear el XML

```js
async function cargarXML(url) {
  try {
    const res = await fetch(url);
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    const xmlText = await res.text();

    // 1) Convertimos el texto XML a un documento DOM
    const parser = new DOMParser();
    const xmlDoc = parser.parseFromString(xmlText, "application/xml");

    // 2) Comprobamos si hubo errores de parseo
    const parseError = xmlDoc.querySelector("parsererror");
    if (parseError) {
      throw new Error("Error al parsear el XML: " + parseError.textContent);
    }

    return xmlDoc;
  } catch (err) {
    console.error(err);
    throw err;
  }
}

// Uso (ajusta la ruta a tu estructura de carpetas)
cargarXML("datasets/xml/eventos.xml").then(xmlDoc => {
  console.log("Documento XML:", xmlDoc);
});
```

---

!!! tip "Consejo"
    `DOMParser` necesita el **MIME type correcto**: usa `"application/xml"`.
    Si aparece `<parsererror>`, el archivo puede estar corrupto o no ser XML válido.

---

## 📌 Extraer datos con `querySelectorAll`

Cada evento suele representarse como un nodo (p. ej. `<element>`, `<evento>` o similar según el dataset).
Primero localizamos todos los nodos de evento; luego, de cada uno, extraemos subnodos (título, fechas, imagen, ubicación…).

```js
function leerEventos(xmlDoc) {
  // Ajusta el selector al nombre del nodo de evento en tu XML
  const nodosEvento = xmlDoc.querySelectorAll("element"); 

  // Recorremos y convertimos cada nodo en un objeto JS
  const eventos = Array.from(nodosEvento).map(nodo => {
    const get = (sel) => nodo.querySelector(sel)?.textContent?.trim() ?? "";

    return {
      titulo: get("Titulo_es"),
      inicio: get("FechaInicio"),
      fin: get("FechaFin"),
      descripcion: get("Descripcion_es"),
      imagen: get("ImagenEvento"),
      // Ejemplo de coordenadas en un texto tipo "lat#lon#alt"
      coords: get("LugarCelebracionDirectorio_Posicion"), 
      lugar: get("LugarCelebracion"),
      municipio: get("Municipio"),
      provincia: get("Provincia")
    };
  });

  return eventos;
}
```

---

## 📌 Normalizar campos (fechas y coordenadas)

Para poder **ordenar por fecha** o **pintar un mapa**, conviene transformar cadenas a tipos útiles.

```js
// 1) Fechas ISO → Date (si el formato es YYYY-MM-DD)
const toDate = (s) => {
  // Acepta "YYYY-MM-DD" o "YYYY-MM-DDTHH:mm"
  const iso = s?.trim();
  if (!iso) return null;
  const d = new Date(iso);
  return isNaN(d) ? null : d;
};

// 2) "lat#lon#alt" → { lat, lon, alt }
const parseCoords = (s) => {
  if (!s) return null;
  const [lat, lon, alt] = s.split("#").map(Number);
  if (Number.isFinite(lat) && Number.isFinite(lon)) {
    return { lat, lon, alt: Number.isFinite(alt) ? alt : null };
  }
  return null;
};
```

Aplicamos la normalización al array de eventos:

```js
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

!!! tip "Consejo"
    **No asumas** que todos los campos existen: usa el operador `?.` y valores por defecto (`?? ""`).
    Comprueba que las fechas y las coordenadas sean válidas antes de usarlas.

---

## 📌 Renderizar en tarjetas accesibles

Un HTML mínimo para la lista:

```html
<section id="lista" aria-live="polite"></section>
```

Código para mostrar los eventos más próximos (ordenados por fecha de inicio):

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

Y el **pegamento**:

```js
(async () => {
  const xmlDoc = await cargarXML("datasets/xml/eventos.xml");
  const eventos = leerEventos(xmlDoc);
  const normalizados = normalizarEventos(eventos);

  // Ordenar por fecha de inicio (nulls al final)
  normalizados.sort((a, b) => {
    if (!a.inicioDate && !b.inicioDate) return 0;
    if (!a.inicioDate) return 1;
    if (!b.inicioDate) return -1;
    return a.inicioDate - b.inicioDate;
  });

  // Mostrar los 10 próximos
  const lista = document.getElementById("lista");
  renderEventos(normalizados.slice(0, 10), lista);
})();
```

---

## 📌 Tratamiento de **namespaces** (si aparecen)

Algunos XML usan **espacios de nombres** y verás etiquetas con prefijo: `<dc:title>`.
En esos casos, `querySelector` requiere **incluir el prefijo** exacto, o es más cómodo usar `getElementsByTagNameNS`.

```js
// Ejemplo: obtener <dc:title> ignorando el prefijo real del documento
function getNS(el, localName) {
  // Busca por nombre local, sin importar el namespace/prefijo
  return Array.from(el.getElementsByTagName("*"))
    .find(n => n.localName === localName)?.textContent?.trim() ?? "";
}

// Uso: getNS(nodo, "title")
```

---

!!! tip "Consejo"
    Si ves etiquetas con prefijo (p. ej. `dc:`), **no las reemplaces**:
    
    - Prueba primero con `querySelector("dc\\:title")`.
    - Si es incómodo, usa `localName` como en el ejemplo anterior.

---

## 📌 Validaciones y errores comunes

* **`parsererror`** tras `DOMParser`: revisa la **codificación** (UTF-8) o si el archivo está bien formado.
* **Nodos con nombres distintos** a los esperados: inspecciona el XML y adapta tus selectores.
* **Fechas en formatos inesperados**: transforma a un formato ISO o usa un parser a medida.
* **Campos vacíos**: recuerda poner valores por defecto o controles en la interfaz.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué devuelve `DOMParser.parseFromString` y por qué es útil?
    2. ¿Cómo extraerías el texto de `<Titulo_es>` y `<FechaInicio>` de cada `<element>`?
    3. ¿Qué harías si la etiqueta de título viniera como `<dc:title>`?
    4. ¿Cómo ordenarías los eventos por fecha de inicio si algunas fechas están vacías o mal formadas?
    5. ¿Qué ventaja tiene transformar `lat#lon#alt` a un objeto `{ lat, lon, alt }` antes de renderizar?