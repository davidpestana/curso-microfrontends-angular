## 🧪 Laboratorio comparativo de Microfrontends con librerías reales

**🎯 Objetivo general:**
Explorar distintas estrategias y librerías modernas para implementar Microfrontends (MFE) en entornos sin frameworks SPA. Se parte de una arquitectura basada en `live-server`, sirviendo cada MFE por un puerto distinto, y se evoluciona progresivamente hacia soluciones más sofisticadas de carga, composición y federación. Cada fase del laboratorio introduce una técnica o librería real distinta, permitiendo comparar sus ventajas y limitaciones de forma práctica.

El enfoque es incremental: se parte de una solución mínima basada en `import()` y se prueba cada técnica con los mismos MFEs. Se excluyen soluciones como Angular o React hasta el final. Solo en la última fase se introduce `Webpack Module Federation`, la opción adoptada por Angular para proyectos reales.

---

## 🧮 Comparativa de librerías utilizadas en el laboratorio

| Librería / Técnica                         | ¿Qué ofrece?                                                                 | ¿Cuándo usarla?                                                |
| ------------------------------------------ | ---------------------------------------------------------------------------- | -------------------------------------------------------------- |
| `import()` + `CustomEvent`                 | Carga remota básica, sin dependencias.                                       | Ideal para prototipos rápidos o PoC de arquitectura.           |
| `event-bus.js` compartido                  | Abstracción mínima para emitir/recibir eventos desacoplados.                 | Aplicaciones con múltiples MFEs vanilla y shell común.         |
| [`single-spa`](https://single-spa.js.org/) | Registro, ciclo de vida y orquestación controlada.                           | Apps distribuidas por equipos, necesidad de montaje/descarga.  |
| [`qiankun`](https://qiankun.umijs.org/)    | Sandbox completo, carga HTML+JS+CSS, soporte de Shadow DOM.                  | Aplicaciones de gran escala con fuerte aislamiento entre MFEs. |
| `SystemJS` + `import maps`                 | Resolución y carga dinámica de módulos por nombre.                           | Integración dinámica en tiempo de ejecución sin bundling.      |
| `Webpack Module Federation`                | Federación nativa, dependencias compartidas, integración oficial en Angular. | Proyectos empresariales con Angular, React o Next.js.          |

## 📚 Índice de fases del laboratorio

A continuación se listan las fases del laboratorio junto a los enlaces a cada archivo y una explicación resumida del alcance de cada técnica o librería utilizada.

* **Fase 1 – Preparación del laboratorio y MFEs base**
  [Bootstrap mínimo. Carga dinámica desde puertos distintos.](./stage001.setup-base-mfes.md)

* **Fase 2 – Event Bus compartido**
  [Desacoplar comunicación shell/MFE mediante bus común.](./stage002.event-bus-abstraction.md)

* **Fase 3 – Integración con `single-spa` (modo Vanilla)**
  [Primer framework real: ciclo de vida y registro controlado](./stage003.ecosystem-single-spa-vanilla.md)

* **Fase 4 – Integración con `qiankun` (modo sin framework)**
  [Sandbox, estilo aislado, múltiples MFEs coexistentes.](./stage004.qiankun-vanilla.md)

* **Fase 5 – Composición con `SystemJS` e import maps**
  [Composición dinámica y gestión de versiones.](./stage005.systemjs-importmaps.md)

* **Fase 6 – Federación real con Webpack Module Federation**
  [Federación real, dependencias compartidas, remotes.](./stage006.module-federation.md)