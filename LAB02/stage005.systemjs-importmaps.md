## 🧱 Fase 5 – Composición dinámica con `SystemJS` e `import maps`

**🌟 Objetivo:**
Explorar la composición de microfrontends en tiempo de ejecución usando `SystemJS`, una librería de carga dinámica de módulos, junto con `import maps`, lo que permite definir rutas virtuales para módulos en el navegador sin necesidad de build o bundler.

Esta técnica permite simular federación sin Webpack, definiendo los MFEs como módulos identificables por nombre y cargables por URL.

---

### 📜 Descripción técnica

`SystemJS` permite cargar dinámicamente módulos JavaScript externos en tiempo de ejecución. Combinado con un `import map`, puedes mapear nombres de módulos (`mfe-one`, `mfe-two`) a sus URLs reales (servidas por `live-server`).

Esto permite:

* Cargar MFEs sin hardcodear rutas completas.
* Actualizar rutas simplemente modificando el `import map`.
* Probar comportamientos similares a `Module Federation`, pero sin build tools.

---

### 🧍‍♂️ Pasos

1. **Preparar el `shell/index.html`**

   Reemplazar el HTML con:

   ```html
   <script src="https://cdn.jsdelivr.net/npm/systemjs/dist/system.min.js"></script>
   <script type="systemjs-importmap">
   {
     "imports": {
       "mfe-one": "http://localhost:3001/render.js",
       "mfe-two": "http://localhost:3002/render.js"
     }
   }
   </script>

   <button onclick="load('mfe-one')">Cargar MFE One</button>
   <button onclick="load('mfe-two')">Cargar MFE Two</button>
   <div id="mfe-container"></div>
   <script type="module" src="./shell.js"></script>
   ```

2. **Actualizar `shell/shell.js`**

   ```js
   async function load(name) {
     const container = document.getElementById('mfe-container');
     container.innerHTML = '';
     const module = await System.import(name);
     module.render(container);
   }
   ```

3. **Mantener los MFEs como en Fase 1**

   Asegurarse de que `mfe-one/render.js` y `mfe-two/render.js` sigan exportando:

   ```js
   export function render(container) {
     container.innerHTML = `<h2>${location.port} cargado</h2>`;
   }
   ```

---

### 🔧 Retos para el alumno

1. Extraer el `import map` a un archivo `import-map.json` y cargarlo dinámicamente desde JS.
2. Añadir un botón para recargar dinámicamente el `import map` (simulando hot swap de remotes).
3. Implementar una función `preload(name)` que cargue el MFE sin montarlo aún (simulando lazy preload).
4. Añadir una tercera entrada en el import map apuntando a un MFE ficticio y manejar el error si falla la carga.

---

### ✅ Validaciones

* El Shell puede cargar los MFEs dinámicamente usando los nombres definidos en el import map.
* El código no contiene rutas absolutas a los MFEs.
* Los retos de preloading, errores y dinámicas de import map están implementados correctamente.

---

### 📌 Resultado final

Con `SystemJS` + `import maps`, se ha logrado una composición runtime avanzada sin necesidad de herramientas de bundling. Este patrón demuestra cómo puede simularse la federación a nivel navegador y sirve como base para exploraciones de federación dinámica en producción.
