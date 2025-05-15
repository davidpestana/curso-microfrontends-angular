# 🧪 Sesión 4 – Angular Elements y Web Components

## 🎯 Objetivos de la sesión

- Comprender el concepto de `Custom Elements` y su interoperabilidad en arquitecturas MFE.
- Exponer componentes Angular como Web Components reutilizables.
- Empaquetar, publicar e integrar Angular Elements en entornos desacoplados.
- Identificar límites de esta aproximación frente a otras como Module Federation.

---

## 📚 Introducción teórica

Angular Elements permite transformar componentes Angular en Web Components estándar del navegador (Custom Elements), lo que facilita su integración en otras aplicaciones, incluso fuera de Angular.

Esta técnica es útil para:
- Compartir widgets autónomos (ej: botones, formularios, tablas).
- Integrarse en portales legacy o CMS.
- Publicar como artefactos (ej: NPM, CDN) sin acoplamiento al framework padre.

---

## 🧰 Prerrequisitos técnicos

- Haber completado la sesión 3 con `dashboard`, `profile`, `shell`, `libs/ui` y `libs/core`.
- Node y Angular CLI instalados en el entorno.
- Build funcional verificado (`nx build dashboard/profile` sin errores).

---

## 🧱 Fases del laboratorio

### 🔹 Fase 1 – Preparar el entorno de Elements

- Seleccionar `dashboard` como app base para exponer un componente.
- Verificar `AppModule` y declarar el componente objetivo (ej: `UserCardComponent`).
- Crear un nuevo módulo `UserCardElementModule` con `ngDoBootstrap`.

### 🔹 Fase 2 – Crear e instanciar el Custom Element

- Usar `createCustomElement` con el `Injector` para exponer `UserCardComponent`.

```ts
const el = createCustomElement(UserCardComponent, { injector });
customElements.define('app-user-card', el);
```

- Modificar `main.ts` para bootstrapping manual.

### 🔹 Fase 3 – Ajustar la configuración del build

- Instalar `ngx-build-plus` para personalizar el bundle.

```bash
ng add ngx-build-plus
```

- Configurar build para generar un único `main.js` sin `output-hashing`:

```bash
ng build dashboard --prod --single-bundle true --output-hashing=none
```

- Comprobar que `main.js` puede ser cargado como script único.

### 🔹 Fase 4 – Crear entorno de prueba externo

- Crear carpeta `external-test/` con `index.html` plano.
- Incluir el `main.js` generado y el tag `<app-user-card></app-user-card>`.
- Validar carga en navegador sin errores.

### 🔹 Fase 5 – Extraer e integrar desde otra app

- Copiar el `main.js` en el directorio público de `shell`.
- Inyectar dinámicamente el componente desde `shell` vía `appendChild` o `innerHTML`.
- Confirmar que el componente renderiza correctamente desde app externa.

### 🔹 Fase 6 – Repetir con un segundo componente

- Repetir fases 1–5 para `profile`, exponiendo `UserPreferencesComponent`.
- Publicar ambos como recursos en el mismo `shell` y cargar de forma independiente.

---

## 🧪 Retos propuestos

1. Crear un Web Component reutilizable desde `profile` (ej: `UserPreferencesComponent`).
2. Publicar ambos bundles en una carpeta accesible por `shell` como recursos estáticos.
3. Inyectarlos dinámicamente desde el `shell` usando `appendChild` o `innerHTML`.
4. Modificar los componentes para aceptar `@Input()` vía atributos HTML.

---

## ✅ Validaciones

- Comprobación de que los elementos se pueden usar en apps no Angular.
- Validación de que el bundle no contiene múltiples copias de Angular (usa `external` si es necesario).
- Verificación de interoperabilidad mínima entre shells distintos.
- Aceptación de `@Input()` y respuesta visual.

---

## 🧠 Reflexión final 

- ¿Qué ganamos y qué perdemos usando Angular Elements frente a MFEs federados?
- ¿Qué tipo de componentes o proyectos son más adecuados para este enfoque?
- ¿Cuáles son las limitaciones prácticas (size, estilos, dependencias)?

