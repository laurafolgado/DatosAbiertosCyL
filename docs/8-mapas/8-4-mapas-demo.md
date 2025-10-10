# 8.4. Demo: Centros docentes en mapa (CSV)

Ya hemos visto cómo representar un dataset geográfico en mapas, tanto con **un enfoque básico (vanilla + iframe)** como con una **librería avanzada (Leaflet)**.
Es momento de ver el resultado final en páginas reales.

En esta demo utilizamos el dataset de **directorio de centros docentes de Castilla y León** en formato CSV.
El sistema carga las coordenadas de los centros y los sitúa en un **mapa interactivo**, permitiendo explorar los datos de manera intuitiva.

---

## 📌 Qué encontrarás en las demos

* **Carga real del dataset** `directorio-de-centros-docentes.csv`.
* Dos enfoques:

    * **Vanilla (iframe OSM)**: muestra un solo centro en un mapa básico.
    * **Librería (Leaflet)**: coloca todos los centros en un mapa interactivo con popups.

* Posibilidad de **navegar, hacer zoom y clicar en cada centro** para ver más información.
* Uso de datos abiertos con **contexto geográfico real**.

---

!!! tip "Consejo"
    Recuerda abrir el proyecto con un **servidor local** (`mkdocs serve`, Live Server, etc.).
    Si intentas abrirlo directamente con `file://`, el navegador puede bloquear la carga del CSV por motivos de seguridad.
    Verifica también en DevTools → Network que el archivo se carga desde `datasets/csv/directorio-de-centros-docentes.csv`.

---

## 📌 Acceso a las demos

### ▶️ Demo con librería (Leaflet)

[**Abrir demo en vivo**](){ target=\_blank }

También puedes verla incrustada aquí:

<iframe src="" width="100%" height="500" loading="lazy"></iframe>

---

### ▶️ Demo con JavaScript puro (vanilla)

[**Abrir demo en vivo (vanilla)**](){ target=\_blank }

También puedes verla incrustada aquí:

<iframe src="" width="100%" height="400" loading="lazy"></iframe>

---

## 📝 Preguntas de reflexión

!!! question "Repaso"
    1. ¿Qué diferencia principal hay entre el mapa con **iframe (vanilla)** y el interactivo con **Leaflet**?
    2. ¿Qué campos del CSV (`Lat`, `Lon`, `Nombre`, `Municipio`) son imprescindibles para situar los centros en el mapa?
    3. ¿Cómo gestionaría Leaflet un dataset con miles de centros para no saturar el mapa?
    4. ¿Qué ventajas aporta un mapa interactivo para explorar datos abiertos frente a una tabla de coordenadas?
    5. ¿Qué usos educativos se te ocurren para un mapa de centros docentes?