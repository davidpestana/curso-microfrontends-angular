## 🧪 Fase 2: Carga dinámica con `import()` y navegación simulada

### 🌟 Objetivo

Demostrar el uso real de `import()` para cargar MFEs bajo demanda, simulando una navegación entre módulos desde el shell, con montaje y desmontaje limpio.

---

### 📜 Descripción

Se ampliará el shell para:

* Cargar los MFEs sólo cuando se soliciten.
* Simular navegación activando uno y ocultando el anterior.
* Preparar el entorno para futura comunicación.

---

### 🧍‍♂️ Pasos

1. **Mejorar la función `loadMFE` en `shell/shell.js`:**

   * Introducir sistema de desmontaje:

     ```js
     let currentModule = null;

     async function loadMFE(name) {
       const container = document.getElementById('mfe-container');
       container.innerHTML = '';

       if (currentModule && currentModule.destroy) {
         currentModule.destroy();
       }

       const module = await import(`../${name}/render.js`);
       currentModule = module;
       module.render(container);
     }
     ```

2. **Actualizar los `render.js` de cada MFE para incluir `destroy()` opcional:**

   * Ejemplo en `mfe-one/render.js`:

     ```js
     let root;

     export function render(container) {
       root = document.createElement('div');
       root.innerHTML = `<h2>MFE One</h2>`;
       container.appendChild(root);
     }

     export function destroy() {
       if (root) root.remove();
     }
     ```

3. **Estilo visual para validar más fácilmente**

   * Añadir un fondo de color o borde a cada MFE para diferenciar visualmente.

4. **Refactor opcional:** permitir que `render()` reciba props o contexto (parámetros iniciales).

---

### ⚠️ Retos

* Asegurar que los MFEs no duplican contenido si se cargan varias veces.
* Evitar fugas de memoria o elementos huérfanos.
* Comprobar que `destroy()` se ejecuta correctamente.

---

### ✅ Validaciones

* Al alternar entre MFE One y Two, el contenido anterior se limpia correctamente.
* No hay colisiones visuales ni contenido duplicado.
* `destroy()` se llama exactamente una vez por MFE al cambiar.
* `render()` sigue siendo idempotente y reutilizable.
