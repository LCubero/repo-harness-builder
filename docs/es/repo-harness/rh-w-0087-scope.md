# Alcance propuesto RH-W-0087

**Estado: completado.** Este documento aprobado por el propietario conserva el limite historico de la correccion minima de session-stop generado, implementada y verificada el 2026-09-04. Las fases de redaccion del alcance e implementacion no autorizaron ni entregaron por si mismas cambios Git; el [registro de la hoja de ruta](roadmap.md#rh-w-0087) posee la evidencia exacta de la entrega posterior.

## Contrato generado exacto

El comportamiento generado del harness nunca debe proponer, agregar ni crear un trailer `Co-Authored-By`, sin importar la identidad nombrada. Tampoco debe agregar una atribucion equivalente de autoria o asistencia de IA en el asunto, cuerpo o trailers de un commit. Claude, OpenAI, ChatGPT, Gemini y cualquier IA son ejemplos, no una lista exhaustiva de proveedores ni una lista de permitidos o prohibidos.

El futuro imperativo conciso pertenece en session-stop generado inmediatamente antes de la accion existente `Then create the commit exactly once`. No debe duplicarse en `REPO-HARNESS.md`, un archivo de reglas ni otro propietario de politica.

La metadata de commit escrita por personas permanece fuera de esta unidad. Sin embargo, si se pide al flujo generado crear un commit cuyo mensaje aprobado contiene un trailer `Co-Authored-By` o una atribucion de IA prohibidos, debe pausar sin crear el commit. Debe informar el conflicto y obtener una nueva decision humana; no debe reescribir en silencio el mensaje aprobado.

## RED enfocado y sincronizacion

La implementacion debe comenzar con un RED enfocado que se ejecute antes de cualquier cambio de producto y falle porque el contrato renderizado de session-stop generado carece de la obligacion. Las aserciones enfocadas del contrato deben demostrar despues:

1. que el contrato generado lleva la prohibicion exactamente una vez y con el significado definido arriba;
2. que la prohibicion aparece inmediatamente antes de `Then create the commit exactly once` y en el mismo parrafo de creacion del commit;
3. que la comprobacion exacta del alcance preparado, el limite de aprobacion conversacional, el limite de un solo commit y la invalidacion del consentimiento cuando cambia el mensaje aprobado permanecen intactos y en su orden existente; y
4. que la salida generada canonica y todas las proyecciones soportadas de runtime para Claude Code, OpenCode y Pi quedan sincronizadas mediante el modelo existente de propiedad del renderer y el checker.

El RED y el GREEN posterior demuestran el contrato de instrucciones generado y la paridad de proyecciones. No autorizan un evaluador de modelo vivo, detector de IA, clasificador de mensajes de commit ni parser general de trailers Git.

## Limites

- Sin escaneo ni reescritura de historia y sin inspeccion de commits ajenos.
- Sin parser o framework general de trailers Git, binario, cambio de schema, estado del manifest o checker, lista especifica de proveedores, deteccion o clasificacion de IA ni instalacion de hooks de commit.
- Sin regla duplicada en el reglamento, un archivo de reglas, otra seccion de skill u otro propietario de politica.
- Sin mutar metadata escrita por personas. Esta unidad gobierna solo si el flujo generado puede crear el commit solicitado actualmente; un mensaje aprobado conflictivo causa una pausa sin reescribirlo.
- Sin debilitar ni reordenar la validacion exacta del alcance preparado, la aprobacion, la ejecucion de un solo commit, la actualizacion de estado posterior al commit, la separacion del push ni la invalidacion del consentimiento cuando cambia el alcance de rutas o el mensaje del commit.
- Sin cambio de fuente de producto, prueba de producto, skill generada, proyeccion de runtime, configuracion, indice Git, commit, remoto, issue, pull request, release ni publicacion bajo esta unidad de redaccion de alcance.
- No implementar el Plan 31 al mismo tiempo que el Plan 18; `RH-W-0068` conserva todo el demas comportamiento de cierre y ruta Git de session-stop generado.

## Evidencia y ciclo de vida

Hecho verificado del repositorio: el parrafo de creacion de commit en session-stop generado ejecuta la comprobacion final del alcance preparado y luego dice `Then create the commit exactly once`; no contiene una prohibicion de atribucion de IA. Evidencia completada y verificada de `RH-W-0084`: la distancia entre la instruccion y el punto de accion, no el enfasis, predice si se aplica una obligacion. Decision humana: los commits creados por el harness generado deben obedecer la prohibicion exacta y el limite de pausa definidos aqui.

El propietario aprobo este alcance bilingue y autorizo por separado su unica implementacion acotada con pruebas primero el 2026-09-04. El RED enfocado fallo solo porque faltaba la obligacion generada; el GREEN enfocado y la regresion relacionada de nueve pruebas pasaron, todas las proyecciones de runtime estan vigentes con cero escrituras del checker y `RH-W-0087` esta `completed`. Esas fases historicas no autorizaron ni entregaron por si mismas cambios Git; la evidencia exacta de la entrega posterior se mantiene en el [registro de la hoja de ruta](roadmap.md#rh-w-0087).
