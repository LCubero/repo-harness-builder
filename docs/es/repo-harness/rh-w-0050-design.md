# Diseno de reconciliacion acotada RH-W-0050

> **Estado: BORRADOR. Pendiente de revision externa y aprobacion del propietario.**
>
> Esta propuesta para revision no es un diseno aprobado, autorizacion de implementacion, avance de ciclo de vida ni entrega. RH-W-0050 permanece `proposed`.

## Contrato de revision y etiquetas de estado

Revise este borrador contra el [registro canonico RH-W-0050](roadmap.md#rh-w-0050), las [decisiones permanentes](decisions.md), las [reglas de memoria](../../../.repo-harness/rules/memory.md) incorporadas y el [contrato de mantenimiento bilingue](i18n-maintenance.md). La evidencia del repositorio es canonica. El [par en ingles](../../repo-harness/rh-w-0050-design.md) debe permanecer completo y semanticamente equivalente.

- **Restriccion aprobada:** ya autorizada por el roadmap, las decisiones, el rulebook o el contrato generado actual.
- **Comportamiento actual verificado:** observado directamente en evidencia actual del repositorio y no propuesto por este borrador.
- **Decision recomendada:** propuesta aqui y pendiente de aprobacion del propietario.
- **Detalle de implementacion restante:** aplazado intencionalmente hasta una tarea de implementacion aprobada.

## Problema, objetivos y exclusiones

Session start y session stop reconcilian evidencia, pero no definen un contrato de activacion determinista para reconciliar hallazgos por completo. Baselines y limites indefinidos pueden causar revision ilimitada o routing falso de la siguiente unidad lista.

Este borrador recomienda un scan acotado hibrido mas reconciliacion completa activada por evidencia. Preserva autoridad del repositorio, aprobacion humana explicita, paridad bilingue, resultados independientes, operacion con y sin Git y observabilidad efimera sin agregar backlog, cache, estado de trigger, recibo, store de conflictos, store de ciclo, ledger, daemon, scheduler ni framework.

Este borrador no implementa comportamiento generado, codigo, pruebas, almacenamiento, migracion, promocion automatica, mutacion de ciclo, juicio de conflictos ni borrado. No avanza RH-W-0050 ni implementa otra unidad RH-W.

## Autoridad y comportamiento actual verificado

- Archivos del repositorio, evidencia Git local aplicable, fuente, pruebas, checker y aprobaciones explicitas son canonicos. Engram solo aporta continuidad semantica.
- El roadmap posee el ciclo; el estado actual es su proyeccion concisa; las decisiones poseen razones aprobadas; los changelogs poseen cronologia completada; los pares localizados presentan contenido humano equivalente.
- Toda operacion Engram del harness usa alcance explicito del proyecto del repositorio e informa su propio resultado. `active`, `needs_review` y `judgment_required` son metadata consultiva, no estados de verdad.
- Lecturas o escrituras Engram exitosas siguen exitosas cuando falla otra operacion Engram. Captura, asociacion de prompt, descubrimiento de conflictos, revision de ciclo y sync exigen evidencia propia.
- El archivo autoalojado incorporado `.repo-harness/rules/memory.md` contiene actualmente reglas mas ricas y especificas de este repositorio a nivel de operacion.
- La inspeccion verificada de fuente muestra que `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs` todavia no genera ese contrato mas rico completo para consumidores. El archivo incorporado mas rico es politica autoalojada actual, no prueba de comportamiento generado del producto.
- Una implementacion futura debe decidir e implementar el contrato canonico de memoria generado y luego verificar por separado renders integrados y standalone. Ninguna redaccion posterior puede implicar que esta generacion ya existe.

## Arquitectura recomendada

**Decision recomendada, no aprobada:** cada inicio y cierre calificante ejecuta un scan acotado determinista. Evidencia obligatoria o una peticion completa explicita ejecuta reconciliacion completa. Senales consultivas solo estrechan la inspeccion. Una peticion acotada permanece acotada salvo que verifique de forma independiente otro trigger obligatorio.

| Propietario | Responsabilidad propuesta |
|---|---|
| Session start | Construir snapshot inicial efimero, ejecutar ruta acotada aplicable y suprimir routing autoritativo cuando el baseline es desconocido o queda un hallazgo obligatorio. Sin mutacion por defecto. |
| Session stop | Comparar contra snapshot inicial verificado, ejecutar reconciliacion obligatoria posterior a entrega y aplicar solo actualizaciones ya autorizadas. |
| Habilidad de documentacion | Elegir propietario canonico menor, clasificar promocion, mantener mapas y paridad localizada y rechazar stores duplicados. |
| Reglas de memoria | Definir proyecto explicito y evidencia por operacion despues de implementar la decision del generador del producto. |
| Roadmap y estado actual | Poseer ciclo y su proyeccion directa. |
| Propietario del repositorio | Aprobar diseno final, implementacion, promocion discrecional, cambios de ciclo y acciones destructivas. |

## Glosario

| Termino | Definicion normativa |
|---|---|
| Entrega | Finalizacion verificada de un resultado aprobado mediante evidencia de entrega aplicable del repositorio. Un mensaje de commit, estado ahead-only, afirmacion de memoria o redaccion humana por si solos no son entrega. |
| Entrega de ciclo | Finalizacion de una correccion canonica de ciclo o de siguiente unidad lista. Obliga reconciliacion posterior a entrega. Una reparacion solo de redaccion localizada no es entrega de ciclo salvo que exponga o cambie un conflicto semantico. |
| Cierre significativo | Session stop despues de mutacion del repositorio, afirmacion de entrega verificada, correccion de ciclo o siguiente unidad, hallazgo retenido o intento de escritura de continuidad. Un cierre de solo lectura sin cambios no es significativo. |
| Proyeccion directa | Campo documental cuyo valor deriva de una afirmacion de un propietario canonico, como status en estado actual desde roadmap o un par localizado de esa afirmacion. |
| Hallazgo relevante | Evidencia dentro del alcance aplicable que puede cambiar clasificacion, ciclo, entrega, paridad, continuidad o routing. |
| Hallazgo retenido | Hallazgo relevante que no se probo duplicado, irrelevante, no sustentado o ya representado canonicamente. |
| Baseline | Snapshot efimero verificado del repositorio usado para comparar durante una invocacion o sesion. Nunca persiste como estado de trigger. |
| Scan acotado | Inspeccion limitada por el alcance exacto y los limites numericos posteriores. |
| Reconciliacion completa | Clasificacion y disposicion de todo hallazgo retenido dentro del alcance aplicable, no del portafolio completo. |
| Trigger obligatorio | Evidencia verificada que exige reconciliacion completa antes del routing autoritativo. |
| Senal consultiva | Evidencia que puede estrechar inspeccion o aparecer en salida, pero no activar reconciliacion completa por si sola. |
| Recomendacion autoritativa de siguiente unidad lista | Recomendacion presentada como derivada de ciclo y entrega canonicos reconciliados. Discusion y opciones tentativas no son recomendaciones autoritativas. |

## Matrices de activacion

### Inicio

| Modo | Entradas | Ruta y salidas | Operaciones Engram | Permiso de mutacion |
|---|---|---|---|---|
| Integrado, objetivo RH-W explicito | Alcance exacto, Git local, checker y evidencia Engram acotada al sujeto | Scan acotado; completo solo por trigger obligatorio; informar estado y siguiente unidad | Resolver proyecto explicito y luego query de sujeto; otras operaciones solo si aplican independientemente | Ninguno por defecto |
| Integrado, sin objetivo RH-W explicito | Proyeccion activa canonica, Git local aplicable y evidencia Engram de sujeto | Derivar baseline acotado; si no es confiable, `baseline: unknown` y suprimir routing autoritativo | Resolver proyecto antes de leer; cero operaciones bajo ambiguedad | Ninguno por defecto |
| Standalone, objetivo RH-W explicito | Mismo alcance del repositorio sin Engram | Mismo routing con memoria `not_applicable` | Ninguna | Ninguno por defecto |
| Standalone, sin objetivo RH-W explicito | Proyeccion activa canonica y Git local aplicable | Mismo comportamiento de baseline conocido o desconocido | Ninguna | Ninguno por defecto |

### Cierre

| Modo | Entradas | Ruta y salidas | Operaciones Engram | Permiso de mutacion |
|---|---|---|---|---|
| Integrado, cierre significativo | Snapshot inicial verificado, evidencia cambiada, resultados de entrega y continuidad | Comparar con snapshot inicial; reconciliacion completa para entrega de ciclo u otro trigger | Informar cada operacion aplicable por separado | Solo propietarios y continuidad aprobados en la misma unidad |
| Integrado, cierre no significativo | Snapshot inicial verificado y evidencia igual | `not_triggered`; sin repetir propietario o continuidad | Ninguna salvo accion de continuidad pedida aparte | Ninguno |
| Standalone, cierre significativo | Snapshot inicial verificado y evidencia cambiada | Misma comparacion y triggers sin memoria | Ninguna | Solo actualizaciones aprobadas del repositorio |
| Standalone, cierre no significativo | Snapshot inicial verificado y evidencia igual | `not_triggered`; sin recibo o escritura no-op | Ninguna | Ninguno |

## Baseline y limites deterministas

### Alcance exacto de propietarios aplicables

Al inicio con objetivo RH-W explicito, el alcance aplicable es exactamente:

1. Ese registro del roadmap.
2. Su proyeccion directa en estado actual, si existe.
3. Sus secciones de pares localizados declarados.
4. Decisiones y entradas de changelog que nombran ese ID RH-W o una afirmacion exacta citada por el registro.
5. Evidencia de rama local actual que afecta esas rutas o una afirmacion explicita de entrega.
6. Resultados Engram del topic exacto `work/RH-W-####` y una query de sujeto con el ID RH-W exacto, solo despues de resolver proyecto explicito.

Al cierre, el baseline es el snapshot inicial verificado de esos elementos. El alcance cambiado se calcula comparando bytes actuales, campos parseados e identidad Git local contra ese snapshot. Ninguna sesion previa, cache o timestamp inferido lo sustituye.

Al inicio sin estado local de sesion, derive un baseline efimero en este orden:

1. Parsear registro aplicable y sus proyecciones directas de estado actual y localizacion.
2. Si Git existe, inspeccionar `HEAD`, estado de rutas propietarias y commits que las tocan desde merge-base con upstream configurado hasta `HEAD`. Sin upstream usable, inspeccionar solo `HEAD` y estado de rutas y no inferir entrega remota.
3. Si toda afirmacion canonica tiene propietario y no queda fallo de comparacion ni cambio inexplicado, baseline es `known`.
4. En otro caso baseline es `unknown`; informar evidencia faltante y suprimir routing autoritativo. Nunca adivinar.

### Limites proporcionales

- **Commits:** maximo 20 commits que toquen rutas aplicables, mas nuevos primero. Terminar antes cuando cada campo cambiado y claim de entrega tenga evidencia directa. Mas de 20 produce `bound_exhausted` y suprime routing.
- **Rutas:** las seis clases anteriores, con maximo un registro, una proyeccion de estado, un par por locale declarado, todos los matches exactos de ID en decisiones y changelog y maximo 12 rutas de evidencia local nombradas directamente. La numero 13 produce `bound_exhausted`.
- **Queries Engram:** maximo dos lecturas, topic exacto y una query de sujeto con ID exacto, cada una con maximo 10 observaciones. Vacio es valido solo con evidencia exitosa de la misma superficie o control positivo validado en la misma superficie. Mecanica de adapter queda como detalle.
- **Dependencias:** inspeccionar registro cambiado y proyecciones directas. Inspeccionar solo dependientes directos cuyo campo de routing declarado referencia status, dependencia, prioridad o condicion cambiada, maximo 8 registros. Nunca recorrer transitivos ni todos automaticamente. El noveno produce `bound_exhausted`.
- **Reconciliacion completa:** expande clasificacion dentro de este alcance acotado. No elimina limites ni revisa backlog completo, todos los commits, toda documentacion o toda memoria.

Scans repetidos sobre evidencia igual deben producir mismo alcance, clasificaciones, estado y conjunto de mutaciones. El segundo scan despues de reparacion exitosa debe converger a cero mutaciones adicionales.

## Peticiones y clasificacion de triggers

| Clase | Senal | Tratamiento requerido |
|---|---|---|
| Obligatorio | Peticion explicita de reconciliacion completa | Ejecutar reconciliacion completa dentro del alcance aplicable. |
| Ruta acotada solicitada | Peticion explicita de inspeccion acotada | Ejecutar solo ruta acotada salvo evidencia independiente de otro trigger obligatorio. |
| Obligatorio | Entrega de producto o de ciclo | Ejecutar reconciliacion completa antes de routing autoritativo. |
| Obligatorio | Inconsistencia canonica o riesgo de routing falso | Reconciliar registro y proyecciones. Agregar dependientes acotados solo por campos de routing cambiados. |
| Obligatorio | Evidencia de entrega incompleta | Retener brecha y suprimir routing. |
| Obligatorio | Contradiccion repositorio-memoria verificada | Gana repositorio; clasificar disposicion y preservar evidencia por operacion. |
| Obligatorio | Hallazgo retenido no clasificado | Ejecutar ruta completa y bloquear solo rama afectada si sigue no clasificado. |
| Consultivo | `judgment_required`, `needs_review`, resumen viejo, hint de commit no inspeccionado | Verificar contra propietario; nunca activar solo. |
| Consultivo | Trabajo sucio no relacionado, Git ahead-only, diferencia solo de redaccion, drift sin ciclo | Informar o enrutar estrecho; nunca inferir entrega o drift de ciclo. |
| Rechazado | Suciedad, edad, numero de invocaciones, volumen, keyword, red ausente, modo no-Git | Sin trigger salvo evidencia obligatoria separada. |
| Rechazado | Search vacio no validado o claim no verificado de captura, prompt, plugin, compactacion o sync | No afirmar exito o ausencia. |

## Reconciliacion completa y disposiciones

1. Inventariar hallazgos retenidos solo dentro del alcance aplicable.
2. Resolver propietario canonico y clase de evidencia.
3. Comparar evidencia canonica, proyeccion directa, localizacion, entrega, Git local y memoria aplicable.
4. Asignar exactamente una disposicion.
5. Aplicar solo reparaciones no discrecionales ya autorizadas.
6. Pausar solo por eleccion humana; bloquear por evidencia requerida ausente; fallar por error de operacion intentada.
7. Repetir scan y exigir convergencia antes de routing autoritativo.

| Disposicion | Condicion legal | Autoridad de mutacion |
|---|---|---|
| `integrate` | Hallazgo valido pertenece a propietario existente y no esta duplicado. | Exige autoridad de tarea; contenido discrecional exige aprobacion. |
| `close` | Propietario actual y entrega prueban satisfaccion o superacion acotada. | Nunca solo memoria; cambio de ciclo exige aprobacion. |
| `defer` | Hallazgo valido tiene bloqueo, prerrequisito o condicion futura nombrada. | Usar roadmap existente o actualizacion aprobada. |
| `reject` | Metas aprobadas chocan o propietario lo declina explicitamente. | Preservar rechazo durable solo en propietario existente. |
| `noise` | Duplicado, irrelevante, no sustentado, solo consultivo o ya canonico. | Sin registro ni mutacion. |

Clasificar nunca autoriza implementacion, borrado o avance de ciclo.

## Estados de resultado y resultados independientes

| Estado | Condicion legal |
|---|---|
| `completed` | Scan o reconciliacion termina dentro de limites, cada hallazgo queda clasificado, actualizaciones autorizadas requeridas pasan, paridad se verifica y ninguna rama afectada queda pendiente. |
| `paused` | Se requiere decision humana especifica; resultados independientes verificados siguen validos. |
| `blocked` | Evidencia, propietario, baseline confiable o capacidad del limite no estan disponibles sin fallo de operacion. |
| `failed` | Fallo una operacion requerida intentada. Nombrarla sin colapsar resultados ajenos. |
| `not_triggered` | Scan acotado termino y no verifico trigger obligatorio. |

Mantener separados resolucion, baseline, scan, trigger, clasificacion, actualizaciones, paridad, entrega, siguiente unidad, Git y cada operacion Engram. No fabricar un estado Engram agregado.

## Evidencia y degradacion Engram

| Operacion | Evidencia de exito | Fallo o ausencia | Resultado preservado |
|---|---|---|---|
| Resolucion de proyecto | Capacidad soportada resuelve proyecto explicito | Con ambiguedad hacer cero operaciones Engram; continuar local | Repositorio y Git |
| Lectura | Respuesta valida acotada, incluido vacio validado | Marcar fallida o ausente; no afirmar continuidad recuperada | Resolucion y scan |
| Escritura | Confirmacion acotada | Marcar fallida; no afirmar continuidad guardada | Repositorio y lecturas |
| Asociacion de prompt | Evidencia directa de la operacion | Marcar desconocida o fallida | Escritura de observacion |
| Descubrimiento de conflictos | Respuesta candidata valida | Marcar fallido o ausente; no inventar ausencia de conflicto | Lectura o escritura |
| Revision de ciclo | Respuesta soportada de antiguedad | Marcar ausente; verificar propietario | Otras operaciones |
| Captura de plugin | Evidencia de captura de sesion y proyecto | Marcar no verificada o fallida | Operaciones manuales |
| Sync | Evidencia acotada de sync | Marcar desconocido o fallido | Engram local y repositorio |

Ausencia Engram degrada continuidad segun politica aplicable, pero nunca prueba drift ni crea almacenamiento alterno.

## Mutacion, paradas y routing posterior a entrega

Se requiere aprobacion humana para diseno final, implementacion, repositorio ambiguo, semantica disputada, promocion discrecional, cambios de ciclo, rechazo de direccion valida, limpieza destructiva y excepciones semanticas de localizacion.

Detener solo rama afectada por ambiguedad, hallazgo pendiente, entrega incompleta, baseline desconocido, limite agotado, fallo de propietario, fallo localizado o alcance sin aprobar. Preservar trabajo sucio no relacionado y resultados independientes exitosos.

Despues de entrega de producto o ciclo: verificar entrega, ejecutar reconciliacion completa, actualizar propietarios aprobados y pares en una unidad, informar continuidad por separado, verificar convergencia y solo entonces permitir routing autoritativo. Reparacion solo de redaccion no cuenta como entrega de ciclo salvo que exponga o cambie conflicto semantico.

## Forma de salida efimera

```text
repository: <resultado y clase de evidencia>
baseline: <known | unknown> (<evidencia>)
bounded_scan: <completed | blocked | failed> (<alcance y limites>)
reconciliation: <completed | paused | blocked | failed | not_triggered>
trigger: <senales obligatorias o none>
findings: <IDs y disposiciones>
canonical_updates: <none | rutas | paused | failed>
localized_parity: <not_applicable | verified | drift | paused | failed>
delivery_evidence: <not_applicable | complete | incomplete | unknown>
next_ready: <unidad recomendada | suppressed | not_requested>
git: <not_git | resultado local | rama opcional>
engram.<operacion>: <resultado independiente>
next_action: <una accion acotada o decision humana>
```

Solo se muestran campos aplicables. Esta salida no es recibo persistido.

## Matriz formal RED/GREEN

| Escenario y entrada observable | Defecto RED | Resultado GREEN | Expectativa de mutacion | Resultados independientes preservados |
|---|---|---|---|---|
| Baseline conocido: propietarios consistentes y Git usable | Baseline se infiere vagamente | `baseline: known`; alcance determinista informado | Ninguna | Git y propietarios |
| Baseline desconocido: falta propietario o falla comparacion | Routing adivina readiness | `blocked`; `next_ready: suppressed` | Ninguna | Evidencia verificada |
| Limite agotado: 21 commits, 13 rutas, 11 resultados o 9 dependientes | Scan expande o trunca en silencio | Limite exacto informado y routing suprimido | Ninguna | Dimensiones completadas |
| Scan repetido sin cambios | Resultados o escrituras varian | Mismo alcance, estado, hallazgos y cero mutaciones | Ninguna | Resultados previos |
| Reparacion exitosa seguida por scan | Loop o repeticion | Segundo scan converge con cero mutaciones | Solo primera reparacion autorizada | Entrega y paridad |
| Peticion completa explicita | Ruta queda parcial | Reconciliacion completa dentro del alcance | Solo escrituras autorizadas aparte | Git y Engram |
| Peticion acotada sin otro trigger | Ruta se expande | Solo inspeccion acotada | Ninguna | Routing normal |
| Peticion acotada mas entrega verificada | Peticion suprime trigger | Ruta completa por trigger independiente | Reconciliacion autorizada | Evidencia de peticion |
| Entrega incompleta | Recomienda siguiente unidad | `next_ready: suppressed`; brecha retenida | Ninguna sin autoridad | Repositorio y Git |
| Hallazgo retenido no clasificado | Se pierde o continua routing | Ruta completa; rama bloquea si persiste | Ninguna sin autoridad | Hallazgos clasificados |
| Trabajo sucio no relacionado | Activa o se modifica | Consultivo y preservado | Nunca tocar rutas ajenas | Reconciliacion |
| Git ahead-only sin prueba | Infiere entrega | Informa commits; entrega desconocida | Ninguna | Git local |
| Search vacio validado | Se rechaza o trata como ausencia global | Se acepta tras evidencia o control positivo de misma superficie | Ninguna | Resolucion de proyecto |
| Captura no verificada | Instalacion implica exito | Captura `unverified` | Ninguna | Lectura o escritura manual |
| Proyecto ambiguo | Query bajo proyecto adivinado | Pausa con cero operaciones Engram | Ninguna | Repositorio local |
| Actualizacion canonica falla parcialmente | Todo se llama fallo o exito | Ruta fallida nombrada, exitos preservados, paridad pendiente | Sin retry fuera del limite | Escrituras exitosas |
| Actualizacion localizada falla | Exito canonico implica paridad | Canonico preservado; paridad `failed`; routing suprimido si semantico | Sin rollback ajeno | Actualizacion canonica |
| Varios triggers simultaneos | Uno oculta los demas | Un ciclo informa todos | Un conjunto autorizado | Evidencia de cada trigger |
| Hallazgo `integrate` | Crea store nuevo | Propietario existente y limite de aprobacion | Solo propietario existente | Otras disposiciones |
| Hallazgo `close` | Memoria lo cierra sola | Propietario y entrega prueban cierre | Solo ciclo aprobado | Resultado de memoria |
| Hallazgo `defer` | Desaparece | Condicion nombrada en roadmap | Solo update aprobado | Otras clasificaciones |
| Hallazgo `reject` | Rechazo silencioso o destructivo | Razon y autoridad retenidas | Solo propietario existente | Evidencia historica |
| Hallazgo `noise` | Crea registro | Sin escritura durable | Ninguna | Hallazgos relevantes |
| Modo standalone | Exige o emula Engram | Ruta termina con memoria `not_applicable` | Solo permisos del repositorio | Git y propietarios |
| Inicio normal sin trigger | Invocacion fuerza ciclo | `not_triggered`; routing normal | Ninguna | Evidencia acotada |
| Cierre no significativo | Crea cronologia o ruido | `not_triggered`; sin no-op write | Ninguna | Snapshot inicial |
| Entrega de ciclo | Evita ciclo posterior | Reconciliacion precede routing | Updates aprobados en una unidad | Evidencia de entrega |
| Reparacion solo de redaccion | Toda edicion es entrega | Sin trigger salvo conflicto semantico | Solo par localizado | Estado de ciclo |
| Metadata Engram consultiva | Prueba conflicto | Consultiva hasta verificar propietario | Ninguna | Resultado Engram |
| Fallo parcial Engram | Colapsa memoria | Operaciones independientes | Escrituras exitosas | Repositorio |
| Sin store duplicado | Agrega cache, recibo o ledger | Solo propietarios y salida efimera | Ningun store nuevo | Continuidad existente |

## Matriz de evidencia

| Afirmacion | Clase de evidencia | Propietario o simbolo canonico | Preservada o cambiada |
|---|---|---|---|
| RH-W-0050 sigue `proposed` | Comportamiento actual verificado | `roadmap.md#rh-w-0050` | Preservada |
| Repositorio supera memoria | Restriccion aprobada | `REPO-HARNESS.md`, reglas de memoria | Preservada |
| Reglas autoalojadas son ricas por operacion | Comportamiento actual verificado | `.repo-harness/rules/memory.md` | Preservada y aclarada |
| Generador no emite contrato rico completo | Comportamiento actual verificado | `harness_tool.py::rule_outputs` | Afirmacion corregida |
| Debe adoptarse activacion hibrida | Decision recomendada | Este borrador | Cambiada y pendiente |
| Search vacio usa evidencia de misma superficie | Disciplina existente aprobada | Evidencia RH-W-0040 del roadmap | Recomendacion resuelta |
| Dependientes solo por campos de routing cambiados | Decision recomendada | Este borrador | Recomendacion resuelta |
| Entrega de ciclo obliga reconciliacion | Decision recomendada | Este borrador | Recomendacion resuelta |

## Mapa de impacto de implementacion

Este mapa identifica trabajo futuro y no autoriza ediciones.

| Simbolo o ruta estable | Decision o cambio futuro requerido |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill` | Generar baseline, activacion acotada de inicio, separacion de peticiones, estados y supresion. |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_stop_skill` | Generar comparacion de snapshot, cierre significativo, entrega de ciclo y convergencia. |
| `skill/repo-harness-builder/scripts/harness_tool.py::documentation_skill` | Generar promocion por clasificacion, proyecciones directas y paridad localizada. |
| `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs` | Decidir y generar contrato rico por operacion. Verificar integrado y standalone por separado. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render` y `render_with_skill_mapping` | Preservar modos y rutas; sin input persistido de trigger. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Agregar cada fila RED/GREEN a suite existente, incluidos renders integrado y standalone. |
| `skill/repo-harness-builder/references/architecture.md` | Documentar limites aprobados de propietario y activacion. |
| `skill/repo-harness-builder/references/validation.md` | Documentar limites y verificacion de convergencia. |
| `.repo-harness/skills/repo-harness-session-start/SKILL.md` y par de cierre | Contratos autoalojados futuros despues de aprobacion e implementacion. |
| `.repo-harness/rules/memory.md` | Politica autoalojada rica actual; futura salida generada debe reconciliarse sin asumir paridad actual. |
| `.opencode/skills/repo-harness-builder/**` y `.claude/skills/repo-harness-builder/**` | Proyecciones exactas solo despues de implementacion canonica. |

## Rollback, detalles restantes y obligaciones de revision

La propuesta no agrega schema, cache ni ledger persistido, por lo que un rollback futuro restauraria juntos wording generado y pruebas enfocadas y resincronizaria proyecciones. Observaciones Engram existentes quedan intactas.

Detalles restantes incluyen estructuras exactas de parser, formato del reporte, mecanica de control positivo por adapter, fixtures y ubicacion en renderer despues de aprobacion. No pueden debilitar baselines, limites, estados, resultados independientes ni obligaciones integradas y standalone.

La revision externa debe verificar:

- [ ] Estado borrador y `proposed` son claros; no se implica implementacion o aprobacion.
- [ ] Cada baseline, limite, trigger, parada, estado y regla de mutacion es determinista.
- [ ] Peticiones humanas completas y acotadas siguen distintas.
- [ ] Generador del producto y politica autoalojada rica no se confunden.
- [ ] Filas formales son observables y preservan resultados independientes.
- [ ] Integrado y standalone, inicio y cierre estan completos.
- [ ] Secciones, tablas normativas, listas y obligaciones inglesas y espanolas son equivalentes.
- [ ] Simbolos existen y ninguna redaccion afirma comportamiento generado existente.
- [ ] Enlaces resuelven y no queda U+2013, U+2014, placeholder, contradiccion ni drift de aprobacion.
