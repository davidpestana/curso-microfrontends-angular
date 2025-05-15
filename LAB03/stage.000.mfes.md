# 📘 Sesión 3 – Escalabilidad y arquitectura federada con Nx + Module Federation

## 🎯 Objetivo de la sesión

Comprender y aplicar una arquitectura de microfrontends federada en Angular usando Nx como herramienta de escalabilidad y gestión modular. Se implementará un monorepo con una aplicación Shell y dos remotos, usando `@angular-architects/module-federation`.

---

## 🧪 Laboratorio guiado – Fase 0: Construcción base de MFEs

### 🎯 Objetivo de la fase

Proporcionar estructura mínima a los remotos (`dashboard` y `profile`) para que tengan contenido funcional y visual propio, sirviendo como punto de partida para la federación.

---

### 🛠 Pasos clave

1. **Crear un componente principal en cada remoto**

   ```bash
   nx g c dashboard/dashboard --project=dashboard
   nx g c profile/profile --project=profile
   ```

2. **Usar esos componentes en los templates principales**

   * En `apps/dashboard/src/app/app.component.html`:

     ```html
     <app-dashboard></app-dashboard>
     ```
   * En `apps/profile/src/app/app.component.html`:

     ```html
     <app-profile></app-profile>
     ```

3. **Código del componente Dashboard**

   * `dashboard.component.ts`

     ```ts
     import { Component } from '@angular/core';

     @Component({
       selector: 'app-dashboard',
       templateUrl: './dashboard.component.html',
       styleUrls: ['./dashboard.component.css']
     })
     export class DashboardComponent {}
     ```

   * `dashboard.component.html`

     ```html
     <section class="dashboard">
       <h2>Dashboard MFE</h2>
       <p>Este es el contenido del microfrontend Dashboard.</p>
       <button (click)="alert('Dashboard activo')">Acción</button>
     </section>
     ```

   * `dashboard.component.css`

     ```css
     .dashboard {
       padding: 2rem;
       background-color: #e3f2fd;
       border: 2px solid #90caf9;
       border-radius: 8px;
     }

     h2 {
       color: #1976d2;
     }
     ```

4. **Código del componente Profile**

   * `profile.component.ts`

     ```ts
     import { Component } from '@angular/core';

     @Component({
       selector: 'app-profile',
       templateUrl: './profile.component.html',
       styleUrls: ['./profile.component.css']
     })
     export class ProfileComponent {}
     ```

   * `profile.component.html`

     ```html
     <section class="profile">
       <h2>Profile MFE</h2>
       <p>Este es el contenido del microfrontend Profile.</p>
       <button (click)="alert('Profile activo')">Acción</button>
     </section>
     ```

   * `profile.component.css`

     ```css
     .profile {
       padding: 2rem;
       background-color: #f3e5f5;
       border: 2px solid #ce93d8;
       border-radius: 8px;
     }

     h2 {
       color: #8e24aa;
     }
     ```

5. **Validar visualmente que cada app tiene aspecto propio**

---

### ✅ Resultado esperado

* Dashboard y Profile muestran interfaz clara, diferenciada y funcional
* Punto de partida visual listo para ser federado en el Shell

---

### 🧩 Retos para el alumno

* Validar que cada app (`dashboard`, `profile`) se ejecuta de forma independiente

  * 💡 *Tip*: Usa `ng serve` para lanzar cada una y verifica en el navegador
* Comprobar que el componente principal reacciona al botón con su `alert()`

  * 💡 *Tip*: Si no responde, revisa bindings, errores de consola o cambios no guardados
* Modificar colores o textos de cada app para reafirmar su identidad

  * 💡 *Tip*: Usa CSS embebido en los componentes para aislar estilos fácilmente
