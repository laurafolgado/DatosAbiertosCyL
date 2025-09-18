# JavaScript - Desarrollo de Aplicaciones Web en Entorno Cliente

**👋 ¡Hola! Te doy la bienvenida al curso de JavaScript moderno**: el lenguaje fundamental para crear experiencias dinámicas en la web.

## 🎯 Objetivos del curso

- **Comprender los fundamentos** de JavaScript moderno (ES2023+)
- Dominar la **sintaxis moderna** (`let`/`const`, funciones flecha, destructuring…)
- Desarrollar **funciones, objetos y estructuras de control** con eficacia
- Aplicar **DOM y eventos** para manipular páginas web dinámicamente
- Utilizar **APIs web** como `fetch`, `localStorage`, `Geolocation`, etc.
- Escribir **código limpio, modular y mantenible**

## 📚 Nivel del curso

Este material está diseñado para estudiantes de **Desarrollo de Aplicaciones Web**, con conocimientos previos en:

- HTML5 y CSS3
- Lógica de programación básica
- Familiaridad con editores de código (por ejemplo, VS Code)

## 🧭 Estructura del curso

### 🟠 1. Conceptos básicos

- 1.0 Introducción a JavaScript
- 1.0.1 Tu primer script en JavaScript
- 1.1 Características de JavaScript
- 1.2 Tu primera aplicación
- 1.3 JavaScript en HTML
- 1.4 Entradas y salidas en JavaScript
- 1.5 Variables
- 1.6 Operadores
- 1.7 Control de flujo
  - 1.7.1 Condicionales
  - 1.7.2 Bucles básicos
  - 1.7.3 Bucles avanzados
  - 1.7.4 Break y continue
- 1.8 Funciones
  - 1.8.1 Funciones (I): Fundamentos
  - 1.8.2 Funciones (II): Uso avanzado
  - 1.8.3 Funciones (III): Parámetros y cierres
- 📝 1.9 Ejercicio Trivial


### 🟡 2. Uso de los objetos predefinidos del lenguaje JavaScript

- 2.1. Objetos nativos  
- 2.2. Objetos del navegador  
- 2.3. Gestión de temporizadores (`setTimeout`, `setInterval`)

### 🟢 3. Funciones, arrays y objetos definidos por el usuario

- 3.1. Funciones  
- 3.2. Arrays y métodos fundamentales  
- 3.3. Objetos definidos por el usuario  
- 3.4. Módulos ES (`import/export`)

### 🔵 4. Interacción con el usuario, eventos y formularios

- 4.1. Eventos (`click`, `input`, etc.)  
- 4.2. Validación de formularios  
- 4.3. Almacenamiento local: `WebStorage`

### 🟣 5. Uso del DOM

- 5.1. Recorrer elementos del DOM  
- 5.2. Editar elementos del DOM  
- 5.3. Crear elementos del DOM dinámicamente

### 🟤 6. Trabajo con datos y APIs

- 6.1. Promesas  
- 6.2. Uso de la API `fetch`

---

## 🛠️ Requisitos técnicos

Para seguir este curso necesitarás:

- Navegador moderno (Chrome, Firefox, Edge, Arc, etc.)
- Editor de código (VS Code recomendado o forks como Windsurf, Cursor, etc.)
- Extensión **Live Server** o similar para ver cambios en tiempo real
- Conocimientos básicos de terminal

## 🚀 Cómo empezar

1. Crea una carpeta de proyecto:
```bash
mkdir javascript
cd javascript
code .
```

2. Crea los archivos base:

    * `index.html`
   * `main.js`
   * `style.css`

3. Instala la extensión **Live Server** o **Five Server** en VS Code y abre el archivo `index.html` en el navegador para ver los cambios en tiempo real.

---

## 📚 Recursos adicionales

* [Documentación oficial de JavaScript (MDN)](https://developer.mozilla.org/es/docs/Web/JavaScript)
* [JavaScript.info (manual completo)](https://javascript.info/)
* [You Don’t Know JS (serie de libros)](https://github.com/getify/You-Dont-Know-JS)
* [JavaScript Patterns (por Addy Osmani)](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)
* [Eloquent JavaScript (por Marijn Haverbeke)](https://eloquentjavascript.net/)

---

## ℹ️ Sobre el curso

Este material ha sido desarrollado para el módulo de **Desarrollo de Aplicaciones Web en Entorno Cliente**, con el objetivo de proporcionar una base sólida y moderna sobre el lenguaje JavaScript y sus aplicaciones en el navegador.

**Autora: Laura Folgado Galache**



---

<div class="cta-buttons">
  <a href="/fundamentos/introduccion" class="button primary">Comenzar Curso</a>
  <a href="/proyecto/ejercicios" class="button secondary">Ver Ejercicios</a>
</div>

<style>
.cta-buttons {
  margin: 2rem 0;
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.cta-buttons .button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.75rem 1.5rem;
  border-radius: 0.5rem;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.2s ease;
}

.cta-buttons .primary {
  background-color: #f7df1e;
  color: #000;
  border: 2px solid #f7df1e;
}

.cta-buttons .primary:hover {
  background-color: #e6cb00;
  border-color: #e6cb00;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(247, 223, 30, 0.3);
}

.cta-buttons .secondary {
  background-color: transparent;
  color: #2c3e50;
  border: 2px solid #2c3e50;
}

.cta-buttons .secondary:hover {
  background-color: #f8f9fa;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(44, 62, 80, 0.1);
}

@media (max-width: 768px) {
  .cta-buttons {
    flex-direction: column;
  }

  .cta-buttons .button {
    width: 100%;
  }
}
</style>