# 5.1. Consumo de datos a través de una API REST (Datos Abiertos JCyL)

Además de CSV, XML o JSON estáticos, muchos conjuntos de datos del portal de **Datos Abiertos de la Junta de Castilla y León** están disponibles mediante **APIs REST**.  
Esto significa que podemos consultar la información en tiempo real, con filtros y parámetros, y recibir la respuesta en formato JSON o XML.

En esta lección veremos **qué es una API REST**, cómo funcionan en el contexto de los datos abiertos y qué ventajas y limitaciones presentan.  
Después aprenderemos a consumirlas con **JavaScript puro** y con librerías externas.

---

## 📌 ¿Qué es una API REST?

Una **API REST** (Application Programming Interface – Representational State Transfer) es un servicio web que expone datos y operaciones a través del protocolo **HTTP**.  
En lugar de descargar un archivo fijo (CSV, XML, JSON), hacemos una **solicitud a una URL** con ciertos parámetros, y la API nos devuelve una **respuesta dinámica**.

Ejemplo genérico de consulta:

```

[https://servidor.datosabiertos.jcyl.es/servicio/recurso?param1=valor1\&param2=valor2](https://servidor.datosabiertos.jcyl.es/servicio/recurso?param1=valor1&param2=valor2)

````

---

## 📌 ¿Cómo funcionan las APIs REST en los datos abiertos?

* Se accede mediante una **URL base** publicada en el portal.  
* Se pueden añadir **parámetros en la URL** (después de `?`) para filtrar la información (ej. por fecha, municipio, categoría…).  
* La respuesta suele ser en **JSON** (fácil de usar con JavaScript) o en **XML** (más común en servicios antiguos).  
* Se basan en **métodos HTTP**:
  - `GET` → obtener datos.  
  - `POST`, `PUT`, `DELETE` → modificar (menos habitual en datos abiertos públicos).

---

## 📌 Ejemplo de respuesta

Una petición correcta suele devolver un objeto en JSON con una lista de elementos.  
Ejemplo simplificado:

```json
{
  "correcto": true,
  "mensaje": "Lista de actividades enviada correctamente",
  "resultados": [
    {
      "nombre": "Curso de iniciación a la informática",
      "fechaInicio": "2025-03-01",
      "fechaFin": "2025-03-05",
      "municipio": "León",
      "provincia": "León",
      "horas": 20,
      "plazas": 15
    }
  ]
}
````

---

## 📌 Ventajas de usar APIs REST

* **Datos en tiempo real**: siempre recibimos la versión más reciente.
* **Filtros y parámetros**: podemos pedir solo lo que necesitamos (ej. actividades en una provincia concreta).
* **Formato flexible**: normalmente JSON, ideal para JavaScript.
* **Escalabilidad**: integrable en aplicaciones web, móviles o dashboards dinámicos.

---

## 📌 Limitaciones y retos

* **Dependencia del servidor**: si la API falla, no podemos acceder a los datos.
* **Necesidad de conexión a internet**: a diferencia de un CSV descargado, no funciona offline.
* **Conocimiento técnico**: hay que entender cómo construir las URLs y parámetros.
* **Políticas de uso**: algunas APIs limitan el número de peticiones o requieren autenticación.

---

!!! tip "Consejo"
Antes de consumir una API, revisa siempre:
\- La **documentación oficial** del recurso.
\- Los **parámetros disponibles** para filtrar datos.
\- El **formato de respuesta** (JSON/XML).
\- Los **códigos de error** que puede devolver el servidor.

---

## 📝 Preguntas de repaso

!!! question "Repaso"
    1. ¿Qué diferencia hay entre descargar un CSV y consultar una API REST?
    2. ¿Qué parámetros se suelen añadir a una URL para filtrar la información?
    3. ¿Qué método HTTP se usa habitualmente para obtener datos de una API abierta?
    4. ¿Qué problemas pueden surgir si el servidor de la API no responde?
    5. ¿Qué ventajas tiene usar una API en vez de un archivo estático para datos que cambian con frecuencia?