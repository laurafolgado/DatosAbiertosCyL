# 5.2. Ejemplo con JavaScript puro (vanilla) – API REST

En este ejemplo aprenderás a **consultar una API REST** usando **solo JavaScript** (sin librerías).  
Trabajaremos con un recurso del portal de **Datos Abiertos de la Junta de Castilla y León**, que devuelve los datos en **JSON** al hacer una petición HTTP.

---

## 📌 Flujo de trabajo con vanilla JS

1. **Hacer la petición** con `fetch` a la URL de la API.  
2. **Convertir la respuesta a objeto** usando `.json()`.  
3. **Explorar la estructura** para identificar la lista de resultados.  
4. **Mapear cada resultado** a un objeto con las claves que interesan.  
5. **Renderizar** la información en pantalla (ej. tabla o tarjetas).  
6. **Gestionar errores** (respuesta no OK, red caída…).  

---

## 🧩 Código paso a paso

### 1) Consultar la API con `fetch`

```js
async function cargarAPI(url) {
  const res = await fetch(url);
  if (!res.ok) throw new Error(`Error HTTP ${res.status}`);
  return await res.json();
}

// Uso
cargarAPI("https://.../api/endpoint").then(data => {
  console.log("Respuesta de la API:", data);
});
````

---

### 2) Explorar la estructura y quedarnos con la lista

En muchos casos, la respuesta de la API tiene una clave como `resultados`, `items` o similar.
Ejemplo:

```js
function obtenerLista(data) {
  // Si ya es un array, lo devolvemos directamente
  if (Array.isArray(data)) return data;

  // Si viene dentro de un objeto, intentamos extraer la lista
  return data?.resultados ?? [];
}
```

---

### 3) Mapear los datos a columnas útiles

```js
function aFilasTabla(resultados) {
  return resultados.map(item => ({
    nombre: item.nombre ?? "",
    municipio: item.municipio ?? "",
    provincia: item.provincia ?? "",
    fechaInicio: item.fechaInicio ?? "",
    fechaFin: item.fechaFin ?? ""
  }));
}
```

---

### 4) Renderizar en una tabla sencilla

```js
function renderTabla(datos, columnas, destino = document.body) {
  const tabla = document.createElement("table");

  // Cabecera
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

  // Filas
  const tbody = document.createElement("tbody");
  datos.forEach(fila => {
    const tr = document.createElement("tr");
    columnas.forEach(col => {
      const td = document.createElement("td");
      td.textContent = fila[col];
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
  try {
    const data = await cargarAPI("https://.../api/endpoint");
    const lista = obtenerLista(data);
    const filas = aFilasTabla(lista);

    renderTabla(filas, ["nombre", "municipio", "provincia", "fechaInicio", "fechaFin"]);
  } catch (err) {
    console.error("Error al cargar la API:", err);
  }
})();
```

---

!!! tip "Consejo"
    No olvides comprobar la **documentación de la API** para saber:

    * La **URL base**.
    * Qué **parámetros** admite (ej. filtrar por provincia o fechas).
    * Cómo es la **estructura exacta** de la respuesta.
    * Qué **códigos de error** puede devolver.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué hace la función `cargarAPI` y por qué usamos `await res.json()`?
    2. ¿Cómo comprobamos si la respuesta viene directamente como array o dentro de un objeto?
    3. ¿Qué función convierte cada resultado en un objeto con las claves que queremos mostrar?
    4. ¿Qué ocurriría si intentamos acceder a una clave que no existe en el objeto JSON?
    5. ¿Por qué es importante capturar errores con `try/catch` al consumir una API REST?