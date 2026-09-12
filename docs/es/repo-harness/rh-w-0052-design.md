# Diseño mínimo solo de archivo RH-W-0052

> **Estado: PROPUESTO.** La decisión técnica delegada por el propietario aprobó la regla de elegibilidad de archivo indicada abajo. Una autoridad separada del propietario autorizó después la porción de implementación solo de archivo, que está completa y verificada. RH-W-0052 sigue `proposed` porque desinstalación, reinstalación y borrado permanecen sin diseño ni autorización; no se infieren avance de ciclo, entrega, acceso a red, commit ni publicación.

## Contrato de revisión

Revise contra el [registro canónico](../../repo-harness/roadmap.md#rh-w-0052), las [decisiones permanentes](decisions.md), la [taxonomía RH-W-0051 completada](rh-w-0051-design.md) y el [propietario en inglés](../../repo-harness/rh-w-0052-design.md). El inglés es canónico. La evidencia del repositorio posee el comportamiento actual.

## Alcance

RH-W-0052 define solo una futura copia explícita de archivo hacia el namespace existente, no autoritativo e ignorado por Git `.repo-harness/archive/`. Un archivo preserva una fuente seleccionada y crea una copia nueva; nunca elimina, mueve, sobrescribe, normaliza, reinstala ni repara nada.

Este diseño autorizado cubre solo la porción mínima de copia de archivo. RH-W-0052 continúa poseyendo el comportamiento futuro del ciclo de vida de archivo, desinstalación, reinstalación y borrado; más allá de esta porción, esas operaciones permanecen sin diseño ni autorización aquí.

El prototipo VPS fallido es únicamente evidencia histórica de origen. No es una entrada de diseño que pruebe seguridad ni comportamiento entregado. RH-W-0048 sigue siendo una unidad completada y separada de recuperación aditiva de proyecciones de runtime; su autoridad, conducta de reparación, pruebas y evidencia de entrega no autorizan esta operación de archivo.

## Propiedad y preservación

- RH-W-0051 posee el namespace de archivo, contención, comportamiento de ignore y estado no autoritativo. RH-W-0052 posee solo el límite futuro de decisión para copia de archivo.
- Se preserva cada ruta de estado local, incluido todo contenido bajo `.repo-harness/` salvo el único destino de archivo nuevo creado por un apply aprobado.
- Se preserva cada ruta desconocida, humana, existente, indirecta o propiedad de herramientas. En particular, `.atl/`, `.codegraph/` y `.engram/` quedan fuera de la propiedad del harness.
- No se supone que exista Git. Cuando falta Git, no se afirma propiedad ni recuperación respaldada por Git.
- El namespace de archivo es estado recuperable, no verdad canónica, manifest, almacén de ciclo, registro de aprobación ni evidencia de que después pueda borrarse la fuente.

## Límite de seguridad de archivo

Un futuro candidato debe ser una fuente regular, directa, relativa al repositorio y nombrada explícitamente dentro de la raíz seleccionada. La fuente no puede estar dentro de `.repo-harness/archive/`, atravesar una ruta indirecta ni escapar de la raíz. Ningún descubrimiento recursivo, inferencia desde nombres de rutas, recorrido de directorios desconocidos ni inspección de rutas de herramientas selecciona un candidato.

La cadena exacta de directorios padre del destino debe existir previamente como directorios reales directos. Un padre ausente, indirecto o que no sea directorio rechaza sin escribir. Apply no ejecuta `mkdir`, borrado de rollback ni limpieza. Esta corrección mínima seleccionada por el propietario evita la combinación imposible de padres ausentes, colisión de carrera, rechazo con cero escrituras y ausencia de borrado de rollback.

**Decisión técnica delegada por el propietario:** después de instruir al agente a leer y verificar la evidencia del repositorio y decidir, el propietario aprobó la propiedad mediante un manifest actual autenticado como regla obligatoria de elegibilidad para archivo. Un candidato es elegible solo cuando un manifest actual autenticado prueba que esa fuente directa exacta es propiedad del harness. Prueba ausente, inválida, obsoleta o sin cobertura produce `ownership-unresolved` y preserva la fuente.

## Inspección, vista previa, aprobación y apply

| Etapa | Límite requerido | Resultado prohibido |
|---|---|---|
| Inspección | Leer solo el candidato nombrado explícitamente, evidencia aplicable de propiedad, contención raíz, tipo de fuente, disponibilidad de destino y cadena de padres directos reales. Informar `eligible`, `ownership-unresolved` o un motivo acotado de rechazo. | No crear directorios, copiar, mover, borrar, cambiar manifest/checker, hacer acción Git ni escanear rutas desconocidas o de herramientas. |
| Vista previa | Producir un único mapeo exacto de fuente a nuevo destino de archivo y evidencia de identidad acotada; informar `writes: 0`. Una colisión, padre ausente/indirecto/no-directorio, ruta insegura, propiedad ausente o evidencia cambiada no produce acción elegible. | No selección implícita, sobrescritura, mutación ni autoridad de una vista previa anterior. |
| Aprobación | Exigir aprobación humana explícita y actual de esa vista previa exacta. Un cambio material de fuente, destino, propiedad o evidencia la invalida. | No autoridad heredada de sesión, Git, manifest, reparación ni aprobación anterior; tampoco aprobación de borrado, reinstalación, actualización o entrega. |
| Apply | Confirmar nuevamente el mapeo aprobado y los hechos de seguridad antes de crear solo el nuevo destino de archivo. El éxito exige fuente intacta y copia nueva completa; de lo contrario informa fallo preservando la fuente. | No mover, borrar ni sobrescribir fuente; tampoco `mkdir`, borrado de rollback, arquitectura de limpieza, inicialización Git, staging, commit, push, red ni publicación. |

Estos son límites de seguridad a nivel de diseño, no un comando, schema, formato de digest, algoritmo transaccional ni plan de implementación.

## No objetivos

- Desinstalación, reinstalación, borrado, limpieza automática, automatización de retención, migración o arquitectura de borrado.
- Cambios a fuente de producto, pruebas, proyecciones de runtime, skills, checker, manifest o implementación.
- Archivar rutas arbitrarias, desconocidas, de estado local, humanas o propiedad de herramientas.
- Cualquier supuesto Git, entrega, acción de red, commit o publicación.
- Base de datos, ledger, caché, segundo manifest, scheduler, daemon, servicio o subsistema genérico de archivo.

## Límite futuro de aceptación

La implementación autorizada por separado cumplió el plan bilingüe: inspección y vista previa no escriben; apply crea solo la copia nueva aprobada; fuentes, rutas desconocidas, estado local y rutas de herramientas permanecen sin cambios; y los padres del destino deben ser directorios reales directos ya existentes. El plan conserva los recibos completos de TDD estricto y verificación. Ninguna autoridad se extiende a desinstalación, reinstalación, borrado, limpieza, entrega ni avance de ciclo.

## Límite de autorización

La regla aprobada por el propietario mantiene RH-W-0052 en estado `PROPUESTO`. La implementación solo de archivo está completa y verificada bajo autoridad separada, sin trabajo restante de implementación para esa porción. Desinstalación, reinstalación, borrado, limpieza, avance de ciclo, entrega, acceso a red, commit, push y publicación permanecen sin diseño o autorización separados.
