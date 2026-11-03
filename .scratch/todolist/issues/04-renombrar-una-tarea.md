# 04: Renombrar una Tarea

**What to build:** La Persona pulsa el título, lo edita ahí mismo y Enter guarda uno válido sin mover la Tarea de su puesto. Escape, o confirmar un título vacío, restaura el anterior y no llama al servidor. Si el servidor rechaza el cambio, se queda el título ya guardado y se ve un error. El puesto no depende de si está Pendiente o Hecha.

**Blocked by:** 02: Crear una Tarea Pendiente

**Status:** ready-for-agent

- [ ] Pulsar el título lo abre para editarlo en el mismo sitio, sin otra pantalla.
- [ ] Enter guarda un título válido, ya recortado, de 1 a 200 caracteres.
- [ ] Tras guardar, la Tarea sigue en el mismo puesto de la Secuencia.
- [ ] Escape restaura el título anterior y no llama al servidor.
- [ ] Confirmar un título vacío o solo espacios restaura el anterior y no llama al servidor.
- [ ] Si el servidor rechaza el cambio, el título visible sigue siendo el guardado y se ve un error.
- [ ] Los textos de la pantalla están en español.
