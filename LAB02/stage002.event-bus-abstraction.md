## 🧱 Fase 2 – Comunicación desacoplada con Event Bus compartido

**🌟 Objetivo:**
Extraer la lógica de eventos fuera del `window` y encapsularla en un bus de eventos reutilizable, compartido entre el shell y los MFEs. Esto permite desacoplar la lógica de comunicación y facilita el mantenimiento en escenarios con múltiples equipos o apps.

Los objetivos específicos de esta fase son:

* Crear una capa común para emisión y escucha de eventos.
* Sustituir `window.dispatchEvent` y `window.addEventListener` por una interfaz compartida.
* Entender el patrón de Event Bus en entornos distribuidos.

---

### 📜 Descripción técnica

Se crea un archivo `event-bus.js` compartido entre todos los MFEs y el Shell. Este archivo define dos métodos:

* `emit(eventName, detail)`: para emitir eventos.
* `on(eventName, callback)`: para escuchar eventos.

Este bus se importa en cada microfrontend y en el shell, actuando como canal de comunicación global pero controlado.

---

### 🧍‍♂️ Pasos

1. **Crear `event-bus.js` compartido**

   Copiar este archivo dentro de cada uno de los cuatro directorios:

   ```js
   // event-bus.js
   export const bus = {
     emit(eventName, detail) {
       window.dispatchEvent(new CustomEvent(eventName, { detail }));
     },
     on(eventName, callback) {
       window.addEventListener(eventName, callback);
     }
   };
   ```

2. **Actualizar los MFEs para usar el bus**

   En cada `render.js`, reemplazar el `CustomEvent` del reto anterior por:

   ```js
   import { bus } from './event-bus.js';

   export function render(container) {
     container.innerHTML = `<h2>MFE activo</h2><button id="send-msg">Emitir mensaje</button>`;
     document.getElementById('send-msg').onclick = () => {
       bus.emit('mfe:mensaje', { from: location.port });
     };
   }
   ```

3. **Actualizar el Shell para usar el bus**

   En `shell.js`, importar el bus:

   ```js
   import { bus } from './event-bus.js';

   bus.on('mfe:mensaje', (e) => {
     console.log(`[Shell] Mensaje recibido desde el MFE ${e.detail.from}`);
   });
   ```

4. **Verificar que se mantiene la carga dinámica**

   Asegúrate de que no se ha roto el `import()` original:

   ```js
   async function loadMFE(name, port) {
     const container = document.getElementById('mfe-container');
     container.innerHTML = '';
     const module = await import(`http://localhost:${port}/render.js`);
     module.render(container);
   }
   window.loadMFE = loadMFE;
   ```

---

### 🔧 Retos para el alumno

1. Añadir un evento específico por MFE (ej: `mfe-one:ready`, `mfe-two:update`) que el Shell escuche.
2. Añadir soporte para `off()` en el bus para eliminar listeners.
3. Mostrar en el DOM del shell el último evento recibido, no solo en consola.
4. Reorganizar el `event-bus.js` en un directorio común fuera de los MFEs.

---

### ✅ Validaciones

* Todos los MFEs emiten eventos correctamente mediante el `bus.emit()`.
* El Shell recibe y maneja eventos mediante `bus.on()`.
* No se usa más `window.addEventListener` ni `dispatchEvent` directamente.
* Los retos de personalización de eventos y refactorización se han cumplido.

---

### 📌 Resultado final

Sistema de comunicación desacoplado y controlado mediante Event Bus, funcional en todos los MFEs y el Shell. Este patrón será base para fases posteriores que introducen librerías externas y gestión de ciclo de vida más compleja.
