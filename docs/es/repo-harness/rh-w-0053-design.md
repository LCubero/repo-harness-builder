# Diseño RH-W-0053 de checkpoint para sesión interrumpida

> **Estado: REGISTRO HISTÓRICO DE DISEÑO COMPLETADO.** El propietario aprobó este diseño propuesto antes de completar su implementación autorizada por separado. Este registro conserva el contexto de aprobación y diseño; en esa etapa de diseño no se afirmaban entrega Git, staging, commit, push, publicación, acceso de red ni frescura de nombres registrados de runtime. La entrega posterior mediante issue #170 y PR #171 se fusionó por squash como `c7290fcf6c703df0839699a9cb8b77686bf18e74`.

## Decisión

RH-W-0053 usa un único archivo Markdown estructurado, ignorado y no autoritativo en `.repo-harness/state/checkpoints/active.md`. Registra un checkpoint operativo incompleto, no el historial de la sesión. Crearlo o reemplazarlo exige una solicitud humana actual y explícita; la interrupción por sí sola no concede autoridad de escritura.

Session-start puede descubrir y leer el archivo sin mutar. Nunca continúa automáticamente el trabajo registrado. Reanudar exige verificar la evidencia actual del repositorio y obtener autoridad nueva para cada acción que todavía necesite aprobación.

Este diseño se revisa contra el [registro canónico](../../repo-harness/roadmap.md#rh-w-0053), la [reconciliación RH-W-0050](../../repo-harness/rh-w-0050-design.md), la [taxonomía RH-W-0051](../../repo-harness/rh-w-0051-design.md), el [diseño de continuidad RH-W-0069](../../repo-harness/rh-w-0069-design.md), las [reglas de memoria](../../../.repo-harness/rules/memory.md) y el [propietario en inglés](../../repo-harness/rh-w-0053-design.md).

## Propiedad y límites

| Propietario | Responsabilidad |
|---|---|
| RH-W-0051 | `.repo-harness/state/`, contención, ignore, manifest y comportamiento de solo lectura del checker |
| RH-W-0053 | `checkpoints/active.md`, esquema, creación, descubrimiento, expiración y reanudación |
| RH-W-0050 | Clases de evidencia y reconciliación acotada |
| RH-W-0069 | Resúmenes de pausa o cierre en `.repo-harness/memory-bk/` y contraste con Engram |
| Propietarios del repositorio | Comportamiento, ciclo, decisiones, cronología, entrega y aprobaciones actuales |

El checkpoint no es verdad canónica ni autoridad. No es backlog, plan, aprobación, recibo, registro de entrega, almacén de conflictos, resumen de memoria ni backup. Conserva solo el estado operativo inconcluso mínimo para evaluar una posible reanudación.

## Persistencia y esquema

El directorio se crea de forma diferida solo durante una escritura autorizada. Existe como máximo un checkpoint activo. Un checkpoint nuevo autorizado reemplaza el archivo regular existente únicamente después de inspeccionar sus bytes actuales, escribir con seguridad y releer la estructura. No se crea historial ni un segundo índice.

Forma obligatoria:

```markdown
---
schema_version: 1
project: <identificador estable del repositorio>
checkpoint_id: <identificador ASCII acotado en minúsculas>
created_at_utc: <fecha y hora UTC RFC 3339>
objective: <objetivo no sensible en una línea>
step_state: not_started | started_unverified | failed
resume_authority: required
---

# Incomplete Operational Checkpoint

## Verified Completed Facts
## Interrupted Step
## Remaining Intent
## Supported Inferences
## Unknowns
## Human Decisions Required
## Relevant Files
```

`Verified Completed Facts` solo incluye resultados respaldados por evidencia actual al escribir. `Interrupted Step` contiene una descripción, su `step_state` y evidencia observada acotada; nunca usa `completed`. `Remaining Intent` es intención, no promesa ni autorización de plan. Cada inferencia nombra evidencia y qué la volvería verificable. Las rutas son relativas al repositorio. Las secciones vacías contienen `None.`

El checkpoint nunca persiste tokens, alcance de aprobación, permisos, credenciales de comandos ni afirmaciones de que una autoridad previa siga vigente. `resume_authority` siempre es `required`; ningún otro valor es válido.

## Creación y expiración

Solo se permite crear o reemplazar cuando la persona solicita explícitamente guardar la operación incompleta actual como checkpoint. Autorizar implementación, pausa, cierre o continuidad no implica autoridad de checkpoint. Si la interrupción ocurre antes de terminar la escritura, no se afirma que el checkpoint se guardó.

Un checkpoint expira lógicamente, sin borrado automático, cuando el objetivo terminó, se abandonó o fue reemplazado; la identidad del proyecto ya no coincide; la evidencia actual contradice una premisa material o muestra el paso resuelto; el propietario del ciclo ya no permite la intención; o la ruta o estructura son inválidas.

La antigüedad por sí sola es consultiva y nunca demuestra expiración. No existen temporizador, scheduler, daemon, limpieza, base de retención ni archivado automático. El archivo expirado queda intacto hasta una acción explícita y autorizada de reemplazo o eliminación.

## Límites de datos sensibles

Se aplica la selección acotada de contenido de RH-W-0069. Se excluyen secretos, credenciales, tokens, cookies, llaves privadas, cadenas de conexión, volcados o valores de entorno, payloads de proveedores, rutas absolutas, identidades privadas externas, datos personales, comandos completos con argumentos sensibles, fuente, diffs, logs, stack traces y salida de pruebas. Se usan rutas relativas y resultados concisos. Si un paso no puede representarse con seguridad, se omite el detalle y el estado necesario se marca `unknown or unverifiable`.

Esto minimiza contenido; no prueba detección de secretos, cifrado ni protección frente a un sistema de archivos hostil.

## Descubrimiento no mutante en session-start

1. Resolver el repositorio y leer primero sus propietarios actuales aplicables.
2. Comprobar solo la ruta exacta `active.md`. Su ausencia es `not_found`, no un fallo.
3. Rechazar entradas no regulares, indirectas, fuera de raíz, malformadas o de otro proyecto sin seguir enlaces.
4. Comparar cada afirmación material con la evidencia actual aplicable de repositorio, Git, checker, ciclo y decisiones.
5. Clasificar como `usable`, `stale`, `contradictory`, `expired` o `invalid` e informar la razón acotada.
6. No reescribir, borrar, archivar, marcar como consumido, actualizar fechas ni continuar trabajo durante el descubrimiento.

`usable` solo significa que puede informar una propuesta de reanudación. `stale` indica evidencia posterior, que prevalece. `contradictory` indica desacuerdo material cuyo significado operativo no resuelven los propietarios. `expired` sigue las condiciones semánticas anteriores. `invalid` cubre ruta insegura o esquema inválido.

## Autoridad de reanudación

Reanudar es una operación nueva, nunca una continuación heredada:

1. Reconstruir el objetivo actual mínimo desde el repositorio y el checkpoint usable.
2. Verificar nuevamente hechos terminados y paso interrumpido; no repetir un paso cuya finalización actual esté probada.
3. Reclasificar intención pendiente contra ciclo, decisiones, alcance, dependencias y estado actuales.
4. Descartar del recorrido propuesto las afirmaciones obsoletas; conservarlas solo como evidencia no autoritativa informada.
5. Preguntar una sola vez únicamente cuando se necesite autoridad humana actual o una decisión real de producto.
6. Ejecutar solo con autoridad nueva para la acción actual exacta. Nunca se reutilizan aprobaciones, consentimiento, digests, tokens ni intención de sesión previos.

Un checkpoint `stale`, `contradictory`, `expired` o `invalid` no autoriza ni activa trabajo. Si la evidencia resuelve una contradicción, prevalece el repositorio y se informa la diferencia. De lo contrario, solo esa rama pausa para la persona.

## Relación con continuidad

El backup RH-W-0069 o resumen Engram responde: "¿Qué ocurrió y qué importa después?" RH-W-0053 responde: "¿Qué paso operativo exacto quedó incompleto y es seguro y autorizado reanudarlo ahora?"

Session-start puede usar ambos, pero verifica ambos contra propietarios actuales. Un backup nunca se convierte en checkpoint, un checkpoint nunca sustituye el resumen de pausa o cierre y ninguna fuente copia a la otra. Los conflictos reutilizan clasificaciones RH-W-0050 sin persistencia adicional.

## Criterios de aceptación

- La única ruta es `.repo-harness/state/checkpoints/active.md`, con un Markdown tipado y sin historial.
- No se escribe sin solicitud humana actual y explícita.
- El paso solo puede ser `not_started`, `started_unverified` o `failed`; nunca se presenta como terminado.
- Se excluyen material sensible y rutas absolutas; una omisión segura queda desconocida, no inventada.
- El descubrimiento lee primero el repositorio, se limita a la ruta exacta y realiza cero escrituras.
- Se distinguen ausencia, obsolescencia, contradicción, expiración, invalidez y uso posible.
- La expiración depende de evidencia y no borra ni limpia automáticamente.
- Toda reanudación verifica evidencia actual y obtiene autoridad nueva; ninguna aprobación previa se reutiliza.
- Los resúmenes RH-W-0069 y Engram siguen siendo continuidad distinta, no checkpoints ejecutables.
- Los modos integrado y standalone no requieren daemon, base de datos, scheduler, servicio, ledger, cache, servicio genérico de parsing ni estado global.

## Exclusiones e impacto futuro

Este diseño aprobado por el propietario no cambió por sí mismo fuente de producto, skill generada, prueba, manifest, checker, directorio local, proyección de runtime ni estado de ciclo. La implementación completada después actualizó solo los contratos canónicos de session-start y session-stop y las pruebas enfocadas. RH-W-0051 conserva namespace e ignore, por lo que RH-W-0053 no agrega campo de manifest y el checker no inspecciona contenido.

El propietario aprobó este diseño propuesto el 2026-09-06 y la planificación de implementación el 2026-09-07. La implementación fue autorizada por separado después y está completada. Este diseño histórico conserva ese contexto de aprobación; futuras migración, eliminación, archivado, entrega y validación de nombres registrados de runtime siguen bajo gobierno separado. En la etapa de diseño no se afirmaban entrega Git, staging, commit, push, publicación, acceso de red ni frescura de nombres registrados. La implementación posterior se entregó mediante issue #170 y PR #171, fusionado por squash como `c7290fcf6c703df0839699a9cb8b77686bf18e74`; la frescura de nombres registrados de runtime sigue sin verificar.
