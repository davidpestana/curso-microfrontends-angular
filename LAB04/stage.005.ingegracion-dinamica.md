### 🔹 Fase 5 – Integración dinámica desde el Shell

---

**🎯 Objetivo**  
Demostrar que los Web Components generados se pueden inyectar dinámicamente desde una aplicación Angular externa como el Shell, sin necesidad de federación, simplemente cargando el script y creando el elemento.

---

**🔧 Pasos detallados**

1. Copia el archivo `main.js` generado desde `dashboard` a la carpeta `assets/widgets/` dentro del proyecto `shell`.
2. Modifica el componente `ShellComponent` o un componente de prueba para cargar el script dinámicamente:

```ts
const script = document.createElement('script');
script.src = '/assets/widgets/main.js';
document.body.appendChild(script);
```

3. Tras cargar el script, crea dinámicamente el elemento personalizado:

```ts
const card = document.createElement('app-user-card');
card.setAttribute('name', 'Carol');
document.body.appendChild(card);
```

4. Asegúrate de que esto ocurre una vez el script se haya cargado completamente (`script.onload`).
5. Revisa en la app Angular (`shell`) que el componente se renderiza correctamente en DOM.

---

**💻 Código destacado**

Carga dinámica en `ngOnInit()`:

```ts
ngOnInit() {
  const script = document.createElement('script');
  script.src = '/assets/widgets/main.js';
  script.onload = () => {
    const el = document.createElement('app-user-card');
    el.setAttribute('name', 'Carol');
    document.body.appendChild(el);
  };
  document.body.appendChild(script);
}
```

---

**✅ Validaciones esperadas**

- El script se carga sin errores desde `assets/widgets`.
- El Web Component se renderiza en tiempo de ejecución dentro del Shell.
- El atributo `name` se propaga correctamente.
- No hay conflictos con Angular internamente (ni errores en consola).

---

**🧪 Retos técnicos**

1. Crea un wrapper Angular (`WidgetHostComponent`) que actúe como host para los elementos Web Component.
   - 💡 *Tip*: Usa `ViewContainerRef` para inyectarlo dinámicamente en un `div` del template.

2. Añade soporte para múltiples scripts (por ejemplo, `main-profile.js` y `main-dashboard.js`).
   - 💡 *Tip*: Usa una función reutilizable para cargar scripts con promesas.

3. Cambia el atributo dinámicamente después de montado el componente.
   - 💡 *Tip*: Usa `setTimeout()` para simular datos asíncronos y cambiar `setAttribute` después de 3 segundos.
