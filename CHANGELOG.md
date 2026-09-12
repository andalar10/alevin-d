# CHANGELOG — Alevín D

Histórico funcional del proyecto (qué cambia para el usuario, no detalles técnicos de implementación).

## 2026-09-12 — Rendimiento, navegación y mejoras en cuartos/goles

- **Carga de un partido más rápida**: al pulsar un partido, antes se pedían los datos de cada cuarto uno detrás de otro (hasta 9 peticiones seguidas); ahora se piden todos a la vez. Además se ve al instante "Cargando…" en vez de quedarse la pantalla en blanco mientras llega la información.
- **Se mantiene la página al recargar**: si estabas dentro de un partido (o en Jugadores/Admin/Libro) y recargas la app, vuelve a abrirse ahí mismo en vez de saltar siempre a "Jugadores".
- **Acceso directo al partido actual**: nuevo botón "⚡ Partido actual" en el menú superior que lleva directamente al último partido abierto, esté donde esté navegando.
- **Resultado total siempre visible**: en la ficha del partido se ve, justo debajo del rival y la fecha, la suma de goles de los cuartos ya jugados (ej. "⚽ 2 - 1"), sin tener que sumar cuarto a cuarto.
- **Resumen en el listado de partidos**: cada partido de la lista muestra ahora el resultado (el oficial si ya se ha guardado, o el parcial calculado a partir de los cuartos si aún no).
- **Botones +1 / -1 en el marcador de cada cuarto**: para sumar o restar un gol al marcador del cuarto sin tener que escribir el número; si el cuarto ya está guardado, se guarda al momento.
- **Gol rápido junto al jugador**: una vez guardada la alineación de un cuarto, aparece un pequeño "⚽ +1" junto a cada jugador (tanto en la vista de campo como en el editor) para registrarle un gol sin ir al desplegable de abajo.
- **Vista de campo tras guardar**: al guardar la alineación de un cuarto, en vez de quedarse el formulario de líneas abierto, se muestra un pequeño campo con los jugadores colocados por línea (portero arriba, delantero abajo) y su código de posición; hay un botón "✏️ Editar alineación" para volver a tocarla si hace falta.
- Los cuartos se siguen mostrando colapsados por defecto (ya estaba así en la versión anterior, que aún no se había publicado — ver aviso más abajo).
- Botones de "Guardar"/"Crear"/"Añadir"/"Eliminar" en toda la app (partido, cuarto, gol, jugador temporal, login…) muestran ahora un pequeño indicador de carga mientras se guarda, para saber que se está procesando y no volver a pulsar por duplicado.

**Aviso importante para el despliegue:** la versión que hay ahora mismo publicada en GitHub (`main`) es más antigua que la que se venía trabajando — no tiene el catálogo editable de tipos de jugador (usa una columna `tipo` en texto que ya no existe en la base de datos), ni los cuartos colapsables, ni el botón de eliminar partido. Al copiar este `index.html` con GitHub Desktop, se sube de una vez tanto lo que ya estaba pendiente de publicar como todo lo de hoy.

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

## 2026-09-09 — Jugadores y convocatoria

- Jugadores: sustituido el botón directo "Dar de baja" por "Editar", que abre un panel con nombre, alias/apodo (para reconocer variantes como "Xavi"/"Xavier" como el mismo jugador), dorsal y tipo; la baja pasa a hacerse desde ese panel y es una baja lógica (el jugador deja de aparecer en la plantilla, pero no se borra: se puede consultar en "Ver dados de baja" y reactivar). La lista principal solo muestra jugadores activos.
- Convocatoria: al crear un partido, todos los jugadores activos quedan convocados por defecto; ahora solo hace falta desmarcar a quien falte (antes había que marcar uno a uno). Añadido botón "Marcar todos" para partidos ya creados. Quitado el distintivo "fijo/temporal" de esta lista, que no aportaba y descuadraba la vista.
- Pendiente (a futuro, si la entrada manual resulta incómoda): reconocer la convocatoria y las alineaciones directamente a partir de la foto que envía el entrenador, como ya hace el proyecto de Fútbol Jueves Mundial.

## 2026-09-09 — Móvil, foto de convocatoria y admin

- Cabecera y formularios adaptados a móvil (antes se veían mal en pantallas estrechas).
- Cambiar contraseña ahora pide primero la contraseña actual, por seguridad.
- La foto de convocatoria se ve directamente en la ficha del partido en vez de solo el nombre del fichero.
- Al crear un partido, el tipo de partido recuerda el último que usaste.
- Nueva pantalla "Admin" para mantener catálogos (de momento, tipos de partido: añadir, renombrar y eliminar si no está en uso).

## 2026-09-09 — Admin ampliado y ficha de partido remaquetada

- El tipo de jugador (antes fijo/temporal fijo en el código) es ahora un catálogo editable desde Admin, igual que el tipo de partido — se puede añadir, renombrar o eliminar (si no hay jugadores usándolo). Solo los jugadores de tipo "Fijo" se marcan automáticamente en la convocatoria de un partido nuevo; los demás tipos (p.ej. "Temporal") hay que marcarlos a mano.
- "Cambiar contraseña" se ha movido a la pantalla Admin (antes estaba en la cabecera, donde no cabía bien en móvil). Sigue pidiendo la contraseña actual antes de dejar poner una nueva.
- Ficha de partido: la convocatoria se muestra en una rejilla más clara; el botón "Marcar todos" ahora cambia a "Desmarcar todos" cuando ya están todos marcados, y vuelve a "Marcar todos" en cuanto se desmarca alguno a mano.
- Añadido botón para eliminar un partido completo (con confirmación), incluyendo sus cuartos, alineaciones y goles.
- Los 4 cuartos de un partido ahora se muestran colapsados por defecto (con el resultado a simple vista) y se abren al pulsar sobre cada uno, para no tener que ver todo el formulario de golpe. Reordenada y separada visualmente la sección de goles/asistencias dentro de cada cuarto, que antes quedaba perdida por la maquetación.
