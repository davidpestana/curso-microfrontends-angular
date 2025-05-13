## 🧠 Nx para arquitectura escalable

### Contenidos

Nx es una herramienta para monorepos que permite gestionar múltiples aplicaciones y librerías Angular (y de otros frameworks) de forma centralizada, eficiente y escalable. Permite desarrollar de forma modular con separación de responsabilidades y un uso optimizado de caché y builds incrementales.

* Concepto de workspace, apps, libs, targets, plugins
* Estructura y separación por dominio funcional
* Comandos clave: `nx generate`, `nx run`, `nx affected`, `nx graph`
* Integración con Angular Standalone, Module Federation y NGRX

### 🧪 Laboratorio básico

1. Crear un workspace con `npx create-nx-workspace@latest` y elegir preset `angular-standalone`.
2. Generar una app `main-app` y una lib `shared/ui`.
3. Crear un componente `HeaderComponent` dentro de `shared/ui`.
4. Importar el componente en `main-app` y usarlo en `app.component.ts`.
5. Ejecutar `nx graph` y `nx affected:graph` tras modificar el componente.

---

## 🧠 Module Federation en Angular

### Contenidos

Module Federation permite que varias aplicaciones Angular funcionen como microfrontends independientes pero conectados, cargándose en tiempo de ejecución. Nx facilita esta integración mediante esquemas de configuración automatizados.

* Arquitectura de host y remotes
* Exposición de módulos vía `RemoteEntryModule`
* Uso de `loadRemoteModule` desde el Shell
* Configuración de `webpack.config.js` para compartir dependencias
* Flujo de build y deploy distribuido

### 🧪 Laboratorio básico

1. Crear un Shell y un Remote con `nx g @nrwl/angular:host` y `:remote`.
2. Exponer un módulo en el Remote (`./Module`).
3. Añadir una ruta en el Shell usando `loadRemoteModule`.
4. Compartir `@angular/core` y `@angular/common` como singletons.
5. Servir ambas apps con `nx serve` y verificar integración en navegador.

---

## 🧠 Gestión de estado distribuido

### Contenidos

En un sistema de MFEs, cada app puede tener estado propio, pero también es necesario compartir parte del estado de forma controlada. Angular permite usar NGRX (clásico) y Signal Store (moderno) para este propósito, manteniendo el desacoplamiento.

* Uso de `@ngrx/store` y `@ngrx/signals`
* Librerías compartidas (`shared/state`) para almacenar datos comunes
* Distinción entre estado global (user, theme) y estado local (UI)
* Comunicación de cambios de estado entre MFEs y Shell

### 🧪 Laboratorio básico

1. Crear una lib `shared/state` con Nx.
2. Implementar un Signal Store con `username` y `isAdmin`.
3. Leer el estado desde Shell y un Remote (`mfe-dashboard`).
4. Agregar botón en Remote que actualice el estado y verifique impacto.

---

## 🧠 Angular Standalone vs Modular

### Contenidos

Desde Angular 14, los componentes pueden ser `standalone`, es decir, no depender de `NgModule`. Esto simplifica la estructura y permite aislar partes de una app como microfrontends sin sobrecarga modular.

* Diferencias: `NgModule` vs `standalone: true`
* `loadComponent()` como alternativa a `loadChildren`
* Migración progresiva de módulos a standalone
* Composición de apps desde librerías standalone compartidas

### 🧪 Laboratorio básico

1. Crear un nuevo componente con `--standalone`.
2. Declararlo como ruta en `app.routes.ts` con `loadComponent()`.
3. Eliminar su módulo (si existía previamente).
4. Verificar en navegador su carga e independencia.

---

## 🧠 Shell y orquestación de MFEs

### Contenidos

El Shell en una arquitectura MFE actúa como host. Es responsable de cargar los remotes dinámicamente, gestionar navegación y en algunos casos orquestar eventos o estado entre aplicaciones.

* Carga dinámica vía `import()` o `loadRemoteModule`
* Patrón de integración: `render(container)` y `destroy()`
* Shell como único punto de navegación
* Comunicación entre apps usando eventos personalizados (`CustomEvent`)
* Verificación de estilos, ciclo de vida y aislamiento

### 🧪 Laboratorio básico

1. Crear un Shell con botones para cargar 3 MFEs (standalone, modular, refactorizado).
2. Cada MFE debe exponer una función `render(container)` y `destroy()`.
3. Al montar un MFE, desmontar el anterior (limpieza de DOM y eventos).
4. Emitir un evento personalizado desde un MFE y capturarlo en el Shell para mostrar en consola.

---
