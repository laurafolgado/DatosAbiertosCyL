
# 5.3. Ejemplo con librería (Axios) – API REST

En este apartado vamos a ver cómo consumir una **API REST** utilizando una **librería externa**, en concreto **Axios**, que simplifica mucho la carga de datos y el manejo de errores.
Trabajaremos con un endpoint real del portal de **Datos Abiertos de la Junta de Castilla y León** en formato JSON (por ejemplo, la **agenda cultural** expuesta como servicio REST).

---

## 📦 Cómo incluir la librería

Usaremos **CDN** para no complicar el entorno. Inserta Axios en tu HTML antes de tu script:

```html
<script src="https://cdn.jsdelivr.net/npm/axios@1.7.7/dist/axios.min.js" crossorigin="anonymous"></script>
<script type="module" src="main.js"></script>
```

> Más adelante, si empaquetas con Vite/Webpack, podrás hacer `import axios from "axios"` sin cambiar nada del resto del código.

---

## 📌 Flujo de trabajo con Axios

1. **Incluir Axios** desde CDN.
2. **Llamar a la API** con `axios.get(url)`.
3. **Obtener los datos** desde `response.data`.
4. **Comprobar la estructura** de la respuesta (array u objeto con claves).
5. **Transformar** los resultados a un formato manejable.
6. **Renderizar** la información en una tabla o tarjetas HTML.
7. **Manejar errores** de red o de API con mensajes claros.

---

## 🧩 Código paso a paso

### 1) Llamada a la API con Axios

```js
const URL_API = "https://datosabiertos.jcyl.es/api/agenda-cultural"; 
// (ejemplo; sustituye por la ruta real de la API)

async function cargarAPI() {
  try {
    const { data } = await axios.get(URL_API, {
      headers: { Accept: "application/json" },
      timeout: 8000 // ms
    });
    return data;
  } catch (err) {
    if (axios.isAxiosError(err)) {
      console.error("Axios error:", err.message, "status:", err.response?.status);
    } else {
      console.error("Error desconocido:", err);
    }
    throw err;
  }
}
```

---

### 2) Detectar la estructura y quedarnos con la lista

En muchas APIs, la respuesta tiene esta forma:

```json
{
  "resultados": [
    { "titulo": "Concierto en Salamanca", "fecha": "2024-09-01", "lugar": "Plaza Mayor" },
    { "titulo": "Exposición en León", "fecha": "2024-09-05", "lugar": "Museo de Arte" }
  ]
}
```

Podemos acceder así:

```js
function obtenerEventos(data) {
  return data?.resultados ?? [];
}
```

---

### 3) Transformar cada registro a un modelo útil

```js
function aFilasTabla(eventos) {
  return eventos.map(ev => ({
    titulo: ev.titulo ?? "",
    fecha: ev.fecha ?? "",
    lugar: ev.lugar ?? ""
  }));
}
```

---

### 4) Renderizar en tabla HTML sencilla

```js
function renderTabla(datos, columnas, destino = document.body) {
  const tabla = document.createElement("table");

  // cabecera
  const thead = document.createElement("thead");
  const trH = document.createElement("tr");
  columnas.forEach(col => {
    const th = document.createElement("th");
    th.scope = "col";
    th.textContent = col;
    trH.appendChild(th);
  });
  thead.appendChild(trH);
  tabla.appendChild(thead);

  // filas
  const tbody = document.createElement("tbody");
  datos.forEach(fila => {
    const tr = document.createElement("tr");
    columnas.forEach(col => {
      const td = document.createElement("td");
      td.textContent = fila[col] ?? "";
      tr.appendChild(td);
    });
    tbody.appendChild(tr);
  });
  tabla.appendChild(tbody);

  destino.appendChild(tabla);
}
```

---

### 5) Ponerlo todo junto

```js
(async () => {
  const data = await cargarAPI();
  const eventos = obtenerEventos(data);
  const filas = aFilasTabla(eventos);
  renderTabla(filas, ["titulo", "fecha", "lugar"]);
})();
```

---

## ⚖️ Comparación con el enfoque *vanilla*

| Aspecto           | Vanilla (`fetch`)                       | Con librería (Axios)                            |
| ----------------- | --------------------------------------- | ----------------------------------------------- |
| Parseo JSON       | `res.json()` manual                     | Automático en `response.data`                   |
| Manejo de errores | `try/catch` + comprobar `res.ok`        | `axios.isAxiosError`, códigos y mensajes claros |
| Timeouts          | Debes usar `AbortController`            | Opción `timeout` integrada                      |
| Cancelación       | `AbortController`                       | Compatible con `{ signal }` en Axios 1.x        |
| Cabeceras         | Hay que pasarlas manualmente (opcional) | Fácil añadir/gestionar globalmente              |
| Código repetitivo | Más “pegamento” en cada endpoint        | Configuración global y defaults                 |
| Dependencias      | Ninguna                                 | 1 librería ligera                               |

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué diferencia hay entre `response.json()` en *vanilla* y `response.data` en Axios?
    2. ¿Qué ventaja aporta el parámetro `timeout` en una llamada a la API?
    3. ¿Cómo accederías al array de resultados si el JSON de la API no viene bajo `resultados`, sino bajo otra clave?
    4. ¿Qué columnas hemos decidido mostrar en la tabla y por qué?
    5. ¿Qué beneficios prácticos aporta Axios frente a `fetch` al trabajar con APIs REST?
