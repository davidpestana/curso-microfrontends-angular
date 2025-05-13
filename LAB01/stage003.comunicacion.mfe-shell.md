## 🧪 Fase 3: Comunicación entre MFEs y shell

### 🌟 Objetivo

Introducir mecanismos básicos de comunicación entre MFEs y el shell utilizando eventos personalizados. Se busca que un MFE pueda emitir eventos que el shell intercepte y reenvíe a otro MFE si es necesario.

---

### 📜 Descripción

Se implementará un flujo de datos donde:

* `mfe-one` emite un evento con datos (por ejemplo, un mensaje).
* El `shell` captura ese evento y lo reemite como un nuevo evento global.
* `mfe-two` escucha ese evento y actualiza su contenido.

Este mecanismo simula una comunicación desacoplada en tiempo de ejecución.

---

### 🧍‍♂️ Pasos

1. **Emitir evento desde `mfe-one`**

   * En `render.js` de `mfe-one`:

     ```js
     export function render(container) {
       const el = document.createElement('div');
       el.innerHTML = `
         <h2>MFE One</h2>
         <button id="send-msg">Enviar mensaje</button>
       `;
       container.appendChild(el);

       el.querySelector('#send-msg').addEventListener('click', () => {
         const event = new CustomEvent('mfe-one:message', {
           detail: { message: 'Hola desde MFE One' }
         });
         window.dispatchEvent(event);
       });
     }
     ```

2. **Interceptar evento en `shell/shell.js` y reenviarlo**

   ```js
   window.addEventListener('mfe-one:message', (e) => {
     const forwarded = new CustomEvent('shell:forward-message', {
       detail: e.detail
     });
     window.dispatchEvent(forwarded);
   });
   ```

3. **Escuchar evento en `mfe-two`**

   * En `render.js` de `mfe-two`:

     ```js
     let root;

     export function render(container) {
       root = document.createElement('div');
       root.innerHTML = `<h2>MFE Two</h2><div id="msg"></div>`;
       container.appendChild(root);

       window.addEventListener('shell:forward-message', handleMessage);
     }

     function handleMessage(e) {
       const msgDiv = root.querySelector('#msg');
       msgDiv.textContent = `Mensaje recibido: ${e.detail.message}`;
     }

     export function destroy() {
       window.removeEventListener('shell:forward-message', handleMessage);
       if (root) root.remove();
     }
     ```

---

### ⚠️ Retos

* Asegurarse de limpiar los listeners en `destroy()` para evitar fugas.
* Validar que el mensaje llega solo si `mfe-two` está activo.
* Adaptar el código para aceptar distintos tipos de mensajes.

---

### ✅ Validaciones

* Al hacer clic en el botón de `mfe-one`, aparece un mensaje en `mfe-two`.
* El shell actúa como canal intermediario sin acoplar directamente ambos MFEs.
* No hay errores si `mfe-two` no está cargado.
* Los eventos no se duplican ni quedan colgados tras alternar entre MFEs.
