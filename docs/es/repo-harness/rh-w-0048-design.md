# Diseño RH-W-0048 de recuperación de proyecciones de runtime

> **Estado: PROPUESTO (reabierto).** El diseño aprobado y la implementación autorizada por separado permanecen como evidencia histórica. El único trabajo restante es reconciliar el contrato de mutabilidad y volver a verificar de forma independiente. La revisión nativa no está disponible porque falta el binario local del paquete, por lo que no existe linaje; eso no es un veredicto. La entrega permanece separada y sin realizar.

## Decisión

Un harness instalado puede recuperar únicamente proyecciones de runtime locales y ausentes mediante el futuro comando explícito `python .repo-harness/check.py repair`. Sin el argumento de reparación, `python .repo-harness/check.py` sigue siendo una comprobación no mutante e informa `writes: 0`.

`repair` solo muestra una vista previa de forma predeterminada. Enumera los archivos exactos aptos para creación y devuelve un `planSha256` determinista. La mutación exige `repair --apply --approved-plan-sha256 <digest>`. Apply repite el preflight y rechaza si cambió el plan. Esta es autoridad explícita de reparación, no aprobación del builder, consentimiento anterior ni permiso para actualizar bytes existentes.

Revise este diseño contra el [registro canónico](../../repo-harness/roadmap.md#rh-w-0048), las [decisiones permanentes](decisions.md), la [taxonomía RH-W-0051](rh-w-0051-design.md), la [arquitectura actual](../../../skill/repo-harness-builder/references/architecture.md), las [reglas de preservación](../../../skill/repo-harness-builder/references/preservation-and-safety.md) y el [diseño canónico en inglés](../../repo-harness/rh-w-0048-design.md).

## Fuente de verdad y conjunto recuperable

El `.repo-harness/manifest.json` autenticado declara los runtimes seleccionados y las relaciones entre archivos canónicos y proyecciones. Los archivos canónicos nombrados por esa declaración suministran los bytes esperados. La recuperación ordinaria usa `canonicalSkills` y `canonicalCommands`, cuyas fuentes viven bajo `.repo-harness/`. Un repositorio fuente recíproco puede evaluar también `canonicalProjections` solo cuando la fuente declarada está presente, es directa, está contenida y puede leerse. Una fuente no disponible se informa y nunca se reconstruye desde una copia de runtime.

No se agrega segundo inventario, paquete de plantillas, caché, ledger, base de datos ni modo del manifiesto. El manifiesto autenticado y los archivos canónicos existentes bastan. Los mapeos siguen siendo los ya declarados: comandos para Claude Code y OpenCode, prompts para Pi y skills para cada runtime seleccionado. El diseño no supone una API de recarga. La recuperación prueba archivos en disco; que el runtime activo los cargue queda sin verificar salvo evidencia independiente.

El conjunto mínimo recuperable son exactamente los destinos derivados de `canonicalSkills` y `canonicalCommands`. `canonicalProjections` sigue siendo opcional y específico del repositorio.

## Clasificación de proyecciones

| Resultado | Significado | Conducta de reparación |
|---|---|---|
| `current` | Un destino regular y directo coincide con sus bytes canónicos. | Sin acción. |
| `missing-local` | El destino no existe, la fuente es válida y Git no declara esa ruta exacta rastreada; sin Git utilizable, es local por defecto. | Creación aditiva. |
| `missing-tracked` | El destino no existe, pero la ruta exacta está rastreada en el índice Git. | Deriva real; solo se informa. |
| `stale` | Existe un destino regular y directo, pero difiere de los bytes esperados. | Se preservan todos sus bytes; repair no actualiza. |
| `damaged` | El manifiesto no es auténtico o válido; una fuente o destino es indirecto, inseguro, ilegible, de tipo incorrecto, externo o no permite comparación segura. | Se rechaza la rama afectada antes de escribir. |
| `canonical-missing` | Falta una fuente canónica declarada. | Se informa; no se crea sin autoridad. |

“Obsoleta” corresponde a `stale`: existen bytes, pero ya no coinciden con su propietario canónico. No es ausencia. La frescura del paquete sigue separada.

## Preflight, vista previa y autoridad

El preflight es de solo lectura y se limita al manifiesto autenticado, archivos canónicos y destinos declarados, contención, tipo y evidencia exacta de ruta rastreada cuando Git es utilizable. No crea directorios ni edita `.gitignore`, `.gitattributes`, manifiesto, checker, fuentes o runtimes.

La vista previa devuelve la clasificación por destino, el `create` exacto y ordenado, exclusiones con motivos, `writes: 0` y `planSha256`. El digest vincula ruta relativa, digest esperado y digest del manifiesto. No contiene rutas absolutas ni contenido. Un conjunto apto vacío termina correctamente.

Apply acepta solo el digest actual, repite el preflight antes de la primera escritura y rechaza autoridad obsoleta. Nunca hace staging, commit, push, `git init`, sigue enlaces ni recarga runtimes.

## Transacción y preservación

Repair crea solo destinos `missing-local` con creación exclusiva y puede crear padres necesarios dentro de la raíz. Todo archivo preexistente, hijo desconocido, archivo humano, proyección obsoleta, ruta rastreada ausente y directorio ajeno queda intacto.

Las creaciones forman una transacción. Tras cada escritura, apply comprueba contención y digest. Ante fallo elimina solo archivos creados por esa invocación cuyos bytes aún coinciden; no toca bytes cambiados o preexistentes. La limpieza de padres vacíos es de mejor esfuerzo y solo alcanza directorios creados por la misma invocación. Se reutiliza el modelo existente sin journal ni archivo nuevo.

Después de apply, el checker repite la comparación. El éxito exige que cada destino intentado quede `current`; el resultado separa creados, actuales, omitidos, rechazados, rollback y post-check. Un rollback parcial, validación del runtime no disponible, obsoleto restante, rastreado ausente o fuente fallida se informa y nunca se resume como `recovered`.

## Reparación frente a actualización

Repair restaura ausentes locales desde material ya instalado. No cambia fuentes canónicas, esquema, versión, runtimes, bytes existentes ni documentación humana.

Update sigue siendo la operación del renderer aprobada por separado. Puede reemplazar bytes generados de propiedad autenticada bajo su plan, atomicidad manifiesto/checker y rollback existentes. Una proyección obsoleta recibe recomendación de update; `repair` no se amplía a update, reinstalación, archivo, desinstalación o borrado. RH-W-0052 conserva esas operaciones.

Para proyecciones solo locales, el render futuro omite reglas nuevas de `.gitattributes`. Las líneas existentes se preservan y repair nunca las elimina. Las proyecciones rastreadas conservan el contrato LF. Este diseño no cambia bytes actuales.

## Criterios de aceptación

- El check ordinario sigue en solo lectura e informa cero escrituras.
- Preview clasifica todos los resultados definidos sin escribir.
- Apply exige el digest exacto y crea solo `missing-local`.
- Clon nuevo y árbol parcial recuperan el mínimo para Claude Code, OpenCode y Pi desde fuentes disponibles, sin builder externo.
- Destinos existentes, obsoletos, desconocidos, humanos, indirectos, inseguros y rastreados ausentes conservan sus bytes.
- Una creación múltiple fallida revierte solo archivos y directorios sin cambios creados por esa invocación.
- El post-check informa límites honestos; paridad en disco no implica recarga.
- Reparación, update y RH-W-0052 conservan autoridades separadas.

## No objetivos

- Sin implementación, plan, migración, reinstalación, archivo, desinstalación, borrado, staging, commit, push, PR, release ni recarga.
- Sin servicio, daemon, scheduler, base de datos, ledger, caché, estado global, segundo manifiesto, plantillas copiadas ni red.
- Sin sobrescribir o normalizar archivos existentes ni inferir que una ruta ignorada es descartable.
- Sin API específica de runtime fuera de las proyecciones ya declaradas.

## Ciclo de vida y decisiones humanas

RH-W-0048 está `proposed`, reabierta solo para reconciliar el contrato de mutabilidad y volver a verificar de forma independiente. El propietario aprobó este diseño después de la revisión técnica delegada y la revisión directa del padre; la implementación autorizada por separado permanece como evidencia histórica completada. Resuelve comando, señal rastreada, autoridad, fuente, conjunto, clasificaciones, preservación, transacción, límite con update, `.gitattributes`, paridad y resultados. La evidencia histórica de verificación es 536 pruebas con 9 omitidas; checker `current` con 63 proyecciones y `writes: 0`; paridad de paquetes canónico, Claude Code, OpenCode y Pi; y verificación independiente sin hallazgos. La revisión nativa no está disponible porque falta el binario local del paquete, por lo que no existe linaje; eso no es un veredicto. Commit, push, PR, entrega, publicación, release y red permanecen separados, no autorizados y sin realizar.
