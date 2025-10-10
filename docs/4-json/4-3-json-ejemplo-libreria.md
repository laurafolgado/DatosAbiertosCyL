# 4.3. Ejemplo con librería (Axios)

En este apartado vamos a ver cómo procesar un JSON utilizando una **librería externa**, en concreto **Axios**, para simplificar la carga y el manejo de errores.  
Trabajaremos con el dataset de **instituciones bibliotecarias de Castilla y León** en formato JSON.

---

## 📦 Cómo incluir la librería

En este curso usaremos **CDN** (no hace falta `npm`). Inserta el script en tu HTML antes de tu código:

```html
<script src="https://cdn.jsdelivr.net/npm/axios@1.7.7/dist/axios.min.js" integrity="" crossorigin="anonymous"></script>
<script type="module" src="main.js"></script>
```

> Si más adelante empaquetas con Vite/Webpack, podrás `import axios from "axios"` sin cambiar el resto del código.

---

## 📌 Flujo de trabajo con librería

1. **Incluir Axios** desde CDN.
2. **Hacer la solicitud** con `axios.get(url)`.
3. **Recibir los datos** desde `response.data` (ya parseados).
4. **Comprobar la estructura** (array u objeto).
5. **Mapear** a un modelo de columnas útil para la interfaz.
6. **Renderizar** la información en una tabla simple.
7. **Gestionar errores** (red, timeout) y, si hace falta, **cancelar** la petición.

---

## 🧩 Código paso a paso

### 1) Cargar el JSON con Axios

```js
// datasets/json/registro-de-instituciones-bibliotecarias-de-castilla-y-leon.json
const URL_JSON = "datasets/json/registro-de-instituciones-bibliotecarias-de-castilla-y-leon.json";

async function cargarJSON() {
  try {
    const { data } = await axios.get(URL_JSON, {
      headers: { Accept: "application/json" },
      timeout: 8000 // ms
    });
    return data;
  } catch (err) {
    // Manejo de errores típico con Axios
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

### 2) Detectar la forma del dataset y quedarnos con la lista

El fichero real de bibliotecas es un **array de objetos**. Por eso comprobamos si `data` ya es un array; si no, intentamos localizar la clave lista (por ejemplo, `instituciones` en otros JSON).

```js
function obtenerListaInstituciones(data) {
  if (Array.isArray(data)) return data;
  // fallback genérico si viniera envuelto en un objeto
  return data?.instituciones ?? [];
}
```

---

### 3) Mapear a un modelo de columnas útil

Seleccionamos las claves que queramos mostrar en la tabla (tú puedes cambiar/añadir):

```js
function aFilasTabla(instituciones) {
  return instituciones.map(i => ({
    nombreentidad: i.nombreentidad ?? "",
    localidad: i.localidad ?? "",
    tipo_de_gestion: i.tipo_de_gestion ?? "",
    codigo: i.codigo ?? "",
    enlace_al_contenido: i.enlace_al_contenido ?? ""
  }));
}
```

---

### 4) Renderizar una tabla HTML sencilla

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
  const data = await cargarJSON();
  const instituciones = obtenerListaInstituciones(data);
  const filas = aFilasTabla(instituciones);
  renderTabla(filas, ["nombreentidad", "localidad", "tipo_de_gestion", "codigo", "enlace_al_contenido"]);
})();
```

---

### (Opcional) Cancelar una petición con `AbortController`

Axios 1.x soporta **AbortController** (útil si el usuario cambia de vista):

```js
const controller = new AbortController();
const peticion = axios.get(URL_JSON, { signal: controller.signal });

// … si necesitas abortar:
controller.abort();
```

---

!!! tip "Consejo"
    En **MkDocs** (o cualquier servidor estático), usa siempre un **servidor local** (`mkdocs serve`, Live Server…).
    Si abres el HTML con `file://`, el navegador puede bloquear la lectura del JSON.
    Revisa en DevTools → Network que la **ruta relativa** a `datasets/json/...` es la correcta.

---

## ⚖️ Comparación con el enfoque *vanilla*

| Aspecto                     | Vanilla (`fetch` + `.json()`)             | Con librería (Axios)                                             |
| --------------------------- | ----------------------------------------- | ---------------------------------------------------------------- |
| Parseo                      | Manual (`res.json()`)                     | Automático en `response.data`                                    |
| Errores                     | `try/catch` + comprobar `res.ok`          | `try/catch` + `axios.isAxiosError`, códigos de estado accesibles |
| Timeouts                    | Hay que implementar señal/abort manual    | Opción `timeout` nativa                                          |
| Cancelación                 | `AbortController` con `fetch`             | `AbortController` soportado (`{ signal }`)                       |
| Cabeceras por defecto       | Debes pasarlas tú (opcional)              | Incluye cabeceras sensatas; fácil añadir `headers`               |
| Interceptores / logging     | A mano en cada llamada                    | Interceptores globales (`axios.interceptors`)                    |
| Tamaño de código repetitivo | Algo más de “pegamento” por cada endpoint | Menos repetición; configuración común y *defaults*               |
| Dependencias                | Ninguna                                   | 1 dependencia (ligera)                                           |
| Curva de aprendizaje        | Muy baja                                  | Muy baja (si sabes `fetch`, te resultará familiar)               |

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué diferencia hay entre `response.json()` en *vanilla* y `response.data` en Axios?
    2. ¿Cómo comprobarías si el dataset viene como **array** o dentro de una **clave** (ej. `instituciones`)?
    3. ¿Qué ventajas aporta `timeout` en Axios frente a *fetch* puro?
    4. ¿Cómo cancelarías una petición en curso con Axios?
    5. ¿Qué columnas has decidido mostrar y por qué?