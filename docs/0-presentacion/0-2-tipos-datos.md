# 0.2. Tipos de datos abiertos y formas de acceso

En el portal de **Datos Abiertos de la Junta de Castilla y León** no todos los datos se presentan de la misma manera.
Según el **formato en el que estén publicados**, necesitaremos estrategias distintas para acceder, interpretar y utilizarlos en nuestras aplicaciones.

Los formatos más habituales son: **CSV, JSON, XML y APIs REST**. Cada uno tiene sus características, ventajas y retos.

---

## 📌 Archivos CSV (Comma Separated Values)

* **Cómo son**: archivos de texto donde cada línea representa un registro y cada columna está separada por comas (`,`), aunque a veces se usa el punto y coma (`;`).
* **Ventajas**:
      * Muy sencillos de entender.
      * Compatibles con Excel, Google Sheets y cualquier editor de texto.
      * Ideales para información tabular (listados, estadísticas, directorios).
* **Retos**:
      * No incluyen estructura jerárquica: solo filas y columnas.
      * A veces es difícil manejar campos con comillas o saltos de línea.
      * Si el dataset es grande, puede volverse pesado de procesar en el navegador.

---

## 📌 Archivos JSON (JavaScript Object Notation)

* **Cómo son**: texto estructurado con **objetos y arrays** anidados, muy usado en aplicaciones web.
* **Ventajas**:
    * Es el formato “nativo” de JavaScript: fácil de convertir en objetos del lenguaje.
    * Permite estructuras complejas (listas dentro de listas, objetos dentro de objetos).
    * Ampliamente utilizado en APIs modernas.
* **Retos**:
    * No es tan cómodo para abrir en Excel o editores de texto no técnicos.
    * Puede ser más difícil de leer a simple vista que un CSV.

---

## 📌 Archivos XML (eXtensible Markup Language)

* **Cómo son**: archivos de texto con etiquetas anidadas, muy parecidos al HTML.
* **Ventajas**:
      * Soporta estructuras jerárquicas complejas.
      * Estándar muy extendido en la administración pública y organismos internacionales.
      * Puede incluir metadatos y atributos junto con el contenido.
* **Retos**:
      * Es más verboso y pesado que JSON.
      * Requiere usar parsers para convertirlo a estructuras manejables en JavaScript.
      * Puede incluir espacios de nombres (namespaces) que complican el acceso a los datos.

---

## 📌 APIs REST (servicios de datos en línea)

* **Cómo son**: interfaces web que permiten consultar los datos directamente desde los servidores de la administración.
* **Ventajas**:
    * Siempre devuelven la información **actualizada en tiempo real**.
    * Permiten filtrar, paginar o buscar datos concretos con parámetros en la URL.
    * Normalmente devuelven datos en JSON o XML.
* **Retos**:
      * Requieren conexión a Internet en el momento de la consulta.
      * A veces necesitan cabeceras específicas o autenticación con tokens.
      * Si la API cambia su formato, el código de nuestra aplicación debe adaptarse.

---

!!! tip "Consejo"
    Como desarrolladoras y desarrolladores, es importante elegir **el formato adecuado para cada caso de uso**:

    * Si necesitas **listas simples** → CSV.  
    * Si quieres **estructuras jerárquicas y directas para JavaScript** → JSON.  
    * Si trabajas con **fuentes institucionales antiguas** → XML.  
    * Si necesitas **datos actualizados al momento y filtrables** → API REST.  


---

## 📝 Preguntas de reflexión

!!! question "Repaso"
    1. ¿Qué diferencias fundamentales hay entre CSV y JSON?
    2. ¿Qué ventajas tiene usar JSON frente a XML en aplicaciones web modernas?
    3. ¿Por qué una API puede ser más útil que descargar un archivo estático?
    4. ¿Qué problemas podrían surgir al trabajar con un CSV de gran tamaño en el navegador?
    5. Si quisieras mostrar un mapa con ubicaciones de monumentos, ¿qué formato te resultaría más práctico y por qué?