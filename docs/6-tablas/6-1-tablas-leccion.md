# 6.1. Visualización en tablas accesibles (Datos Abiertos JCyL)

Uno de los modos más directos de **visualizar datos abiertos en la web** es mediante **tablas HTML**.
En esta lección aprenderás a mostrar un dataset en una tabla accesible, clara y adaptable, utilizando como ejemplo las **estadísticas de uso de redes sociales** publicadas en el portal de Datos Abiertos de la Junta de Castilla y León.

---

## 📌 ¿Por qué tablas?

Aunque existen gráficos y mapas muy vistosos, las **tablas** siguen siendo la herramienta fundamental para:

* **Comparar valores de forma precisa** (ver porcentajes, cifras exactas, años…).
* **Permitir la búsqueda rápida** de una fila o columna concreta.
* **Garantizar accesibilidad** a usuarios que usan lectores de pantalla.
* **Facilitar exportaciones** a CSV, Excel u otros formatos.

---

## 📌 Particularidades al trabajar con tablas en la web

1. **Accesibilidad**:

   * Usa etiquetas semánticas (`<table>`, `<thead>`, `<tbody>`, `<th>`, `<td>`).
   * Añade `scope="col"` en cabeceras y descripciones ARIA si es necesario.

2. **Tamaño de datos**:

   * Con pocos registros, una tabla simple funciona bien.
   * Con miles de filas, conviene añadir **paginación, filtros y ordenación**.

3. **Diseño responsive**:

   * En pantallas pequeñas las tablas tienden a desbordarse.
   * Se puede añadir **scroll horizontal** o transformar filas en tarjetas.

4. **Estilizado**:

   * Es importante resaltar cabeceras y alternar colores de fila para mejorar la legibilidad.

---

## 📌 Dataset de ejemplo: redes sociales

El dataset `estadisticas-de-uso-de-redes-sociales.csv` contiene información sobre el uso de distintas plataformas a lo largo de varios años.
Las columnas principales son, por ejemplo:

* **Año**
* **Red social**
* **Porcentaje de uso**

Ejemplo simplificado:

```
Año,Red,Porcentaje
2020,Facebook,74.5
2020,Instagram,68.2
2020,Twitter,42.1
```

---

## 📌 Ventajas de representar en tablas

* **Claridad**: cada dato está en su fila y columna correspondiente.
* **Precisión**: los usuarios pueden leer valores exactos sin interpretar gráficos.
* **Flexibilidad**: se pueden añadir controles de ordenación, búsqueda y filtrado.
* **Reutilización**: una vez construida la tabla, los mismos datos pueden pasarse a un gráfico o a un mapa.

---

!!! tip "Consejo"
Antes de mostrar un dataset en una tabla:
\* Elige solo las **columnas más relevantes** (evita la sobrecarga de información).
\* Ordena por una columna clave (ej. año o red social).
\* Asegúrate de que el diseño sea **accesible en móviles** y legible en dispositivos pequeños.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Por qué sigue siendo útil mostrar datos en tablas, aunque existan gráficos más atractivos?
    2. ¿Qué etiquetas semánticas son necesarias en una tabla HTML accesible?
    3. ¿Qué problemas puede tener una tabla con miles de filas y cómo podrías solucionarlos?
    4. ¿Qué ventajas ofrece la representación en tabla frente a un gráfico de barras?
    5. ¿Qué criterios usarías para decidir qué columnas mostrar en una tabla web?