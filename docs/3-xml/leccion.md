# 3.1. Consumo de datos en formato XML (Datos Abiertos JCyL)

Además de CSV, muchos conjuntos del portal de **Datos Abiertos de la Junta de Castilla y León** se publican en **XML** (eXtensible Markup Language).  
Este formato es muy común en administraciones públicas porque durante años ha sido un estándar para el intercambio de información estructurada.

En esta lección veremos **qué es un XML**, cuáles son sus **características**, qué **ventajas** tiene y también sus **limitaciones**.  
Después, en los siguientes apartados, aprenderemos a procesarlo con **JavaScript puro** y con **librerías externas**.

---

## 📌 ¿Qué es un XML?

XML es un **lenguaje de marcado** similar a HTML, pero diseñado para **representar y transportar datos**.  
Cada elemento se estructura con **etiquetas de apertura y cierre**, que pueden contener a su vez **otros elementos o atributos**.

Ejemplo simplificado (basado en un dataset de monumentos):

```xml
<monumento>
  <nombre>Catedral de León</nombre>
  <localidad>León</localidad>
  <categoria>Catedral</categoria>
</monumento>
```

En este ejemplo:

* `<monumento>` es el elemento principal.
* Dentro aparecen subelementos: `<nombre>`, `<localidad>`, `<categoria>`.
* El contenido de cada etiqueta son los **datos reales**.

---

## 📌 ¿Por qué se utiliza tanto en datos abiertos?

Aunque en la actualidad JSON es más popular en entornos web, el XML se sigue utilizando en muchos portales institucionales porque:

1. **Estándar internacional**: fue uno de los primeros formatos oficiales de intercambio de información.
2. **Flexibilidad**: permite representar datos jerárquicos complejos.
3. **Legibilidad**: es texto plano y se puede abrir en cualquier editor.
4. **Compatibilidad histórica**: muchas aplicaciones y sistemas antiguos todavía dependen de XML.
5. **Soporte oficial**: estándares como RSS, Atom, SOAP, etc. se basan en XML.

En los **datos abiertos de Castilla y León** encontrarás XML en conjuntos como la **agenda cultural** o los **monumentos**.

---

## 📌 Particularidades de los XML en el portal de CyL

Al trabajar con XML de datos abiertos, conviene tener en cuenta:

* **Estructura jerárquica**: los datos se organizan en nodos y subnodos.
* **Atributos**: un mismo elemento puede llevar información adicional en sus atributos:

  ```xml
  <evento id="1234" categoria="música">Concierto</evento>
  ```
* **Espacios de nombres (namespaces)**: a veces aparecen prefijos como `<dc:title>`; indican el estándar de metadatos usado.
* **Codificación**: suelen estar en **UTF-8**, pero es importante comprobarlo en la cabecera:

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  ```
* **Tamaño**: algunos XML pueden ser muy extensos; procesarlos en el navegador puede requerir técnicas de filtrado.
* **Formato más “verbo”** que JSON\*\*: los datos ocupan más espacio debido a las etiquetas.

---

## 📌 Ventajas de usar XML

* **Estructura jerárquica clara**: ideal para datos que tienen subniveles o agrupaciones.
* **Estándar maduro**: existen muchas herramientas de validación y transformación (ej. XSLT).
* **Interoperabilidad**: todavía se usa en múltiples ámbitos (bibliotecas, archivos, catálogos culturales).
* **Legible por humanos**: aunque más pesado que JSON, sigue siendo texto entendible.
* **Metadatos ricos**: se pueden incluir atributos adicionales y esquemas para validar la información.

---

## 📌 Limitaciones y retos

* **Verboso**: mucho más pesado que CSV o JSON para representar los mismos datos.
* **Parseo en JavaScript más complejo**: requiere convertir la cadena en un árbol DOM y recorrerlo.
* **Namespaces**: pueden complicar la consulta de nodos.
* **Rendimiento**: con archivos muy grandes, el procesamiento puede ser lento en el navegador.
* **Menos directo para la web**: JSON encaja mejor de forma nativa con JavaScript.

---

!!! tip "Consejo"
    Antes de programar, abre el XML en un editor o navegador y analiza:

    - Cuál es el **elemento raíz**.
    - Cómo se llaman los **nodos principales** que contienen los datos (ej. `<evento>`, `<monumento>`).
    - Qué **atributos** llevan y si son importantes para tu aplicación.
    - Si existen **espacios de nombres (namespaces)** que debas tener en cuenta al consultar.

    Este análisis previo te ahorrará tiempo cuando empieces a escribir consultas con `querySelectorAll` o librerías de parseo.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué diferencia hay entre XML y HTML?
    2. ¿Qué ventajas tiene XML frente a CSV?
    3. ¿Qué problemas puede darte un XML muy grande en la web?
    4. ¿Qué son los atributos en un elemento XML? Pon un ejemplo.
    5. ¿Qué pasos previos harías antes de procesar un XML con JavaScript?
