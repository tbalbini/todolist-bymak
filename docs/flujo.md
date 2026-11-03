# Flujo

Abre un chat nuevo y pega esto:

```
/implement .scratch/todolist/issues/01-entrar-y-salir.md
```

Eso son unas 2–4 horas: todavía no hay app. Crear el proyecto de Supabase y apagar la confirmación por correo son unos 15 minutos. Hazlo cuando el agente te lo pida; sin eso no se puede comprobar el ticket 01 contra un servidor real.

Paso 4 de 6 hecho: los tickets ya están escritos. No hay código.

## Qué hicimos

1. `/setup-matt-pocock-skills`. Dejamos dicho dónde viven los issues (archivos en `.scratch/`), los cinco estados (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`) y que el dominio es un solo contexto. Eso creó `AGENTS.md` y `docs/agents/`.
2. `/grill-with-docs`. Entrevista hasta cerrar el producto. De ahí salieron `GLOSSARY.md` y los dos ADR. No se escribió código.
3. `/to-spec`. El acuerdo pasó a `.scratch/todolist/spec.md`. Los tests miran la pantalla, no el SQL ni el interior de los componentes. Estado: `ready-for-agent`.
4. `/to-tickets`. El spec se partió en cinco rodajas. Cada una se puede demostrar sola. Están en `.scratch/todolist/issues/`.

El grill, el spec y los tickets se hicieron en un solo chat para no perder el hilo.

## Qué sigue

1. Chat nuevo: `/implement` del ticket 01. Un chat, un ticket. Al terminar, el comportamiento de ese ticket funciona y el diff queda revisado.
2. Otro chat nuevo: ticket 02. No lo empieces hasta que entrar y salir funcione.
3. Tres chats, pueden ir a la vez: tickets 03, 04 y 05. Los tres solo esperan al 02.
4. `/retro` en el chat del último ticket. Mira el entorno del agente (comprobaciones, reglas), no el producto.

`/triage` no entra. Esos tickets ya salieron listos para un agente.
