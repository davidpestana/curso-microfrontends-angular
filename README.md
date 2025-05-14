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

### 🔹 Sesión 3 – Angular + Module Federation

- Qué es Module Federation y cómo se integra con Angular
- Shell y remotes, sincronización de rutas, lazy loading compartido
- Comunicación entre MFEs Angular
- **Laboratorio:** Integración de Angular MFEs con Module Federation

---

### 🔹 Sesión 4 – Nx y Monorepos: arquitectura empresarial

- Nx vs MultiRepo
- Organización de dominios y bibliotecas compartidas
- Comunicación desacoplada: servicios comunes, Event Bus
- **Laboratorio:** Reorganización del sistema MFE con Nx

---

### 🔹 Sesión 5 – Refactorización y migración de proyectos legacy

- Estrategias de migración progresiva a MFE
- Angular Elements como wrapper para código legacy
- Exposición segura de módulos y componentes antiguos
- **Laboratorio:** Integración de un proyecto modular clásico como MFE

---

### 🔹 Sesión 6 – Seguridad, despliegue y patrones avanzados

- Seguridad y autenticación en aplicaciones distribuidas
- Single Sign-On (SSO), OAuth2 y OIDC en Angular
- Despliegue independiente de MFEs, estrategia CI/CD
- **Laboratorio final:** Shell + MFEs con rutas protegidas y login común

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

