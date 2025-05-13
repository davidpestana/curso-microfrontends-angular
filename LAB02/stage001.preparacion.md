## 🧱 Fase 1 – Preparación de los MFEs base (sin Shell)

**🌟 Objetivo:**
Bootstrapear tres aplicaciones Angular base, dos modulares y una standalone, que funcionarán como Microfrontends independientes. Esta fase no incluye aún la federación ni la Shell.

---

### 📜 Descripción

Se preparan las tres aplicaciones que más adelante serán federadas:

* `mfe-dashboard`: arquitectura modular clásica
* `mfe-settings`: arquitectura modular clásica que se refactorizará
* `mfe-profile`: arquitectura Angular standalone desde el inicio

Estas apps deben compilar correctamente, tener un módulo funcional o componente raíz enrutado, y ser accesibles por navegador.

---

### 🧍‍♂️ Pasos

Cada aplicación debe tener contenido mínimo y visual diferenciado. A continuación, se incluye una plantilla para cada componente principal de las apps:

Antes de comenzar, decide si vas a trabajar en paralelo con tres terminales, o si irás una a una. Estas apps deben desarrollarse de forma **autónoma y coherente**, como si fuesen proyectos separados gestionados por distintos equipos.

1. Crear `mfe-dashboard` (modular):

   ```bash
   ng new mfe-dashboard --routing --style=scss --no-standalone
   cd mfe-dashboard
   ng generate module features/dashboard --route dashboard --module app.module
   ng generate component features/dashboard/pages/dashboard-list
   ```

2. Crear `mfe-settings` (modular):

   ```bash
   cd ..
   ng new mfe-settings --routing --style=scss --no-standalone
   cd mfe-settings
   ng generate module features/settings --route settings --module app.module
   ng generate component features/settings/pages/settings-main
   ```

3. Crear `mfe-profile` (standalone):

   ```bash
   cd ..
   ng new mfe-profile --routing --style=scss
   cd mfe-profile
   ng generate component pages/profile --standalone --flat
   ```

   Añadir en `app.routes.ts`:

   ```ts
   export const routes: Routes = [
     { path: 'profile', loadComponent: () => import('./pages/profile.component').then(m => m.ProfileComponent) }
   ];
   ```

4. Servir cada app en su puerto:

   * `mfe-dashboard`: `ng serve --port 4201`
   * `mfe-settings`: `ng serve --port 4202`
   * `mfe-profile`: `ng serve --port 4203`

---

### 📄 Ejemplo de contenido

**mfe-dashboard** (`dashboard-list.component.ts`)

```ts
export class DashboardListComponent {
  items = ['Usuarios', 'Reportes', 'Notificaciones'];
}
```

```html
<h2 class="title">Dashboard</h2>
<ul>
  <li *ngFor="let item of items">📌 {{ item }}</li>
</ul>
```

```scss
.title {
  color: #2b6cb0;
  font-weight: bold;
}
```

**mfe-settings** (`settings-main.component.ts`)

```ts
export class SettingsMainComponent {
  version = '1.0.0';
}
```

```html
<h2 class="title">Configuración</h2>
<p>Versión actual: {{ version }}</p>
```

```scss
.title {
  color: #c53030;
  font-weight: bold;
}
```

**mfe-profile** (`profile.component.ts`)

```ts
export class ProfileComponent {
  name = 'Jane Doe';
  role = 'Frontend Engineer';
}
```

```html
<h2 class="title">Perfil de Usuario</h2>
<p><strong>{{ name }}</strong> – {{ role }}</p>
```

```scss
.title {
  color: #38a169;
  font-weight: bold;
}
```

### 🔧 Retos

* Personalizar el contenido de cada app para que tenga una identidad visual clara (dashboard, settings, profile).

* Explorar cada estructura de carpetas y preparar un layout mínimo (navegación, encabezado o contenido) para que cada app sea navegable y tenga sentido por sí sola.

* Añadir estilos o colores característicos a cada app para facilitar su futura integración visual en el Shell.

* Cambiar los títulos y estilos de cada componente raíz para identificarlos visualmente.

* Añadir una segunda ruta en `mfe-dashboard` con un segundo módulo lazy.

* Validar que cada app tiene su identidad y carga correctamente su estructura.

---

### ✅ Validaciones

* Cada MFE compila y levanta en su puerto asignado.
* Navegar a `/dashboard`, `/settings`, `/profile` muestra componentes independientes.
* Las apps no están federadas ni conectadas aún.
* Se entienden las diferencias entre `--no-standalone` y modo standalone por defecto.
* Los alumnos visualizan el punto de partida previo a la federación.
