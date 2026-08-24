# Alcance escrito de reconciliacion RH-W-0011

> **Estado: COMPLETADO. El propietario acepto las cinco clasificaciones `close` y autorizo el cierre del ciclo de vida el 2026-08-24.**
>
> El cuerpo del diseno conserva el alcance historico y los limites de aprobacion. El [registro RH-W-0011 de la hoja de ruta](roadmap.md#rh-w-0011) posee el ciclo de vida completado.

## Objetivo y exclusiones

El objetivo es definir el alcance acotado y revisable por el propietario para una reconciliacion posterior de los cinco registros de hallazgos de la ronda 1 representados por RH-W-0003 a RH-W-0007, mediante evidencia actual del repositorio y evidencia de continuidad verificada. El rango de prerrequisitos completado es RH-W-0003 a RH-W-0010.

Este alcance no ejecuta el inventario, clasifica hallazgos, cambia propietarios canonicos, promueve continuidad a documentacion durable, avanza RH-W-0011 mas alla de `ready`, implementa comportamiento del producto, reconstruye resultados de revision no disponibles ni crea recibos, ledgers, caches, backlogs u otras superficies de persistencia.

## Limite de evidencia

### Evidencia permitida

Una reconciliacion futura aprobada puede inspeccionar solamente:

1. Registros locales actuales del ciclo de vida para RH-W-0003 a RH-W-0011 y sus proyecciones declaradas en ingles y espanol.
2. Decisiones, entradas de changelog, pruebas, fuente y evidencia Git local actuales del repositorio citadas directamente por esos registros o que los afecten directamente.
3. Los cinco registros existentes de hallazgos de la ronda 1, RH-W-0003 a RH-W-0007. Sus registros actuales son evidencia; el texto ausente del informe historico no lo es.
4. El contrato completado de reconciliacion acotada y clasificacion exacta de RH-W-0050. Su cuerpo de diseno es solamente evidencia historica de diseno: la redaccion del ciclo de vida anterior a la entrega no es evidencia actual del ciclo.
5. Continuidad de Engram resuelta explicitamente para el proyecto `repo-harness-builder`, limitada a los topics exactos `work/RH-W-0003` a `work/RH-W-0011` y consultas por ID exacto, cuando la operacion devuelve evidencia verificable y acotada.
6. Decisiones explicitas del propietario suministradas para la reconciliacion y registradas con su alcance aplicable.

Los propietarios del repositorio conservan la autoridad. La continuidad verificada puede ubicar o sustentar una afirmacion, pero no reemplaza evidencia del repositorio sobre ciclo de vida, entrega o cierre.

### Evidencia prohibida o no disponible

- El contenido del informe historico esta ausente del repositorio y no es verificable. No se debe inferir, resumir, citar ni recrear.
- La salida del juez B de la ronda 2 de Judgment Day no esta disponible porque el limite de sesion termino esa rama. Su ausencia no es un resultado limpio ni establece un sexto hallazgo.
- No se deben leer rutas externas, privadas, personales, ignoradas o fuera del repositorio.
- No se deben usar fuentes de red, estado Git remoto, memoria obsoleta por si sola, resultados Engram sin alcance ni afirmaciones no sustentadas de busquedas vacias.
- No se debe tratar la redaccion historica de RH-W-0050 anterior a la entrega como evidencia del estado actual.

## Procedimiento acotado de inventario

Solo despues de una aprobacion separada del propietario para ejecutar la reconciliacion:

1. Resolver el registro actual RH-W-0011 y sus proyecciones declaradas.
2. Abrir exactamente los cinco registros RH-W-0003 a RH-W-0007 y los registros de prerrequisitos completados hasta RH-W-0010.
3. Seguir solamente citas exactas de esos registros, con un maximo de 12 rutas locales distintas y 20 commits locales que toquen esas rutas. Una ruta numero 13 o un commit numero 21 requerido detiene la revision afectada como `bound_exhausted`.
4. Si la continuidad esta disponible, ejecutar como maximo una lectura por topic exacto y una consulta por ID exacto para cada hallazgo, cada una limitada a 10 observaciones y con alcance explicito `repo-harness-builder`. Mantener independiente el resultado de cada operacion.
5. Para cada hallazgo retenido, identificar su clase de evidencia y el menor propietario canonico existente, comparar las afirmaciones en ingles y espanol y asignar exactamente una disposicion.
6. Registrar resultados sin mutar propietarios. Toda escritura propuesta, promocion, cambio de ciclo o implementacion procede solamente por su limite de aprobacion separado.
7. Detenerse ante limites agotados, propietarios contradictorios, evidencia requerida no disponible, alcance ambiguo del repositorio o fallo de una operacion intentada. Nunca ampliar el scan automaticamente.

El recibo incompleto de la ronda 2 es una limitacion de evidencia vinculada a esta reconciliacion. Registrar `judge_b_output: unavailable_session_limit`, preservar los cinco registros conocidos de la ronda 1 y no intentar reconstruir el razonamiento del juez B ni inventar otro hallazgo.

## Dominio de clasificacion

Cada hallazgo retenido recibe exactamente una disposicion de este dominio cerrado:

| Disposicion | Condicion requerida |
|---|---|
| `integrate` | El hallazgo valido pertenece a un propietario canonico existente y aun no esta representado. |
| `close` | El propietario canonico actual y evidencia verificada de entrega prueban satisfaccion o superacion acotada. |
| `defer` | El hallazgo valido tiene bloqueo, prerrequisito, propietario y condicion observable de retorno. |
| `reject` | La autoridad propietaria aplicable lo declina o una restriccion aprobada entra en conflicto; la razon durable se conserva en un propietario existente. |
| `noise` | Es duplicado, irrelevante, no sustentado, solo consultivo o ya esta representado canonicamente; no crea registro durable. |

La clasificacion por si sola no autoriza implementacion ni avance del ciclo de vida.

## Campos requeridos del resultado

El resultado de la reconciliacion posterior debe contener:

```text
scope: RH-W-0011
status_before: ready
inventory_bounds: <rutas, commits, lecturas de continuidad y estado de agotamiento>
historical_report: unavailable_unverifiable
judgment_day_round_2: incomplete
judge_b_output: unavailable_session_limit
known_round_1_records: RH-W-0003..RH-W-0007
finding_id: <ID de registro existente>
claim: <afirmacion acotada>
evidence_class: <verified_fact | supported_inference | unknown_or_unverifiable | human_decision_required>
evidence: <rutas, identidad Git local, operacion de continuidad acotada o capacidad ausente>
canonical_owner: <ruta y seccion del propietario existente>
disposition: <integrate | close | defer | reject | noise>
rationale: <por que la evidencia permite esa disposicion>
proposed_writes: <none o rutas exactas de propietarios existentes>
parity_effect: <none | required | blocked | failed>
engram_effect: <not_applicable o resultados independientes por operacion>
approval_required: <scope | execution | promotion | lifecycle | implementation>
unresolved: <none o incognita, conflicto o decision exactos>
next_decision: <una decision explicita del propietario>
```

No se debe agregar un hallazgo sintetico por la salida ausente del juez B.

## Clases de evidencia

- `verified_fact`: sustentado directamente por evidencia actual y aplicable del repositorio o por una operacion acotada exitosa con prueba proporcional.
- `supported_inference`: derivado de evidencia verificada nombrada y razonamiento explicito; indica que evidencia lo elevaria a verificado.
- `unknown_or_unverifiable`: no existe un mecanismo valido o falta evidencia requerida. La brecha se preserva sin sustitucion.
- `human_decision_required`: la autoridad o el criterio pertenece al propietario y no se puede inferir de evidencia tecnica.

## Propiedad y efectos del ciclo de vida

La hoja de ruta es el propietario canonico del ciclo de vida. El estado actual es su proyeccion concisa, las decisiones poseen la justificacion durable, los changelogs poseen la cronologia completada, los pares localizados deben mantener paridad semantica y Engram conserva continuidad, no verdad del repositorio.

El propietario aprobo explicitamente este alcance el 2026-08-24, avanzando RH-W-0011 a `ready`. Esa aprobacion cubre solamente estos limites y no autoriza inventario. Ejecutar la reconciliacion puede producir clasificaciones, pero no cambiar el estado. La promocion puede actualizar propietarios y pares aprobados, pero no implica implementacion. El avance de ciclo requiere una decision explicita separada del propietario sustentada por la condicion de la hoja de ruta. La implementacion requiere otra aprobacion y otra unidad de trabajo.

## Limites de aprobacion

1. **Aprobacion del alcance:** aprueba solamente este limite escrito. No autoriza inventario ni mutacion.
2. **Aprobacion de ejecucion de la reconciliacion:** autoriza solamente el inventario acotado y la clasificacion. No autoriza promocion, cambio de ciclo ni implementacion.
3. **Aprobacion de promocion:** autoriza hallazgos nombrados y escrituras exactas en propietarios canonicos existentes, con pares localizados y reconciliacion de continuidad en la misma unidad de trabajo.
4. **Aprobacion del ciclo de vida:** autoriza por separado cualquier cambio de estado o condicion de avance en la hoja de ruta despues de revisar la evidencia.
5. **Aprobacion de implementacion:** autoriza por separado comportamiento del producto, pruebas, proyecciones generadas u otro trabajo de implementacion.

Ninguna aprobacion anterior implica una posterior.

## Obligaciones de paridad y Engram

El [documento canonico en ingles](../../repo-harness/rh-w-0011-design.md) y este par en espanol deben permanecer completos y semanticamente equivalentes. Todo cambio semantico promovido posteriormente debe actualizar los propietarios aplicables en ingles y espanol dentro de la misma unidad autorizada. Un fallo localizado preserva el trabajo canonico verificado, pero deja la paridad sin resolver y suprime el avance autoritativo.

Las lecturas y escrituras Engram requieren el proyecto explicito `repo-harness-builder`. Informar por separado la resolucion del proyecto, cada lectura, cada escritura, la asociacion de prompt, el descubrimiento de conflictos y la revision de ciclo. Operaciones Engram fallidas o no disponibles no invalidan evidencia verificada del repositorio, no prueban ausencia ni autorizan almacenamiento alterno. Una promocion aprobada debe reconciliar el topic aplicable `work/RH-W-####` sin sobrescribir ciegamente la verdad del repositorio.

## Comportamiento ante fallos e incognitas

- `blocked`: no estan disponibles la evidencia requerida, la resolucion de propietario, el alcance del repositorio o la capacidad del limite, sin fallo de una operacion intentada.
- `failed`: falla una operacion requerida intentada; se nombra y se preservan resultados exitosos independientes.
- `paused`: se requiere una decision especifica del propietario.
- Las afirmaciones desconocidas o no verificables permanecen explicitas y sin clasificar hasta que exista la evidencia o el criterio propietario.
- Las escrituras parciales canonicas o localizadas, si se autorizan posteriormente, se informan ruta por ruta. No se afirma paridad, promocion ni avance a partir de exito parcial.
- Nunca se reintenta ampliando alcance, leyendo evidencia prohibida, usando la red ni reconstruyendo resultados ausentes.

## Criterios de aceptacion

- RH-W-0011 esta `ready` y la hoja de ruta conserva la propiedad del ciclo de vida.
- El inventario se limita a los cinco registros existentes de ronda 1 y la evidencia acotada anterior.
- El contenido del informe historico y la salida del juez B permanecen explicitamente no disponibles y sin reconstruir.
- Cada hallazgo retenido usa una clase de evidencia, un propietario canonico existente y exactamente una disposicion permitida.
- Los campos del resultado exponen limites, incognitas, aprobaciones, paridad y resultados Engram independientes.
- Las aprobaciones de alcance, ejecucion, promocion, ciclo de vida e implementacion permanecen separadas.
- Ninguna clasificacion implica implementacion o avance.
- Los documentos en ingles y espanol permanecen como pares fieles, sin nueva superficie de persistencia ni dependencia de evidencia externa.

## Siguiente decision explicita

El propietario autorizo despues la reconciliacion acotada, acepto las cinco clasificaciones `close` resultantes y autorizo el cierre del ciclo de vida el 2026-08-24. No se requirio promocion ni implementacion y no queda ninguna decision de RH-W-0011.
