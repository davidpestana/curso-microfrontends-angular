### 🔹 Fase 4 – Integración en entorno externo

---

**🎯 Objetivo**  
Validar que el Web Component generado con Angular puede utilizarse en una aplicación externa sin framework, cargando el bundle manualmente desde HTML plano.

---

**🔧 Pasos detallados**

1. Crea una carpeta `external-test/` en la raíz del proyecto.
2. Copia dentro el archivo `main.js` generado desde `dist/apps/dashboard/`.
3. Crea un archivo `index.html` dentro de `external-test/` con la siguiente estructura:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Test Web Component</title>
  </head>
  <body>
    <script src="main.js"></script>
    <app-user-card name="Alice"></app-user-card>
  </body>
</html>
```

4. Ejecuta un servidor web local desde esa carpeta para evitar problemas con CORS:

```bash
npx http-server external-test/
```

5. Abre `http://localhost:8080` en tu navegador y comprueba que el componente se muestra correctamente.

---

**💻 Código destacado**

Archivo de prueba `index.html`:

```html
<script src="main.js"></script>
<app-user-card name="Alice"></app-user-card>
```

Comando para servir la carpeta:

```bash
npx http-server external-test/
```

---

**✅ Validaciones esperadas**

- El Web Component se carga correctamente sin errores en consola.
- El contenido del componente se renderiza y acepta el atributo `name` como `@Input()`.
- No hay dependencias rotas ni errores de carga por rutas relativas.

---

**🧪 Retos técnicos**

1. Modifica el atributo `name` dinámicamente desde consola del navegador (`document.querySelector('app-user-card').setAttribute('name', 'Bob')`).
   - 💡 *Tip*: Comprueba si se actualiza el binding en Angular automáticamente.

2. Añade un segundo Web Component al mismo HTML (`<user-avatar></user-avatar>`) y valida que no hay conflictos de carga.
   - 💡 *Tip*: Asegúrate de que tienen nombres únicos en `customElements.define()`.

3. Elimina el `script` y verifica el comportamiento si el componente no está registrado.
   - 💡 *Tip*: Observa cómo falla silenciosamente o lanza error en consola.
