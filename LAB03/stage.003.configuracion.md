# 📘 Sesión 3 – Escalabilidad y arquitectura federada con Nx + Module Federation

## 🎯 Objetivo de la sesión

Comprender y aplicar una arquitectura de microfrontends federada en Angular usando Nx como herramienta de escalabilidad y gestión modular. Se implementará un monorepo con una aplicación Shell y dos remotos, usando `@angular-architects/module-federation`.

---

## 🧪 Laboratorio guiado – Fase 3: Librerías compartidas y diseño modular

### 🎯 Objetivo de la fase

Extraer componentes y lógica reutilizable desde los remotos hacia librerías compartidas en `libs/`, evitando duplicación de código, y validar la eficacia del sistema de compartición de dependencias en tiempo de ejecución.

---

### 🧱 Estructura que se extenderá

```
mfe-workspace/
├── apps/
│   ├── shell/
│   ├── dashboard/
│   └── profile/
├── libs/
│   ├── ui/         # Componentes comunes (botones, cards, layout)
│   └── core/       # Servicios, modelos, utilidades compartidas
```

Cada remote y el shell harán uso de librerías compartidas desde `libs/`.

---

### 🛠 Pasos detallados

1. **Crear librerías comunes en Nx**

   ```bash
   nx g @nrwl/angular:lib ui
   nx g @nrwl/angular:lib core
   ```

2. **Extraer funcionalidad reutilizable**

   * Mover componentes básicos (`CardComponent`, `ButtonComponent`) a `libs/ui`
   * Mover servicios (`UserService`, `AuthService`) y tipos a `libs/core`

3. **Actualizar remotes y shell para usar las librerías**

   * Importar módulos de `ui` y `core` en `AppModule` de cada app
   * Reemplazar el uso interno por imports desde `libs/`

4. **Configurar compartición de dependencias en federation**

   * En `module-federation.config.js`:

     ```ts
     shared: share({
       '@angular/core': { singleton: true, strictVersion: true },
       '@angular/common': { singleton: true },
       'libs/ui': { singleton: true, import: 'libs/ui' },
       'libs/core': { singleton: true, import: 'libs/core' }
     })
     ```

5. **Reconstruir y servir todas las apps**

   ```bash
   nx run-many --target=build --projects=shell,dashboard,profile
   nx serve shell
   nx serve dashboard
   nx serve profile
   ```

---

### 🔍 Validaciones clave

* ¿Se ha eliminado la duplicación de código compartido en los bundles?
* ¿Las dependencias compartidas están correctamente marcadas como `singleton`?
* ¿Las apps siguen funcionando correctamente tras extraer las librerías?
* ¿`nx graph` refleja ahora dependencias desde apps a `libs/`?

---

### 🧩 Retos para el alumno

* Romper la compartición: modificar el `UserService` en el remote para que no apunte al `lib`

  * 💡 *Tip*: Observa cómo la app se recompila y si hay errores de importación cruzada
* Añadir un nuevo componente a `libs/ui` y reutilizarlo en los tres MFEs

  * 💡 *Tip*: Valida con `nx affected:apps` si se propaga correctamente
* Analizar los bundles finales con `source-map-explorer` buscando duplicaciones

  * 💡 *Tip*: Revisa si `libs/ui` aparece más de una vez en los gráficos

---

### ✅ Resultado esperado

* Arquitectura con librerías extraídas y compartidas entre MFEs
* Duplicación de código eliminada
* Aislamiento de lógica común validado
* Proyecto listo para aplicar optimizaciones y estrategias de rendimiento
