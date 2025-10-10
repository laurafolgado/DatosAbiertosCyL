# 7.3. Ejemplo con librería – Chart.js

En este ejemplo vamos a crear un gráfico de **barras** con la librería **Chart.js** a partir del dataset de **estadísticas de uso de redes sociales**.
Con Chart.js podemos generar gráficos de manera sencilla, con estilos atractivos y funcionalidades adicionales como **leyendas, tooltips y escalas automáticas**.

---

## 📦 Cómo incluir la librería

Incluimos Chart.js directamente desde un **CDN** en el HTML:

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<script type="module" src="main.js"></script>
```

Y añadimos un `<canvas>` para el gráfico:

```html
<canvas id="graficoRedes" width="600" height="400"></canvas>
```

---

## 📌 Flujo de trabajo con Chart.js

1. **Incluir Chart.js** desde CDN.
2. **Cargar el dataset CSV** con `fetch`.
3. **Parsear el contenido a objetos**.
4. **Filtrar por año** (ej. 2020).
5. **Preparar arrays de etiquetas y valores**.
6. **Configurar el gráfico** con `new Chart()`.
7. **Mostrar el gráfico** en el canvas.

---

## 🧩 Código paso a paso

### 1) Cargar y parsear el CSV

```js
async function cargarCSV() {
  const res = await fetch("datasets/csv/estadisticas-de-uso-de-redes-sociales.csv");
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

### 2) Filtrar datos por año

```js
function filtrarPorAno(datos, ano = "2020") {
  return datos.filter(d => d.Año === ano);
}
```

---

### 3) Preparar arrays para Chart.js

```js
function prepararDatos(datos) {
  const etiquetas = datos.map(d => d.Red);
  const valores = datos.map(d => parseFloat(d.Porcentaje));
  return { etiquetas, valores };
}
```

---

### 4) Crear el gráfico

```js
async function crearGrafico() {
  const datos = await cargarCSV();
  const datos2020 = filtrarPorAno(datos, "2020");
  const { etiquetas, valores } = prepararDatos(datos2020);

  const ctx = document.getElementById("graficoRedes").getContext("2d");

  new Chart(ctx, {
    type: "bar",
    data: {
      labels: etiquetas,
      datasets: [{
        label: "Porcentaje de uso (%)",
        data: valores,
        backgroundColor: "#ba273b"
      }]
    },
    options: {
      responsive: true,
      plugins: {
        legend: { display: true },
        tooltip: { enabled: true }
      },
      scales: {
        y: { beginAtZero: true, title: { display: true, text: "Porcentaje" } },
        x: { title: { display: true, text: "Red social" } }
      }
    }
  });
}

crearGrafico();
```

---

!!! tip "Consejo"
    Chart.js soporta muchos tipos de gráficos:

    * `bar` (barras)
    * `line` (líneas)
    * `pie` (tarta)
    * `doughnut` (anillo)
    * `radar`, `polarArea`, etc.

    Puedes cambiar `type: "bar"` por otro para experimentar.  

---

## ⚖️ Comparación con el enfoque *vanilla*

| Aspecto          | Canvas (vanilla)                  | Chart.js                                    |
| ---------------- | --------------------------------- | ------------------------------------------- |
| Dibujo básico    | A mano con `fillRect`, `fillText` | Automático con `datasets` y `labels`        |
| Escalas          | Manual (cálculo de proporciones)  | Automáticas y configurables                 |
| Estilos          | Manual (colores, fuentes)         | Declarativos (`backgroundColor`, `border…`) |
| Interactividad   | Hay que programarla               | Incluida (tooltips, leyenda, hover)         |
| Tipos de gráfico | Solo lo que programes             | Múltiples tipos listos para usar            |
| Curva de uso     | Baja pero requiere más trabajo    | Muy baja: gráfico listo en pocas líneas     |

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué diferencia principal hay entre el gráfico en canvas (vanilla) y el de Chart.js?
    2. ¿Qué arrays preparamos para pasarlos al gráfico en Chart.js?
    3. ¿Qué propiedad permite indicar el tipo de gráfico (`bar`, `line`, `pie`…)?
    4. ¿Cómo activarías una etiqueta en el eje X que muestre “Red social”?
    5. ¿Qué otro tipo de gráfico probarías con este dataset y por qué?