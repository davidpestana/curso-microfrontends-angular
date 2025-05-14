## 🧱 Fase 3 – Federation de MFE standalone (mfe-profile)

**🌟 Objetivo:**
Federar e integrar únicamente la aplicación `mfe-profile`, construida como aplicación Angular standalone, usando `@angular-architects/module-federation`. Este será el primer MFE federado en el Shell.

---

### 📜 Descripción

En esta fase se integrará el primer Microfrontend real en el Shell. Se trata de una app standalone (sin `NgModule`), expuesta como componente y consumida dinámicamente mediante `loadComponent`. Se configura el entorno de federation para este caso específico y se valida toda la cadena.

---

### 🧍‍♂️ Pasos

1. **Instalar el plugin en `shell` y `mfe-profile`:**

```bash
ng add @angular-architects/module-federation --project shell --type host
ng add @angular-architects/module-federation --project mfe-profile --type remote --port 4203
```

2. **Exponer el componente en `mfe-profile`:**
   Editar `webpack.config.js`:

```ts
exposes: {
  './Component': './src/app/pages/profile.component.ts'
}
```

3. **Verificar que `profile.component.ts` es `standalone: true`:**

```ts
@Component({
  selector: 'app-profile',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './profile.component.html',
  styleUrls: ['./profile.component.scss']
})
export class ProfileComponent {
  name = 'Jane Doe';
  role = 'Frontend Engineer';
}
```

4. **Modificar rutas en Shell:**

```ts
{
  path: 'profile',
  loadComponent: () => loadRemoteModule({
    type: 'module',
    remoteEntry: 'http://localhost:4203/remoteEntry.js',
    exposedModule: './Component'
  }).then(m => m.ProfileComponent)
}
```

5. **Configurar `shared` en el Shell (`webpack.config.js`):**

```ts
shared: {
  '@angular/core': { singleton: true, strictVersion: true },
  '@angular/common': { singleton: true, strictVersion: true },
  '@angular/router': { singleton: true, strictVersion: true },
}
```

6. **Ejecutar y validar:**

```bash
npm run start:shell
npm run start:mfe-profile
```

---

### 🔧 Retos

* Cambiar el contenido del componente `ProfileComponent` para que muestre una imagen o avatar.
* Añadir una segunda ruta standalone en `mfe-profile` y federarla como `./ComponentB` (sin integrarla todavía).
* Inspeccionar la carga dinámica en DevTools y confirmar que solo se descarga `remoteEntry.js` al acceder a `/profile`.

---

### ✅ Validaciones

* El Shell carga `/profile` dinámicamente sin errores
* El componente standalone se muestra correctamente
* La estructura standalone no requiere `NgModule`
* La federación funciona con `loadComponent()`
* El alumno entiende cómo federar componentes individuales
