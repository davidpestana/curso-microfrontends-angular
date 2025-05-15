## 🧱 Fase 4 – Integración con `qiankun` (modo sin framework)

**🌟 Objetivo:**
Implementar un sistema de microfrontends basado en `qiankun`, una librería basada en `single-spa` que añade aislamiento completo entre MFEs, soporte para estilo encapsulado y carga HTML+JS+CSS remota. El objetivo es simular un entorno real donde cada MFE se comporta como una aplicación independiente, con su propio contexto y ciclo de vida.

---

### 📜 Descripción técnica

`qiankun` permite cargar microfrontends que no solo exponen funciones JS, sino que se empaquetan como aplicaciones HTML completas. Cada MFE será servido con un `index.html` propio, y el Shell los integrará como si fueran iframes avanzados, pero compartiendo estado.

Los MFEs deben estar servidos en puertos separados con HTML + JS propios, y el Shell usará `registerMicroApps` para activarlos según rutas o eventos.

---

### 🧍‍♂️ Pasos

1. **Preparar los MFEs para ser cargados como HTML remotos**

   En `mfe-one/index.html`:

   ```html
   <div id="root"></div>
   <script>
     window.__POWERED_BY_QIANKUN__ = true;
   </script>
   <script type="module" src="./render.js"></script>
   ```

   En `render.js`:

   ```js
   let container;

   export async function bootstrap() {
     console.log('mfe-one bootstrap');
   }

   export async function mount(props) {
     container = props.container || document.getElementById('root');
     container.innerHTML = '<h2>Qiankun - MFE One</h2>';
   }

   export async function unmount() {
     container.innerHTML = '';
   }
   ```

   Repetir para `mfe-two`.

2. **Configurar el Shell con `qiankun`**

   En `shell/index.html`, incluir:

   ```html
   <script src="https://cdn.jsdelivr.net/npm/qiankun/dist/qiankun.min.js"></script>
   <script type="module" src="./shell.js"></script>
   ```

3. **Registrar las apps en `shell.js`**

   ```js
   import { registerMicroApps, start } from 'https://cdn.jsdelivr.net/npm/qiankun/es/qiankun.min.js';

   registerMicroApps([
     {
       name: 'mfe-one',
       entry: '//localhost:3001',
       container: '#mfe-container',
       activeRule: () => location.hash === '#one'
     },
     {
       name: 'mfe-two',
       entry: '//localhost:3002',
       container: '#mfe-container',
       activeRule: () => location.hash === '#two'
     }
   ]);

   start();
   ```

4. **Actualizar HTML del Shell**

   ```html
   <button onclick="location.hash = '#one'">MFE One</button>
   <button onclick="location.hash = '#two'">MFE Two</button>
   <div id="mfe-container"></div>
   ```

---

### 🔧 Retos para el alumno

1. Personalizar el `index.html` de cada MFE para que tenga su propio estilo CSS aislado.
2. Emitir un evento desde cada MFE (`qiankun:ready`) y capturarlo desde el Shell.
3. Implementar una función en el Shell para desmontar manualmente un MFE usando `unmountMicroApp()`.
4. Validar qué ocurre si ambos MFEs usan IDs duplicados (problema clásico sin aislamiento).

---

### ✅ Validaciones

* Cada MFE se monta correctamente desde su `index.html` sin errores.
* Los estilos se encapsulan sin interferencia entre MFEs.
* El Shell detecta los eventos emitidos por los MFEs.
* Las rutas con hash activan correctamente el microfrontend correspondiente.

---

### 📌 Resultado final

Con `qiankun`, se logra un modelo de integración más realista, donde los MFEs están completamente encapsulados, cargados como aplicaciones autónomas y gestionados por el Shell sin compartir DOM o contexto directo. Este patrón se acerca a modelos de producción en grandes organizaciones.
