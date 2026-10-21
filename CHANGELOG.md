# CHANGELOG — Alevín D

Histórico funcional del proyecto (qué cambia para el usuario, no detalles técnicos de implementación).

## 2026-10-21 (4) — Libro de temporada con páginas por partido y descarga en PDF

- **Una página por partido**: arriba el cartel de la convocatoria del partido (con los escudos y el rótulo del equipo), la fecha/hora/condición, el resultado en grande (con el equipo local primero), la lista de goleadores y asistentes con el cuarto en que se marcó, y debajo **4 dibujos de campo, uno por cuarto**, con la alineación (código y nombre de cada jugador, portero abajo) y el marcador del cuarto. Si el partido no tiene cartel, se pone un rótulo con los nombres de los equipos. Se mantiene la línea de "No convocados" con su motivo.
- **Portada por sección** con letras de cartel: "Pretemporada Temporada 26-27 Alevín D San Cayetano", "Liga 26-27 Alevín D San Cayetano", etc. La temporada se deduce de la fecha de los partidos (la nueva temporada empieza en julio) y los partidos van ordenados por fecha dentro de cada sección.
- **Elegir qué incluir**: casillas por tipo de partido (con el número de partidos de cada uno), botones "Todos" / "Ninguno", "👁 Ver libro" para ver las páginas en la app y "⬇ Descargar PDF" para bajarlo (A4, una página por partido). Se pueden marcar uno o varios tipos.
- Los escudos no se suben aparte: salen del propio cartel de convocatoria de cada partido.
- Se sustituye la antigua vista previa en texto del libro.

## 2026-10-21 (3) — Goleadores bajo el resultado y resumen de cuartos jugados

- **Goleadores y asistentes bajo el resultado**: en la ficha del partido, justo debajo del resultado total, aparece la lista de todos los goles del partido en orden. Cada gol indica el autor, el asistente si lo hay y el cuarto en que se marcó (p.ej. "⚽ Jaime · asist. Noa (C1)").
- **Resumen de cuartos jugados al final de la ficha**: un bloque "📊 Resumen: cuartos jugados" agrupa a los jugadores según cuántos cuartos han jugado (4, 3, 2, 1 o 0). Cada grupo lleva un círculo de color con el número, de verde (4 cuartos) a naranja (1), y gris para los convocados que no han jugado. Así se ve de un vistazo quién ha jugado menos.
  - Junto al nombre aparece 🧤 con el número de cuartos jugados de portero, si es el caso.
  - El resumen se calcula a partir de las alineaciones guardadas y solo aparece cuando hay al menos un cuarto con alineación.

## 2026-10-21 (2) — Motivo de no convocatoria

- **Motivo de no convocatoria por jugador**: en la convocatoria de cada partido, debajo de cada jugador que no está marcado aparece un desplegable para indicar por qué no viene. Las opciones son "Decisión técnica", "Enfermedad", "Viaje" y "+ Otro nuevo…". Esta última pide el nombre del nuevo motivo, lo añade a la lista para siempre y lo deja seleccionado. Indicar el motivo es opcional.
- Si después se convoca a ese jugador, el motivo se borra solo. Lo mismo pasa al usar "Marcar todos".
- **Nueva sección en Admin, "Motivos de no convocatoria"**, para añadir, renombrar o eliminar motivos. Igual que con los tipos de partido y de jugador, solo se pueden eliminar los que no se han usado nunca.
- **En el libro de temporada**, cada partido muestra la línea "No convocados: Nombre (motivo), …" cuando se ha anotado algún motivo.

## 2026-10-21 — Convocatoria plegable y marcador con el equipo local primero

- **Convocatoria plegable**: en la ficha del partido, el bloque de convocatoria se puede plegar y desplegar igual que los cuartos, y en la cabecera muestra cuántos jugadores hay convocados. Mientras el partido no tiene ningún cuarto guardado aparece abierto (para preparar la convocatoria). En cuanto se guarda el primer cuarto aparece plegado, para que no estorbe durante el partido. Si se abre o cierra a mano, se respeta mientras se sigue en el partido.
- **El equipo local siempre va primero en el resultado**: si jugamos en casa se muestra "Alevín D 2 - 1 Rival"; si jugamos fuera, "Rival 1 - 2 Alevín D". Se aplica en el listado de partidos, en el resultado total de la ficha, en el resumen de cada cuarto y en el libro de temporada.
  - Los campos para introducir goles (el resultado oficial y el marcador de cada cuarto) siguen también ese orden y llevan el nombre de cada equipo ("Goles Alevín D" / "Goles <rival>") en vez de "a favor/en contra" o "equipo/rival", para no confundirse al jugar fuera.
  - No cambia nada de lo guardado: solo el orden en que se muestra.

## 2026-09-29 (2) — Alineación por jugador, foto plegable y corrección de errores al guardar cuartos

- **Nuevo modo de alineación "👤 Por jugador"**: al editar la alineación de un cuarto se elige, junto a cada convocado, su posición (GK, LD, DCD, DCC, DCI, LI, MD, MCD, MC, MCI, MI, MP, ED, DD, DC, DI, EI) o "no juega". La app deduce sola la línea y el orden derecha→izquierda. Debajo se ve un resumen en vivo de quién juega y dónde.
- **Se mantiene el modo clásico "↔️ Por líneas"** (añadir jugadores a cada línea en orden). Se cambia de uno a otro con dos botones encima del editor, sin perder lo que ya esté puesto, y la app recuerda el último modo usado.
- Los códigos de posición corregidos a mano ya no se pierden al volver a editar un cuarto ni al cambiar de modo.
- **Foto de convocatoria plegable**: en la ficha del partido la foto aparece plegada por defecto (como los cuartos) y se despliega al pulsar. Al subir una foto nueva se despliega sola para verla.
- **Corregidos los errores de "clave duplicada" al guardar un cuarto** (vistos el 26/09 por la mañana en el cuarto 1). Qué pasaba:
  1. Se podía poner al mismo jugador en dos líneas del mismo cuarto; al guardar, la base de datos lo rechazaba, pero el cuarto ya se había creado a medias.
  2. La pantalla no se enteraba de que el cuarto ya existía, así que cada nuevo intento de guardar trataba de crearlo otra vez y fallaba siempre con el mismo error, hasta recargar la página.
  Ahora un jugador colocado en una línea ya no aparece para añadirlo en otra, y antes de guardar se comprueba que nadie esté repetido. Si el cuarto ya existe se actualiza en vez de intentar crearlo de nuevo. Si algo falla, se restaura la alineación anterior y la pantalla se resincroniza con lo guardado sin perder lo que se estaba editando.
  Además, el portero heredado del cuarto anterior ya no se añade si ese jugador está jugando en otra posición en este cuarto (antes también provocaba el duplicado); se avisa para elegir otro.

## 2026-09-29 — Editar datos de un partido ya creado

- **Botón "✏️ Editar datos"** en la ficha del partido (junto al título): permite corregir rival, fecha, hora, condición (casa/fuera) y tipo de partido después de haberlo dado de alta. Al guardar se actualiza también el listado de partidos; "Cancelar" deja todo como estaba. La convocatoria, cuartos, alineaciones y goles no se tocan.
- **Foto de convocatoria editable**: el selector de fichero se sustituye por un botón "📷 Subir foto" que pasa a ser "📷 Cambiar foto" cuando ya hay una, y un botón "Quitar foto" (con confirmación). Al cambiar la foto se ve la nueva al momento (antes podía seguir apareciendo la anterior por caché) y la antigua se borra del almacenamiento para no acumular ficheros huérfanos.

## 2026-09-12 (2) — Orientación del campo, botón de eliminar, gol rápido y estadísticas/libro

- **Portero abajo en la vista de campo**: se ha invertido el orden de las líneas — ahora se pinta delantero arriba y portero abajo, como un campo visto desde el propio banquillo. El lateral/central izquierdo sigue apareciendo a la izquierda de la pantalla y el derecho a la derecha (no se ha tocado ese eje).
- **Quitado el botón de gol rápido (⚽ +1) de la vista de campo** — se mantiene solo en el formulario de edición de líneas, para evitar toques accidentales al mirar la alineación.
- **"Eliminar partido" menos accesible**: ya no está arriba junto al título del partido; ahora hay que desplegar "Opciones avanzadas" al final de la ficha del partido para verlo, y se muestra más pequeño.
- **Nueva pestaña "Estadísticas"**: tabla por jugador con cuartos jugados, goles, asistencias y — si ha jugado de portero — cuartos de portero y goles encajados (calculado sumando los goles del rival de esos cuartos). Se recalcula a partir de lo que ya hay guardado, así que también sirve para comprobar de un vistazo que todo se está registrando bien.
- **"Libro de temporada" con contenido real**: en vez del aviso de "Próximamente", ahora se ve una vista previa con todos los partidos guardados — resultado, y por cada cuarto la alineación (línea a línea) y los goles/asistencias. Es la base sobre la que se construirá el PDF final (con diseño y estadísticas de equipo), que sigue pendiente.

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
