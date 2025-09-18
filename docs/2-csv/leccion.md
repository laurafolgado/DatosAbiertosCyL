# 2.1. Consumo de datos en formato CSV (Datos Abiertos JCyL)

El portal de **Datos Abiertos de la Junta de Castilla y León** publica numerosos conjuntos en formato **CSV**.  
Este formato es uno de los más sencillos y extendidos, y por eso se convierte en un punto de partida ideal para aprender a consumir datos en la web.

En esta lección vamos a detenernos en **qué es un CSV**, cuáles son sus **características principales**, qué **ventajas** ofrece, qué **problemas** puede plantear y cómo lo utilizaremos en nuestras aplicaciones.  
En los siguientes apartados de la unidad veremos cómo procesarlo con JavaScript puro y con librerías externas, pero ahora nos centraremos en **comprender bien el formato**.

---

## 📌 ¿Qué es un CSV?

CSV significa *Comma Separated Values* (valores separados por comas).  
Un archivo CSV es, en esencia, **una tabla guardada como texto plano**:  
- Cada **línea** corresponde a un registro (una fila de la tabla).  
- Los **valores de cada columna** se separan con un carácter delimitador, normalmente una coma `,`.  

Ejemplo simplificado:

```

Nombre,Localidad,Telefono
Bar La Plaza,Salamanca,923123456
Café Sol,Valladolid,983654321

```

Aquí vemos tres registros: la primera línea son los **nombres de las columnas** (Nombre, Localidad, Teléfono) y las siguientes son los **datos reales**.

---

## 📌 ¿Por qué se utiliza tanto en datos abiertos?

El CSV se ha convertido en un estándar de facto en la publicación de datos por varias razones:

1. **Simplicidad**: es texto plano, lo que significa que cualquier editor puede abrirlo (desde el Bloc de notas hasta Excel).  
2. **Compatibilidad**: casi todas las aplicaciones de hojas de cálculo y bases de datos aceptan CSV.  
3. **Ligereza**: ocupa poco espacio en comparación con otros formatos más complejos como XML o JSON.  
4. **Transparencia**: al ser texto, cualquier persona puede inspeccionarlo y comprobar rápidamente qué contiene.  
5. **Interoperabilidad**: puede ser leído desde programas escritos en casi cualquier lenguaje de programación.

En los **datos abiertos de Castilla y León** encontrarás CSV de temáticas muy diversas: desde listados de hostelería a domicilio (que usaremos aquí) hasta catálogos de monumentos, transporte, educación o medio ambiente.

---

## 📌 Particularidades de los CSV en el portal de CyL

Aunque la idea es sencilla, los CSV de datos abiertos presentan algunos matices importantes que conviene conocer:

* **Separador**: aunque el nombre dice *coma*, en España es habitual usar **punto y coma (`;`)** como separador, porque la coma se utiliza para decimales.  
* **Codificación**: los archivos suelen estar en **UTF-8**, pero no siempre; si ves símbolos raros en las tildes o la ñ, puede ser un problema de codificación.  
* **Cabeceras**: la primera línea incluye los nombres de las columnas, lo cual es muy útil para automatizar el parseo.  
* **Valores especiales**: a veces hay celdas vacías o con valores como `NULL`, que deberás interpretar correctamente.  
* **Comillas**: cuando una celda contiene comas o saltos de línea, ese campo aparece entrecomillado, por ejemplo:  
```

"Bar, Cafetería y Restaurante",Salamanca,923123456

```
* **Volumen de datos**: algunos CSV tienen miles de registros; cargarlos enteros en una tabla web puede ralentizar la aplicación.

---

## 📌 Ventajas de usar CSV

* **Acceso universal**: cualquier persona puede abrir un CSV sin necesidad de programas especializados.  
* **Formato abierto**: no está ligado a una aplicación propietaria.  
* **Facilidad de análisis**: se integra sin esfuerzo con hojas de cálculo y bases de datos.  
* **Rapidez en la web**: leer un archivo de texto es más rápido y ligero que otros formatos complejos.  
* **Consistencia**: casi todos los datasets abiertos en España incluyen al menos una versión en CSV, lo que garantiza que lo encontrarás en la mayoría de proyectos.

---

## 📌 Limitaciones y retos

Sin embargo, no todo son ventajas:

* Un CSV **no guarda jerarquía**: no podemos representar estructuras complejas (por ejemplo, un monumento con varias fotos y varias direcciones).  
* El **manejo de comillas y saltos de línea** puede complicar el parseo manual.  
* Si hay **miles de filas**, la visualización directa en el navegador puede ser lenta.  
* Es fácil que los **nombres de las columnas cambien** entre versiones, lo que rompe el código que las consume.

Por eso, aunque empezaremos con CSV, más adelante veremos cómo trabajar con **JSON, XML y APIs REST**, que resuelven algunos de estos problemas.

---

!!! tip "Consejo"
    Antes de empezar a programar, **abre siempre el CSV en un editor**:  
    - Comprueba qué separador usa (`;` o `,`).  
    - Verifica que los acentos y eñes se vean bien (codificación UTF-8).  
    - Identifica cuáles son las **columnas clave** (nombre, municipio, teléfono…).  
    - Revisa si hay valores vacíos o inconsistentes.  

    Este análisis previo evita muchos errores al procesar los datos después.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. Explica con tus palabras qué es un archivo CSV.  
    2. ¿Por qué es tan habitual en los portales de datos abiertos como el de Castilla y León?  
    3. ¿Qué diferencias hay entre un CSV y un JSON?  
    4. ¿Qué problemas prácticos pueden encontrarse al procesar un CSV real?  
    5. ¿Qué pasos seguirías para mostrar en una página web una tabla con datos de un CSV?
