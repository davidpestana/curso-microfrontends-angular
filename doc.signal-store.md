## 🧠 Arquitectura Signal Store (framework-agnostic)

### Contenidos

Signal Store es un patrón de arquitectura de estado basado en señales reactivas. Si bien Angular ha impulsado su adopción a través de `@ngrx/signals`, el modelo es agnóstico y puede aplicarse en cualquier entorno que soporte señales, como frameworks con reactive primitives o librerías funcionales.

* Fundamentos de `createSignalStore()` y estructura reactiva básica
* API típicas: `patch()`, `update()`, `reset()`, `subscribe()`
* Derivación de estado con `computed()` o equivalentes
* Composición de múltiples stores
* Consumo desacoplado por múltiples vistas o módulos
* Aplicaciones posibles: apps frontend, workers, edge functions, microservicios

### 🧪 Laboratorio básico

1. Crear un archivo `store.ts` y definir una función `createSignalStore()` que use un objeto de señales simples (ej. `createSignal`, `computed`, `effect`).
2. Añadir funciones `patch()`, `update()`, `reset()` como métodos controladores del estado.
3. Crear un estado inicial con 2 propiedades: `count` y `user`.
4. Crear una señal derivada `isLoggedIn = computed(() => user() !== null)`.
5. Exportar el store y consumirlo desde dos módulos independientes que compartan el mismo contexto.
