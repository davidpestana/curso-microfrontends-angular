## 🧱 Fase 3 – Integración con `single-spa` (modo Vanilla)

**🌟 Objetivo:**
Incorporar una librería real de microfrontends (`single-spa`) para gestionar el ciclo de vida completo (`bootstrap`, `mount`, `unmount`) de los MFEs desde el shell. Esta fase introduce la separación explícita entre apps registradas y el control de activación.

Los objetivos específicos son:

* Aprender a registrar MFEs como apps independientes.
* Controlar su carga mediante activadores (rutas o botones).
* Implementar ciclo de vida explícito con `single-spa` en un entorno Vanilla JS.

---

### 📜 Descripción técnica

Se instala y utiliza `single-spa` directamente desde CDN. Los MFEs deben exponer las funciones `bootstrap`, `mount` y `unmount` en lugar de `render`. El Shell actúa como orquestador: registra las apps y las monta o desmonta según el botón pulsado.

---

### 🧍‍♂️ Pasos

1. **Actualizar cada MFE para exponer ciclo de vida `single-spa`**

   En `mfe-one/render.js`, cambiar a:

   ```js
   let containerRef = null;

   export async function bootstrap() {
     console.log('MFE One bootstrap');
   }

   export async function mount(props) {
     containerRef = props.container;
     containerRef.innerHTML = '<h2>MFE One cargado</h2>';
   }

   export async function unmount() {
     containerRef.innerHTML = '';
     containerRef = null;
   }
   ```

   Repetir lo mismo en `mfe-two`.

2. **Incluir `single-spa` en el shell**

   En `shell/index.html`, cargar la librería:

   ```html
   <script src="https://cdn.jsdelivr.net/npm/single-spa/lib/system/single-spa.min.js"></script>
   <script src="https://unpkg.com/systemjs/dist/system.min.js"></script>
   <script type="module" src="./shell.js"></script>
   ```

3. **Configurar registro de aplicaciones en `shell.js`**

   ```js
   import * as singleSpa from 'https://cdn.jsdelivr.net/npm/single-spa/lib/system/single-spa.min.js';

   const container = document.getElementById('mfe-container');

   singleSpa.registerApplication({
     name: 'mfe-one',
     app: () => System.import('http://localhost:3001/render.js'),
     activeWhen: () => window.location.hash === '#one',
     customProps: { container },
   });

   singleSpa.registerApplication({
     name: 'mfe-two',
     app: () => System.import('http://localhost:3002/render.js'),
     activeWhen: () => window.location.hash === '#two',
     customProps: { container },
   });

   singleSpa.start();
   ```

4. **Modificar el HTML del Shell para usar rutas con hash**

   ```html
   <button onclick="location.hash = '#one'">Cargar MFE One</button>
   <button onclick="location.hash = '#two'">Cargar MFE Two</button>
   <div id="mfe-container"></div>
   ```

---

### 🔧 Retos para el alumno

1. Hacer que cada MFE registre un evento personalizado (`mfe:ready`) al montarse.
2. Mostrar en el DOM del Shell qué MFE está activo actualmente.
3. Incluir un botón global que desmonte todos los MFEs (`singleSpa.unmountApplication(...)`).

---

### ✅ Validaciones

* Cada MFE puede montarse y desmontarse desde el Shell.
* La navegación por `location.hash` activa correctamente los MFEs.
* Cada MFE usa `bootstrap`, `mount`, `unmount` y limpia su contenido.
* Se reciben eventos desde los MFEs y se muestra en el DOM qué app está activa.

---

### 📌 Resultado final

Se ha migrado de una carga manual con `import()` a una orquestación controlada mediante `single-spa`, con ciclo de vida explícito. Esto introduce conceptos clave de gestión modular y sienta la base para integraciones más complejas en fases posteriores.
