## 🧪 Fase 4: Introducción de Nx y arquitectura distribuida

### 🌟 Objetivo

Iniciar un Monorepo con Nx que permita organizar múltiples aplicaciones Angular como Microfrontends bajo una arquitectura escalable, compartida y desacoplada. Esta fase sirve como base para incorporar Module Federation en fases posteriores.

---

### 🧠 Introducción básica a Nx

**Nx** es un framework de desarrollo para monorepositorios que facilita la organización de múltiples aplicaciones y librerías en un mismo repositorio. Está diseñado para escalabilidad, modularidad y colaboración entre equipos.

Algunos conceptos clave que debes conocer:

* **Workspace**: conjunto de apps y libs dentro de un solo repositorio Nx.
* **App**: una aplicación Angular (o de otro tipo) ejecutable.
* **Lib**: una librería reutilizable, de UI, lógica, servicios, utilidades, etc.
* **Generadores (`nx generate`)**: comandos para crear apps, libs, componentes, rutas, etc.
* **Targets (`nx serve`, `nx build`, etc.)**: acciones específicas para apps o libs.

**Ventajas frente a proyectos Angular clásicos:**

* Organización por dominios o áreas funcionales.
* Reutilización de código compartido sin duplicación.
* Configuración unificada del entorno.
* Ejecución rápida y paralela de builds y tests.
* Integración nativa con herramientas como Module Federation.

---

### 📜 Descripción

El alumno creará un nuevo workspace Nx, generará una aplicación shell y dos MFEs remotos. Todo se organizará dentro de un único repositorio, lo que permitirá:

* Separación por dominios funcionales
* Reutilización de librerías compartidas
* Desarrollo paralelo entre equipos o contextos
* Preparación para deploys independientes y carga dinámica entre apps

---

### 🧍‍♂️ Pasos detallados

#### 🧱 Paso 1: Crear el workspace

```bash
npx create-nx-workspace@latest mfe-workspace --preset=angular-standalone --appName=shell --style=scss --routing=true --standalone=true
cd mfe-workspace
```

#### ➕ Paso 2: Crear dos aplicaciones Angular remotas

```bash
nx generate @nrwl/angular:application mfe-dashboard --routing --style=scss --standalone
nx generate @nrwl/angular:application mfe-settings --routing --style=scss --standalone
```

#### 🧩 Paso 3: Crear una librería compartida

```bash
nx generate @nrwl/angular:library shared-ui --style=scss --standalone
```

* Añadir un componente visual como `PageTitleComponent` y exportarlo.
* Importarlo en `mfe-dashboard` y `mfe-settings` para validar compartición real de código.

#### 🔀 Paso 4: Añadir rutas en el Shell para cargar los MFEs

* Usar lazy loading con `loadRemoteModule` (se simulará con `import()` si MF aún no están federados).
* Simular integración dentro de `app.routes.ts`:

```ts
{
  path: 'dashboard',
  loadChildren: () => import('mfe-dashboard/Module').then(m => m.RemoteEntryModule)
}
```

* Nota: en esta fase aún puede no estar federado, solo se deja preparada la estructura.

#### 🧪 Paso 5: Ejecutar apps en paralelo y validar

```bash
nx serve shell
nx serve mfe-dashboard
nx serve mfe-settings
```

* Asegurarse de que cada app corre en su puerto.
* Confirmar que el shell navega correctamente entre MFEs usando sus rutas.

#### 📚 Paso 6: Documentar estructura Nx generada

* Explicar en README o verbalmente:

  * ¿Dónde está cada app?
  * ¿Dónde están las librerías?
  * ¿Qué comando sirve para crear una nueva app, lib, etc.?

---

### ✅ Validaciones

* El workspace contiene 1 Shell + 2 apps MFE + 1 librería compartida.
* Cada MFE puede usar código de la librería sin duplicar.
* El Shell accede a los MFEs como rutas independientes.
* El alumno comprende el propósito y ventajas de Nx para escalar proyectos Angular distribuidos.

---

### ⚠️ Retos para el alumno

* Crear una nueva librería `utils-date` que exporte una función `formatDate` y usarla en ambos MFEs.
  *Tip:* Usa `nx generate @nrwl/js:library utils-date` y exporta desde `index.ts`.
* Personalizar el `PageTitleComponent` de `shared-ui` para aceptar `@Input()` y aplicar estilos por defecto.
  *Tip:* Refuerza el patrón de diseño compartido adaptable por dominio.
* Modificar el Shell para mostrar dinámicamente desde qué app se ha cargado la vista actual.
  *Tip:* Escucha los cambios de ruta con el `Router` y muestra un banner o badge.

---

> 🔄 Esta fase establece una base sólida para introducir Webpack Module Federation, que se abordará en la próxima fase del curso.
