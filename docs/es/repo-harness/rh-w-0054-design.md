# Diseno RH-W-0054 de invalidacion por correccion material

> **Estado: DISENO APROBADO POR EL PROPIETARIO. La implementacion esta pendiente.**
>
> El propietario aprobo el 2026-08-21 las decisiones de comportamiento representadas aqui. La aprobacion del diseno avanza RH-W-0054 a `ready`; no autoriza ni afirma implementacion o entrega.

## Contrato de revision y etiquetas de estado

Este diseno se revisa contra el [registro canonico RH-W-0054](../../repo-harness/roadmap.md#rh-w-0054), el modelo existente de preview y apply ligado al digest y la [base completada de reconciliacion acotada RH-W-0050](../../repo-harness/rh-w-0050-design.md). La evidencia del repositorio es canonica. Este par debe permanecer completo y semanticamente equivalente al [diseno canonico en ingles](../../repo-harness/rh-w-0054-design.md).

- **Restriccion aprobada:** ya autorizada por la politica del repositorio o por la seguridad ejecutable existente.
- **Comportamiento actual verificado:** observado directamente en la evidencia actual del repositorio y no introducido por este diseno.
- **Decision de diseno aprobada:** aprobada por el propietario para una implementacion futura de RH-W-0054.
- **Detalle de implementacion restante:** aplazado intencionalmente hasta una tarea de implementacion aprobada.

## Problema, objetivos y no objetivos

Un preview puede derivar afirmaciones, argumentos seleccionados, salidas, un plan, un digest y un limite de aprobacion a partir de respuestas de entrevista, decisiones humanas, interpretaciones del asistente y evidencia del repositorio. Si despues se corrige una premisa material, conservar autoridad derivada de la premisa anterior puede ejecutar trabajo que la persona no reviso de nuevo.

El diseno aprobado usa la primera escritura al repositorio como limite de correccion. Antes de esa escritura, toda correccion material invalida el plan, digest y aprobacion completos de la generacion afectada, incluso cuando el recalculo produce archivos y acciones byte a byte identicos. El flujo conserva la evidencia anterior solo como historia, restablece la autoridad factual o humana requerida, recalcula desde entradas actuales y presenta un preview totalmente nuevo para una aprobacion nueva.

Despues de cualquier escritura del apply, invalidar autoridad no puede deshacer la mutacion. Una correccion material descubierta entonces hace que RH-W-0054 falle cerrado, informe que el estado actual del repositorio requiere verificacion nueva y enrute a un flujo separado de remediacion o recuperacion. Nunca hace rollback silencioso, correccion automatica, reutilizacion de aprobacion ni afirma que la invalidacion recupero el estado del repositorio. Ningun registro existente posee explicitamente ese flujo posterior al apply, por lo que asignar su propiedad es una decision de diseno futura y separada. RH-W-0053 sigue limitada a recuperacion de sesiones interrumpidas.

Este diseno no implementa comportamiento, pruebas, almacenamiento, migracion, recuperacion de interrupciones ni estado de correccion entre sesiones. No agrega almacen de ciclo de vida, recibo, ledger, cache, autoridad duplicada ni mutacion automatica. La recuperacion persistente de interrupciones pertenece a RH-W-0053. La semantica de almacenamiento propia del harness pertenece a RH-W-0051.

## Modelo de autoridad

| Autoridad | Funcion |
|---|---|
| Evidencia del repositorio | Fuente canonica de hechos actuales y entradas ejecutables del repositorio. |
| Decision humana | Autoridad actual para elecciones humanas despues de una decision o correccion explicita. |
| Generacion del preview | Limite efimero que liga el preview derivado completo con las entradas y evidencia usadas para crearlo. |
| Plan y digest | Guardas ejecutables dentro de una generacion valida. No sobreviven una correccion material. |
| Aprobacion | Autoridad humana para exactamente una generacion actual y su plan y digest ligados. |
| Evidencia y previews anteriores | Solo contexto historico despues de invalidarse; nunca autoridad actual. |

Las comprobaciones existentes de plan, digest y bytes siguen siendo las guardas ejecutables finales. RH-W-0054 agrega una regla anterior: una generacion invalidada no puede alcanzar esas guardas como generacion actual aprobada, aunque sus bytes, acciones, plan o digest coincidan con un resultado recalculado.

## Definiciones

| Termino | Definicion normativa |
|---|---|
| Correccion material | Correccion que cambia o cuestiona una entrada, afirmacion, decision, interpretacion o hecho del repositorio del que se derivo algun resultado o autoridad ejecutable. La materialidad depende de la derivacion y autoridad, no de que cambien los bytes finales. |
| Correccion irrelevante | Correccion que no tiene ninguna ruta de dependencia hacia respuestas, afirmaciones, argumentos seleccionados, salidas derivadas, elementos del plan, entradas del digest o aprobacion de la generacion actual. |
| Generacion del preview | Ciclo efimero de derivacion desde entradas clasificadas y evidencia actual hasta preview, plan, digest y limite de aprobacion completos. |
| Conjunto afectado | Entrada corregida y cada respuesta, afirmacion, argumento seleccionado, salida derivada y elemento del plan que dependa transitivamente de ella. |
| Invalidacion | Antes de la primera escritura al repositorio, retiro de autoridad actual al plan, digest y aprobacion completos de la generacion afectada. No borra historia ni revierte una mutacion. |
| Preview nuevo | Preview completo recalculado en una nueva generacion despues de actualizar hechos y decisiones. No es repeticion ni cambio de etiqueta del preview anterior. |

## Clasificacion de correcciones

Cada correccion se clasifica antes del recalculo:

| Clase de entrada | Recuperacion de autoridad requerida |
|---|---|
| Afirmacion factual | Volver a verificar con evidencia actual aplicable. Si no esta disponible o se contradice, bloquear el flujo afectado y nombrar la evidencia faltante. |
| Decision humana | Obtener una nueva decision humana explicita. El texto corregido no funciona tambien como aprobacion del preview resultante. |
| Interpretacion del asistente | Regresar a hechos verificados y decisiones humanas subyacentes, reemplazar la interpretacion y recalcular cada resultado dependiente. |
| Cambio de evidencia del repositorio | Volver a leer la evidencia actual aplicable y recalcular desde ella. La instantanea anterior queda solo como historia. |

Clasificar no autoriza por si solo el recalculo ni la ejecucion. El recalculo comienza solo despues de volver a verificar cada hecho afectado y obtener una decision explicita actual para cada eleccion humana afectada.

## Invalidacion consciente de dependencias

Antes de la primera escritura al repositorio, el flujo usa conocimiento de dependencias acotado y efimero dentro de la sesion y generacion actuales:

1. Identificar y clasificar la entrada corregida.
2. Trazar dependencias por respuestas, afirmaciones, argumentos seleccionados, salidas derivadas y elementos del plan afectados.
3. Si no existe ruta de dependencia, clasificar la correccion como irrelevante para el plan y conservar la generacion actual.
4. Si existe cualquier ruta, clasificarla como material.
5. Invalidar plan, digest y aprobacion completos de la generacion afectada.
6. Conservar entradas, evidencia, preview, plan, digest y aprobacion anteriores solo como contexto historico.
7. Volver a verificar hechos afectados u obtener nuevas decisiones humanas afectadas.
8. Recalcular respuestas, afirmaciones, argumentos, salidas y plan completo dependientes en una nueva generacion.
9. Producir un digest nuevo y un preview totalmente nuevo.
10. Exigir aprobacion nueva antes del apply.

La dependencia acota la decision de si una correccion es material. Una vez que es material para una generacion, la invalidacion del plan, digest y aprobacion abarca toda esa generacion. Se prohiben aprobacion parcial, arrastre del digest y rescate de aprobacion porque crearian dos autoridades dentro de un preview ejecutable.

## Contrato de transiciones

| Estado actual | Evento | Estado siguiente requerido |
|---|---|---|
| Entrevista o derivacion antes del preview | Correccion material | Marcar estado derivado afectado como no autoritativo, recuperar autoridad y continuar solo mediante nueva generacion. |
| Preview producido sin aprobar | Correccion material | Invalidar plan y digest completos, recuperar autoridad y producir preview nuevo. |
| Preview aprobado sin aplicar | Correccion material | Invalidar de inmediato plan, digest y aprobacion; recuperar autoridad y exigir preview y aprobacion nuevos. |
| Existe preview o aprobacion | Correccion irrelevante probada | Conservar la generacion actual sin crear autoridad ejecutable nueva. |
| Cambia evidencia del repositorio despues del preview | Cambio aplicable | Tratarlo como correccion material e invalidar antes del apply. |
| Se presenta plan, digest o aprobacion anterior tras invalidacion | Intento de reutilizacion | Rechazarlo como obsoleto sin importar igualdad de bytes o acciones. |
| Preview nuevo recibe aprobacion nueva | Se solicita apply | Ejecutar las guardas existentes de plan, digest, bytes y cambio material. |
| Ya ocurrio cualquier escritura del apply | Correccion material | Hacer que RH-W-0054 falle cerrado, informar que el estado actual requiere verificacion nueva y enrutar a un flujo de remediacion o recuperacion con propiedad separada, sin afirmar rollback ni recuperacion. |

Ningun estado de correccion sobrevive la sesion. Si la sesion termina antes de aprobar un preview nuevo, RH-W-0054 no aporta checkpoint reanudable. Cualquier flujo persistente futuro pertenece a RH-W-0053 y debe usar la semantica de RH-W-0051.

## Estados de resultado y salida

| Resultado | Significado |
|---|---|
| `irrelevant` | No se encontro ruta hacia la generacion actual; la autoridad del plan no cambia. |
| `invalidated` | Una correccion material retiro autoridad al plan, digest y aprobacion completos. |
| `blocked` | Falta evidencia factual o decision humana requerida. |
| `fresh_preview_required` | La autoridad se recupero y el recalculo debe producir un preview completo nuevo. |
| `fresh_approval_required` | Existe preview nuevo sin aprobacion para su generacion. |
| `ready_for_existing_guards` | Existe aprobacion nueva y apply puede pasar por todas las guardas existentes. |
| `post_apply_remediation_required` | Se descubrio una correccion material despues de una escritura del apply; RH-W-0054 no puede recuperar el estado mutado y un flujo separado debe verificarlo de nuevo. |

Una implementacion puede elegir nombres exactos, pero debe identificar clase, conjunto afectado, generacion invalidada, recuperacion de autoridad, generacion nueva y siguiente limite. La salida no se persiste como recibo o ledger.

## Limites de fallo y seguridad

- Evidencia factual ausente o contradictoria bloquea solo el flujo afectado y nunca restaura autoridad al preview anterior.
- Una decision humana ausente pausa antes de recalcular salidas dependientes.
- Si no se puede calcular el conjunto afectado, se falla cerrado y la correccion se trata como material.
- Un cambio de evidencia descubierto antes del apply invalida antes de cualquier escritura.
- Despues de cualquier escritura del apply, una correccion material hace que RH-W-0054 falle cerrado. Informa que se requiere verificacion nueva y remediacion o recuperacion separada, sin rollback, correccion automatica, aprobacion reutilizada ni afirmacion falsa de recuperacion.
- Un recalculo con bytes o acciones identicos nunca restaura la aprobacion anterior. La igualdad confirma estabilidad de salida, no continuidad de autoridad humana.
- La evidencia historica se conserva sin convertirse en almacen alterno de entradas actuales.
- Se prohiben correccion automatica, preview silencioso, aprobacion automatica y mutacion del repositorio.

## Contrato requerido de pruebas RED/GREEN

| Escenario | Defecto RED | Resultado GREEN |
|---|---|---|
| La correccion cambia bytes o acciones | Preview o aprobacion anterior sigue utilizable | Se invalida la generacion y el cambio aparece solo en preview nuevo con aprobacion nueva. |
| La correccion produce bytes y acciones identicos | La igualdad conserva aprobacion anterior | Se invalida la generacion y aun se exigen preview y aprobacion nuevos. |
| La correccion es irrelevante para el plan | Se invalida todo sin dependencia | La irrelevancia se prueba desde dependencias efimeras y la autoridad actual se conserva. |
| Cambia una decision humana | El texto corregido funciona como decision y aprobacion | Una nueva decision precede el recalculo y la aprobacion del preview sigue separada. |
| Cambia evidencia despues del preview | Apply usa solo la instantanea anterior | Se invalida, se relee evidencia y se exige preview nuevo. |
| Se reutiliza digest o aprobacion anterior | La autoridad obsoleta alcanza apply porque los bytes coinciden | Se rechaza antes de ejecutar, sin importar igualdad. |
| El flujo corregido tiene exito | Apply reanuda autoridad vieja parcial | Solo tiene exito con preview completo nuevo, aprobacion nueva y guardas finales existentes. |
| Se descubre una correccion material despues de una escritura del apply | RH-W-0054 hace rollback silencioso, corrige automaticamente o reutiliza autoridad anterior | RH-W-0054 falla cerrado, informa que el estado actual requiere verificacion nueva y enruta a un flujo separado de remediacion o recuperacion. |
| Se informa el enrutamiento posterior al apply | La invalidacion se describe como si deshiciera la escritura o recuperara el repositorio | La salida declara explicitamente que no ocurrio rollback ni recuperacion y que la propiedad del flujo separado sigue como decision futura hasta que se apruebe otro propietario canonico. |

La cobertura enfocada tambien debe probar conservacion historica, recuperacion bloqueada, analisis que falla cerrado, estado solo de sesion, enrutamiento de correcciones posteriores al apply, ausencia de afirmaciones falsas de rollback o recuperacion y ausencia de almacen, recibo, ledger, cache o autoridad duplicada.

## Mapa de impacto de implementacion

Este mapa solo identifica trabajo futuro y no autoriza ninguna edicion.

| Simbolo o ruta estable | Cambio futuro requerido |
|---|---|
| Flujo de preview y apply en `skill/repo-harness-builder/scripts/harness_tool.py` | Agregar identidad de generacion, clasificacion, dependencias efimeras, invalidacion completa antes de escribir, preview nuevo, rechazo de generaciones obsoletas y enrutamiento cerrado posterior al apply sin persistencia ni afirmacion de recuperacion. |
| Validacion existente de plan y digest | Conservarla como guarda final y exigir aprobacion actual de la misma generacion. |
| Comprobaciones de evidencia antes del apply | Enrutar cambios aplicables por invalidacion antes de escribir. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Agregar todo el contrato RED/GREEN, incluidos resultado identico, irrelevancia, enrutamiento posterior al apply y prueba de que no se afirma rollback ni recuperacion. |
| Documentacion de comportamiento | Documentar ingreso de correcciones y aprobacion nueva sin afirmar recuperacion entre sesiones. |
| Proyecciones de runtime | Sincronizar solo despues de implementar la fuente canonica mediante el mantenimiento existente ligado al digest. |

## Detalles restantes

Identificadores internos, representacion de dependencias, superficie conversacional o de comando, nombres de campos y fixtures siguen siendo detalles de implementacion. No pueden debilitar la invalidacion completa antes de escribir tras una correccion material, la separacion entre decision y aprobacion, el preview nuevo, las guardas finales existentes, el enrutamiento cerrado posterior al apply ni la prohibicion de persistir estado RH-W-0054 o afirmar falsamente rollback o recuperacion.

## Matriz de evidencia

| Afirmacion | Clase de evidencia | Propietario | Tratamiento |
|---|---|---|---|
| RH-W-0054 esta `ready` con implementacion pendiente | Decision de ciclo de vida aprobada | `roadmap.md#rh-w-0054` | Avanza solo por este diseno aprobado. |
| La correccion material invalida incluso con resultado identico | Decision de diseno aprobada | Este diseno | Requerida. |
| Las dependencias son acotadas y efimeras | Decision de diseno aprobada | Este diseno | Requerida. |
| La invalidacion posterior al apply no puede deshacer la mutacion | Decision de diseno aprobada | Este diseno | Fallar cerrado y enrutar a remediacion o recuperacion separada. |
| La propiedad de la remediacion posterior al apply no esta asignada | Decision humana requerida | Diseno futuro | RH-W-0053 no se asigna sin evidencia explicita del repositorio. |
| Las comprobaciones existentes siguen como guardas finales | Restriccion aprobada y comportamiento verificado | Modelo existente | Conservadas. |
| La recuperacion entre sesiones pertenece a RH-W-0053 | Limite aprobado | `roadmap.md#rh-w-0053` | Conservado. |
| La semantica de almacenamiento pertenece a RH-W-0051 | Limite aprobado | `roadmap.md#rh-w-0051` | Conservado. |
| No se agrega autoridad ni persistencia nueva | Decision de diseno aprobada | Este diseno | Requerida. |
