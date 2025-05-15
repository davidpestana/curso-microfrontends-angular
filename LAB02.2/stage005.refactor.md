## 🧱 Fase 5 – Evaluación técnica y reconfiguración federada

**🌟 Objetivo:**
Analizar los cambios tras la refactorización de `mfe-settings` de modular a standalone, con énfasis en las ventajas de este enfoque y la evaluación técnica de las implicaciones en la federación.

---

### 📜 Descripción

En esta fase, el alumno evaluará las diferencias en la configuración y comportamiento de las aplicaciones tras la refactorización de un MFE modular a standalone. Se analizarán ventajas, problemas y características en la federación.

---

### 🧍‍♂️ Pasos

1. **Refactorizar `mfe-settings` de modular a standalone:**

   * Convertir `AppModule` en un componente standalone.
   * Eliminar `NgModule`.
   * Actualizar `webpack.config.js` para exponer el componente standalone.

   ```ts
   exposes: {
     './Component': './src/app/pages/settings/settings.component.ts'
   }
   ```

2. **Configurar Shell para consumir el componente standalone:**

   ```ts
   {
     path: 'settings',
     loadComponent: () => loadRemoteModule({
       type: 'module',
       remoteEntry: 'http://localhost:4202/remoteEntry.js',
       exposedModule: './Component'
     }).then(m => m.SettingsComponent)
   }
   ```

3. **Revisar y comparar la estructura de `remoteEntry.js` antes y después:**

   * Comprobar que solo se exporta un componente en lugar de un módulo.
   * Verificar que el componente funciona correctamente desde el Shell.

4. **Realizar pruebas de rendimiento:**

   * Medir el tamaño del bundle y el impacto de la refactorización.
   * Comparar la latencia de carga entre un módulo federado y un componente standalone.

5. **Documentar diferencias:**

   * ¿Qué ventajas ofrece federar componentes standalone en comparación con módulos?
   * ¿Cómo impacta esto en la escalabilidad y en la complejidad del proyecto?

---

### 🔧 Retos

* Añadir una segunda ruta en `mfe-settings` como componente standalone y federarla.
* Probar la federación de un tercer MFE modular convertido a standalone y evaluar su integración.
* Crear un informe técnico que compare el enfoque modular vs standalone para federación.

---

### ✅ Validaciones

* El componente `SettingsComponent` se carga correctamente desde el Shell.
* La estructura de `remoteEntry.js` refleja correctamente la exportación del componente standalone.
* El alumno ha analizado las diferencias entre el enfoque modular y standalone, y puede justificar las elecciones de arquitectura.
