## 🧪 Fase 5: Gestión mínima del estado

### 🌟 Objetivo

Simular una gestión compartida de estado entre el shell y los MFEs utilizando una solución simple y explícita. Esta fase no introduce librerías ni frameworks, solo una base para entender la complejidad futura del estado distribuido.

---

### 📜 Descripción

Se introducirá un objeto global controlado (`window.appState`) que contendrá información compartida. El shell y los MFEs podrán leer y modificar su contenido bajo reglas claras. Se trata de un ejercicio de práctica conceptual, no una solución de producción.

---

### 🧍‍♂️ Pasos

1. **Definir el estado global mínimo en `shell/shell.js`**

   ```js
   window.appState = {
     user: null,
     lastMessage: null
   };
   ```

2. **Modificar `mfe-one` para actualizar el estado**

   * Al hacer clic en el botón de mensaje:

     ```js
     window.appState.lastMessage = 'Mensaje desde MFE One';
     window.dispatchEvent(new CustomEvent('app:state-updated'));
     ```

3. **Modificar `mfe-two` para reaccionar a cambios**

   * Escuchar el evento y leer el estado:

     ```js
     function handleStateUpdate() {
       const msgDiv = root.querySelector('#msg');
       msgDiv.textContent = `Último mensaje: ${window.appState.lastMessage}`;
     }

     window.addEventListener('app:state-updated', handleStateUpdate);
     ```

4. **Evitar dependencia directa del DOM del shell**

   * Toda interacción debe hacerse a través de eventos y del objeto global, no accediendo a elementos fuera del propio MFE.

5. **(Opcional) Añadir un campo de entrada en `mfe-one`**

   * Para que el usuario pueda escribir un mensaje personalizado que se guarde como estado global.

---

### ⚠️ Retos

* Mantener la consistencia del estado cuando se desmontan y vuelven a montar MFEs.
* Evitar dependencias circulares.
* No acceder directamente a variables internas entre MFEs.

---

### ✅ Validaciones

* El mensaje enviado desde `mfe-one` se guarda correctamente en `window.appState`.
* `mfe-two` refleja los cambios cuando se produce el evento.
* El estado se conserva aunque se desmonten temporalmente los MFEs.
* Todo el sistema funciona sin errores de sincronización o acceso nulo.
