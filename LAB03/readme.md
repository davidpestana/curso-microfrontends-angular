# 📘 Sesión 3 – Escalabilidad y arquitectura federada con Nx + Module Federation

## 🎯 Objetivo de la sesión

Comprender y aplicar una arquitectura de microfrontends federada en Angular usando Nx como herramienta de escalabilidad y gestión modular. Se implementará un monorepo con una aplicación Shell y dos remotos, usando `@angular-architects/module-federation`.

---

## 📦 Contenidos

### 1. Introducción a Nx y monorepos

* Qué es Nx y cómo organiza aplicaciones y librerías
* Ventajas del monorepo para microfrontends
* Estructura típica: `apps/`, `libs/`
* Independencia y reusabilidad por dominio funcional

### 2. Angular + Module Federation

* Instalación y uso del plugin `@angular-architects/module-federation`
* Concepto de Shell y Remotes
* Exposición de módulos y carga dinámica
* Compartición de dependencias comunes

### 3. Buenas prácticas de diseño MFE

* Separación por contextos (bounded contexts)
* Aislamiento de rutas, dependencias y responsabilidades
* Comunicación indirecta entre MFEs

---

## 🧪 Laboratorio guiado (por fases)

### Fase 1 – Creación del workspace y configuración inicial

* Estructura básica del proyecto Nx
* Generación de apps Shell y Remotes
* Preparación del entorno de trabajo

### Fase 2 – Integración de Module Federation

* Configuración de Webpack en Shell y Remotes
* Exposición y consumo de módulos federados
* Configuración de rutas cargadas dinámicamente

### Fase 3 – Librerías compartidas y diseño modular

* Creación de librerías comunes (UI, lógica)
* Integración desde múltiples apps
* Validación de la no duplicación en bundle final

### Fase 4 – Validación y análisis de rendimiento

* Verificación del comportamiento federado
* Análisis de carga de bundles y dependencias
* Observaciones sobre escalabilidad y mantenimiento

---

## 🧩 Resultados esperados

* Shell funcional con rutas federadas
* Dos MFEs integrados como remotos
* Librerías comunes compartidas sin duplicación
* Flujo escalable y modular en Angular con Nx y Module Federation

---

## 📊 Indicadores de impacto estimado (organización)

| Métrica              | Estimación | Justificación técnica                                  |
| -------------------- | ---------- | ------------------------------------------------------ |
| **Autonomía**        | ↑ Alta     | Mayor independencia por equipo, ciclo de vida separado |
| **Tiempo de deploy** | ↓ 50%      | Menores tiempos de build y despliegues más segmentados |
| **Escalabilidad**    | ↑ 25%      | Separación clara de dominios, módulos autónomos        |
| **Coordinación**     | ↓ 40%      | Menor necesidad de sincronización entre equipos        |

---

## 📚 Comparativa de herramientas de monorepo

| Herramienta         | Soporte Angular | Estructura apps/libs | Enforce boundaries | CI optimizado (`affected`) | Visualización dependencias | Integración MFEs (Angular) |
| ------------------- | --------------- | -------------------- | ------------------ | -------------------------- | -------------------------- | -------------------------- |
| **Nx**              | ✅ Oficial       | ✅ apps/, libs/       | ✅ Sí               | ✅ Sí                       | ✅ `nx graph`               | ✅ Con plugin oficial       |
| **Lerna**           | ⚠️ Parcial      | 🚫 Solo paquetes NPM | ❌ No               | ❌ No                       | ❌ No                       | ❌ Manual                   |
| **Turborepo**       | ⚠️ Genérico     | ✅ apps/, packages/   | ❌ Parcial          | ✅ Sí (pero más simple)     | ⚠️ Parcial (`turbo graph`) | ❌ Manual                   |
| **Pnpm Workspaces** | ❌ No            | 🚫 Solo packages     | ❌ No               | ❌ No                       | ❌ No                       | ❌ Manual                   |

> 📝 Nota: Para proyectos Angular con MFEs y estructuras modulares por dominio, **Nx** es la opción más completa y productiva.
