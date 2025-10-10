# 4.1. Consumo de datos en formato JSON (Datos Abiertos JCyL)

Además de CSV y XML, muchos conjuntos del portal de **Datos Abiertos de la Junta de Castilla y León** se publican en **JSON** (*JavaScript Object Notation*).  
Este formato se ha convertido en el **estándar actual en aplicaciones web**, ya que está pensado para representar datos estructurados de forma sencilla y es totalmente compatible con JavaScript.

En esta lección veremos **qué es un JSON**, cuáles son sus **características**, qué **ventajas** ofrece y también sus **limitaciones**.  
Después, en los siguientes apartados, aprenderemos a procesarlo con **JavaScript puro** y con **librerías externas** como Axios.

---

## 📌 ¿Qué es un JSON?

JSON es un formato basado en texto que representa datos como **objetos y arrays** de JavaScript.  
Se compone de pares `clave: valor`, organizados con llaves `{}` y corchetes `[]`.

Ejemplo simplificado (basado en un dataset de instituciones bibliotecarias):

```json
{
  "institucion": {
    "nombre": "Biblioteca Pública de Burgos",
    "provincia": "Burgos",
    "servicios": ["Préstamo", "Sala de lectura", "Acceso a Internet"]
  }
}
``` 

En este ejemplo:

* `institucion` es un **objeto** con tres claves.
* `nombre` y `provincia` son **cadenas de texto**.
* `servicios` es un **array** de valores.

---

## 📌 ¿Por qué se utiliza tanto en datos abiertos?

JSON se ha popularizado en los portales de datos y en APIs por varias razones:

1. **Compatibilidad nativa con JavaScript**: no necesita transformaciones para usarlo en el navegador.
2. **Estructura clara**: permite representar tanto listas simples como datos jerárquicos.
3. **Ligereza**: ocupa menos que XML, sin etiquetas repetitivas.
4. **Legibilidad**: es texto plano y se entiende fácilmente a simple vista.
5. **Interoperabilidad**: casi todos los lenguajes modernos incluyen soporte para JSON.

En los **datos abiertos de Castilla y León** encontrarás JSON en conjuntos como el **registro de instituciones bibliotecarias** o los **monumentos**.

---

## 📌 Particularidades de los JSON en el portal de CyL

Al trabajar con JSON del portal, conviene fijarse en:

* **Objeto raíz**: a veces todo el dataset está dentro de un único objeto, otras veces es un array.
* **Listas anidadas**: por ejemplo, un monumento puede incluir varias direcciones o servicios.
* **Valores nulos o vacíos**: algunas claves pueden no tener información (`null` o `""`).
* **Codificación**: los archivos suelen estar en **UTF-8**, lo que garantiza la compatibilidad con acentos y ñ.
* **Consistencia de claves**: conviene comprobar si todas las entradas tienen las mismas claves (no siempre es así).

Ejemplo real simplificado del dataset de monumentos:

```json
{
  "monumentos": [
    { "nombre": "Muralla de Ávila", "provincia": "Ávila", "municipio": "Ávila" },
    { "nombre": "Catedral de Burgos", "provincia": "Burgos", "municipio": "Burgos" }
  ]
}
```

---

## 📌 Ventajas de usar JSON

* **Nativo en JavaScript**: se convierte fácilmente con `JSON.parse()`.
* **Estructuras jerárquicas**: admite arrays dentro de objetos y viceversa.
* **Más ligero que XML**: ocupa menos espacio en la red.
* **Formato abierto y estándar**: soportado en casi todos los lenguajes.
* **Lectura sencilla**: fácil de inspeccionar para humanos.

---

## 📌 Limitaciones y retos

* **Menos adecuado para documentos muy extensos**: XML puede ser más claro en datos con mucho texto y metadatos.
* **No incluye comentarios**: no se puede documentar dentro del propio JSON.
* **Estructura rígida**: si faltan o cambian claves, el código puede fallar.
* **Tamaño**: aunque más ligero que XML, puede crecer mucho con datasets masivos.
* **Validación externa**: no tiene un esquema obligatorio (aunque existen estándares como JSON Schema).

---

!!! tip "Consejo"
    Antes de trabajar con un JSON:
    - Abre el archivo en un editor para confirmar si el **raíz** es un objeto o un array.  
    - Verifica que todas las entradas tienen las **claves importantes** (ej. `nombre`, `provincia`).  
    - Comprueba si aparecen valores vacíos o `null` y decide cómo tratarlos.  
    - Si es muy grande, **filtra** antes de mostrar todo en la web.  
  
---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué diferencia principal hay entre un JSON y un XML?
    2. ¿Qué ventajas tiene JSON frente a CSV?
    3. ¿Qué problemas puedes encontrar si un JSON no tiene siempre las mismas claves en todos sus objetos?
    4. ¿Por qué es útil que JSON sea el formato nativo de JavaScript?
    5. ¿Qué pasos previos harías antes de procesar un JSON en tu aplicación web?

