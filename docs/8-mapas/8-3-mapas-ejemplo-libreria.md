# 8.3. Ejemplo con librería – Leaflet

En este ejemplo vamos a mostrar el dataset de **centros docentes de Castilla y León** en un mapa interactivo usando la librería **Leaflet**.
Con Leaflet podemos representar **muchos puntos a la vez**, añadir **popups con información** y permitir al usuario interactuar con el mapa.

---

## 📦 Cómo incluir la librería

Añadimos los estilos y el script de Leaflet desde un **CDN** en el HTML:

```html
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script type="module" src="main.js"></script>
```

Creamos un contenedor para el mapa:

```html
<div id="mapa" style="height: 500px; border: 1px solid #ccc;"></div>
```

---

## 📌 Flujo de trabajo con Leaflet

1. **Incluir Leaflet** desde CDN.
2. **Cargar el CSV** con `fetch`.
3. **Parsear el contenido a objetos**.
4. **Inicializar el mapa** con un centro y nivel de zoom.
5. **Añadir una capa base** de OpenStreetMap.
6. **Recorrer los centros** y colocar un marcador en cada uno.
7. **Configurar popups** para mostrar nombre, localidad y tipo de centro.

---

## 🧩 Código paso a paso

### 1) Cargar y parsear el CSV

```js
async function cargarCSV() {
  const res = await fetch("datasets/csv/directorio-de-centros-docentes.csv");
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

### 2) Inicializar el mapa

```js
function initMapa() {
  // Centro geográfico aproximado de Castilla y León
  const mapa = L.map("mapa").setView([41.65, -4.72], 7);

  // Capa base de OpenStreetMap
  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OSM</a> contributors'
  }).addTo(mapa);

  return mapa;
}
```

---

### 3) Añadir los centros al mapa

```js
function dibujarCentros(mapa, centros) {
  centros.forEach(c => {
    const lat = parseFloat(c.Lat);
    const lon = parseFloat(c.Lon);

    if (!Number.isFinite(lat) || !Number.isFinite(lon)) return;

    const popup = `
      <strong>${c.Nombre || "Centro sin nombre"}</strong><br>
      ${c.Municipio || ""} (${c.Provincia || ""})<br>
      Tipo: ${c.Tipo || "—"}
    `;

    L.marker([lat, lon]).addTo(mapa).bindPopup(popup);
  });
}
```

---

### 4) Ponerlo todo junto

```js
(async () => {
  const datos = await cargarCSV();
  const mapa = initMapa();
  dibujarCentros(mapa, datos);
})();
```

---

!!! tip "Consejo"
    Leaflet permite añadir muchas funcionalidades extra:

    * **Clustering** de puntos si hay miles de registros (`Leaflet.markercluster`).
    * **Capas adicionales** (satélite, topografía, etc.).
    * **Iconos personalizados** según el tipo de centro.

---

## ⚖️ Comparación con el enfoque *vanilla*

| Aspecto       | Vanilla (`iframe`)            | Con Leaflet                             |
| ------------- | ----------------------------- | --------------------------------------- |
| Puntos        | Solo 1 punto fijo             | Múltiples puntos dinámicos              |
| Interacción   | No (mapa estático)            | Sí (zoom, pan, popups)                  |
| Estilo        | Limitado al servicio embebido | Personalizable (iconos, colores, capas) |
| Popups        | No                            | Sí, muy flexibles                       |
| Escalabilidad | Muy limitado                  | Miles de puntos con clustering          |

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué librería hemos usado para crear el mapa interactivo?
    2. ¿Qué campos del CSV se necesitan para situar los centros en el mapa?
    3. ¿Cómo se añade una capa base de OpenStreetMap en Leaflet?
    4. ¿Qué método de Leaflet se usa para añadir marcadores (`marker`) y popups (`bindPopup`)?
    5. ¿Qué mejoras implementarías si el dataset tuviera miles de centros?
