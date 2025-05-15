# Curso: Microfrontends con Angular

## 🧭 Objetivo del curso

Este curso tiene como objetivo capacitar a desarrolladores con experiencia previa en Angular para comprender, aplicar y escalar arquitecturas basadas en **Microfrontends (MFE)**. A lo largo de 6 sesiones, se abordará desde el modelo vanilla hasta su implementación en Angular con Module Federation, integración en proyectos legacy y despliegue seguro de aplicaciones distribuidas. Se revisarán también conceptos fundamentales de Angular (modular, standalone, DI, routing, lazy loading) en el contexto de MFEs.

---

## 📅 Estructura del curso

- **Duración total:** 6 sesiones (30 horas efectivas)
- **Horario diario:** 09:30 – 14:30 (con descanso de 30 minutos)
- **Modalidad:** Presencial / Online

Cada sesión se divide en **dos bloques principales**: teoría + laboratorio guiado y autogestionado.

---

## 📘 Temario por sesiones

### 🔹 Sesión 1 – [Fundamentos y modelo mental: Microfrontends Vanilla](https://arquitectura-de-microfro-iroaxq1.gamma.site/)

- Qué son los Microfrontends
- Motivaciones, beneficios y riesgos
- Modelos de integración (iframe, runtime, web components)
- Separación por dominio funcional
- Contratos, comunicación y aislamiento
- **Laboratorio:** Implementación de MFEs vanilla con `import()` y comunicación básica

---

### 🔹 Sesión 2.1 – [Laboratorio comparativo de MFE sin framework (Vanilla JS)](https://evaluacion-tecnica-de-en-1t33wzb.gamma.site/)

- Evaluación técnica de múltiples enfoques de integración: `import()`, Event Bus, `single-spa`, `qiankun`, `SystemJS`, y `Webpack Module Federation`
- Comprensión progresiva de cómo se pasa de integración manual a federación avanzada
- Comparativa entre enfoques runtime dinámicos y soluciones basadas en bundler
- **Laboratorio:** Implementación paso a paso de un sistema shell + MFEs en 6 fases técnicas, analizando las ventajas, limitaciones y casos de uso reales de cada enfoque.


### 🔹 Sesión 2.2 – [Angular Modular vs Standalone: bases para MFEs](https://arquitectura-angular-par-wf9w8oc.gamma.site/)

- Revisión de arquitectura Angular clásica (`NgModules`)
- Introducción a Angular Standalone
- Ventajas, limitaciones y estrategias de migración
- **Laboratorio:** Refactor de componentes y rutas de modular a standalone

---

## 🔹 Sesión 3 – [Escalabilidad y arquitectura federada con Nx + Module Federation](https://escalando-angular-con-nx-i4s0qyp.gamma.site/)

### 🎯 Contenidos

* Monorepo con Nx: estructura modular por dominios funcionales
* Comparativa con multirepo: implicaciones en grandes equipos
* Integración de Angular + Webpack Module Federation
* Diseño estratégico de shell y remotes
* Rutas sincronizadas, lazy loading y composición desde el shell

### 🧪 Laboratorio

* Crear un workspace Nx con un shell y dos MFEs federados
* Definir dominios funcionales (`feature/`, `ui/`, `data/`) en libs compartidas
* Cargar remotes desde shell usando rutas perezosas y `loadRemoteModule`
* Validar aislamiento, comunicación y compartición de dependencias

> **Aplicación práctica:** este laboratorio reproduce una arquitectura real a escala empresarial, donde cada dominio puede ser desarrollado y desplegado por equipos separados sin duplicación de lógica.

---

## 🔹 Sesión 4 – [Web Components y compatibilidad con Angular Elements](https://angular-elements-y-web-c-nnj4hln.gamma.site/)

### 🎯 Contenidos

* Angular Elements: convertir cualquier componente Angular en Web Component
* Estrategias de interoperabilidad con otras tecnologías (React, Webpack, JSP)
* Limitaciones y mejores prácticas de Angular Elements
* Integración de Elements en el shell o en MFEs

### 🧪 Laboratorio

* Exportar un componente Angular como Web Component
* Consumirlo desde una app externa (shell en Angular o app externa simulada)
* Aplicar estilos aislados y comunicación por atributos/eventos
* Usar `customElements.define()` de forma controlada desde remotes

> **Aplicación práctica:** permite integrar Angular en portales corporativos heterogéneos o sistemas legacy sin necesidad de reescritura total.

---

## 🔹 Sesión 5 – Rendimiento y arquitectura reactiva con RxJS

### 🎯 Contenidos

* Diagnóstico de problemas de rendimiento: bundle, change detection, carga inicial
* ChangeDetectionStrategy: default vs OnPush
* Desacoplamiento con arquitectura reactiva
* RxJS: patrones profesionales (`shareReplay`, `switchMap`, `auditTime`, etc.)

### 🧪 Laboratorio

* Refactor de un flujo imperativo → reactivo con `BehaviorSubject` y `Observables`
* Aplicar `OnPush` y eliminar bindings costosos
* Analizar el tamaño del bundle por MFE con `source-map-explorer`
* Medir impacto de `preloadingStrategy`, `providedIn: 'root'` y técnicas de tree-shaking

> **Aplicación práctica:** refuerza la calidad de experiencia del usuario y reduce el coste de mantenimiento en grandes ecosistemas MFE.

---

## 🔹 Sesión 6 – Seguridad federada y autenticación distribuida

### 🎯 Contenidos

* SSO en microfrontends: OAuth2 + OIDC
* Uso de tokens y guardias (`canActivate`) en MFEs
* Protección de rutas desde el shell y delegación en remotes
* Carga condicional de MFEs según rol o sesión

### 🧪 Laboratorio

* Simulación de un sistema con login común en el shell
* Compartición del estado de sesión vía librería común (`auth-lib`)
* Protección de rutas privadas en cada MFE federado
* Renderizado condicional de MFEs según permisos

> **Aplicación práctica:** ideal para aplicaciones con múltiples dominios (RRHH, Finanzas, BI...) donde cada módulo tiene su acceso delegado y lógica de visibilidad.

---

## 📎 Requisitos previos

- Nivel medio en Angular y TypeScript
- Conocimientos básicos de RxJS y arquitectura frontend
- Familiaridad con desarrollo modular y herramientas modernas (Node.js, Git)

---

## 🔁 Metodología

- Explicación teórica breve con apoyo visual
- Demostración práctica guiada por el docente
- Laboratorio progresivo y autogestionado
- Resolución de dudas en tiempo real y repaso transversal de Angular

---

## 📦 Recursos adicionales

El repositorio incluye ejemplos base, plantillas de configuración y ejercicios para cada laboratorio. Las sesiones están diseñadas para ser independientes pero encadenadas temáticamente.

