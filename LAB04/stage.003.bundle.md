### 🔹 Fase 3 – Configurar el build para generar un bundle autónomo

---

**🎯 Objetivo**  
Configurar el proceso de compilación de Angular para generar un archivo `main.js` único que pueda cargarse en cualquier entorno sin depender del CLI ni del index HTML tradicional.

---

**🔧 Pasos detallados**

1. Instala `ngx-build-plus`, herramienta que permite modificar la configuración de build sin ejectuar Angular CLI internamente:

```bash
ng add ngx-build-plus
```

2. Modifica el script de build del proyecto en `angular.json`:
   - Asegúrate de incluir:
     - `--single-bundle true`
     - `--output-hashing=none`

```bash
ng build dashboard --prod --single-bundle true --output-hashing=none
```

3. Elimina el `index.html` generado por Angular si no se va a usar.
4. Copia el `main.js` generado a una ruta accesible o úsalo directamente en pruebas con HTML plano.

---

**💻 Código destacado**

Comando completo de build:

```bash
ng build dashboard --prod --single-bundle true --output-hashing=none
```

---

**✅ Validaciones esperadas**

- El resultado de la compilación es un único `main.js` sin hash en el nombre.
- No se generan archivos innecesarios como `runtime`, `polyfills`, `styles`, `vendor` por separado.
- El fichero generado puede ser cargado directamente desde un HTML sin `base href` ni rutas relativas internas.

---

**🧪 Retos técnicos**

1. Prueba a mover `main.js` a una ruta distinta (por ejemplo, `assets/widgets/`) y ajusta la ruta en el HTML manualmente.
   - 💡 *Tip*: Puedes usar un servidor HTTP estático como `http-server` para validar la carga.

2. Intenta generar el build sin `--single-bundle` y analiza la diferencia de archivos.
   - 💡 *Tip*: Usa `ls dist/apps/dashboard` para comparar la salida.

3. Usa `source-map-explorer` para analizar si Angular está incluido en el bundle final.
   - 💡 *Tip*: Puedes instalarlo con `npm install -g source-map-explorer` y ejecutarlo con:

```bash
source-map-explorer dist/apps/dashboard/main.js
```
