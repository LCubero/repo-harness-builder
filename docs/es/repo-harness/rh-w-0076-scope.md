# Alcance de correccion RH-W-0076

## Decision

RH-W-0076 posee tres limites declarados sin nada capaz de detectar una violacion. Este alcance nombra que cambia cada correccion, que control detecta una recurrencia y como se corta el trabajo. No autoriza implementacion.

Revise este alcance contra el [registro canonico RH-W-0076](roadmap.md#rh-w-0076), el [diseno RH-W-0073](rh-w-0073-design.md) aprobado cuyo patron de control reutiliza, y el [propietario en ingles](../../repo-harness/rh-w-0076-scope.md).

## La respuesta del propietario sobre la que se apoya este alcance

La condicion de avance del registro pregunta que gobierna el techo de cuerpo empaquetado. `references/gentle-ai-style-guide.md` declara "Target 180 to 450 body tokens, recommended maximum 700, hard maximum 1000" en un parrafo que no declara alcance; la frase que nombra a esta skill y a todo lo que genera pertenece al parrafo siguiente y gobierna la regla del prefijo `Trigger:`.

El propietario respondio el 2026-08-27: **el techo obliga a ambos, porque el constructor y el harness de este repositorio son reciprocos y recursivos.** Eso esta verificado, no afirmado. `REPO-HARNESS.md` declara que el repositorio se autohospeda y usa el harness generado aqui para mejorar el propio constructor; `decisions.md` declara que este repositorio autohospedado aplica el mismo contrato generico a su propia fuente de producto; y el `SKILL.md` canonico del constructor y su proyeccion en `.claude/` son identicos byte a byte con 16279 bytes. Una regla que el constructor obedece y su propia salida no, no es un contrato aplicado de forma recursiva, son dos contratos.

Un borrador anterior de este alcance proponia lo contrario, que los contratos generados son una excepcion declarada. Ese borrador queda retirado, porque el proyecto ya habia rechazado la misma movida un dia antes: el registro `RH-W-0073` rechaza subir la constante 700 y, en el mismo campo, **rechaza tratar el incumplimiento como deriva aceptable**. Renombrar deriva como excepcion no la convierte en otra decision.

Asi que los contratos generados en 2822 y 2707 son un **incumplimiento medido y rastreado**, no una exencion. La distincion es el punto entero: una excepcion dice que la regla no aplica, un incumplimiento rastreado dice que la regla aplica, que la salida la viola, y que lo que cuesta cerrarla queda registrado en vez de descartado.

Lo que cuesta esta verificado: `references/` aparece cero veces en `harness_tool.py` para la salida generada. Las skills generadas se renderizan a exactamente una ruta, `outputs[f"{base}/skills/{name}/SKILL.md"]`, asi que no tienen superficie donde bajar detalle. Cerrar ese incumplimiento exige ensenar al renderer a emitir una skill generada de varios archivos, que es trabajo de renderer que esta unidad no carga. `RH-W-0078` lo posee.

RH-W-0076 por lo tanto corrige la guia para que el techo nombre a que obliga, lleva el cuerpo del constructor por debajo de el, y registra el incumplimiento de los contratos generados con su duenio y su tamanio medido. No cierra ese incumplimiento y no lo exceptua.

## Correcciones

### 1. El comando inejecutable, paso 6 de `SKILL.md`

Reemplazar el comando impreso por una forma que corra. No copiar la forma de `references/validation.md`: esa forma es una invocacion minima de humo y omite `--conversation-language`, `--documentation-language` y `--engram-evidence-json`, que los pasos 1 y 3 existen para obtener. `--conversation-language` no tiene valor por defecto en argparse y se resuelve mediante `args.conversation_language or DEFAULT_REPORT_LANGUAGE`, asi que copiar esa forma caeria en silencio a ingles y reproduciria, sobre tres banderas mas, el defecto exacto que esta correccion diagnostica para `--runtime`.

El reemplazo conserva cada bandera que el paso 6 ya imprime y agrega las cuatro que faltan:

`scripts/harness_tool.py render --root ROOT --project-name NAME --runtime RUNTIME [--runtime RUNTIME ...] --license LICENSE --report-language REPORT_LANGUAGE [--documentation-language en | --documentation-language es | --documentation-language en --documentation-language es] --conversation-language CONVERSATION_LANGUAGE --engram-evidence-json HOST_EVIDENCE --activation builder --apply --approved-plan-sha256 HASH --approval TOKEN`

Se cierran tres defectos juntos: los obligatorios `--root` y `--project-name` ausentes, el `--activation builder` ausente que es lo que hace aceptable a `--approval`, y el `--runtime` ausente que de otro modo descarta el permiso que el paso 2 existe para obtener. La prosa que rodea al paso 6 ya resuelve `--license`, asi que la bandera se nombra aqui en lugar de quedar implicita.

Que las banderas omitidas son materiales es evidencia ejecutada, no razonamiento: renderizar el mismo proyecto dos veces bajo `--standalone --activation builder`, una con `--conversation-language espanol` y otra sin ella, produce dos digests de plan distintos, `a31fd667...` y `ecc3fec3...`. Las dos ejecuciones instalan harnesses diferentes. Aparte, la forma de reemplazo de arriba se ejecuto y devolvio un plan valido con su propio digest y prompt de aprobacion, mientras la forma que el paso 6 imprime hoy sale con codigo 2 en el parser antes de que exista plan alguno.

### 2. El techo de la guia de estilo

Reescribir la frase del presupuesto en `references/gentle-ai-style-guide.md` para que nombre a que obliga: la skill del constructor y toda skill que genera. Registrar, en el mismo lugar, que los contratos generados hoy lo exceden con 2822 y 2707, que `RH-W-0078` posee el cierre de ese incumplimiento, y que no se concede exencion. Decir las dos cosas que un lector confunde: el techo es un limite, verificado en tiempo de desarrollo y nunca en tiempo de ejecucion, y las bandas aprobadas por el propietario que registra la completada `RH-W-0013` son un vocabulario de gravedad, no una escala de permiso. Una revision del 2026-08-27 de este repositorio confundio una con la otra teniendo todos los archivos delante, lo que es evidencia de que un lector futuro tambien lo hara. Luego llevar el cuerpo de `SKILL.md` del constructor por debajo del techo declarado, bajando detalle a los archivos de `references/` que ya poseen esos temas. Ninguna referencia gana un presupuesto que no tenia.

### 3. Planes de unidades terminadas

Retirar los siete planes de implementacion cuya unidad llego a `completed`. Un solo mecanismo, aplicado de forma uniforme: una linea de estado justo debajo del titulo que nombre la unidad, su estado terminal y que el cuerpo se conserva como evidencia historica de ejecucion. Las casillas quedan como estan, porque son el registro historico del plan tal como se ejecuto, y la linea de estado retira la instruccion de obedecerlas. La fila inglesa del mapa de documentacion para `rh-w-0025-implementation-plan.md` deja de llamarlo ejecutable. Su par en espanol ya se lee como espejo humano y nunca lo llamo ejecutable, asi que ese par es trabajo de paridad, no una correccion.

## Control

Una familia de aserciones en tiempo de desarrollo dentro de la suite canonica, reutilizando el patron que establece el diseno de RH-W-0073 y que las pruebas autoalojadas existentes ya usan mediante `ROOT.parents[1]`:

- Todo comando impreso en `SKILL.md` como invocacion ejecutable parsea contra el parser real de la CLI.
- El cuerpo de `SKILL.md` del constructor mide por debajo del techo declarado.
- Ningun plan de implementacion cuyo registro sea terminal carece de su linea de estado.

Nada entra en `render()` ni en `check()`. No se reabre la retirada del rechazo de presupuesto en tiempo de ejecucion que hizo RH-W-0013.

## Cortes

Tres cortes, entregables de forma independiente, en este orden:

1. **Solo el paso 6.** Un archivo, un comando, una prueba RED que parsee la invocacion documentada. Es el unico defecto que rompe una primera instalacion y no depende de la respuesta del techo.
2. **Retiro de planes.** Siete lineas de estado mas una correccion de la fila inglesa del mapa y su par en espanol mantenido en paridad, con la asercion de estado terminal.
3. **El techo.** La reescritura de la guia y la compresion de `SKILL.md`, al final, porque es el corte mas grande y el menos urgente. Menos urgente no es menos riesgoso: mueve alrededor del 65 por ciento del cuerpo de instrucciones de runtime del constructor a archivos de `references/` que un agente lee solo cuando un paso lo manda alli, asi que carga el mayor riesgo de comportamiento de los tres y es el unico corte cuya aceptacion depende de lo que el archivo reescrito todavia hace hacer a un agente.

El corte 1 puede entregarse antes que RH-W-0073 porque no necesita control de presupuesto. Los cortes 2 y 3 van despues de RH-W-0073, porque consumen su patron de asercion.

## Criterios de aceptacion

- El comando impreso en el paso 6 de `SKILL.md` parsea contra el parser real de la CLI, comprobado por una prueba y no por lectura, y lleva cada valor que resuelven los pasos 1 al 4, de modo que ninguna respuesta de la entrevista se descarta en silencio por un valor por defecto de bandera.
- `references/gentle-ai-style-guide.md` declara que su techo de cuerpo obliga a la skill del constructor y a toda skill que genera, y registra el incumplimiento de los contratos generados y su unidad duenia en vez de una exencion, y distingue el techo como limite de las bandas de `RH-W-0013` como vocabulario de gravedad.
- El cuerpo de `SKILL.md` del constructor mide por debajo del techo declarado, y el detalle movido aterriza en un archivo existente de `references/`.
- Cada paso de ejecucion que sobrevive a la compresion sigue nombrando la referencia que ahora posee su detalle movido, de modo que ninguna instruccion queda inalcanzable para un agente que solo lee `SKILL.md`. El corte 3 no se integra hasta que eso se compruebe paso por paso contra el cuerpo previo a la compresion.
- Todo plan de implementacion de una unidad terminal lleva su linea de estado, y ninguna fila del mapa de documentacion llama ejecutable a un plan terminado.
- El comportamiento de `render()` y `check()` es identico byte a byte para toda entrada existente; `RULEBOOK_TOKEN_BUDGET` no cambia.
- El verificador informa `current` con cero escrituras y ambas proyecciones de runtime quedan identicas byte a byte a la canonica.

## No objetivos

Sin rechazo en tiempo de ejecucion, sin campo de manifiesto, sin estado de checker, sin cambio de esquema, sin archivo de reglas nuevo, sin directorio `references/` para las skills generadas en esta unidad, que es trabajo de `RH-W-0078` y no una respuesta permanente, sin subir el techo, sin reabrir RH-W-0013, RH-W-0072 ni RH-W-0073, y sin borrar evidencia historica de ejecucion conservada.

## Relacion con el ciclo de vida

RH-W-0076 sigue `proposed` hasta que el propietario apruebe este alcance. Agrupa tres defectos bajo una causa comun, asi que no puede quedar `completed` hasta que los tres se resuelvan o reciban una disposicion terminal con autoridad. RH-W-0072 conserva todas las demas superficies de documentacion de producto; RH-W-0073 conserva el presupuesto del reglamento y sus archivos de reglas; RH-W-0078 conserva el incumplimiento de cuerpo de los contratos generados y el trabajo de renderer que cerrarlo requiere. RH-W-0076 puede llegar a `completed` con ese incumplimiento abierto, porque posee declarar el techo con honestidad, no cerrar cada superficie que el techo obliga. Un plan de implementacion y su implementacion requieren cada uno autorizacion explicita separada.

## Decisiones humanas

1. Respondida el 2026-08-27: el techo obliga a ambos, y el incumplimiento de los contratos generados queda rastreado bajo `RH-W-0078` en vez de exceptuado.
2. Es la linea de estado el retiro correcto para los planes terminados, o deberian archivarse?
3. El corte 1 se entrega solo antes de RH-W-0073, o toda la unidad espera?

Este alcance fue a su vez revisado en solo lectura el 2026-08-27 y corregido antes de ofrecerse para aprobacion: su comando del corte 1 reproducia el defecto que diagnostica, tres de sus mediciones mezclaban dos metodos, y el corte 2 exageraba el trabajo en espanol. Esas correcciones estan aplicadas arriba. El mismo dia el propietario respondio la pregunta del techo, y la respuesta retiro el razonamiento original de exencion de este alcance; quedan dos preguntas.
