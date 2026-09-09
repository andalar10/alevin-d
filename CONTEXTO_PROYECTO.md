# Contexto del proyecto — Alevín D

## Protocolo anti-regresión — aplicar antes de editar `index.html`

1. **Descargar siempre la versión real y desplegada antes de editar**: `https://raw.githubusercontent.com/andalar10/alevin-d/main/index.html` (ajustar si el repo/rama cambian). No editar nunca a partir de la copia que esté en el contexto de la conversación sin verificar antes que coincide con la desplegada.
2. **Comprobar funciones marcadoras tras cada cambio** (grep sobre el fichero para confirmar que siguen presentes): `calcularCodigos`, `renderCuarto`, `guardarCuarto`, `renderPartidoDetalle`, `subirFotoConvocatoria`, `anadirGol`.
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
- Restringir el envío del enlace de acceso a que solo lo pueda solicitar el email autorizado (hoy cualquiera puede pedir un enlace, pero solo ese email pasa la RLS y ve datos).
