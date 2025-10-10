
# 8.2. Ejemplo con JavaScript puro (vanilla) – Mapa básico

En este ejemplo vamos a mostrar cómo representar la localización de los **centros docentes de Castilla y León** en un mapa sencillo usando **solo HTML y JavaScript puro**, sin librerías externas.

---

## 📌 Flujo de trabajo con vanilla JS

1. **Cargar el CSV** con `fetch`.
2. **Parsearlo a objetos** (leer cabecera y filas).
3. **Seleccionar un subconjunto** (por ejemplo, los primeros centros o los de una provincia concreta).
4. **Obtener sus coordenadas** (latitud y longitud).
5. **Generar un mapa básico** con un `<iframe>` de OpenStreetMap o Google Maps.

---

## 🧩 Código paso a paso

### 1) HTML base

```html
<div id="mapa"></div>
```

---

### 2) Cargar y parsear el CSV

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

### 3) Seleccionar un centro y sus coordenadas

```js
function obtenerCentroEjemplo(datos) {
  // Nos quedamos con el primer centro que tenga coordenadas válidas
  return datos.find(d => d.Lat && d.Lon);
}
```

---

### 4) Insertar un mapa con iframe

```js
function mostrarMapa(centro) {
  const lat = centro.Lat;
  const lon = centro.Lon;

  // Usamos OpenStreetMap con zoom 15
  const url = `https://www.openstreetmap.org/export/embed.html?bbox=${lon-0.01},${lat-0.01},${lon+0.01},${lat+0.01}&layer=mapnik&marker=${lat},${lon}`;

  const iframe = document.createElement("iframe");
  iframe.src = url;
  iframe.width = "600";
  iframe.height = "400";
  iframe.style.border = "1px solid #ccc";

  document.getElementById("mapa").appendChild(iframe);
}
```

---

### 5) Ponerlo todo junto

```js
(async () => {
  const datos = await cargarCSV();
  const centro = obtenerCentroEjemplo(datos);

  if (centro) {
    mostrarMapa(centro);
    console.log("Centro mostrado en el mapa:", centro.Nombre, centro.Municipio);
  } else {
    console.error("No se encontraron centros con coordenadas válidas.");
  }
})();
```

---

!!! tip "Consejo"
    Este enfoque con `<iframe>` es muy básico:
    
    * Solo permite mostrar **un punto fijo** en el mapa.
    * No ofrece interacción avanzada (zoom dinámico, varios puntos, popups…).

    En la siguiente lección usaremos **Leaflet**, una librería libre y ligera, para añadir todas esas funciones.  


---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué campos del CSV son necesarios para situar un centro en un mapa?
    2. ¿Qué servicio gratuito hemos usado para generar el mapa dentro de un `<iframe>`?
    3. ¿Qué limitaciones tiene este enfoque frente a una librería como Leaflet?
    4. ¿Cómo cambiarías el código para mostrar un centro diferente al primero?
    5. ¿Qué parámetro de la URL controla el nivel de zoom en OpenStreetMap?