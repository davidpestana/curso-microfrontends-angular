# 📘 Sesión 3 – Escalabilidad y arquitectura federada con Nx + Module Federation

## 🎯 Objetivo de la sesión

Comprender y aplicar una arquitectura de microfrontends federada en Angular usando Nx como herramienta de escalabilidad y gestión modular. Se implementará un monorepo con una aplicación Shell y dos remotos, usando `@angular-architects/module-federation`.

---

## 🧪 Laboratorio guiado – Fase 5: Reflexión y arquitectura futura

### 🎯 Objetivo

Consolidar aprendizajes clave y abrir un debate estructurado sobre cómo evolucionar una arquitectura MFE en producción real. Esta fase busca provocar pensamiento crítico, compartir experiencias del laboratorio y discutir decisiones estratégicas reales.

---

### 🧠 Preguntas para abrir el debate

* ¿Qué partes del sistema deberían federarse y cuáles podrían permanecer locales?
* ¿Qué síntomas justifican aplicar MFEs y cuáles no?
* ¿Qué dificultades técnicas o de coordinación han aparecido durante el laboratorio?
* ¿Dónde se ha ganado escalabilidad real y dónde se ha introducido complejidad innecesaria?
* ¿Se han respetado adecuadamente los bounded contexts?

---

### 🗺 Siguientes pasos recomendados

* Incluir CI/CD con `nx affected` y despliegue segmentado
* Evaluar `Standalone Components` para los MFEs nuevos
* Documentar contratos públicos entre remotes y shell
* Incluir pruebas de integración entre MFEs

---

### ✅ Resultado esperado

* Visión crítica de cuándo usar (y cuándo no) MFEs
* Equipo capacitado para argumentar arquitectura federada
* Entorno base sólido para iterar en siguientes sesiones
