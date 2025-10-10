# 1.1. Fundamentos de `fetch`, promesas y `async/await`

Para poder trabajar con los datasets del portal de **Datos Abiertos de la Junta de Castilla y León**, necesitamos una herramienta que nos permita **traer información externa a nuestras aplicaciones web**.
En JavaScript, esa herramienta es **`fetch`**, el método moderno y estándar para realizar peticiones HTTP desde el navegador.

---

## 📌 ¿Qué es `fetch`?

`fetch(url)` es una función nativa que realiza una solicitud al servidor indicado y devuelve una **promesa**.
Esa promesa se resuelve en un objeto de tipo **`Response`**, que encapsula tanto el estado de la petición como los datos obtenidos.

Características clave:

* Es parte del estándar web (no requiere instalar librerías).
* Sustituye a la antigua API `XMLHttpRequest`, con una sintaxis más clara y potente.
* Permite trabajar con distintos formatos: JSON, CSV (como texto), XML, imágenes, binarios, etc.

---

## 📌 Sintaxis básica

```js
fetch("datasets/json/marcas-calidad.json")
  .then(respuesta => {
    if (!respuesta.ok) throw new Error("HTTP " + respuesta.status);
    return respuesta.json();
  })
  .then(datos => {
    console.log("Datos recibidos:", datos);
  })
  .catch(error => {
    console.error("Error en la petición:", error);
  });
```

En este ejemplo:

1. `fetch(...)` inicia la petición.
2. El primer `.then()` recibe un objeto `Response` y lo convierte a JSON.
3. El segundo `.then()` procesa los datos ya transformados.
4. `.catch()` captura cualquier error de red o de procesamiento.

---

## 📌 El objeto `Response`

Cuando `fetch` responde, lo primero que obtenemos es un objeto `Response`.
Este objeto nos da acceso a:

* `status` → código HTTP (`200` éxito, `404` no encontrado, `500` error interno).
* `ok` → booleano que indica si la petición se resolvió correctamente (`true` si `status` ∈ 200–299).
* `headers` → lista de cabeceras HTTP de la respuesta.

Y, lo más importante, **métodos para acceder al cuerpo de la respuesta**:

* `.json()` → interpreta el cuerpo como JSON.
* `.text()` → devuelve el cuerpo como texto plano (CSV, XML…).
* `.blob()` → devuelve datos binarios (imágenes, PDF).
* `.arrayBuffer()` → acceso de bajo nivel a datos binarios.

---

## 📌 Uso con `async/await`

El mismo ejemplo anterior puede escribirse de forma más clara con `async/await`:

```js
async function cargarDatos() {
  try {
    let respuesta = await fetch("datasets/xml/eventos.xml");
    if (!respuesta.ok) throw new Error("HTTP " + respuesta.status);

    let texto = await respuesta.text();
    console.log("Contenido del XML:", texto);
  } catch (error) {
    console.error("Error en la carga:", error);
  }
}
```

Ventajas de `async/await`:

* El código se lee “de arriba a abajo”, como si fuera síncrono.
* El bloque `try/catch` centraliza el manejo de errores.
* Facilita la comprensión en programas más largos con varias peticiones.

---

## 📌 Ejemplos prácticos

### 1. Cargar y mostrar un JSON

```js
async function cargarMarcas() {
  let res = await fetch("datasets/json/marcas-calidad.json");
  if (!res.ok) throw new Error("HTTP " + res.status);

  let datos = await res.json();
  console.log("Listado de marcas:", datos);
}
```

### 2. Cargar un CSV como texto

```js
async function cargarCSV() {
  let res = await fetch("datasets/csv/hosteleria-a-domicilio.csv");
  let texto = await res.text();

  let lineas = texto.trim().split("\n");
  console.log("Número de registros:", lineas.length - 1);
}
```

### 3. Descargar y mostrar una imagen

```js
async function cargarImagen() {
  let res = await fetch("imagenes/logo.png");
  let blob = await res.blob();

  document.getElementById("logo").src = URL.createObjectURL(blob);
}
```

```html
<img id="logo" alt="Imagen cargada con fetch">
```

---

## 📌 Opciones avanzadas de `fetch`

Además de descargar datos, `fetch` permite **enviar información** al servidor.
Ejemplo de envío de un objeto JSON:

```js
fetch("https://ejemplo.com/api/usuarios", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({ nombre: "Laura", activo: true })
});
```

Opciones más comunes:

* `method`: tipo de petición (`GET`, `POST`, `PUT`, `DELETE`).
* `headers`: añadir cabeceras HTTP (ej. `Content-Type`).
* `body`: datos enviados (normalmente en JSON o `FormData`).

---

## 📌 Limitaciones y retos

Aunque `fetch` es muy potente y hoy en día es el estándar para trabajar con datos en la web, conviene ser conscientes de algunas **limitaciones prácticas**:

* **Errores HTTP**: `fetch` no lanza una excepción cuando la respuesta es un `404` o un `500`. El objeto `Response` sigue devolviéndose como válido y solo sabremos que algo fue mal si comprobamos la propiedad `res.ok` o el código de estado. Esto obliga a incluir siempre una comprobación explícita.
* **Restricciones CORS**: por motivos de seguridad, un navegador no puede acceder a datos de cualquier servidor libremente. El propio servidor tiene que declarar que permite ser consumido desde otros orígenes. Si no, veremos el típico error de *CORS policy* en consola.
* **Protocolos seguros**: cuando nuestra página se carga en `https://`, el navegador bloquea cualquier petición a `http://` porque lo considera inseguro. Por eso, en proyectos reales hay que asegurarse de que todas las fuentes de datos estén publicadas en HTTPS.
* **Acceso local**: abrir un archivo directamente con `file://` no funciona, ya que los navegadores bloquean estas lecturas por seguridad. Siempre debemos trabajar con un **servidor local** como Live Server, `mkdocs serve` o un servidor integrado.
* **Control de tiempo de espera**: `fetch` no incorpora un sistema de *timeout*. Si un servidor tarda demasiado en responder, la petición se queda bloqueada hasta que se cierre. Para evitarlo, es recomendable usar `AbortController` y definir un límite de tiempo.

---

## 📌 Ventajas

A pesar de esas limitaciones, `fetch` se ha convertido en el método estándar porque ofrece **grandes ventajas** frente a alternativas anteriores como `XMLHttpRequest`:

* **API moderna y nativa**: forma parte del propio lenguaje y no requiere librerías externas, lo que simplifica el desarrollo.
* **Compatibilidad universal**: está soportado en todos los navegadores modernos, lo que asegura que el código funcionará en cualquier entorno actual.
* **Sintaxis más clara**: su diseño basado en promesas y la posibilidad de usar `async/await` hacen que el código sea más legible y fácil de mantener.
* **Versatilidad**: permite trabajar con prácticamente cualquier recurso: JSON, texto plano, XML, imágenes o incluso archivos binarios complejos, adaptándose a distintos escenarios.

En conjunto, `fetch` combina **simplicidad, potencia y estandarización**, lo que lo convierte en la herramienta ideal para aprender y trabajar con datos abiertos en aplicaciones web.


!!! tip "Consejo práctico"
    Piensa en `fetch` como un proceso en **dos fases**:
    1. Hacer la petición y obtener un objeto `Response`.
    2. Transformar la respuesta al formato correcto (`.json()`, `.text()`, `.blob()`).

    Revisar la documentación de los datasets es fundamental para saber qué método usar.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué devuelve `fetch()` y qué representa?
    2. ¿Qué diferencia hay entre el objeto `Response` y los datos obtenidos con `.json()` o `.text()`?
    3. ¿Qué ventajas tiene escribir con `async/await` frente a encadenar `.then()`?
    4. ¿Por qué es necesario comprobar siempre `res.ok` tras una petición?
    5. ¿Qué problemas pueden surgir si trabajamos con datos desde `file://` en lugar de un servidor local?
