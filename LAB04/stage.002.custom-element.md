### 🔹 Fase 2 – Instanciar y registrar el Custom Element

---

**🎯 Objetivo**  
Registrar un componente Angular como Web Component funcional y usable en cualquier documento HTML, siguiendo el estándar `customElements`.

---

**🔧 Pasos detallados**

1. Asegúrate de tener definido un módulo como `UserCardElementModule`, independiente del `AppComponent`.
2. En ese módulo, implementa `ngDoBootstrap` y registra el componente usando `createCustomElement`.
3. Define el nombre del selector HTML (`app-user-card`, por ejemplo).
4. Modifica `main.ts` para arrancar el módulo personalizado y no el `AppModule`.
5. Elimina referencias innecesarias como `bootstrap`, `app-root`, o `AppComponent` si no se usan.
6. Prepara un HTML externo para probar la ejecución fuera del contexto Angular.

---

**💻 Código destacado**

```ts
// main.ts
platformBrowserDynamic()
  .bootstrapModule(UserCardElementModule)
  .catch(err => console.error(err));
```

```ts
// ngDoBootstrap
const el = createCustomElement(UserCardComponent, { injector: this.injector });
customElements.define('app-user-card', el);
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
  <head><title>Test</title></head>
  <body>
    <script src="main.js"></script>
    <app-user-card></app-user-card>
  </body>
</html>
```

---

**✅ Validaciones esperadas**

- El navegador renderiza el componente Angular como un Custom Element (`app-user-card`).
- No se muestran errores en consola.
- El componente funciona sin `AppComponent`.
- El HTML de prueba lo carga directamente con `<script src="main.js">`.

---

**🧪 Retos técnicos**

1. Cambia el nombre del selector a `llama-user-card` y verifica el comportamiento.
   - 💡 *Tip*: Usa un nombre que no entre en conflicto con otros elementos registrados.

2. Registra un segundo componente como Web Component en el mismo módulo.
   - 💡 *Tip*: Puedes usar otro componente ya existente, como `UserAvatarComponent`.

3. Intenta registrar el mismo selector dos veces y observa la excepción lanzada.
   - 💡 *Tip*: Usa un `try/catch` para atrapar y entender el error.

4. Añade un `@Input()` en el componente (`@Input() name: string`) y pásalo como atributo HTML.
   - 💡 *Tip*: Declara el input como `@Input({ required: false })` si estás en Angular 16+.

5. Comprueba si el `@Input()` se propaga correctamente tras inicialización (`<app-user-card name="Alice"></app-user-card>`).
   - 💡 *Tip*: Implementa `ngOnChanges` o usa `ngAfterViewInit` para comprobar el valor..
