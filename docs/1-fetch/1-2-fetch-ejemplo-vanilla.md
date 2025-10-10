# 1.2. Ejemplo con JavaScript puro (vanilla) – `fetch` y promesas

En este ejemplo aprenderás a **cargar datos desde un archivo remoto** usando la función **`fetch`** del navegador.
Usaremos dos ejemplos reales del portal de datos abiertos de Castilla y León:

* Un archivo **TXT** con metadatos de fotogramas aéreos históricos (`Leeme.txt`).
* Un archivo **JSON** con las **marcas de calidad agroalimentarias** de la región (`garant.json`).

---

## 📌 Flujo de trabajo con vanilla JS

1. **Llamar a `fetch`** indicando la URL del recurso.
2. **Esperar la respuesta** y comprobar que sea correcta (`res.ok`).
3. **Convertir el contenido** a texto (`.text()`) o a objeto (`.json()`).
4. **Usar el resultado**: mostrarlo en consola o recorrerlo.
5. **Gestionar errores** con `.catch()` o `try/catch`.

---

## 🧩 Código paso a paso

### 1) Cargar un archivo de texto (fotogramas aéreos)

```js
async function cargarTXT() {
  try {
    let res = await fetch("https://ftp.itacyl.es/cartografia/03_FotogramasAereos/Vuelo-Americano_1956-57/Leeme.txt");
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    let texto = await res.text();
    console.log("Contenido del TXT:", texto.slice(0, 300) + "...");
  } catch (err) {
    console.error("Error al cargar el TXT:", err);
  }
}

cargarTXT();
```

Aquí usamos `.text()` porque el archivo es un **documento de texto plano**.
Mostramos solo los **primeros 300 caracteres** para no llenar la consola.

---

### 2) Cargar un archivo JSON (marcas de calidad)

```js
async function cargarJSON() {
  try {
    let res = await fetch("https://servicios.itacyl.es/resources/public/opendata/garant.json");
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    let datos = await res.json();
    console.log("Objeto JSON completo:", datos);
  } catch (err) {
    console.error("Error al cargar el JSON:", err);
  }
}

cargarJSON();
```

Aquí usamos `.json()` porque el archivo contiene datos en formato JSON.
El resultado ya es un **objeto JavaScript** que podemos recorrer.

---

### 3) Recorrer el JSON y mostrar datos concretos

```js
async function mostrarMarcas() {
  let res = await fetch("https://servicios.itacyl.es/resources/public/opendata/garant.json");
  let datos = await res.json();

  datos.slice(0, 5).forEach(marca => {
    console.log(`🥇 ${marca.nombre} (${marca.tipo})`);
  });
}

mostrarMarcas();
```

Esto imprime en consola las 5 primeras marcas de calidad con su nombre y tipo.

---

!!! tip "Consejo"
Cuando trabajes con `fetch`:
\* Usa `await res.text()` para ficheros planos como `.txt` o `.csv`.
\* Usa `await res.json()` solo si el archivo realmente es JSON.
\* Comprueba siempre `res.ok` para detectar errores como `404 Not Found`.
\* Muestra solo una parte con `.slice()` si el dataset es muy grande.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué diferencia hay entre usar `.text()` y `.json()`?
    2. ¿Qué significa `res.ok` y por qué es importante comprobarlo?
    3. ¿Qué ocurre si intentamos usar `.json()` en un TXT?
    4. ¿Cómo mostrarías solo las primeras 10 líneas de un TXT cargado con `fetch`?
    5. ¿Por qué es útil usar datasets reales (como fotogramas aéreos o marcas de calidad) en vez de ejemplos inventados?