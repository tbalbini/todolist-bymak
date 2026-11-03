# 02: Crear una Tarea Pendiente

**What to build:** Con sesión, la Persona escribe un título en el campo de arriba y aparece una Tarea Pendiente en su Secuencia. El título se recorta; si queda vacío o pasa de 200 caracteres, no se guarda. Dos Tareas pueden llamarse igual. La Pendiente más reciente queda la primera. La Secuencia sobrevive a recargar y a salir y volver a entrar. Otra Persona no la ve. Si el servidor rechaza el alta, la Secuencia no cambia y se ve un error.

**Blocked by:** 01: Entrar y salir

**Status:** ready-for-agent

- [ ] Con sesión, el campo para crear está arriba del todo.
- [ ] Una Secuencia vacía muestra ese campo y ninguna Tarea.
- [ ] Crear con un título válido añade una Tarea Pendiente: casilla vacía y título normal.
- [ ] El título se guarda recortado. Uno vacío, o solo espacios, no añade una Tarea.
- [ ] Un título de más de 200 caracteres, ya recortado, no añade una Tarea.
- [ ] Dos Tareas con el mismo título conviven.
- [ ] Entre las Pendientes, la creada más recientemente queda arriba.
- [ ] Recargar muestra las mismas Tareas, en el mismo orden.
- [ ] Salir y volver a entrar muestra las mismas Tareas.
- [ ] Otra Persona, en su propia sesión, no ve esas Tareas. Cada una ve solo las suyas.
- [ ] Si el servidor rechaza el alta, la Secuencia sigue igual y se ve un error.
- [ ] Los textos de la pantalla están en español.
