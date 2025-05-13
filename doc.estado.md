## 🧠 Técnicas de gestión de estado en Angular y MFEs

### Contenidos

La gestión del estado en aplicaciones distribuidas debe equilibrar control, rendimiento y desacoplamiento. Existen múltiples enfoques:

* **Redux/NGRX**: gestión predictiva y centralizada del estado.
* **RxJS puro**: flujos de datos reactivos sin estructura rígida.
* **Signal Store**: minimalismo reactivo nativo en Angular moderno.
* **Custom context services**: objetos singleton compartidos.
* **Eventos globales (`CustomEvent`)**: útil para integración ligera entre MFEs.
* **State containers externos**: uso de localStorage, IndexedDB, o incluso soluciones cloud (Firebase, GraphQL cache).

Comparativa:

| Técnica            | Ventajas                         | Limitaciones                     |
| ------------------ | -------------------------------- | -------------------------------- |
| NGRX               | Escalabilidad, tooling, DevTools | Verbosidad, curva de aprendizaje |
| Signal Store       | Simple, nativo, reactivo         | Aún joven, menos tooling         |
| RxJS manual        | Flexible, bajo nivel             | Difícil de mantener en grande    |
| Custom Services    | Rápido, claro, no externo        | Peligro de acoplamiento          |
| Eventos (`window`) | MFE-friendly, agnóstico          | Sin trazabilidad                 |

### 🧪 Laboratorio básico

1. Implementar un `SharedService` singleton con `BehaviorSubject`.
2. Usar `next()` para emitir valores desde `mfe-one`.
3. Suscribirse a los valores desde `mfe-two` y actualizar vista.
4. Repetir usando `window.dispatchEvent()` y comparar comportamiento.

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
