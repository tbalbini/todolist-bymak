# Qué es este repo

Este folder es la memoria del producto. El chat que lo diseñó se cierra. El siguiente chat no recuerda nada: lee estos archivos.

Todavía no hay app. Hay decisiones, un contrato y una cola de trabajo.

## Por qué existen los archivos

Un agente olvida al cerrar el chat. Si la decisión solo se dijo, el siguiente chat la inventa otra vez.

Cada archivo guarda un tipo de cosa y nada más:

- Palabras que no debemos confundir.
- Decisiones caras de deshacer.
- El comportamiento acordado.
- El trozo que toca construir ahora.

El código, cuando exista, tiene que obedecer esos archivos. No al revés.

## Cómo se conectan

```
AGENTS.md
  └─ docs/agents/          instrucciones para cualquier chat
       ├─ issue-tracker.md    dónde están los issues
       ├─ triage-labels.md    qué significa cada estado
       └─ domain.md           lee el glosario y los ADR antes de tocar código

GLOSSARY.md                 las palabras del producto
docs/adr/                   por qué el stack y por qué una sola Secuencia

.scratch/todolist/spec.md   el contrato completo
.scratch/todolist/issues/   el contrato partido en cinco tickets
```

`AGENTS.md` es la puerta. Un chat nuevo lo lee y de ahí sabe a qué tres archivos ir. `domain.md` lo manda al glosario y a los ADR. El ticket que le pases es el único trozo de trabajo. El spec es el contrato del que salieron los tickets; no se implementa entero de un golpe.

## Las cinco piezas

### 1. Puerta

`AGENTS.md` tiene tres líneas: issues en `.scratch/`, estados de triage, dominio de un solo contexto. No describe la app.

### 2. Instrucciones del agente

`docs/agents/` no es el producto. Es el manual de cómo trabajar en este repo.

- `issue-tracker.md`. No hay GitHub. Un issue es un markdown. El spec es `.scratch/<tema>/spec.md`. Cada ticket es `.scratch/<tema>/issues/01-….md`. El estado va en una línea `Status:`.
- `triage-labels.md`. Cinco palabras fijas. Los nuestros usan `ready-for-agent`: un agente puede cogerlos sin preguntar.
- `domain.md`. Antes de explorar, leer `GLOSSARY.md` y `docs/adr/`. Si no existen, seguir en silencio. No inventarlos "por si acaso".

### 3. Palabras

`GLOSSARY.md` define el lenguaje y nada de implementación.

- **Persona.** Dueña de sus Tareas. Nadie más las ve.
- **Secuencia.** Un solo grupo ordenado. No digas lista, proyecto ni carpeta.
- **Tarea.** Un título y un estado.
- **Pendiente** / **Hecha.** Los dos estados. No digas completada ni done.

Si el código o un ticket usa otra palabra, está mal el código, no el glosario.

### 4. Decisiones caras

Un ADR existe solo si deshacerlo duele, si dentro de seis meses alguien preguntaría por qué, y si había otra opción real.

- `docs/adr/0001-nextjs-supabase.md`. La app es Next.js (App Router). Los datos y la sesión son Supabase (Postgres y Auth).
- `docs/adr/0002-una-secuencia-sin-listas.md`. Una Persona, una Secuencia. Sin listas.

Elegir el texto del botón no va aquí. Se cambia en una hora.

### 5. El trabajo

`.scratch/` es la cola. No es la app y no se despliega.

- `spec.md`. Las 39 historias y las reglas: orden, título de 1 a 200 caracteres, la pantalla no cambia hasta que el servidor acepta, tests contra la pantalla. `Status: ready-for-agent`.
- `issues/01` a `05`. Cada archivo es una rodaja vertical: se puede mostrar funcionando sola. `Blocked by` dice a quién espera.

Orden real:

1. Entrar y salir. No espera a nadie.
2. Crear una Tarea Pendiente. Espera a la 01.
3. Pasar a Hecha, renombrar y borrar. Las tres esperan a la 02 y no se esperan entre sí.

## Qué se viene

Aparecerá una app Next.js. Supabase guarda la sesión y las Tareas. La pantalla es la única puerta: formulario sin sesión; correo, "Cerrar sesión" y la Secuencia con sesión.

Los tests abren esa pantalla y comprueban lo que la Persona ve. No comprueban el SQL ni el interior de un componente.

El orden de construcción es el de los tickets. El 01 crea el esqueleto (entrar y salir). El 02 crea la primera Tarea de punta a punta, tabla incluida, y la oculta a las demás Personas. El 03, el 04 y el 05 añaden cada acción sobre una Tarea que ya existe.

## Cómo se maneja a partir de ahora

- Un chat nuevo por ticket. El chat lee ese archivo y no reabre el diseño.
- Al cumplir un criterio, se marca el checkbox de ese ticket. El archivo se queda: es el registro de qué se pidió.
- Una palabra nueva entra en `GLOSSARY.md` solo cuando la acordamos. Hasta entonces no se usa.
- Una decisión cara entra como el siguiente ADR (`0003-…`). Una decisión barata no se escribe.
- Un comportamiento nuevo que no está en el spec no se cuela en el código. Se vuelve a grill, spec y tickets.
- `.scratch/` no se convierte en código, y el código no se explica dentro del glosario.
