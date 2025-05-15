### 🔹 Fase 6 – Publicación de múltiples Web Components y coexistencia

---

**🎯 Objetivo**  
Exponer varios Web Components desde distintos orígenes (`dashboard`, `profile`, etc.), validar su publicación conjunta y asegurar que pueden coexistir en un mismo entorno sin conflictos de estilo o carga.

---

**🔧 Pasos detallados**

1. Genera un segundo Web Component desde `profile`, por ejemplo, `UserPreferencesComponent`.
2. Repite el proceso de build con `ngx-build-plus`:

```bash
ng build profile --prod --single-bundle true --output-hashing=none
```

3. Copia ambos archivos `main-dashboard.js` y `main-profile.js` a una carpeta común accesible (por ejemplo, `public/widgets/`).
4. Crea un HTML plano o una app externa que los cargue de forma secuencial o condicional:

```html
<script src="main-dashboard.js"></script>
<script src="main-profile.js"></script>
<app-user-card name="Alice"></app-user-card>
<app-user-preferences></app-user-preferences>
```

5. Asegúrate de que ambos Web Components se renderizan correctamente y que no hay colisiones en `customElements.define(...)`.
6. Evalúa si ambos bundles están cargando Angular o si puedes optimizar usando `external` para compartir dependencias.

---

**💻 Código destacado**

Carga secuencial en HTML:

```html
<script src="main-dashboard.js"></script>
<script src="main-profile.js"></script>
```

Uso de ambos Web Components:

```html
<app-user-card name="Alice"></app-user-card>
<app-user-preferences></app-user-preferences>
```

---

**✅ Validaciones esperadas**

- Ambos bundles cargan sin errores de redefinición.
- Los elementos se muestran correctamente y de forma independiente.
- No se carga Angular más de una vez (si está configurado como `external`).
- El HTML funciona en un entorno plano o en otro framework.

---

**🧪 Retos técnicos**

1. Configura `angular.json` o `webpack.config.js` para usar `externals` y evitar múltiples cargas de Angular.
   - 💡 *Tip*: Declara `@angular/core`, `@angular/common`, etc. como `external` en los remotos.

2. Comprueba que ambos componentes aceptan `@Input()` distintos y responden adecuadamente.
   - 💡 *Tip*: Usa atributos como `theme`, `lang` o `data-user-id` para diferenciarlos.

3. Publica ambos componentes en un servidor web como `Nginx` o `S3` y cárgalos desde una app externa real.
   - 💡 *Tip*: Asegúrate de configurar CORS si los cargas desde otro dominio.
