# 8.1. Visualización en mapas (Datos Abiertos JCyL)

Además de tablas y gráficos, muchos datasets del portal de **Datos Abiertos de la Junta de Castilla y León** pueden representarse en **mapas web**.
Esto es especialmente útil cuando los datos incluyen una **referencia geográfica**: coordenadas, direcciones o municipios.

---

## 📌 ¿Por qué mapas web?

Los mapas son una de las formas más potentes de **visualizar y explorar información** porque:

* Permiten **situar** los datos en un territorio real.
* Facilitan descubrir **patrones espaciales** (concentraciones, dispersión, áreas de influencia).
* Hacen más intuitiva la relación entre **datos y geografía**.
* Son interactivos: permiten **zoom, desplazamiento y clics en los puntos**.

En datos abiertos, se usan mucho para representar **infraestructuras, monumentos, servicios públicos, eventos o equipamientos educativos**.

---

## 📌 Requisitos de un dataset para mapas

Para poder usar un dataset en mapas web, este debe contener:

1. **Coordenadas** en formato decimal (latitud y longitud).

   * Ejemplo: `40.6565, -4.7003`.
   * A veces vienen en un único campo (`lat#lon`), que habrá que dividir.

2. **Identificación clara del punto** (nombre, dirección, código).

3. (Opcional) **Categoría o tipo** del lugar.

   * Ejemplo: en monumentos: “Catedral”, “Castillo”, “Museo”.
   * En centros docentes: “IES”, “CEIP”, “FP”.

4. (Opcional) **Información adicional** para mostrar en un popup.

   * Descripción, enlace web, teléfono, etc.

---

## 📌 Ejemplo de dataset válido

```csv
Nombre,Lat,Lon
Catedral de León,42.5991,-5.5718
Murallas de Ávila,40.6565,-4.7003
Plaza Mayor de Salamanca,40.9650,-5.6635
```

Con estas tres columnas ya podemos situar puntos en un mapa.

---

## 📌 Herramientas habituales

* **Vanilla JS (básico)**:

  * Insertar un mapa estático de Google Maps o OpenStreetMap con `<iframe>`.
  * Solo útil para mostrar un punto o dirección.

* **Leaflet (librería ligera)**:

  * Permite cargar múltiples puntos.
  * Soporta popups, iconos personalizados y capas.
  * Es libre, gratuito y muy usado en proyectos educativos.

* **Otras librerías**:

  * **Mapbox GL JS** (alta calidad visual, requiere API key).
  * **Google Maps JS API** (muy popular, pero con limitaciones de uso gratuito).

---

## 📌 Ventajas de representar en mapas

* **Intuitivo**: cualquiera entiende una localización en un mapa.
* **Exploración libre**: el usuario puede desplazarse, ampliar y descubrir.
* **Contexto espacial**: no solo ves el dato, también el entorno.
* **Interactividad**: popups, capas, iconos… enriquecen la experiencia.

---

## 📌 Limitaciones y retos

* Requieren **coordenadas fiables**.
* Si el dataset es muy grande (miles de puntos), el mapa puede volverse lento.
* En móviles, los mapas interactivos consumen más recursos.
* Es necesario **ajustar zoom y centro inicial** para no mostrar un mapa vacío.

---

!!! tip "Consejo"
    Antes de crear un mapa, revisa:

    * Que todas las coordenadas estén en formato decimal.
    * Que no falten valores en la latitud o longitud.
    * Que el rango de valores sea válido: latitudes entre -90 y 90, longitudes entre -180 y 180.
    * Qué información extra puedes incluir en los popups para que el mapa sea más útil.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué campos son imprescindibles en un dataset para representarlo en un mapa?
    2. ¿Qué ventajas tienen los mapas frente a tablas o gráficos?
    3. ¿Qué problemas pueden surgir al mostrar un dataset con miles de puntos?
    4. ¿Por qué es importante normalizar las coordenadas antes de usarlas en un mapa?
    5. ¿Qué librería libre y muy usada podemos emplear para mapas interactivos?