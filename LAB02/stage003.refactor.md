## 🧪 Fase 3: Migración de MFE modular a standalone e integración comparada

### 🌟 Objetivo

Refactorizar una aplicación Angular modular existente (`mfe-refactored`) para convertirla completamente en standalone. A continuación, se volverá a integrar desde el Shell para validar los beneficios obtenidos frente a la versión modular anterior.

---

### 📜 Descripción

La app `mfe-refactored`, creada como aplicación modular clásica, será migrada a standalone. El alumno aplicará los conocimientos adquiridos para:

* Convertir el componente principal y sus rutas a formato standalone.
* Eliminar gradualmente el uso de `NgModule`.
* Revalidar la integración desde el Shell para confirmar:

  * Menor acoplamiento
  * Reducción de dependencias
  * Mayor claridad en la integración y exportación de funcionalidades

Este laboratorio reproduce un caso real común: **actualizar una app Angular legacy para convertirla en candidata a MFE sin reescribirla desde cero**.

---

### 🧍‍♂️ Pasos detallados

#### 🔧 Paso 1: Migrar el componente `DashboardComponent` a standalone

* Modificar el decorador `@Component`:

```ts
@Component({
  standalone: true,
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: ['./dashboard.component.scss'],
  imports: [CommonModule] // Añadir cualquier dependencia necesaria
})
```

* Eliminar su declaración desde `DashboardModule`.

#### 🧹 Paso 2: Sustituir `loadChildren` por `loadComponent` en `app.routes.ts`

```ts
{
  path: 'dashboard',
  loadComponent: () => import('./dashboard/pages/dashboard/dashboard.component').then(m => m.DashboardComponent)
}
```

#### 🧻 Paso 3: Eliminar el módulo si ya no se usa

* Si `DashboardModule` ya no contiene nada útil:

```bash
rm -rf src/app/dashboard/dashboard.module.ts
```

* Asegúrate de eliminar también su importación desde `app.module.ts` si existía.

#### 🧪 Paso 4: Reconstruir y validar la app

```bash
ng build --base-href /mfe-refactored/
```

* Servir desde el Shell y probar `/mfe-refactored/main.js` vía el botón "Refactorizada".
* Verificar que la carga dinámica funciona sin el `NgModule`.

#### 🔁 Paso 5: Comparar antes y después

* ¿El tamaño del bundle cambió?
* ¿Se simplificaron los imports?
* ¿Cambió la experiencia de integración?

---

### ✅ Validaciones

* `mfe-refactored` sigue funcionando tras la migración a standalone.
* No quedan referencias al `NgModule` eliminado.
* El Shell lo integra sin necesidad de modificar su lógica.
* El alumno observa una mejora real en la estructura y simplicidad del MFE.

---

### ⚠️ Retos para el alumno

* Repetir el proceso de migración con otra feature secundaria (crear un segundo componente y módulo auxiliar y migrarlo).
  *Tip:* Usa `ng generate module` y luego elimina progresivamente el `NgModule`.
* Añadir un botón al Shell para verificar desde consola si el MFE refactorizado fue migrado correctamente (`typeof module.DashboardComponent.standalone`).
  *Tip:* Exponer la clase como propiedad y comprobar en tiempo de ejecución.
* Adaptar el `render()` del MFE refactorizado para usar solo APIs standalone.
  *Tip:* Usa `createComponent()` desde Angular internamente si lo deseas, o mantén un wrapper limpio de DOM.

---

> 🧭 Esta fase cierra el ciclo de comparación entre apps modulares y standalone en contextos MFE. Las siguientes fases abordarán arquitectura con Nx y federation real.
