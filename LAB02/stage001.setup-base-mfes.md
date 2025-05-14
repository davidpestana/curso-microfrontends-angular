## 🧱 Fase 1 – Preparación del laboratorio y MFEs base

**🌟 Objetivo:**
Crear una base mínima funcional para probar múltiples técnicas de microfrontends en local. Esta fase incluye tres MFEs independientes y un Shell cargador. No se usa aún ninguna librería externa.

Los objetivos específicos de esta fase son:

* Familiarizarse con la estructura básica de MFEs independientes.
* Entender cómo servirlos desde distintos puertos.
* Implementar la carga dinámica usando `import()`.

---

### 📜 Descripción técnica

Se preparan cuatro aplicaciones:

* `mfe-one`, `mfe-two`: microfrontends que renderizan contenido autónomo.
* `shell`: aplicación principal que carga cualquier MFE dinámicamente.

Cada MFE está compuesto por un único archivo `render.js` que expone una función `render(container)` para inyectar su contenido. No existe enrutamiento ni integración aún. Se utiliza `live-server` para levantar cada app en un puerto distinto.

---

### 🧍‍♂️ Pasos

1. **Crear estructura base del proyecto**

   Ejecutar desde una carpeta raíz del laboratorio:

   ```bash
   mkdir mfe-one mfe-two shell
   ```

2. **Crear los microfrontends base**

   En cada carpeta `mfe-one`, `mfe-two`, crear el archivo `render.js` con el siguiente contenido:

   ```js
   // render.js
   export function render(container) {
     container.innerHTML = '<h2>MFE activo</h2>';
   }
   ```

3. **Crear el Shell cargador**

   En `shell/index.html`:

   ```html
   <h1>Shell principal</h1>
   <button onclick="loadMFE('mfe-one', 3001)">Cargar MFE One</button>
   <button onclick="loadMFE('mfe-two', 3002)">Cargar MFE Two</button>
   <div id="mfe-container"></div>
   <script type="module" src="./shell.js"></script>
   ```

   En `shell/shell.js`:

   ```js
   async function loadMFE(name, port) {
     const container = document.getElementById('mfe-container');
     container.innerHTML = '';
     const module = await import(`http://localhost:${port}/render.js`);
     module.render(container);
   }
   window.loadMFE = loadMFE;
   ```

4. **Levantar los servidores locales**

   En cuatro terminales distintas, lanzar:

   ```bash
   live-server mfe-one --port=3001 --cors
   live-server mfe-two --port=3002 --cors
   live-server shell --port=3000 --cors
   ```

   Asegúrate de tener instalado `live-server` globalmente:

   ```bash
   npm install -g live-server
   ```

---

### 🔧 Retos para el alumno

1. Modificar el contenido de cada `render.js` para que sea diferente visualmente en cada MFE (colores, títulos, íconos).
2. Añadir un `console.log` en cada render que indique desde qué puerto se ha cargado.
3. Crear un segundo botón en cada MFE que, al hacer clic, emita un `CustomEvent` en `window` con un mensaje.
4. En el `shell`, añadir un `window.addEventListener()` que escuche estos mensajes y los muestre en consola.

---

### ✅ Validaciones

* Cada MFE se sirve en su puerto sin errores de CORS.
* El `shell` puede cargar cada uno dinámicamente desde el navegador.
* Cada botón funciona y activa su respectivo render.
* Los retos visuales y de eventos están implementados correctamente.

---

### 📌 Resultado final

Una estructura base funcional de Microfrontends independientes listos para ser integrados en fases posteriores con diferentes técnicas y librerías. Esta base servirá para probar estrategias de comunicación, orquestación, sandbox y federación de código entre múltiples MFEs.
