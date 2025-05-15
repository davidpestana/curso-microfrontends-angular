# 📘 Sesión 3 – Escalabilidad y arquitectura federada con Nx + Module Federation

## 🎯 Objetivo de la sesión

Comprender y aplicar una arquitectura de microfrontends federada en Angular usando Nx como herramienta de escalabilidad y gestión modular. Se implementará un monorepo con una aplicación Shell y dos remotos, usando `@angular-architects/module-federation`.

---

## 🧪 Laboratorio guiado – Fase 2: Integración de Module Federation

### 🎯 Objetivo de la fase

Configurar la federación entre la app Shell y las aplicaciones Remote (`dashboard`, `profile`) usando `@angular-architects/module-federation`, permitiendo la carga dinámica de rutas desde remotos en tiempo de ejecución.

---

### 🧱 Estructura que se modificará

```
mfe-workspace/
├── apps/
│   ├── shell/            # Ahora configurado como Host
│   ├── dashboard/        # Remote expone su módulo principal
│   └── profile/          # Remote expone su módulo principal
```

Cada remote expondrá un `Module`, y el Shell lo cargará vía `loadRemoteModule` desde `remoteEntry.js`.

---

### 🛠 Pasos detallados

1. **Instalar plugin de federation**

   ```bash
   npm install @angular-architects/module-federation --save-dev
   ```

2. **Inicializar federation en cada app**

   ```bash
   nx g @angular-architects/module-federation:init --project shell --type host
   nx g @angular-architects/module-federation:init --project dashboard --type remote --host shell
   nx g @angular-architects/module-federation:init --project profile --type remote --host shell
   ```

   * Esto actualiza `webpack.config.js` y `module-federation.config.js` en cada app

3. **Exponer módulos desde remotes**

   * En `module-federation.config.js` de cada remote:

     ```ts
     exposes: {
       './Module': './apps/dashboard/src/app/dashboard/dashboard.module.ts'
     }
     ```

4. **Cargar rutas remotas desde el Shell**

   * En `app.routes.ts` del Shell:

     ```ts
     {
       path: 'dashboard',
       loadChildren: () => loadRemoteModule({
         type: 'module',
         remoteEntry: 'http://localhost:4201/remoteEntry.js',
         exposedModule: './Module'
       }).then(m => m.DashboardModule)
     }
     ```

5. **Actualizar puertos para evitar conflicto** (si es necesario)

   * Confirmar que cada app corre en puerto distinto (`4200`, `4201`, `4202`)

6. **Lanzar entorno federado**

   ```bash
   nx serve shell
   nx serve dashboard
   nx serve profile
   ```

   * Acceder al Shell y navegar a `/dashboard` o `/profile`

---

### 🔍 Validaciones clave

* ¿La consola del navegador carga correctamente `remoteEntry.js`?
* ¿El módulo remoto se carga en tiempo de ejecución sin errores?
* ¿Hay errores 404, CORS o de sincronización de versiones?
* ¿Los bundles se cargan dinámicamente y no están en el bundle del shell?

---

### 🧩 Retos para el alumno

* Forzar un error en `remoteEntry` (puerto incorrecto, nombre mal escrito) y diagnosticarlo

  * 💡 *Tip*: Observa la consola de red y errores en el navegador. El Shell mostrará un error de carga fallida.
* Comparar el tamaño del bundle del Shell antes y después de la federación

  * 💡 *Tip*: Usa `source-map-explorer dist/apps/shell/main.js` antes y después de federar. Busca si `dashboard` o `profile` aparecen embebidos.
* Comprobar qué pasa si se comenta el `expose` en el remote: ¿cómo falla el Shell?

  * 💡 *Tip*: El Shell lanzará un error de `undefined` al resolver el módulo remoto. También puedes revisar el `remoteEntry.js` directamente en el navegador y ver si contiene el módulo expuesto esperado.

---

### ✅ Resultado esperado

* Shell federando dinámicamente los módulos `dashboard` y `profile`
* Rutas funcionales en tiempo de ejecución
* Confirmación visual de que los MFEs no están embebidos en el bundle del host
* Preparación para compartir librerías en la siguiente fase
