### 🔹 Fase 7 – Reflexión final sobre Angular Elements

---

**🎯 Objetivo**  
Evaluar de forma crítica la aproximación basada en Angular Elements frente a otras técnicas de integración de microfrontends, destacando sus fortalezas, limitaciones y casos de uso recomendados.

---

**🔧 Puntos de reflexión técnica**

1. **Desacoplamiento real:**
   - Los Web Components permiten la máxima independencia del framework anfitrión.
   - Se comportan como HTML nativo: insertables en cualquier plataforma, incluso sin JavaScript moderno.

2. **Simplicidad de integración:**
   - Cualquier app (React, Vue, WordPress...) puede incluir el script y usar el componente.
   - No requiere configuración compleja de runtime como Module Federation.

3. **Problemas detectados:**
   - Tamaño elevado del bundle (Angular completo si no se usa `externals`).
   - No hay sistema de `shared` o `singleton`: duplicación probable.
   - Pocas garantías de sincronización entre componentes o eventos compartidos.

4. **Casos de uso ideales:**
   - Widgets empresariales exportables.
   - Embebido en portales o apps legacy.
   - Publicación en CDN o NPM para reutilización.

5. **Casos donde NO aplicar:**
   - Apps grandes Angular-to-Angular: mejor usar Module Federation.
   - Necesidad de compartir servicios o estado: no es trivial con Elements.

---



**🧩 Debate final**

- Angular Elements vs Module Federation: ¿cuál es más acoplado? ¿cuál escala mejor?
- Pregunta abierta para reflexión individual: 
  > "¿En qué tipo de proyecto actual podrías usar Angular Elements con éxito?"
- Posibles mejoras para aplicarlo en equipos.
