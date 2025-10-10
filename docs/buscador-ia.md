# 🔎 IAura: el buscador inteligente de Datos Abiertos (JCyL)

Soy IAura, tu asistente virtual que te ayuda a resolver tus dudas sobre los datos abiertos de la Junta de Castilla y León y cómo consumir los diferentes tipos de datos con JavaScript y librerías
Encuentra respuestas rápidas sobre **los apuntes del curso**, el **catálogo de conjuntos** y el **portal de Datos Abiertos de la Junta de Castilla y León**.

!!! tip "Qué puedo hacer"
    - Explicarte **cómo consumir** un dataset (CSV, XML, JSON, API).
    - **Sugerirte datasets** concretos del portal para crear proyectos.
    - Proponerte **ejemplos y demos** basados en los materiales del recurso.

---

## 📌 Haz tu consulta


  <!-- Bloque de ejemplos -->

!!! memo "Ejemplos de consultas"
      - Recomiéndame un dataset en CSV del portal JCyL para hacer una tabla filtrable.
      - ¿Cómo leo un XML con JavaScript?
      - Dame una demo sencilla para consumir un JSON de Datos Abiertos.
      - ¿Qué pasos sigo para llamar a una API con fetch y manejar errores?

---

  <!-- Área de consulta -->

<label for="prompt" style="font-weight:600; display:block; margin:.5rem 0;">Escribe tu consulta:</label>

<textarea id="prompt" rows="4"
  style="width: 100%; border-radius: 8px; border: 1px solid #ba273b; padding: .6rem;">
  ¿Qué datos abiertos hay disponibles sobre el transporte público de la Junta de Castilla y León?
</textarea>

<button id="btnEnviar" onclick="enviar()" style="background:#ba273b; color:white; border:none; padding:.6rem 1.2rem; border-radius:8px; font-size:0.8rem; cursor:pointer;">
  Enviar
</button>

<!-- Spinner accesible -->
<span id="spinner" class="spinner" hidden role="status" aria-live="polite" aria-label="Cargando">
  <span class="spinner__dot" aria-hidden="true"></span>
  Pensando… (¡necesito mi tiempo!)
</span>

<label for="prompt" style="font-weight:600; display:block; margin:.5rem 0;">Respuesta:</label>
<div id="respuesta"
  style="text-align:left; width:100%; white-space:pre-wrap; background:#fff; border:1px solid #ccc; padding:1rem; border-radius:8px;">
</div>

</div>

<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>

<script>

  const btn = document.getElementById("btnEnviar");
  const spinner = document.getElementById("spinner");
  const out = document.getElementById("respuesta");
  const textarea = document.getElementById("prompt");

  function setLoading(on) {
    btn.disabled = on;
    spinner.hidden = !on;
  }

  async function enviar() {
    setLoading(true);
    out.textContent = ""; // limpia la salida
    try {
      const prompt = textarea.value.trim();
      if (!prompt) {
        out.textContent = "Escribe una consulta primero.";
        return;
      }

      const res = await fetch("/.netlify/functions/chat", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ prompt })
      });

      if (!res.ok) {
        out.textContent = "Error: " + res.status + " (" + res.statusText + ")";
        return;
      }

    const data = await res.json();

    // Elimina cualquier bloque dentro de corchetes normales o "fullwidth"
    let cleanText = (data.text || "")
      .replace(/\[[^\]]*\]/g, "")   // elimina [... cualquier cosa ...]
      .replace(/【[^】]*】/g, "");   // elimina 【... cualquier cosa ...】

    cleanText = cleanText.replace(/\s{2,}/g, " ").trim();

    out.innerHTML = marked.parse(cleanText || "(Sin respuesta)");

    } catch (err) {
      out.textContent = "Se produjo un error de red o CORS. " + err;
    } finally {
      setLoading(false);
    }
  }


</script> 

<style>
  .spinner {
    display: inline-flex;
    align-items: center;
    gap: .5rem;
    margin-left: .75rem;
    font-weight: 600;
    color: #ba273b;
  }
  .spinner[hidden] { display: none; }
  .spinner__dot {
    width: 1rem; height: 1rem;
    border-radius: 50%;
    border: 2px solid color-mix(in srgb, #ba273b, transparent 70%);
    border-top-color: #ba273b;
    animation: spin 0.9s linear infinite;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  /* (Opcional) mejor lectura de código en la respuesta */
  pre, code {
    background: #f5f5f5; border-radius: 6px; padding: .6rem .8rem;
    display: block; white-space: pre-wrap;
    font-family: "Fira Code", ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
    font-size: .9rem; line-height: 1.45;
  }
</style>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.7.0/styles/github.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.7.0/highlight.min.js"></script>
<script>
  hljs.highlightAll();
</script>
