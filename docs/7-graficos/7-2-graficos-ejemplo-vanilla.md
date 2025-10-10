# 7.2. Ejemplo con JavaScript puro (vanilla) – Gráfico

En este ejemplo vamos a crear un gráfico de **barras** usando únicamente la API de **Canvas** de HTML5, sin librerías externas.
El dataset será el de **estadísticas de uso de redes sociales** de la Junta de Castilla y León.

---

## 📌 Flujo de trabajo con vanilla JS

1. **Cargar el CSV** con `fetch`.
2. **Parsearlo a objetos** (como en los ejemplos anteriores).
3. **Filtrar los datos** de un año concreto.
4. **Configurar las dimensiones del canvas**.
5. **Calcular escalas** para representar porcentajes como alturas de barras.
6. **Dibujar ejes, etiquetas y barras** con la API `CanvasRenderingContext2D`.

---

## 🧩 Código paso a paso

### 1) HTML base

```html
<canvas id="grafico" width="600" height="400" aria-label="Gráfico de uso de redes sociales" role="img"></canvas>
```

---

### 2) Cargar y parsear el CSV

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

### 3) Filtrar por año (ej. 2020)

```js
function filtrarPorAno(datos, ano = "2020") {
  return datos.filter(d => d.Año === ano);
}
```

---

### 4) Dibujar gráfico en canvas

```js
function dibujarGrafico(ctx, datos) {
  const ancho = ctx.canvas.width;
  const alto = ctx.canvas.height;

  ctx.clearRect(0, 0, ancho, alto);

  const margen = 50;
  const anchoBarra = (ancho - 2 * margen) / datos.length;
  const maxValor = Math.max(...datos.map(d => parseFloat(d.Porcentaje)));

  // Eje Y
  ctx.beginPath();
  ctx.moveTo(margen, margen);
  ctx.lineTo(margen, alto - margen);
  ctx.stroke();

  // Eje X
  ctx.beginPath();
  ctx.moveTo(margen, alto - margen);
  ctx.lineTo(ancho - margen, alto - margen);
  ctx.stroke();

  // Dibujar barras
  datos.forEach((d, i) => {
    const valor = parseFloat(d.Porcentaje);
    const altura = (valor / maxValor) * (alto - 2 * margen);

    const x = margen + i * anchoBarra;
    const y = alto - margen - altura;

    ctx.fillStyle = "#ba273b"; // rojo corporativo JCyL
    ctx.fillRect(x, y, anchoBarra * 0.6, altura);

    // Etiqueta X
    ctx.fillStyle = "black";
    ctx.font = "12px sans-serif";
    ctx.fillText(d.Red, x, alto - margen + 15);

    // Etiqueta valor
    ctx.fillText(valor + "%", x, y - 5);
  });
}
```

---

### 5) Ponerlo todo junto

```js
(async () => {
  const datos = await cargarCSV();
  const datos2020 = filtrarPorAno(datos, "2020");

  const canvas = document.getElementById("grafico");
  const ctx = canvas.getContext("2d");

  dibujarGrafico(ctx, datos2020);
})();
```

---

!!! tip "Consejo"
Este enfoque con **Canvas** te ayuda a comprender cómo se dibujan gráficos “desde cero”.
Sin embargo, si quieres añadir leyendas, escalas automáticas o varios tipos de gráficos, es más práctico usar una librería como **Chart.js**.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
1\. ¿Qué representa el valor `maxValor` en el código del gráfico?
2\. ¿Por qué normalizamos las alturas de las barras en función del valor máximo?
3\. ¿Qué función del canvas usamos para dibujar rectángulos (barras)?
4\. ¿Cómo cambiarías el código para representar el año 2021 en lugar de 2020?
5\. ¿Qué limitaciones tiene este enfoque frente a una librería especializada en gráficos?