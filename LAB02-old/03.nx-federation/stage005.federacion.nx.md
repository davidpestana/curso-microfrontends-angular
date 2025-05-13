## 🧪 Fase 5: Integración real con Module Federation en Nx

### 🌟 Objetivo

Configurar Webpack Module Federation en un workspace Nx para permitir la carga dinámica real de Microfrontends. El objetivo es que el Shell consuma remotamente las aplicaciones `mfe-dashboard` y `mfe-settings`, expuestas como módulos remotos federados.

---

### 🧠 Fundamentos clave

**Webpack Module Federation** permite a una aplicación (Shell) cargar código desde otras aplicaciones (Remotes) en tiempo de ejecución, incluso si fueron desarrolladas y desplegadas por separado. Nx ofrece soporte nativo para esta integración.

Conceptos importantes:

* **Host**: app que consume módulos remotos (el Shell).
* **Remote**: app que expone uno o más módulos.
* **RemoteEntry**: punto de entrada del Remote, accesible por URL.
* **Shared libraries**: dependencias que no deben duplicarse entre apps.

---

### 📜 Descripción

Esta fase transformará el Shell y las apps `mfe-dashboard` y `mfe-settings` en un sistema federado funcional, con navegación entre apps, exposición de módulos y código compartido.

---

### 🧍‍♂️ Pasos detallados

#### 🧱 Paso 1: Convertir el Shell en host federado

```bash
nx g @nrwl/angular:host shell --remotes=mfe-dashboard,mfe-settings
```

* Esto generará automáticamente:

  * `webpack.config.js` en el Shell
  * Exposición de los remotes
  * Entradas de ruta con `loadRemoteModule`

#### 🛰️ Paso 2: Revisar configuración en remotes

* En `mfe-dashboard/project.json` y `mfe-settings/project.json`:

  * Asegurar que existe la opción `mf` y que se define el `exposes` con su módulo de entrada:

```json
"exposes": {
  "./Module": "apps/mfe-dashboard/src/app/remote-entry/entry.module.ts"
}
```

#### 🔐 Paso 3: Definir qué se comparte entre apps

* En `webpack.config.js` del Shell:

```ts
shared: {
  '@angular/core': { singleton: true, strictVersion: true },
  '@angular/common': { singleton: true, strictVersion: true },
  '@angular/router': { singleton: true, strictVersion: true },
  ...
}
```

#### 🚀 Paso 4: Ejecutar apps federadas en paralelo

```bash
nx run-many --target=serve --projects=shell,mfe-dashboard,mfe-settings
```

#### 🌐 Paso 5: Acceder al Shell en navegador

* Abrir `http://localhost:4200/dashboard` y `http://localhost:4200/settings`
* Confirmar que cada ruta carga el MFE correspondiente desde su propia build

#### 🛠️ Paso 6: Simular deploy federado (opcional)

```bash
nx build mfe-dashboard --prod
nx build mfe-settings --prod
nx build shell --prod
```

* Puedes servir cada `dist/` con `npx serve` en puertos separados y configurar el `remoteEntry` en `webpack.config.js` con URLs absolutas.

---

### ✅ Validaciones

* El Shell carga dinámicamente ambos MFEs vía Module Federation.
* El código expuesto es funcional y mantiene estilos y rutas.
* Las librerías compartidas no se duplican (ver en DevTools: solo una copia de Angular).
* Cada MFE puede evolucionar y desplegarse de forma independiente.

---

### ⚠️ Retos para el alumno

* Añadir un nuevo Remote llamado `mfe-admin` y exponer un módulo con routing y componentes.
  *Tip:* Usa el generador: `nx g @nrwl/angular:remote mfe-admin --host=shell`

* Crear un componente en `shared-ui` usado por todos los MFEs con estilo y lógica común.
  *Tip:* Asegúrate de importarlo como `shared:@workspace/shared-ui`

* Configurar una ruta en `mfe-settings` que navegue internamente hacia `mfe-dashboard`, usando `Router.navigateByUrl('/dashboard')`.
  *Tip:* Refuerza el entendimiento de navegación entre MFEs federados.

---

> 🚀 Esta fase permite comprender cómo implementar y mantener un ecosistema real de Microfrontends Angular con Nx y Module Federation.
