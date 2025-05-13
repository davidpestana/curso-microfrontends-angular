## 🧪 Stage 4 del Lab 2 — Module Federation: enfoques comparados

### 🎯 Objetivo

Comprender y aplicar **tres enfoques distintos de Module Federation en Angular**, como preparación progresiva para arquitecturas desacopladas de MFEs:

1. **Module Federation clásica** mediante `@angular-architects/module-federation` (plugin para Webpack 5)
2. **Module Federation nativa** con `@angular-architects/native-federation` (basado en ESM y componentes standalone)
3. *(Día 3)* Federation gestionada por **Nx** (automatización completa y escala monorepo)

---

### 🧱 Prerrequisitos

* Dos aplicaciones Angular independientes creadas con `ng new` (por ejemplo: `shell` y `mfe-dashboard`)
* Ambas deben tener `routing` habilitado y `style=scss`

---

## 🚀 Parte A — Module Federation clásica con `@angular-architects/module-federation`

### 1. Instalar el plugin

```bash
ng add @angular-architects/module-federation --project shell --type host
ng add @angular-architects/module-federation --project mfe-dashboard --type remote --port 4201
```

### 2. Exponer un módulo en el remote

En `mfe-dashboard`:

```ts
@NgModule({
  declarations: [DashboardComponent],
  imports: [CommonModule, RouterModule.forChild([{ path: '', component: DashboardComponent }])],
})
export class RemoteEntryModule {}
```

Y exponerlo en `webpack.config.js`:

```ts
exposes: {
  './Module': './src/app/remote-entry/entry.module.ts',
}
```

### 3. Consumir en el host

```ts
{
  path: 'dashboard',
  loadChildren: () => loadRemoteModule({
    type: 'module',
    remoteEntry: 'http://localhost:4201/remoteEntry.js',
    exposedModule: './Module'
  }).then(m => m.RemoteEntryModule)
}
```

### 4. Servir y validar

```bash
npm run start:shell
npm run start:mfe-dashboard
```

---

## 🚀 Parte B — Module Federation nativa con `@angular-architects/native-federation`

### 1. Instalar el plugin nativo

```bash
ng add @angular-architects/native-federation --project shell
ng add @angular-architects/native-federation --project mfe-dashboard --port 4201
```

### 2. Exponer componente standalone

```ts
exposes: {
  './Component': './src/app/dashboard/dashboard.component.ts'
}
```

### 3. Consumir desde el host

```ts
{
  path: 'dashboard',
  loadComponent: () => loadRemoteComponent({
    remoteEntry: 'http://localhost:4201/remoteEntry.mjs',
    remoteName: 'mfeDashboard',
    exposedModule: './Component'
  })
}
```

### 4. Validar

* Asegurar carga del componente standalone sin `NgModule`.

---

## ✅ Cierre

Este laboratorio establece las bases comparativas de los dos modelos **naturales** de Federation en Angular. En el **Día 3**, se abordará la **tercera variante: Federation con Nx**, comparando:

* Automatización
* Reutilización estructural
* Despliegue segmentado
