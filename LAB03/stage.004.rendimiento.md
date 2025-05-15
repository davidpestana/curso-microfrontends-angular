# 📘 Sesión 3 – Escalabilidad y arquitectura federada con Nx + Module Federation

## 🎯 Objetivo de la sesión

Comprender y aplicar una arquitectura de microfrontends federada en Angular usando Nx como herramienta de escalabilidad y gestión modular. Se implementará un monorepo con una aplicación Shell y dos remotos, usando `@angular-architects/module-federation`.

---

## 🧪 Laboratorio guiado – Fase 4: Validación y análisis de rendimiento

### 🎯 Objetivo de la fase

Comprobar que la federación y la compartición de librerías se han configurado correctamente, y analizar el rendimiento del sistema desde el punto de vista de carga de bundles, uso de red y tamaño del shell.

---

### 🧱 Estructura estable

```
mfe-workspace/
├── apps/
│   ├── shell/        # Federación funcional con carga remota activa
│   ├── dashboard/    # Remote funcional, usando librerías compartidas
│   └── profile/      # Remote funcional, usando librerías compartidas
├── libs/
│   ├── ui/           # Componente común en uso real
│   └── core/         # Servicio compartido
```

La infraestructura de microfrontends está lista para auditoría de eficiencia.

---

### 🛠 Pasos detallados

1. **Medir el tamaño de los bundles**

   ```bash
   npm install -g source-map-explorer
   source-map-explorer dist/apps/shell/main.js
   ```

   * Revisar si `libs/ui`, `libs/core` y `@angular/*` aparecen solo una vez

2. **Comparar resultados entre apps**

   * Ejecutar también en `dashboard` y `profile`
   * Comparar la carga de librerías duplicadas (o no) entre MFEs

3. **Verificar lazy loading real en red**

   * Abrir DevTools del navegador > pestaña Network
   * Acceder desde el Shell a `/dashboard` y `/profile`
   * Observar si se cargan dinámicamente `remoteEntry.js` y módulos

4. **Validar estrategia de `singleton`**

   * Confirmar en consola que no hay warnings de dependencias duplicadas
   * Revisar `webpack` para ver si las versiones coinciden entre MFEs

5. **Auditoría visual del grafo de dependencias**

   ```bash
   nx graph
   ```

   * Verificar dependencias desde Shell a `dashboard`, `profile`, `libs/ui`, `libs/core`

---

### 🔍 Validaciones clave

* ¿Los remotes cargan solo lo necesario, bajo demanda?
* ¿Se eliminó duplicación de Angular y librerías propias?
* ¿La navegación entre MFEs es fluida y sin recarga?
* ¿El `remoteEntry.js` se descarga correctamente?

---

### 🧩 Retos para el alumno

* Ejecutar `nx affected:libs` tras modificar `libs/ui` y analizar los resultados

  * 💡 *Tip*: Asegúrate de cambiar algo pequeño como el color de un botón
* Provocar una incompatibilidad de versión entre remotes y Shell

  * 💡 *Tip*: Baja manualmente la versión de `@angular/common` en `profile` y reconstruye
* Analizar la red al navegar directamente a un remote sin pasar por el Shell

  * 💡 *Tip*: Intenta acceder a `localhost:4201` y analiza diferencias con acceso federado

---

### ✅ Resultado esperado

* Confirmación de carga eficiente de módulos federados
* Verificación del lazy loading y estrategia singleton
* Herramientas de análisis aplicadas correctamente
* Fundamento listo para aplicar mejoras de rendimiento adicionales
