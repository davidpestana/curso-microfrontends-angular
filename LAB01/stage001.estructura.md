## 🧪 Fase 1: Estructura inicial del proyecto

### 🌟 Objetivo

Establecer la estructura mínima de un sistema Microfrontend vanilla sin frameworks. Cada MFE será una miniaplicación independiente, y el shell actuará como cargador y orquestador.

---

### 📜 Descripción

Se crearán tres directorios:

* `shell/`: contenedor principal que carga dinámicamente los MFEs.
* `mfe-one/`: primer microfrontend funcional.
* `mfe-two/`: segundo microfrontend.

Cada carpeta incluirá su propio `index.html` y ficheros JavaScript. No habrá aún comunicación ni integración real, solo estructura, renderizado básico y preparación para carga dinámica.

---

### 🧍‍♂️ Pasos

1. **Estructura de carpetas**

   ```bash
   mkdir shell mfe-one mfe-two
   ```

2. **Archivo `index.html` en cada carpeta**

   * Ejemplo mínimo para `mfe-one/index.html`:

     ```html
     <!DOCTYPE html>
     <html>
     <head><title>MFE One</title></head>
     <body>
       <h1>MFE One</h1>
       <script type="module" src="./bootstrap.js"></script>
     </body>
     </html>
     ```

3. **Archivo `bootstrap.js` por MFE**

   * Código mínimo de renderizado:

     ```js
     const root = document.createElement('div');
     root.innerHTML = `<p>Hello from MFE One</p>`;
     document.body.appendChild(root);
     ```

4. **Shell básico con botones**

   * `shell/index.html`:

     ```html
     <!DOCTYPE html>
     <html>
     <head><title>Shell</title></head>
     <body>
       <h1>Shell</h1>
       <button onclick="loadMFE('mfe-one')">Load MFE One</button>
       <button onclick="loadMFE('mfe-two')">Load MFE Two</button>
       <div id="mfe-container"></div>
       <script type="module" src="./shell.js"></script>
     </body>
     </html>
     ```

5. **Shell con lógica de carga (`shell/shell.js`)**

   * Código ejemplo:

     ```js
     async function loadMFE(name) {
       const container = document.getElementById('mfe-container');
       container.innerHTML = '';
       const module = await import(`../${name}/render.js`);
       module.render(container);
     }
     ```

6. **Cada MFE define su `render.js`**

   * `mfe-one/render.js`:

     ```js
     export function render(container) {
       const el = document.createElement('div');
       el.innerHTML = `<h2>MFE One Loaded</h2>`;
       container.appendChild(el);
     }
     ```

---

### ⚠️ Retos

* Comprobar que la ruta relativa `../${name}/render.js` funciona desde `shell/`
* Usar un servidor estático (`npx serve .` o similar) para evitar problemas con `import()`
* Verificar que no hay errores de CORS ni MIME

---

### ✅ Validaciones

* Al pulsar los botones en el shell se cargan dinámicamente `mfe-one` y `mfe-two`.
* El HTML de cada uno se renderiza dentro de `#mfe-container`.
* Cada MFE tiene su HTML y JS aislado y autoejecutable.
