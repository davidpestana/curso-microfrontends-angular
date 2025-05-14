## 🧱 Fase 6 – Federación real con Webpack Module Federation

**🌟 Objetivo:**
Implementar una federación real de microfrontends utilizando `Webpack Module Federation`, la técnica adoptada oficialmente por Angular, React y otros frameworks modernos. En esta fase se configurará un shell como `host` y dos MFEs como `remotes`, compartiendo dependencias y exponiendo módulos reales.

Este es el patrón más robusto y usado en entornos empresariales reales.

---

### 📜 Descripción técnica

Webpack 5 permite configurar `Module Federation` mediante su plugin `ModuleFederationPlugin`. Cada microfrontend se compila como un `remote` que expone módulos (componentes, funciones, clases) y el Shell actúa como `host`, consumiéndolos dinámicamente en tiempo de ejecución.

La federación se produce en tiempo de carga del bundle, no en runtime puro como `SystemJS`. Además, puede compartir dependencias como React, Angular, etc.

---

### 🧍‍♂️ Pasos

1. **Crear estructura del proyecto con Webpack**

   Si no lo tienes aún:

   ```bash
   mkdir mfe-one mfe-two shell
   cd mfe-one && npm init -y && npm install webpack webpack-cli webpack-dev-server html-webpack-plugin --save-dev
   # Repetir para mfe-two y shell
   ```

2. **Configurar `webpack.config.js` en los remotes**

   En `mfe-one/webpack.config.js`:

   ```js
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   const { ModuleFederationPlugin } = require('webpack').container;

   module.exports = {
     mode: 'development',
     devServer: { port: 3001 },
     output: { publicPath: 'auto' },
     plugins: [
       new ModuleFederationPlugin({
         name: 'mfe_one',
         filename: 'remoteEntry.js',
         exposes: {
           './App': './src/App.js'
         }
       }),
       new HtmlWebpackPlugin({ template: './public/index.html' })
     ]
   };
   ```

   Crear `src/App.js`:

   ```js
   export function render(container) {
     container.innerHTML = '<h2>Hola desde MFE One</h2>';
   }
   ```

3. **Configurar el Shell como host**

   En `shell/webpack.config.js`:

   ```js
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   const { ModuleFederationPlugin } = require('webpack').container;

   module.exports = {
     mode: 'development',
     devServer: { port: 3000 },
     output: { publicPath: 'auto' },
     plugins: [
       new ModuleFederationPlugin({
         name: 'shell',
         remotes: {
           mfe_one: 'mfe_one@http://localhost:3001/remoteEntry.js',
           mfe_two: 'mfe_two@http://localhost:3002/remoteEntry.js'
         }
       }),
       new HtmlWebpackPlugin({ template: './public/index.html' })
     ]
   };
   ```

4. **Usar el MFE desde el Shell**

   En `shell/src/index.js`:

   ```js
   import('mfe_one/App').then(mod => {
     mod.render(document.getElementById('mfe-container'));
   });
   ```

---

### 🔧 Retos para el alumno

1. Configurar `mfe-two` como segundo remote y verificar su carga.
2. Compartir un módulo común (por ejemplo, `event-bus.js`) entre host y remotes.
3. Añadir `singleton: true` a una dependencia compartida y validar que no se duplique.
4. Implementar navegación entre MFEs con comunicación a través de eventos federados.

---

### ✅ Validaciones

* El shell carga remotamente los MFEs a través de `remoteEntry.js`.
* Los MFEs exponen sus módulos y los usan correctamente.
* La federación funciona aunque estén en puertos distintos.
* Las dependencias compartidas no se duplican.

---

### 📌 Resultado final

Con Webpack Module Federation, se logra una arquitectura madura y robusta para microfrontends reales, compartiendo módulos y dependencias de forma transparente. Es el modelo adoptado por Angular y otros frameworks como base de producción escalable.
