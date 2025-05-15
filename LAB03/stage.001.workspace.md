# 📘 Sesión 3 – Escalabilidad y arquitectura federada con Nx + Module Federation

## 🎯 Objetivo de la sesión

Comprender y aplicar una arquitectura de microfrontends federada en Angular usando Nx como herramienta de escalabilidad y gestión modular. Se implementará un monorepo con una aplicación Shell y dos remotos, usando `@angular-architects/module-federation`.

---

## 🧪 Laboratorio guiado – Fase 1: Creación del workspace y configuración inicial

### 🎯 Objetivo de la fase

Establecer la base técnica del sistema de microfrontends creando un Nx Workspace que incluya una aplicación Shell y dos aplicaciones Remote. Esta fase no contempla aún federación funcional, pero sienta los cimientos para aplicar module federation en fases posteriores.

---

### 🧱 Estructura que se construirá

```
mfe-workspace/
├── apps/
│   ├── shell/
│   ├── dashboard/
│   └── profile/
├── libs/
└── workspace.json
```

Cada app contendrá su `app.module.ts`, `main.ts`, `webpack.config.js` y estructura de routing básica.

---

### 🛠 Pasos detallados

1. **Inicializar el workspace Nx**

   ```bash
   npx create-nx-workspace@latest mfe-workspace --preset=angular
   cd mfe-workspace
   ```

   * Nombrar la app inicial como `shell`
   * Confirmar que se generan las carpetas `apps/` y `libs/`

2. **Generar los remotes (sin federar aún)**

   ```bash
   nx g @nrwl/angular:remote dashboard --host=shell
   nx g @nrwl/angular:remote profile --host=shell
   ```

   * Esto genera configuración inicial de Webpack, rutas, y apps independientes

3. **Validar la estructura generada**

   * Revisar `apps/shell/project.json`, `webpack.config.js`, `main.ts`
   * Verificar que las rutas `/dashboard` y `/profile` están registradas en el router del shell

4. **Servir las aplicaciones para validación**

   ```bash
   nx serve shell
   nx serve dashboard
   nx serve profile
   ```

   * Abrir cada una en navegador (por defecto: 4200, 4201, 4202)
   * Confirmar que muestran contenido base y están enlazadas

---

### 🔍 Validaciones clave

* ¿Se generaron correctamente las apps en `apps/`?
* ¿Cada app se puede lanzar de forma independiente con `nx serve`?
* ¿El shell tiene enrutamiento activo hacia los remotes aunque aún no federen?
* ¿Los puertos son coherentes y no hay conflictos?

---

### 🧩 Retos para el alumno

* Comprobar si el shell realmente carga algo desde los remotes o solo los enlaza como placeholders
* Validar si puede modificar algo en una de las remotas y ver el resultado sin recompilar todo
* Inspeccionar el árbol de dependencias con `nx graph`

---

### ✅ Resultado esperado

* Nx Workspace operativo con 3 apps independientes (Shell, Dashboard, Profile)
* Configuración de `webpack.config.js` inicial disponible
* Navegación base funcional y entorno listo para integrar federation
