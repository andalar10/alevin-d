# CHANGELOG — Alevín D

Histórico funcional del proyecto (qué cambia para el usuario, no detalles técnicos de implementación).

## 2026-09-09 — Primera versión

- Acceso privado mediante enlace de un solo uso al correo autorizado (solo Andrés puede entrar).
- Gestión de la plantilla: alta de jugadores fijos y temporales, con dorsal, y baja/reactivación.
- Alta de partidos: rival, fecha, hora, condición (casa/fuera) y tipo de partido (Amistoso/Liga, ampliable desde la propia app).
- Convocatoria por partido: marcar quién viene de la plantilla, o añadir un jugador temporal solo para ese partido.
- Subida de la foto de convocatoria del entrenador, asociada al partido.
- Alineación por cuarto (1 a 4 cuartos por partido): para cada línea táctica (portero, defensa, medio, media punta, delantero) se añaden los jugadores en orden de derecha a izquierda, y la app calcula automáticamente el código de posición (LD, DCC, MCD, MP, ED…) según la tabla acordada; el código es editable a mano si la combinación no está prevista.
- El portero de un cuarto, si no se indica, se hereda automáticamente del cuarto anterior del mismo partido.
- Resultado por cuarto y resultado final del partido.
- Registro de goles y asistencias por cuarto.
- Pendiente: generación del libro de temporada en PDF (una sección por partido + estadísticas finales separadas por amistoso/liga).

## 2026-09-09 — Corrección de acceso

- Arreglado un fallo por el que, tras entrar con el enlace de acceso, la app podía mostrar "email no autorizado" por diferencias de mayúsculas/espacios en el correo. Ahora la comprobación ignora eso, y si el email no coincide igualmente, el mensaje muestra qué email se usó para poder diagnosticarlo.

## 2026-09-09 — Login por contraseña

- Sustituido el acceso por enlace de un solo uso (que agotaba enseguida el límite de envíos de correo de Supabase, compartido con otras apps) por acceso con email y contraseña. La sesión queda guardada en el dispositivo, sin depender del correo en cada partido. Incluye un botón para crear la cuenta la primera vez.
- Añadido botón "Cambiar contraseña" dentro de la app, para poder actualizarla sin depender de nadie más.
