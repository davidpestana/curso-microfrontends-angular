## 🧪 Fase 6: Gestión de estado entre Microfrontends con NGRX y Signal Store

### 🌟 Objetivo

Explorar distintas estrategias para gestionar estado global y local entre múltiples MFEs Angular. En esta fase se trabajará con:

* `@ngrx/store` (gestión de estado clásica)
* `@ngrx/signals` (Signal Store)
* Contexto de estado entre Shell y Remotes federados

Se busca implementar una arquitectura compartida pero desacoplada, que permita mantener la independencia entre MFEs sin sacrificar coherencia de datos.

---

### 🧠 Fundamentos clave

* **NGRX Store**: librería basada en Redux que centraliza el estado con reducers, actions y selectors.
* **NGRX Signal Store**: API moderna basada en Signals, reactiva, simple y con menor boilerplate.
* **State sharing entre MFEs**: puede hacerse vía:

  * Librerías compartidas (`shared-state-store`)
  * Event emitters entre MFEs
  * Comunicación a través del Shell

---

### 📜 Descripción

El alumno configurará dos tipos de estado:

* Estado global compartido entre el Shell y los MFEs (ej: usuario autenticado)
* Estado local en cada MFE para datos propios (ej: UI interna, configuración de vistas)

Se demostrará cómo integrar ambos enfoques con buenas prácticas.

---

### 🧍‍♂️ Pasos detallados

#### 📦 Paso 1: Crear una librería de estado compartido en Nx

```bash
nx g @nrwl/angular:lib shared-state --directory=shared --importPath=@shared/state
```

#### 🧠 Paso 2: Implementar un Signal Store simple

* En `shared-state/src/lib/user.store.ts`:

```ts
import { signalStore, withState } from '@ngrx/signals';

export const useUserStore = signalStore(
  withState(() => ({
    username: '',
    isAdmin: false,
  }))
);
```

#### 🔗 Paso 3: Importar el store en el Shell y actualizar estado global

```ts
const user = useUserStore();
user.patch({ username: 'admin' });
```

#### 🔀 Paso 4: Leer estado desde un Remote federado

* En `mfe-dashboard`, importar el store desde `@shared/state` y mostrar su valor:

```ts
const user = useUserStore();
console.log('Usuario:', user.username());
```

#### 🧪 Paso 5: Alternativamente usar NGRX Store clásico para otra feature

* Crear slice `feature-counter` dentro del MFE `settings`:

```bash
nx g @ngrx/slice counter --project=mfe-settings
```

* Añadir `StoreModule.forFeature()` y `EffectsModule.forFeature()` si aplica

#### 🧭 Paso 6: Comparar ambos enfoques

* Signal Store vs Store clásico
* Boilerplate, legibilidad, testing, trazabilidad, herramientas DevTools

---

### ✅ Validaciones

* Shell y MFEs comparten correctamente estado reactivo con Signal Store.
* Cada MFE puede modificar y leer su propio estado sin colisión.
* Se valida la utilidad de librerías compartidas para mantener coherencia.
* Se comprende cuándo conviene usar Signal Store o NGRX clásico.

---

### ⚠️ Retos para el alumno

* Añadir un nuevo slice `theme` al Shell usando Signal Store con `darkMode: boolean`.
  *Tip:* Crea un signal `toggle()` y vincúlalo a un botón del layout.

* Enviar un evento desde `mfe-dashboard` que actualice el estado global en el Shell.
  *Tip:* Usa `CustomEvent` y `window.dispatchEvent()` o una función inyectada vía props.

* Añadir `NgrxStoreDevtoolsModule.instrument()` en `main.ts` del Shell y activar Redux DevTools.
  *Tip:* Requiere importar `StoreDevtoolsModule` desde `@ngrx/store-devtools` y establecer `maxAge` y `logOnly`.

---

> 📊 Esta fase permite manejar el estado de forma coherente entre aplicaciones Angular distribuidas, minimizando el acoplamiento y maximizando la trazabilidad.
