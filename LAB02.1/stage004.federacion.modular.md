## 🧱 Fase 4 – Federation de MFEs modulares (`mfe-dashboard` y `mfe-settings`) con resolución de errores comunes

**🌟 Objetivo:**
Federar los Microfrontends `mfe-dashboard` y `mfe-settings` (ambos modulares) mediante `@angular-architects/module-federation`, enfrentando errores típicos y comprendiendo cómo solucionarlos progresivamente.

---

### 📜 Descripción

Esta fase permite experimentar los errores más habituales al federar módulos en Angular. El objetivo es que el alumno comprenda los requisitos técnicos y las particularidades del ciclo de vida de los módulos federados, enfrentando fallos reales que suelen ocurrir en entornos corporativos o integraciones entre equipos.

---

### 🧍‍♂️ Pasos y errores esperados

1. **Intentar integrar sin usar `loadChildren` en el Shell:**

```ts
{ path: 'dashboard', component: DummyComponent } // ❌
```

➡️ *Error: el MFE no se carga dinámicamente, el Shell no ejecuta `remoteEntry.js`.*

✅ **Solución:** Reemplazar por `loadChildren()` con `loadRemoteModule`:

```ts
{ path: 'dashboard', loadChildren: () => loadRemoteModule(...) }
```

---

2. **Exponer módulo incorrecto o mal tipado en `webpack.config.js`:**

```ts
exposes: { './Module': './src/app/dashboard.module.ts' } // ❌ ruta incorrecta
```

➡️ *Error en consola al cargar la ruta federada.*

✅ **Solución:** Asegurar ruta correcta al módulo lazy:

```ts
exposes: { './Module': './src/app/features/dashboard/dashboard.module.ts' }
```

---

3. **No usar `RouterModule.forChild()` en el módulo remoto:**
   ➡️ *Error: ruta cargada pero sin contenido.*

✅ **Solución:**

```ts
@NgModule({
  imports: [CommonModule, RouterModule.forChild([{ path: '', component: DashboardComponent }])]
})
```

---

4. **No compartir Angular como singleton en `webpack.config.js`:**
   ➡️ *Error: múltiples instancias de Angular. Warnings y fallos de inyección.*

✅ **Solución:** Asegurar configuración:

```ts
shared: {
 '@angular/core': { singleton: true, strictVersion: true },
 '@angular/common': { singleton: true, strictVersion: true },
 '@angular/router': { singleton: true, strictVersion: true },
}
```

---

5. **Olvidar precargar el `remoteEntry.js`:**
   ➡️ *Error intermitente de carga: el remote no está listo al navegar.*

✅ **Solución:** Añadir en `angular.json` del Shell:

```json
"assets": [
  { "glob": "remoteEntry.js", "input": "dist/mfe-dashboard/", "output": "." },
  { "glob": "remoteEntry.js", "input": "dist/mfe-settings/", "output": "." }
]
```

---

6. **Validar integraciones completas:**

```bash
npm run start:shell
npm run start:mfe-dashboard
npm run start:mfe-settings
```

Verificar:

* Carga dinámica desde `/dashboard` y `/settings`
* `remoteEntry.js` aparece en DevTools → Network → JS
* Angular no se duplica en fuentes

---

### 🔧 Retos (a ejecutar tras resolver errores)

* Añadir una segunda ruta `/dashboard/stats` y exponerla como otro módulo.
* Introducir un error intencional en la ruta federada (`./WrongModule`) y depurarlo.
* Configurar logs de consola para cada MFE indicando que han sido cargados correctamente.

---

### ✅ Validaciones

* Las rutas federadas cargan sin errores.
* El alumno ha detectado, comprendido y resuelto al menos 3 problemas reales.
* El Shell se comporta como orquestador y carga MFEs de forma lazy.
* La configuración de `webpack.config.js` y `shared` está completa y funcional.
