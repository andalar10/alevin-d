# Contexto del proyecto — Alevín D

## Protocolo anti-regresión — aplicar antes de editar `index.html`

1. **Descargar siempre la versión real y desplegada antes de editar**: `https://raw.githubusercontent.com/andalar10/alevin-d/main/index.html` (ajustar si el repo/rama cambian). No editar nunca a partir de la copia que esté en el contexto de la conversación sin verificar antes que coincide con la desplegada.
2. **Comprobar funciones marcadoras tras cada cambio** (grep sobre el fichero para confirmar que siguen presentes): `calcularCodigos`, `renderCuarto`, `renderEditorLineasCuarto`, `renderCampoCuarto`, `guardarCuarto`, `golRapido`, `incrementarMarcador`, `renderPartidoDetalle`, `subirFotoConvocatoria`, `anadirGol`, `renderAdmin`, `cambiarContrasena`, `conSpinner`, `irAPartidoActual`.
3. **Actualizar `CHANGELOG.md`** tras cada cambio relevante, describiendo la funcionalidad, no detalles técnicos.
4. **Validar sintaxis JS** con `node --check` tras cada edición (extraer el bloque `<script>` inline, comprobar el último match).

## Modelo de datos (Supabase, proyecto `fxgjzfpasyxwiegmiwzb`, compartido con `compra-familiar`)

Todas las tablas del proyecto viven en el esquema `public` con el prefijo `alevin_d_` para no mezclarse con las de otras apps que comparten este mismo proyecto de Supabase (usado como backend de identidad común). El login usa el mismo proyecto pero los datos están aislados por prefijo + RLS restringido al email `andres.alarcon@gmail.com`.

- `alevin_d_jugadores` — plantilla (fijos y temporales), con `dorsal` y `activo`.
- `alevin_d_tipos_partido` — catálogo ampliable (Amistoso, Liga, …).
- `alevin_d_partidos` — rival, fecha, hora, condición, tipo, foto de convocatoria (ruta en Storage), resultado final.
- `alevin_d_convocatoria` — qué jugadores fueron convocados a cada partido (permite dorsal puntual distinto, para temporales).
- `alevin_d_cuartos` — hasta 4 por partido, con resultado parcial.
- `alevin_d_alineacion_cuarto` — jugador + línea táctica + orden dentro de la línea + código de posición calculado (o corregido a mano).
- `alevin_d_goles` — goleador y asistente (opcional) por cuarto.
- Bucket de Storage `alevin-d-fotos` (privado, RLS restringido al mismo email) para las fotos de convocatoria.

## Lógica de posiciones

La línea táctica de cada jugador (portero/defensa/medio/media punta/delantero) la elige el usuario al introducir la alineación — es una decisión táctica, no deducible del número de líneas. Dentro de cada línea, el orden de introducción es siempre de derecha a izquierda (visto desde el equipo, que ataca hacia abajo). El código de posición se calcula a partir de (línea, nº de jugadores en esa línea) según esta tabla:

- Defensa: 1→DCC · 2→DCD,DCI · 3→LD,DCC,LI · 4→LD,DCD,DCI,LI
- Medio: 1→MC · 2→MCD,MCI · 3→MCD,MC,MCI · 4→MD,MCD,MCI,MI
- Media punta: siempre 1 jugador → MP
- Delantero: 1→DC · 2→DD,DI · 3→ED,DC,EI

Si aparece una combinación no prevista, el código queda vacío/editable a mano en la interfaz — no bloquea el guardado. Al confirmarse un nuevo caso, añadir la entrada correspondiente a `TABLA_POSICIONES` en `index.html` y a esta tabla.

El portero de un cuarto, si se deja vacío, se hereda del cuarto anterior del mismo partido (se busca hacia atrás hasta encontrar uno).

## Pendiente

- Generador de PDF del libro de temporada (jsPDF, siguiendo el patrón ya usado en el proyecto Fútbol Jueves Mundial): una sección por partido con alineaciones esquemáticas por cuarto, goles/asistencias, y estadísticas finales (más cuartos jugados, más goles, más asistencias) separadas por tipo de partido.
- Reconocimiento de convocatoria/alineación a partir de foto (como `extraer-alineacion` en Fútbol Jueves Mundial): de momento se prueba la entrada manual; si resulta incómoda, se añadirá una Edge Function con Claude Vision para extraerlo directamente de la foto que envía el entrenador.

## Estado de la vista de cuartos (desde 2026-09-12)

Cada cuarto puede estar en dos modos, independientes del acordeón abierto/cerrado (`CUARTOS_ABIERTOS`):
- **Editor de líneas** (`renderEditorLineasCuarto`): el formulario clásico de añadir/quitar jugadores por línea. Se muestra si el cuarto no tiene alineación guardada aún, o si está en el set `EDITANDO_CUARTO` (el usuario pulsó "✏️ Editar alineación").
- **Vista de campo** (`renderCampoCuarto`): solo lectura, jugadores agrupados por línea con su código de posición, y un botón "⚽ +1" por jugador para gol rápido (`golRapido`). Es el modo por defecto en cuanto el cuarto tiene alineación guardada.

Al guardar (`guardarCuarto`) se hace `EDITANDO_CUARTO.delete(n)`, así que tras guardar pasa automáticamente a la vista de campo.

El marcador de cada cuarto tiene botones +1/-1 (`incrementarMarcador`) que, si el cuarto ya existe en BD, actualizan `goles_equipo`/`goles_rival` al momento (sin esperar al botón "Guardar alineación").

## Persistencia de navegación (localStorage)

- `alevin_d_vista`: última pantalla visitada (`jugadores`/`partidos`/`libro`/`admin`/`partido-detalle`). Al recargar, `cargarTodo()` la usa para no volver siempre a "Jugadores".
- `alevin_d_partido_actual_id`: último partido abierto, independientemente de por dónde se navegue después. Alimenta el botón "⚡ Partido actual" del menú (`irAPartidoActual`).
- `alevin_d_ultimo_tipo_partido`: (ya existía) último tipo de partido usado al crear uno nuevo.

## Rendimiento de `cargarDetallePartido`

Antes se pedía la alineación y los goles cuarto a cuarto dentro de un bucle `for` con `await` secuencial (hasta 8 peticiones extra). Ahora se piden todas las alineaciones y todos los goles del partido de una vez con `.in('cuarto_id', idsCuartos)`, en paralelo con la convocatoria y los cuartos vía `Promise.all`, y se reparten en memoria por número de cuarto. Si se toca esta función, mantener el patrón de "una consulta por tabla para todo el partido", no una por cuarto.

## Histórico de decisiones

- El acceso pasó de "enlace mágico" (OTP por email) a email+contraseña: el envío de correo por defecto de Supabase tiene un límite muy bajo (pensado solo para pruebas) y se compartía con `compra-familiar`, lo que lo agotaba enseguida. Además, para una app de uso al pie del campo, depender del correo en cada entrada era poco práctico.
- La cuenta `andres.alarcon@gmail.com` ya existía en este proyecto de Supabase (de pruebas anteriores de `compra-familiar`) sin contraseña; se le añadió una directamente por SQL la primera vez en vez de recrearla, para no perder su `id` ni duplicar usuarios.
