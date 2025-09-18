# 1.1. Fundamentos de `fetch` en JavaScript

En la web moderna, el método **`fetch()`** permite obtener datos externos de manera sencilla, ya sea un archivo local (CSV, XML, JSON) o una API en Internet.
Es la base sobre la que construiremos todas las prácticas de este proyecto.

---

## 📌 ¿Qué es `fetch`?

* Es una **función global** de JavaScript para hacer peticiones HTTP.
* Devuelve siempre una **Promesa**, un objeto que representa un valor futuro.
* Permite trabajar con distintos formatos: JSON, texto, XML, binarios, etc.
* Sustituye a la antigua API `XMLHttpRequest`, con una sintaxis más clara.

---

## 📌 Sintaxis básica

```js
fetch("datos.json")
  .then(respuesta => respuesta.json())
  .then(datos => {
    console.log(datos);
  })
  .catch(error => {
    console.error("Error:", error);
  });
```

En este ejemplo:

1. `fetch("datos.json")` pide el recurso.
2. El primer `.then` convierte la respuesta a objeto con `.json()`.
3. El segundo `.then` trabaja con los datos ya procesados.
4. `.catch` captura cualquier error de red.

---

## 📌 El objeto `Response`

La primera promesa resuelta por `fetch` no es el dato final, sino un objeto `Response`.
Este incluye información sobre la petición:

```js
fetch("datos.json")
  .then(respuesta => {
    console.log(respuesta.status); // 200, 404, 500...
    console.log(respuesta.ok);     // true si 200–299
    console.log(respuesta.headers); // cabeceras HTTP
  });
```

Para acceder al contenido hay que usar métodos como:

* `.json()` → para datos en formato JSON.
* `.text()` → para CSV, XML o cualquier texto.
* `.blob()` → para imágenes o binarios.
* `.arrayBuffer()` → para datos de bajo nivel.

---

## 📌 Uso con `async/await`

El mismo ejemplo puede escribirse con `async/await`, más legible:

```js
async function cargarDatos() {
  try {
    let respuesta = await fetch("datos.json");
    if (!respuesta.ok) {
      throw new Error("HTTP " + respuesta.status);
    }
    let datos = await respuesta.json();
    console.log(datos);
  } catch (error) {
    console.error("Error:", error);
  }
}

cargarDatos();
```

---

!!! tip "Consejo"
    `fetch` **solo lanza error si falla la red**.
    Si la respuesta es `404 Not Found`, no lanza error: por eso hay que comprobar siempre `respuesta.ok`.

---

## 📌 Ejemplo con texto (CSV o XML)

Cuando los datos son texto (CSV o XML), se utiliza `.text()`:

```js
async function cargarCSV() {
  let respuesta = await fetch("datasets/csv/hosteleria-a-domicilio.csv");
  let texto = await respuesta.text();
  console.log(texto);
}

cargarCSV();
```

Después podremos dividirlo en líneas o parsearlo con un `DOMParser`.

---

## 📌 Ejemplo con imágenes

`fetch` también sirve para descargar imágenes u otros binarios:

```js
async function cargarImagen() {
  let respuesta = await fetch("foto.jpg");
  let blob = await respuesta.blob();
  document.getElementById("imagen").src = URL.createObjectURL(blob);
}
```

```html
<img id="imagen" alt="Imagen cargada con fetch">
```

---

## 📌 Opciones avanzadas de `fetch`

La sintaxis completa permite indicar opciones:

```js
fetch("https://ejemplo.com/api", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({ usuario: "Laura", activo: true })
});
```

Opciones comunes:

* `method`: GET, POST, PUT, DELETE.
* `headers`: cabeceras personalizadas.
* `body`: datos enviados al servidor.

---

## 📝 Preguntas de repaso

!!! question "Reflexiona sobre lo aprendido"
    1. ¿Qué devuelve siempre `fetch()` y qué significa que sea una promesa?
    2. ¿Cuál es la diferencia entre el objeto `Response` y los datos procesados con `.json()` o `.text()`?
    3. ¿Qué ocurre si no compruebas `respuesta.ok` antes de usar los datos?
    4. Reescribe el ejemplo básico usando `async/await`.
    5. ¿Cuándo usarías `.blob()` en lugar de `.json()` o `.text()`?
