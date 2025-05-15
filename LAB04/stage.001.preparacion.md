### 🔹 Fase 1 – Preparar el entorno para Angular Elements

---

**🎯 Objetivo**  
Configurar el proyecto Angular para exponer un componente como Web Component, separando la lógica de bootstrap y preparando la estructura para generar bundles independientes.

---

**🔧 Pasos detallados**

1. Verifica que `dashboard` tiene un componente funcional como `UserCardComponent`.
2. Crea un módulo específico para exponer el componente:
   - Este módulo no debe tener `AppComponent`.
   - Debe implementar `ngDoBootstrap`.
3. Declara el componente en este nuevo módulo (`UserCardElementModule`).
4. Asegúrate de importar `BrowserModule` y `Injector`.
5. Instala el paquete necesario:

```bash
ng add @angular/elements
```

---

**💻 Código destacado**

```ts
@NgModule({
  declarations: [UserCardComponent],
  imports: [BrowserModule],
  providers: [],
})
export class UserCardElementModule {
  constructor(private injector: Injector) {}

  ngDoBootstrap() {
    const el = createCustomElement(UserCardComponent, { injector: this.injector });
    customElements.define('app-user-card', el);
  }
}
```

---

**✅ Validaciones esperadas**

- No hay referencia alguna a `AppComponent`.
- El módulo compila sin errores.
- Se ha instalado correctamente `@angular/elements` y sus dependencias.
- El componente está preparado para ser registrado como Web Component.

---

**🧪 Retos técnicos**

1. Crea un módulo similar para un segundo componente (`UserAvatarComponent`).
   - 💡 *Tip*: Puedes ubicarlo en `libs/ui` si ya existe allí.

2. Verifica que ambos módulos pueden coexistir sin conflictos de bootstrap.
   - 💡 *Tip*: Usa `ngDoBootstrap` en cada módulo por separado y prueba uno a la vez.

3. Comprueba que eliminar `AppComponent` no genera errores de compilación.
   - 💡 *Tip*: Asegúrate de que no esté declarado en ningún módulo.
