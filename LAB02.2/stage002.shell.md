## 🧱 Fase 2 – Creación del Shell e integración mínima

**🌟 Objetivo:**
Crear una aplicación Angular adicional que actuará como Shell. Esta app no tiene lógica propia, solo enruta y renderiza los Microfrontends que más adelante serán federados.

---

### 📜 Descripción

El Shell será una aplicación ligera con navegación básica. No consume aún los MFEs, pero prepara su estructura para alojarlos posteriormente. Sirve como contenedor neutral para futuras integraciones federadas.

---

### 🧍‍♂️ Pasos

1. Crear la app Shell:

   ```bash
   ng new shell --routing --style=scss
   cd shell
   ```

2. Crear los enlaces de navegación:

   ```html
   <!-- src/app/app.component.html -->
   <nav>
     <a routerLink="/dashboard">Dashboard</a>
     <a routerLink="/settings">Settings</a>
     <a routerLink="/profile">Profile</a>
   </nav>
   <router-outlet></router-outlet>
   ```

3. Servir en puerto dedicado:

   ```bash
   ng serve --port 4200
   ```

---

### 🔧 Retos

* Añadir un banner de bienvenida con el texto "Contenedor Shell".
* Aplicar estilos para mostrar claramente que es el entorno host.
* Añadir un footer con la fecha y nombre del equipo (editable).

---

### ✅ Validaciones

* El Shell levanta en `http://localhost:4200`
* Tiene navegación entre enlaces, aunque aún no renderiza contenido remoto.
* El layout está preparado para alojar MFEs en rutas separadas.
* El alumno visualiza el rol del Shell como integrador.
