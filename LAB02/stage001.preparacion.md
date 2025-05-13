## 🧪 Fase 1: Preparación de entorno Angular modular desde cero

### 🌟 Objetivo

Bootstrapear un proyecto Angular base (sin Nx inicialmente) con una arquitectura modular clásica. Este entorno servirá como punto de partida para la migración progresiva a Angular Standalone y luego a Microfrontend.

---

### 📜 Descripción

Partimos desde cero, utilizando `npx` y el CLI de Angular para generar una aplicación con estructura modular, incluyendo:

* Un módulo `UserModule` con su componente asociado
* Routing tradicional con `loadChildren`
* Separación clara de responsabilidades

> 💡 **Nota importante**: A partir de Angular 17, el CLI genera por defecto aplicaciones en modo `standalone`. Para crear una aplicación modular clásica es necesario usar el flag `--no-standalone` en el comando `ng new`. Aun así, Angular permite arquitecturas **híbridas**: una app puede tener componentes standalone y módulos clásicos coexistiendo. Lo importante es cómo se realiza el **bootstrap** inicial de la aplicación.

Este laboratorio simula el escenario legacy habitual en empresas, donde se parte de una base modular.

---

### 🧍‍♂️ Pasos

1. **Crear el proyecto Angular en modo modular**

   ```bash
   npx @angular/cli new angular-mf --routing --style=scss --no-standalone
   cd angular-mf
   ```

2. **Generar un módulo funcional separado**

   ```bash
   ng generate module features/user --route user --module app.module
   ng generate component features/user/pages/user-list
   ```

3. **Comprobar estructura del proyecto**
   Deben existir:

   * `src/app/features/user/user.module.ts`
   * `src/app/features/user/pages/user-list/user-list.component.ts`
   * Entradas de ruta en `app-routing.module.ts` con `loadChildren`

4. **Agregar contenido mínimo**
   En `user-list.component.html`:

   ```html
   <h2>User List (modular)</h2>
   ```

5. **Levantar el proyecto y validar navegación**

   ```bash
   ng serve
   ```

   * Acceder a `/user` y comprobar que se carga correctamente el componente.

6. **Commit opcional del estado inicial**

   ```bash
   git init && git add . && git commit -m "Initial modular setup"
   ```

---

### ⚠️ Retos

* Evitar errores al usar rutas y módulos anidados desde el CLI.
* Comprobar que el módulo es completamente autónomo y no depende de otros features.
* No añadir componentes directamente en `AppModule`.
* Comprender la diferencia entre apps bootstrapeadas como standalone vs modulares.

---

### ✅ Validaciones

* El proyecto se ejecuta correctamente.
* Se puede navegar a `/user` y renderiza un componente funcional.
* `UserModule` está cargado de forma lazy vía `loadChildren`.
* La estructura es coherente con una app modular real.
* El alumno comprende cómo influye el modo de bootstrap en la arquitectura final.
