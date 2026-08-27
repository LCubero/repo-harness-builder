# Diseno RH-W-0073 de control del presupuesto del rulebook

## Decision

RH-W-0073 cierra una Regla Dura que se esta incumpliendo sin que nada pueda detectarlo. El `REPO-HARNESS.md` autoalojado de este repositorio supera el presupuesto de 700 tokens proxy que el propio archivo declara, y el criterio de aceptacion que lo habria detectado describe un mecanismo que RH-W-0013 elimino deliberadamente.

El diseno responde tres preguntas y ninguna otra: que superficie de control detecta un incumplimiento, que detalle exacto baja del rulebook para devolverlo bajo presupuesto, y que reemplaza al criterio de aceptacion muerto.

Revise este diseno contra el [registro canonico RH-W-0073](roadmap.md#rh-w-0073), el [diseno RH-W-0013](rh-w-0013-design.md) cuya eliminacion no debe reabrirse, y el [propietario en ingles](../../repo-harness/rh-w-0073-design.md).

## Linea base medida

Medida en solo lectura con el propio `TOOL.token_proxy` y `TOOL.RULEBOOK_TOKEN_BUDGET` del producto, en `HEAD` `e290c53`:

| Superficie | Tokens proxy | Presupuesto | Estado |
|---|---|---|---|
| `REPO-HARNESS.md` (autoalojado) | 763 | 700 | excedido |
| `REPO-HARNESS.es.md` (declara el mismo presupuesto) | 581 | 700 | dentro |
| `.repo-harness/rules/response-contract.md` | 436 | 700 | dentro |
| `.repo-harness/rules/working-agreements.md` | 344 | 700 | dentro |
| `.repo-harness/rules/memory.md` | 317 | 700 | dentro |
| Rulebook renderizado con nombre de proyecto largo | 650 | 700 | dentro |

El maximo de palabras que aun mide por debajo de 700 es 525. El rulebook autoalojado tiene 574 palabras, asi que deben salir al menos 49.

Dos hechos mas enmarcan la brecha. `token_proxy` no tiene ninguna persona llamadora en produccion: sobrevive solo en pruebas, y toda prueba de presupuesto que queda evalua la salida de `TOOL.render(...)` en lugar de un archivo en disco. Y `over-budget` aparece hoy exactamente una vez en todo el producto, como criterio de aceptacion en `references/validation.md`, sin codigo, rama del verificador ni prueba capaz de emitirlo.

El incumplimiento es reciente y monotono: 699.58 proxy en `a2257ef` el 2026-08-23, 715.54 en `7ef4993` el 2026-08-24 y 763.42 en `41a23d7` el 2026-08-25. Se abrio el mismo dia en que se entrego la eliminacion y se ha ensanchado desde entonces.

## Dos problemas separados

Se separan a proposito, porque pueden responderse de forma distinta.

1. **El incumplimiento.** El rulebook de este repositorio excede hoy su propio presupuesto. Corregirlo es un movimiento editorial acotado, no un mecanismo.
2. **El control ausente.** Nada mide en disco un rulebook o un archivo de reglas evolucionado a mano contra el presupuesto que declara. Sin eso, el incumplimiento se reabre la proxima vez que una unidad agregue una Regla Dura.

Corregir solo lo primero deja al harness con una regla que nadie puede hacer cumplir. Corregir solo lo segundo deja una violacion conocida en el archivo que se carga en cada sesion.

## Superficie de control

El control es una **prueba en tiempo de desarrollo dentro de la suite canonica de este repositorio** que mide en disco las superficies presupuestadas autoalojadas contra `RULEBOOK_TOKEN_BUDGET`.

Esto reutiliza un mecanismo que la suite ya tiene en lugar de inventar uno. `test_harness_tool.py` ya resuelve el repositorio con `ROOT.parents[1]` y evalua archivos reales versionados: campos de ciclo de vida del roadmap, concordancia del estado actual con su propietario, finales de linea y cobertura de pares del changelog funcionan asi. Una asercion de presupuesto se suma a esa familia existente.

Conjunto medido:

- `REPO-HARNESS.md`, el archivo que se carga en cada sesion.
- Cada archivo bajo `.repo-harness/rules/`, cada uno de los cuales declara el mismo presupuesto en su propia seccion `Budget`.

`REPO-HARNESS.es.md` queda excluido a proposito. Declara el mismo presupuesto y mide 581, asi que no hay incumplimiento delante, y el registro acota esta unidad al rulebook y sus archivos de reglas. Medirlo seria resolver un problema que aun no llego.

Lo que el control no debe llegar a ser importa tanto como lo que es:

- Sin rechazo en render. `render_with_skill_mapping()` no recupera ningun `require()` de presupuesto. RH-W-0013 elimino exactamente eso y no se reabre.
- Sin rama en el verificador. `check()` no gana estado `over-budget`, ni estado nuevo, ni ruta de drift nueva. Sigue siendo de solo lectura y no bloqueante.
- Sin tope a la documentacion de la persona consumidora. El control mide las superficies autoalojadas de este repositorio. El rulebook evolucionado a mano de una persona consumidora sigue siendo suyo, y la exclusion de RH-W-0013 contra un tope arbitrario sobre documentacion humana se mantiene.
- Sin subir la constante. `RULEBOOK_TOKEN_BUDGET` se queda en 700. El rulebook declara que el presupuesto nunca sube, y esta unidad rechaza subirlo.

Se considero y se rechazo una medicion informativa reportada por `check.py` sin cambiar el estado. Devolveria logica de presupuesto al verificador en tiempo de ejecucion sin ninguna ganancia que la prueba en tiempo de desarrollo no entregue ya, y cualquier unidad posterior podria convertir un campo informativo en uno bloqueante sin reabrir esta decision.

## Compresion

Bajan dos bloques, ambos a `.repo-harness/rules/working-agreements.md`. Siguen el precedente de RH-W-0067, que dejo la obligacion en el rulebook como una linea corta y bajo el detalle al archivo de reglas que lo posee, detras de una fila de compuerta. Aqui ese archivo ya existe y ya tiene su fila, asi que no se agrega ninguna superficie.

**Movimiento 1: la taxonomia de clasificacion de evidencia (81 palabras).** La regla dura que empieza con "Classify each claim as verified fact" carga cuatro definiciones, una prohibicion de sustitucion y un procedimiento de verificar antes de preguntar. La obligacion es de las que no se pueden pasar por alto y se queda como una linea. La taxonomia es detalle y baja al `.repo-harness/rules/working-agreements.md` existente, bajo su seccion `Honesty`, que ya carga "Verify before claiming". Sin archivo de reglas nuevo y sin fila de compuerta nueva, porque el rulebook ya enruta a ese archivo. Ahorro neto estimado: 57 palabras.

Se considero y se rechazo un `evidence-classification.md` dedicado. Una taxonomia de 81 palabras no justifica una superficie propia nueva, una fila de compuerta nueva y un tercer lugar donde mirar. Los acuerdos de trabajo ya poseen la obligacion de honestidad que esta taxonomia refina.

**Movimiento 2: la regla de anunciar contra ejecutar (35 palabras).** La regla dura que empieza con "Do not stop after merely announcing" es disciplina de ejecucion, que `.repo-harness/rules/working-agreements.md` ya posee detras de una fila de compuerta existente. Baja alli completa y deja atras una linea comprimida. No hace falta fila nueva. Ahorro neto estimado: 23 palabras.

Ambos movimientos aterrizan en el mismo archivo existente. En conjunto devuelven el rulebook a unas 494 palabras, unos 657 tokens proxy, con unos 43 tokens de margen. `working-agreements.md` sube de 344 a unos 499 tokens proxy, bien dentro del mismo presupuesto.

Ese margen es deliberadamente modesto y no es el mecanismo. El mecanismo es el control. Un margen amplio solo aplazaria el proximo incumplimiento; una asercion medida lo detecta el mismo dia en que entra.

El texto exacto de reemplazo es trabajo de implementacion bajo este diseno. Los bloques nombrados aqui son el compromiso del diseno; un plan no puede sustituirlos por otros sin volver a la persona propietaria.

## Criterio de aceptacion corregido

`references/validation.md:12` dice hoy, en parte, "each budgeted file reports `over-budget` once it passes the limit". Ningun mecanismo hace eso. El criterio se corrige para declarar lo que si es cierto despues de RH-W-0013: cada archivo presupuestado declara su limite, mientras render y check en tiempo de ejecucion siguen sin bloquear. Un segundo criterio registra la medicion en tiempo de desarrollo de las superficies autoalojadas.

Segun la relacion de coherencia de planes ya registrada, RH-W-0073 posee este archivo para este cambio y RH-W-0072 no lo edita. RH-W-0072 conserva todas las demas superficies de deriva documental.

## Criterios de aceptacion

- El `REPO-HARNESS.md` autoalojado mide por debajo de 700 tokens proxy, y `.repo-harness/rules/working-agreements.md` sigue por debajo tras recibir ambos bloques movidos.
- El par en espanol sigue siendo un espejo equivalente bajo el contrato i18n, reconciliado en la misma unidad de trabajo pero fuera del conjunto medido.
- No se pierde ninguna obligacion de Regla Dura. Cada bloque movido es alcanzable desde el rulebook por exactamente una fila de compuerta.
- Una prueba en tiempo de desarrollo falla cuando cualquier superficie autoalojada medida alcanza el presupuesto, y nombra la ruta infractora.
- El comportamiento de `render()` y `check()` es identico byte a byte para toda entrada existente. Los rulebooks de exactamente 700 y 701 siguen renderizando, y un archivo de reglas grande evolucionado a mano sigue reportando `human-evolved` con estado global `current`.
- `RULEBOOK_TOKEN_BUDGET` no cambia: sigue en 700.
- `references/validation.md` no contiene ninguna afirmacion de que un mecanismo eliminado emita `over-budget`.
- El verificador reporta `current` con cero escrituras, y ambas proyecciones de runtime quedan identicas byte a byte a la canonica.

## Pruebas y evidencia

La implementacion es test-first y focalizada, no una suite recursiva completa. Casos RED esperados:

1. El `REPO-HARNESS.md` autoalojado mide 700 o mas. Debe fallar antes de la compresion y pasar despues.
2. Cada archivo de `.repo-harness/rules/` mide por debajo de 700, nombrando la ruta que falla.
3. Cada obligacion movida se resuelve desde el rulebook por exactamente una fila de compuerta.
4. `render()` sigue emitiendo sin cambios un rulebook de exactamente 700 y otro de 701, probando que no volvio ningun rechazo.
5. `check()` no reporta estado `over-budget` para un archivo de reglas sobredimensionado evolucionado a mano.

GREEN focalizado mas la suite canonica, verificador `current` con cero escrituras y paridad de proyecciones son evidencia de cierre suficiente.

## No objetivos

Este diseno no agrega funcionalidad de presupuesto en tiempo de ejecucion, campo de manifiesto, estado del verificador, cambio de esquema, parser, vocabulario de estado nuevo, tope sobre documentacion humana o de la persona consumidora, constante elevada, ni reapertura de RH-W-0013. No corrige ninguna superficie documental de RH-W-0072 distinta del unico criterio de aceptacion nombrado arriba, y no autoriza implementacion.

## Migracion y compatibilidad

El comportamiento visible para la persona consumidora no cambia. Ningun manifiesto instalado se reescribe, ninguna salida generada cambia de forma y ninguna aprobacion existente se invalida. Una persona consumidora cuyo rulebook exceda el presupuesto sigue renderizando y verificando exactamente igual que hoy.

La compresion cambia el `REPO-HARNESS.md` y el `REPO-HARNESS.es.md` autoalojados de este repositorio y extiende un archivo de reglas existente. No agrega archivo de reglas ni fila de compuerta. Ambos pares de idioma se reconcilian en la misma unidad de trabajo bajo el contrato de mantenimiento i18n.

## Relacion con el ciclo de vida

RH-W-0073 sigue en `proposed` hasta que la persona propietaria apruebe este diseno. Se coordina con otros tres planes sin fusionar ninguno:

- **RH-W-0072** cede `references/validation.md` solo para este cambio.
- **RH-W-0074** y **RH-W-0075** aterrizan ambos en `.repo-harness/rules/working-agreements.md` y ambos tienen prohibido agregar bytes al rulebook mientras esta unidad siga abierta. Completar RH-W-0073 es lo que levanta esa restriccion. Ambos movimientos de aqui aterrizan en ese mismo archivo, asi que las tres unidades no deben implementarse de forma concurrente y RH-W-0073 va primero. Tras esta unidad el archivo queda cerca de 499 de 700 tokens proxy, dejando unas 150 palabras para las dos unidades posteriores. Es viable pero no holgado, y una unidad posterior que necesite mas debe soltar detalle en lugar de subir el presupuesto.

Un plan de implementacion y su implementacion requieren cada uno autorizacion explicita separada.

## Decisiones humanas

1. Se quiere un control en tiempo de desarrollo, o la compresion sola es toda la unidad? Este diseno recomienda el control, porque ya se demostro que la compresion sin el se degrada en dos dias.
2. Son los dos bloques nombrados el detalle correcto para bajar? Ambos aterrizan en los acuerdos de trabajo existentes, lo que mantiene igual el numero de superficies pero concentra tres unidades en un archivo.
3. Es aceptable el margen resultante de `working-agreements.md` para RH-W-0074 y RH-W-0075, o deberia uno de los dos bloques ir a otro lugar para dejarles mas espacio?
