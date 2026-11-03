Status: ready-for-agent

# Tareas

## Problem Statement

A Persona needs a private place to keep Tareas. She needs to add one, fix its title, mark it Pendiente or Hecha, and remove it, and she needs to find the unfinished ones first the next time she opens the app. Nobody else should see them, and they should still be there after she signs out or switches devices.

## Solution

A Persona signs in with correo and contraseña and lands on one screen. That screen is her Secuencia: a field to create a Tarea at the top, Pendientes above, Hechas below. She renames a Tarea in place, toggles it with a checkbox, and deletes it with "Borrar". The screen changes only after the server accepts the change. Another Persona never sees those Tareas.

## User Stories

1. As a Persona, I want to register with correo and contraseña and enter immediately, so that I can keep Tareas without waiting for a confirmation email.
2. As a Persona, I want to sign in with correo and contraseña, so that I see my Secuencia.
3. As a Persona, I want a wrong contraseña to show an error and leave me signed out, so that I don't enter under the wrong identity.
4. As a Persona, I want registering an already used correo to show an error and leave me signed out, so that one correo belongs to one Persona.
5. As a Persona, I want to sign out, so that the next person at this browser does not see my Tareas.
6. As a Persona, I want to sign in again and find the same Tareas, so that leaving does not lose them.
7. As a Persona without a session, I want to see only a form with correo, contraseña, entrar, and crear cuenta, so that no Tarea is visible.
8. As a Persona with a session, I want to see my correo and "Cerrar sesión" at the top, so that I know who is signed in.
9. As a Persona, I want the screen's text in Spanish, so that I can use it in my language.
10. As a Persona, I want a create field at the very top of my Secuencia, so that I can add a Tarea without leaving the screen.
11. As a Persona, I want a new Tarea to start Pendiente, so that unfinished is the default.
12. As a Persona, I want an empty or whitespace-only title to be refused, so that every Tarea has a title.
13. As a Persona, I want the title trimmed before it is saved, so that surrounding spaces do not become part of it.
14. As a Persona, I want a title longer than 200 characters to be refused, so that a Tarea stays one line.
15. As a Persona, I want two Tareas with the same title to coexist, so that a repeated errand is still its own Tarea.
16. As a Persona with no Tareas, I want to see the create field and an empty Secuencia, so that I know where to start.
17. As a Persona, I want Pendientes above Hechas, so that unfinished Tareas come first.
18. As a Persona, I want the most recently created Pendiente first among Pendientes, so that the latest addition is at the top of that group.
19. As a Persona, I want a Hecha to stay visible in the Secuencia, so that I don't need a separate archive.
20. As a Persona, I want the Tarea I just marked Hecha to appear first among Hechas, so that the latest completion leads that group.
21. As a Persona, I want a Pendiente to show an empty checkbox and a normal title, so that I can tell it is not done.
22. As a Persona, I want a Hecha to show a checked checkbox and a struck-through title, so that I can tell it is done.
23. As a Persona, I want the checkbox to mark a Pendiente as Hecha, so that I can record that it is done.
24. As a Persona, I want the checkbox to mark a Hecha back to Pendiente, so that I can reverse a completion.
25. As a Persona, I want a Tarea that returns to Pendiente to take the place its creation time gives it, so that completing and un-completing does not make it look newly created.
26. As a Persona, I want to edit a title in place, so that I can fix it without another screen.
27. As a Persona, I want Enter to save a non-empty title, so that confirming the edit is one key.
28. As a Persona, I want Escape to restore the previous title and not contact the server, so that cancelling changes nothing.
29. As a Persona, I want confirming an empty title to restore the previous title and not contact the server, so that a Tarea cannot lose its title.
30. As a Persona, I want a rename to leave the Tarea in the same position, so that editing does not reorder the Secuencia.
31. As a Persona, I want "Borrar" to remove the Tarea immediately, with no confirmation, so that deletion is one action.
32. As a Persona, I want a deleted Tarea to stay deleted, so that there is no undo.
33. As a Persona, I want the screen to change only after the server accepts the action, so that I never see a Tarea or a state the server rejected.
34. As a Persona, I want a failed create, rename, toggle, or delete to leave the Secuencia unchanged and show an error, so that I can tell the action did not happen.
35. As a Persona, I want a failed sign-in or registration to leave me on the form with an error, so that no Secuencia appears.
36. As a Persona on another device, I want a reload to show the Tareas the server has, so that both devices share one Secuencia.
37. As a Persona saving from two devices, I want the last change the server accepted to be the one I see, so that there is a single outcome.
38. As a Persona, I want another Persona's Tareas to be invisible to me, so that my Secuencia stays private.
39. As a second Persona, I want to see none of the first Persona's Tareas, so that privacy holds even if I use the same app.

## Implementation Decisions

- The app is Next.js (App Router). Data and sessions are Supabase (Postgres and Auth). See the accepted stack decision.
- There is one screen. Without a session it shows the form (correo, contraseña, entrar, crear cuenta). With a session it shows the Persona's correo, "Cerrar sesión", the create field, and her Secuencia. Copy on the screen is Spanish.
- A Persona is a Supabase Auth identity using correo and contraseña. Email confirmation stays off, so registration opens a session immediately. Signing out ends that session and returns to the form.
- A Tarea belongs to exactly one Persona. The database refuses every read and write of that Tarea by anyone else. There is one Secuencia per Persona, derived as "her Tareas in order", not stored as a separate grouping.
- A Tarea stores a title, a state (Pendiente or Hecha), the time it was created, and the time it became Hecha. The Hecha time is set when it becomes Hecha and cleared when it returns to Pendiente. Position is not stored.
- Pendientes sort by creation time, newest first. Hechas sort by the time they became Hecha, newest first. Pendientes are shown above Hechas. Renaming does not change either time.
- The title is a single line. It is trimmed before a save. After trimming it must be from 1 to 200 characters. Creating with an invalid title does not insert a Tarea. Enter on an edit saves only a valid title. Escape, or confirming an empty title, closes the edit on the previous title and does not call the server.
- Delete removes the Tarea. Nothing is kept to undo it.
- The screen does not change the Secuencia until the server accepts. On failure the previous Secuencia stays and an error is shown. Two devices do not push updates to each other; a reload reads the current server state, and the last accepted write wins.
- The screen is the only interface tests and the Persona use. Supabase sits behind that screen.

## Testing Decisions

- A good test exercises the screen the Persona sees and uses. It asserts visible text, order, checkbox state, and errors. It does not assert SQL, row shapes, or the interior of components.
- There is one seam: that screen. Sign-in, registration, sign-out, create, rename, toggle, delete, order, failure, and privacy are all tested there.
- Privacy is tested by signing in as two Personas and asserting that each screen shows only that Persona's Tareas.
- Failure is tested by making the server reject a create, rename, toggle, or delete, and asserting the Secuencia is unchanged and an error is visible.
- The repo has no existing tests. There is no prior art to copy.

## Out of Scope

- Realtime updates. Another device sees changes on reload.
- Search, hiding Hechas, and reordering by hand.
- Password recovery, deleting a Persona, and changing the correo.
- Sharing a Secuencia, or any second grouping (lists, projects, folders, tags).
- Due dates, priority, notes, and subtasks.
- A confirmation step or undo for "Borrar".
- Any language other than Spanish on the screen.

## Further Notes

- Domain words are defined in `GLOSSARY.md`. Use Persona, Secuencia, Tarea, Pendiente, and Hecha. Don't substitute the words listed under Avoid.
- The stack decision is `docs/adr/0001-nextjs-supabase.md`. The single Secuencia is `docs/adr/0002-una-secuencia-sin-listas.md`.
- A human still has to create the Supabase project, turn email confirmation off, and provide the project URL and key before the screen can talk to a real server.
