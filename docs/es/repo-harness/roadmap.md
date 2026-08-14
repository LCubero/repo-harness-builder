# Hoja de ruta del harness adaptativo

<!-- doc-role:lifecycle-owner -->

La fuente factual canonica es [`../../repo-harness/roadmap.md`](../../repo-harness/roadmap.md). Este es su espejo humano completo en espanol. La hoja canonica controla todo pendiente no trivial, deseo, capacidad aplazada, exclusion, rechazo y resultado completado. `current-state.md` solo proyecta actividad concisa y Engram solo refleja resultados bajo `work/RH-W-####`.

## Contrato del ciclo de vida

<!-- doc-role:lifecycle-rules -->

- Los IDs son inmutables e independientes del orden, issue y prioridad.
- La prioridad es `P0`, `P1`, `P2` o `P3` con justificacion. El estado es `proposed`, `ready`, `active`, `blocked`, `deferred`, `completed`, `excluded` o `rejected`.
- `blocked` nombra el bloqueo, `deferred` una condicion observable, y los registros terminales usan trabajo restante `none`.
- `excluded` puede reconsiderarse fuera del alcance actual. `rejected` se declina hasta que una decision explicita posterior lo reemplace.
- Cuando cambie la realidad, se reconcilian en la misma unidad la hoja canonica, estado actual, pares localizados y la observacion pequena de Engram. Las observaciones triviales no necesitan registro.

## Autoridad y base

Codigo y pruebas poseen el comportamiento; la hoja canonica posee el ciclo de vida; [estado actual](current-state.md) posee la proyeccion activa; [decisiones](decisions.md) posee razones permanentes; [CHANGELOG.md](../../../CHANGELOG.md) posee la cronologia.

Se conserva la base anterior: checker `current`, salida 0 y 0 escrituras; PR #2 integrado como `ec56b1b` para issue #1; ampliacion de runtimes completa; issue #8 completo para `.codegraph/`; ronda 2 de Judgment Day incompleta por limite de sesion del juez B; cinco hallazgos de ronda 1 y capacidades posteriores identificados.

## Registros activos

<!-- doc-role:active-records -->

### RH-W-0016

Origen: issue #7. Prioridad: `P0`, excepcion porque el control durable debe existir antes de acumular deseos. Dependencia: RH-W-0001. Estado: `completed`. Trabajo completado: IDs, registros, proyeccion concisa, propiedad bilingue, contratos y validacion. Trabajo restante: `none`. Adicion: issue #7 se presenta antes de la antigua unidad 3 sin renumerar. Exclusiones: sin base de datos, daemon, scheduler, sync de issues, motor generico, API de transiciones, calculo automatico ni registro trivial. Aplazamientos: ninguno. Rechazo: Engram y estado actual no son propietarios. Evidencia: TDD y verificacion de `feat/pending-lifecycle`. Historial: diseno aprobado el 2026-08-13. Siguiente condicion: mantener el contrato; RH-W-0003 sigue como proximo comportamiento.

### RH-W-0017

Origen: ocurrencia informada por la persona usuaria durante un cierre en espanol y conflicto canonico en `skill/repo-harness-builder/scripts/harness_tool.py`, dentro del contrato de la pregunta para solo commit. Prioridad: `P1`, el defecto viola el idioma activo en una compuerta obligatoria de aprobacion, pero no causa perdida de datos, mutacion insegura ni un flujo irrecuperable. Dependencias: ninguna. Estado: `ready`. Trabajo completado: se registro la fuga observada, se reconcilio con el contrato canonico exacto y se hizo durable el alcance de la correccion. Trabajo restante: conservar exactamente el alcance semantico de la aprobacion, las rutas y el mensaje de commit propuesto mientras se localiza la frase visible para la persona en el idioma activo de la conversacion. Adicion: agregar cobertura enfocada para una conversacion activa que no este en ingles en la compuerta de aprobacion de solo commit cuando comience la implementacion. Exclusiones: sin corregir el defecto en esta unidad, sin cambiar la semantica de aprobacion, sin reescribir ampliamente session-stop y sin mutar Git ni el proveedor. Aplazamientos: ninguno. Rechazo: se rechazo mantener la redaccion inglesa exacta como obligatoria cuando el contrato circundante exige el idioma activo. Evidencia: durante una conversacion activa en espanol, session-stop termino con `May I stage exactly these paths and create this proposed commit: ...?`; el contrato canonico fija esa frase inglesa y a la vez exige continuidad en el idioma conversacional. Historial: el 2026-08-13 la persona usuaria informo la ocurrencia y pidio continuidad durable; se clasifico P1 y listo porque la causa y la correccion acotada se conocen y no tienen dependencias. Siguiente condicion: comenzar con una prueba fallida del contrato de session-stop para un cierre de solo commit en espanol y despues localizar la frase visible sin cambiar alcance, rutas ni mensaje.

### RH-W-0003

Origen: unidad 3 y hallazgo de Judgment Day. Prioridad: `P1`, un harness bilingue heredado puede perder comprobacion en espanol. Dependencia: RH-W-0016. Estado: `completed`. Completado: se agrego la regresion canonica y se infieren solo arboles localizados conocidos ya instalados cuando un manifest heredado carece de `language`; las declaraciones modernas explicitas siguen siendo autoritativas. Restante: `none`. Adicion: conservar la ronda 2 incompleta como evidencia. Exclusion: sin framework de migracion. Aplazamientos y rechazos: ninguno. Evidencia: issue #11 en `fix/legacy-bilingual-manifest`; la regresion focal fallo porque la ruta espanola ausente no estaba en los outputs esperados, luego paso con la inferencia minima; la suite canonica paso 197 pruebas con 6 omisiones. Historial: se mantuvo como proximo comportamiento el 2026-08-13 y se completo el 2026-08-14 sin migracion ni rediseno del CLI. Siguiente condicion: conservar la regresion cuando evolucionen los arboles conocidos o las declaraciones explicitas.

### RH-W-0004

Origen: unidad 4 y hallazgo de Judgment Day. Prioridad: `P1`, flags ignorados vuelven ambiguo el contrato. Dependencia: RH-W-0003. Estado: `ready`. Completado: se identificaron los tres flags ignorados y RH-W-0003 establecio el descubrimiento heredado. Restante: eliminarlos o respetarlos y probar CLI y ayuda. Adiciones: ninguna. Exclusion: sin rediseno amplio del CLI. Aplazamientos y rechazos: ninguno. Evidencia: `check` acepta flags sin usarlos. Historial: ordenado despues de locale heredado. Siguiente condicion: comenzar con pruebas focales del CLI que expongan los flags de idioma ignorados.

### RH-W-0005

Origen: unidad 5 y hallazgo de Judgment Day. Prioridad: `P1`, declaraciones invalidas fallan con diagnostico enganoso. Dependencias: RH-W-0003 y RH-W-0004. Estado: `blocked`. Completado: valores invalidos y campo incorrecto identificados. Restante: normalizar o rechazar con matriz de regresion. Adiciones: ninguna. Exclusion: sin nuevos idiomas. Aplazamientos y rechazos: ninguno. Evidencia: `declared_languages` deja pasar valores invalidos. Historial: queda tras el contrato de idiomas. Siguiente condicion: completar las dos dependencias.

### RH-W-0006

Origen: unidad 6 y hallazgo de Judgment Day. Prioridad: `P2`, pruebas validas fallan en layout monolingue. Dependencia: RH-W-0003. Estado: `ready`. Completado: dos pruebas que suponen espanol identificadas y RH-W-0003 establecio el descubrimiento instalado. Restante: respetar idiomas declarados y probar modos ingles y bilingue. Adiciones: ninguna. Exclusion: no debilitar controles bilingues. Aplazamientos y rechazos: ninguno. Evidencia: las pruebas fallan en vez de omitirse. Historial: correccion enfocada. Siguiente condicion: comenzar con fixtures focales solo en ingles y conservar comprobaciones bilingues cuando se declara espanol.

### RH-W-0008

Origen: unidad 8, brecha de reemplazo e integracion. Prioridad: `P1`, planes aprobados no ejecutan. Dependencias: RH-W-0003 a RH-W-0007. Estado: `blocked`. Completado: limites de aprobacion ligada al contenido, preservacion, cambio de plan y rollback especificados. Restante: ejecutar una accion con digest, bytes, rollback y preview. Adiciones: ninguna. Exclusion: sin motor masivo. Aplazamientos y rechazos: ninguno. Evidencia: el planner solo describe. Historial: primera mutacion tras corregir prerrequisitos. Siguiente condicion: completar o demostrar irrelevantes esas dependencias.

### RH-W-0009

Origen: unidad 9, selector README existente. Prioridad: `P1`, la reciprocidad localizada sigue bloqueada. Dependencia: RH-W-0008. Estado: `blocked`. Completado: cierre seguro y casos LF, CRLF, reciproco, duplicado y plan cambiado especificados. Restante: accion reciproca acotada. Adiciones: ninguna. Exclusion: sin reescritura general de README. Aplazamientos y rechazos: ninguno. Evidencia: hoy no existe integracion segura. Historial: dependencia explicita para que la idea no desaparezca. Siguiente condicion: RH-W-0008 entrega el ejecutor.

### RH-W-0010

Origen: unidad 10, builder instalado obsoleto. Prioridad: `P2`, no se distingue paquete de proyeccion. Dependencia: RH-W-0008. Estado: `blocked`. Completado: fixtures vigente, obsoleto, ausente y no verificable especificados. Restante: detectar vigencia del paquete por separado. Adiciones: ninguna. Exclusion: sin promesa de recarga. Aplazamientos y rechazos: ninguno. Evidencia: el checker generado no prueba la vigencia del builder. Historial: concepto separado de drift. Siguiente condicion: RH-W-0008 define evolucion segura.

### RH-W-0011

Origen: unidad 11, revision de `skills-pack-ld9`. Prioridad: `P2`, hallazgos vigentes requieren evidencia publica o cierre. Dependencias: RH-W-0003 a RH-W-0010. Estado: `deferred`. Completado: se prohibio importar notas privadas u obsoletas a ciegas. Restante: trazar cada hallazgo. Adicion: recibo incompleto de ronda 2. Exclusion: sin importacion ciega. Aplazamiento: hasta reducir hallazgos obsoletos. Rechazos: ninguno. Evidencia: informe externo y juez B ausente. Historial: umbral observable. Siguiente condicion: resolver dependencias o hallar un riesgo urgente independiente.

### RH-W-0012

Origen: unidad 12, investigacion externa segura. Prioridad: `P2`, reproducibilidad sin ejecutar codigo ajeno. Dependencia: RH-W-0011. Estado: `deferred`. Completado: fuentes primarias, clones ignorados, licencia, revision, no ejecucion, citas y retencion definidos. Restante: documentar, probar limites y validar enlaces. Adiciones: ninguna. Exclusion: sin dependencia publicada de rutas privadas, ignoradas o locales. Aplazamiento: hasta una necesidad concreta. Rechazos: ninguno. Evidencia: diseno anterior. Historial: capacidad condicional. Siguiente condicion: RH-W-0011 identifica una pregunta externa.

### RH-W-0013

Origen: unidad 13, presupuesto de contexto. Prioridad: `P2`, contratos cargados siempre necesitan limites medidos. Dependencia: crecimiento medido. Estado: `deferred`. Completado: presupuesto del rulebook y direccion de indices, referencias y revelacion progresiva. Restante: presupuestos medidos para skills y contratos. Adiciones: ninguna. Exclusion: sin limite arbitrario a toda documentacion humana. Aplazamiento: hasta medir friccion. Rechazos: ninguno. Evidencia: solo `REPO-HARNESS.md` tiene proxy de 700 tokens. Historial: medir antes. Siguiente condicion: acercarse a un limite acordado o demostrar friccion.

### RH-W-0014

Origen: unidad 14, contribucion en consumidores. Prioridad: `P3`, falta evidencia de consumidores y proveedor. Dependencia: RH-W-0012. Estado: `deferred`. Completado: escenarios neutrales y conscientes de capacidades. Restante: disenar descubrimiento antes de proponer archivos, issues, ramas, PR, templates, labels o checks. Adiciones: ninguna. Exclusion: sin mutacion ni implementacion del proveedor. Aplazamiento: hasta un consumidor real. Rechazos: ninguno. Evidencia: este repo tiene flujo propio, no descubrimiento. Historial: disenar antes de implementar. Siguiente condicion: caso real y capacidades verificadas.

### RH-W-0015

Origen: unidad 15, ideas MCP, CLI, binario y terceros. Prioridad: `P3`, comprometer arquitectura sin necesidad seria sobreingenieria. Dependencias: RH-W-0010, RH-W-0012, RH-W-0013 y RH-W-0014. Estado: `deferred`. Completado: formas candidatas y comparacion requerida registradas. Restante: reevaluar necesidades y compromisos. Adiciones: ninguna. Exclusion: sin compromiso actual. Aplazamiento: hasta evidencia medida. Rechazos: ninguno. Evidencia: el paquete Python actual resuelve el uso presente. Historial: aplazado explicitamente, no descartado. Siguiente condicion: demostrar una necesidad que el paquete actual no resuelve limpiamente.

## Registros resueltos

<!-- doc-role:resolved-records -->

### RH-W-0001

Origen: unidad 1, deriva de evidencia. Prioridad: `P0`, todo depende de verdad reconciliada. Dependencias: ninguna. Estado: `completed`. Completado: reconciliacion entre docs, fuente, pruebas, historial, Git y Engram. Restante: `none`. Adicion: resolucion organica sin lecturas exhaustivas. Exclusion: sin cambiar backlog desde una fuente. Aplazamientos: ninguno. Rechazo: confiar en una fuente. Evidencia: contratos y pares bilingues. Historial: completado el 2026-08-13. Siguiente condicion: reabrir solo ante regresion.

### RH-W-0002

Origen: unidad 2, issue #8. Prioridad: `P1`, conflicto de `.codegraph/`. Dependencia: RH-W-0001. Estado: `completed`. Completado: ignore exacto, opt-out persistente, manifiesto, bytes y cambio de plan. Restante: `none`. Adicion: `project.ignoreCodegraph`. Exclusion: sin politica ajena ni escrituras internas. Aplazamientos: ninguno. Rechazo: correccion solo documental. Evidencia: TDD de todos los casos. Historial: completado el 2026-08-13. Siguiente condicion: reabrir ante regresion.

### RH-W-0007

Origen: unidad 7 y hallazgo de Judgment Day. Prioridad: `P2`, falsos faltantes en documentos antiguos. Dependencia: RH-W-0001. Estado: `completed`. Completado: el documento prometido elige vocabulario y ambas direcciones tienen cobertura. Restante: `none`. Adicion: prueba de seccion realmente perdida. Exclusion: sin reescribir parser. Aplazamientos: ninguno. Rechazo: extraer vocabularios independientes. Evidencia: pruebas `test_section_comparison_*`. Historial: evidencia actual reemplazo la fila obsoleta. Siguiente condicion: reabrir ante regresion.

## Registro de decisiones

- Los issues son solo origen, nunca ID ni orden.
- Las unidades antiguas conservan `RH-W-0001` a `RH-W-0015`; issue #7 es `RH-W-0016` y su excepcion P0 no renumera.
- La fuga de idioma de session-stop informada por la persona usuaria es `RH-W-0017`; esta lista de forma independiente y no desplaza la unidad de producto RH-W-0003 ya seleccionada.
- Se evitaron MCP, CLI, binario, terceros, framework de migracion, reescritura general, motor masivo, mutacion de proveedor, promesa de recarga y limite universal sin evidencia.
