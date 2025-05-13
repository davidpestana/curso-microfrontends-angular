## 🧪 Fase 2: Validación previa — Integración de MFEs en el Shell

### 🌟 Objetivo

Antes de migrar ninguna feature, el alumno debe experimentar **cómo se integra una aplicación Angular modular clásica como un Microfrontend** en el shell, y contrastarlo con un MFE standalone. Esta validación permite entender las fricciones reales de la arquitectura tradicional, y justificar el esfuerzo de migración.

---

### 🧾 Justificación técnica

En este laboratorio usamos `import()` en lugar de `routerLink` porque:

* El propósito es **simular la carga dinámica real** de un MFE remoto, como ocurre con Webpack Module Federation, Web Components o `import()` puro.
* En un entorno real de MFEs, cada microfrontend es una app independiente, con su propio enrutamiento y ciclo de vida.
* `routerLink` sería válido solo si todos los MFEs estuvieran compilados juntos en una única SPA, lo que contradice la filosofía de desacoplamiento.
* Aquí, el Shell actúa como **orquestador**, no como router global.

Esto refuerza la comprensión del alumno sobre el **desacoplamiento, los límites del router Angular, y la transición hacia arquitecturas modulares distribuidas.**

---

### 📜 Descripción

El shell integrará 3 tipos de MFEs:

1. Una aplicación Angular **completamente modular** (ej: `mfe-modular/`)
2. Una aplicación Angular **standalone desde su creación** (ej: `mfe-standalone/`)
3. Una aplicación **modular refactorizada a standalone** (ej: `mfe-refactored/`)

El objetivo es integrarlas usando `import()` como simulacro de carga remota (previo a usar Module Federation), comparar el esfuerzo requerido, dependencias compartidas y resultado final.

---

### 🧍‍♂️ Pasos detallados

#### 🧱 Paso 1: Crear el entorno base

```bash
mkdir mfe-shell mfe-modular mfe-standalone mfe-refactored
```

#### 🚀 Paso 2: Crear cada aplicación Angular

```bash
# Shell (puede ser standalone o modular)
cd mfe-shell
ng new mfe-shell --routing --style=scss --no-standalone

# App modular
cd ../mfe-modular
ng new mfe-modular --routing --style=scss --no-standalone

# App standalone
cd ../mfe-standalone
ng new mfe-standalone --routing --style=scss --standalone

# App refactorizada
cd ../mfe-refactored
ng new mfe-refactored --routing --style=scss --no-standalone
```

#### 🧩 Paso 3: Crear un `DashboardComponent` en cada app

```bash
# Modular:
cd mfe-modular
ng generate module dashboard --route dashboard --module app.module
ng generate component dashboard/pages/dashboard

# Standalone:
cd ../mfe-standalone
ng generate component dashboard/pages/dashboard --standalone

# Refactorizada:
cd ../mfe-refactored
ng generate module dashboard --route dashboard --module app.module
ng generate component dashboard/pages/dashboard
```

En cada `dashboard.component.html`, añade contenido distinto:

```html
<h2>Dashboard de MFE Modular</h2> <!-- o Standalone / Refactorizada -->
```

#### 🧪 Paso 4: Construir las 3 apps

```bash
ng build --base-href /mfe-modular/    # Repetir para cada MFE
```

#### 🧠 Paso 5: Preparar `mfe-shell` para simular integración

1. En `mfe-shell/src/app`, crear un archivo `remote-loader.service.ts` que use `import()`:

```ts
export async function loadRemoteComponent(container: HTMLElement, mfePath: string, exportFn: string) {
  const module = await import(mfePath);
  if (module[exportFn]) module[exportFn](container);
}
```

2. En `shell.component.ts`, añadir tres botones:

```html
<button (click)="load('modular')">Cargar Modular</button>
<button (click)="load('standalone')">Cargar Standalone</button>
<button (click)="load('refactored')">Cargar Refactorizada</button>
<div id="mfe-container"></div>
```

3. Simular carga de scripts (temporal):

```ts
load(type: string) {
  const paths = {
    modular: '/mfe-modular/main.js',
    standalone: '/mfe-standalone/main.js',
    refactored: '/mfe-refactored/main.js'
  };
  loadRemoteComponent(document.getElementById('mfe-container')!, paths[type], 'render');
}
```

#### 🧹 Paso 6: Validar montaje/desmontaje manualmente

* Comprobar que cada MFE renderiza su dashboard.
* Añadir borde o color de fondo diferente a cada uno.

---

### ✅ Validaciones

* Las 3 MFEs se integran correctamente desde el Shell mediante `import()`.
* Cada una mantiene su estilo, estructura y comportamiento.
* Se identifican las diferencias de complejidad entre standalone y modular.
* El alumno comprende por qué migrar puede reducir complejidad futura.

---

### ⚠️ Retos para el alumno

Cada reto incluye un *tip* para ayudar al alumno a completarlo correctamente:

* Crear un nuevo `remote-loader.service.ts` alternativo que acepte parámetros adicionales (por ejemplo, para pasar props o callbacks).
  *Tip:* Define una interfaz `RenderOptions` y pásala como segundo parámetro al `render()`, luego usa `Object.assign()` o destructuring para acceder a los datos.
* Adaptar cada uno de los MFEs para exponer explícitamente una función `render(container: HTMLElement)` desde un archivo `expose.ts`, si no existe.
  *Tip:* Puedes simplemente reexportar `render()` desde `main.ts` o crear un archivo aparte con `export function render(container) { ... }` y configurarlo como entrypoint alternativo en el build.
* Aplicar estilos CSS diferenciadores a cada MFE desde su propio contexto (no desde el Shell).
  *Tip:* Usa una clase única por app (`.mfe-modular-style`, `.mfe-standalone-style`, etc.) y aplica fondo/borde/distintivos visuales.
* Añadir una animación o transición simple al montar cada MFE.
  *Tip:* Puedes usar una transición CSS con `opacity` y `transform`, o bien animar la entrada del contenedor con `setTimeout()` y una clase.
* Agregar en cada MFE un botón que dispare un `CustomEvent`, y que el Shell lo capture y lo muestre por consola (simulación de comunicación ascendente).
  *Tip:* Usa `window.dispatchEvent(new CustomEvent('mfe:event', { detail: { mensaje: 'Hola desde MFE' } }))` y escucha en el Shell con `window.addEventListener()`.
* Modificar la estructura del Shell para permitir la carga condicional de MFEs basándose en una variable de entorno o una configuración JSON externa.
  *Tip:* Puedes crear un archivo `mfe.config.json` con rutas por entorno y cargarlo al iniciar la app usando `fetch()` o `HttpClient`.
* Comprobar qué ocurre si un MFE no implementa correctamente la función esperada (`render()`), y capturar el error de forma controlada en el Shell.
  *Tip:* Rodea la lógica de `await import()` con `try/catch` y muestra un mensaje de error dentro del contenedor HTML si falla la carga.

---

> 🔄 En la siguiente fase migraremos una de las apps modulares a standalone y repetiremos el proceso de integración, comparando antes y después.
