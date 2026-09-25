# Plan de implementacion RH-W-0044 de sugerencias para documentos humanos

> **Estado vigente: plan corregido aceptado; ciclo `proposed`.** Las porciones de vista previa del núcleo del rulebook (`63220c047e4e3c589f274f36ac1462c478618763`), del resumen bilingüe de uso (`f31c49446b770f4502e9f5c40dc217bd75757276`), de Visibility (`018cc4aa829c410a8d1f89a6443387205f0ddc81`) y de Translation Sync EN/ES (`27ca929c747d7612c03c29ce44eea1fea87de30a`), autorizadas por el propietario, están verificadas independientemente, confirmadas en commits locales y reconocidas mediante revisión nativa; el commit documental anterior (`9c20eb4721903bac00dd40644a8fba573dba13e4`) es separado. R44-1 sigue abierto con ambigüedad conservadora pendiente para otras familias EN/ES; R44-2/R44-3 no se han iniciado y falta el informe final de toda la funcionalidad R44-4. No se afirma push, PR de producto, fusión, release ni implementación completa.

Derivado solo del [diseno](rh-w-0044-design.md) aprobado. Par canonico: [rh-w-0044-implementation-plan.md](../../repo-harness/rh-w-0044-implementation-plan.md).

## Superficies canonicas y autoridad comprobadas

`skill/repo-harness-builder/scripts/harness_tool.py::render_repository()` y `render_with_skill_mapping()` poseen la evidencia renderizada. `human_owned()`, `semantic_sections()`, `missing_sections()` y `check()` poseen la clasificacion actual. `main()` posee el preview, reporte y flujo de aprobacion del comando `render` existente. `plan_changes()`, `plan_digest()`, `validate_write_preconditions()`, `write_outputs()` y `verify_outputs()` poseen el plan ligado a contenido, la repeticion previa a escribir, el rollback atomico y la verificacion posterior. `skill/repo-harness-builder/tests/test_harness_tool.py` posee las pruebas mediante una clase enfocada `HumanOwnedSuggestionTests` y los controles `CheckTests`, `AdditiveWriteTests` y `ApprovalUxTests`. Las copias de runtime son solo proyecciones.

Un `suggestionId` puede ser el SHA-256 de una tupla canónica versionada con idioma, ruta relativa e ID estable de unidad contractual. Esto liga la identidad semántica sin un almacén. Un `suggestionEvidenceSha256`, mostrado y suministrado por separado, liga a contenido la versión del contrato, `suggestionId`, idioma, ruta relativa, ID estable de unidad contractual, SHA-256 de la fuente actual, tipo de acción y bytes UTF-8 propuestos exactos. La selección extiende `render` con un ID, su digest de evidencia y una opción. El builder debe releer y volver a renderizar, recalcular el digest y verificar igualdad antes de construir el plan existente, de modo que un cambio de fuente o de renderer/bytes propuestos invalide la selección. No crea comando, servicio, estado, modelo semántico, dependencia ni ruta de autoridad.

## Tarea 1: RED enfocado para comparacion acotada

- [ ] Agregar `HumanOwnedSuggestionTests.test_preview_reports_one_deterministic_contract_unit_suggestion_with_zero_writes`, `test_project_truth_and_unstable_prose_never_receive_suggestions`, `test_project_specific_surrounding_prose_does_not_trigger_whole_section_replacement` y `test_unavailable_git_tracked_file_evidence_fails_closed_with_zero_suggestions_and_writes`.
- [ ] Exigir solo el conjunto Markdown aprobado, directo, legible y contenido. Archivos generados, ignorados, no rastreados, indirectos, incompletos por idioma, cronologia, ciclo, decisiones, planes, disenos, archivo y prosa arbitraria conservan clasificaciones existentes.
- [ ] Exigir identidad ruta/idioma/unidad, clasificación, motivo, evidencia, SHA-256 fuente, bytes UTF-8 exactos, extractos acotados, opciones, orden, `suggestionEvidenceSha256` mostrado por separado y `writes: 0`; el archivo renderizado completo solo es evidencia. Si Git no puede suministrar evidencia de archivos rastreados, clasificar el conjunto como no disponible/ambiguo, emitir cero sugerencias, realizar cero escrituras y nunca inventar un conjunto rastreado limpio.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py HumanOwnedSuggestionTests.test_preview_reports_one_deterministic_contract_unit_suggestion_with_zero_writes HumanOwnedSuggestionTests.test_project_truth_and_unstable_prose_never_receive_suggestions HumanOwnedSuggestionTests.test_project_specific_surrounding_prose_does_not_trigger_whole_section_replacement HumanOwnedSuggestionTests.test_unavailable_git_tracked_file_evidence_fails_closed_with_zero_suggestions_and_writes -v`. RED esperado: faltan los contratos de comparación/reporte y de cierre seguro ante evidencia Git no disponible, mientras los imports funcionan.

## Tarea 2: RED enfocado para opciones y divergencia

- [ ] Agregar `HumanOwnedSuggestionTests.test_keep_is_ephemeral_and_each_path_exposes_only_one_suggestion`, `test_accepting_one_suggestion_requires_fresh_comparison_before_another` y `test_divergence_marker_suppresses_only_one_valid_local_unit`.
- [ ] Agregar `test_invalid_or_protected_divergence_marker_reports_ambiguity_without_suppression` y `test_english_choice_never_accepts_or_mutates_the_spanish_peer`.
- [ ] Exigir solo `accept`, `keep` y `declare-intentional-divergence`. `keep` no retiene estado. Crear la declaracion es otra sugerencia individual. Declaraciones desconocidas, duplicadas, mal ubicadas, obsoletas, de otro idioma o no suprimibles no autorizan nada.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py HumanOwnedSuggestionTests.test_keep_is_ephemeral_and_each_path_exposes_only_one_suggestion HumanOwnedSuggestionTests.test_accepting_one_suggestion_requires_fresh_comparison_before_another HumanOwnedSuggestionTests.test_divergence_marker_suppresses_only_one_valid_local_unit HumanOwnedSuggestionTests.test_invalid_or_protected_divergence_marker_reports_ambiguity_without_suppression HumanOwnedSuggestionTests.test_english_choice_never_accepts_or_mutates_the_spanish_peer -v`. RED esperado: faltan opciones, secuencia y divergencia.

## Tarea 3: RED enfocado para la autoridad del plan existente

- [ ] Agregar `HumanOwnedSuggestionTests.test_accept_places_one_human_integration_action_in_the_existing_plan`, `test_changed_source_invalidates_selection_before_plan_construction`, `test_changed_rendered_proposal_invalidates_selection_before_plan_construction` y `test_mixed_plan_failure_rolls_back_human_and_existing_actions`.
- [ ] Ligar una acción aceptada a ruta, idioma, unidad, digest fuente, digest destino, bytes exactos, tipo de acción, ID, `suggestionEvidenceSha256` suministrado y digest del plan. Antes de construir el plan, releer la fuente, volver a renderizar los bytes propuestos exactos, recalcular el digest de evidencia y rechazar diferencias; conservar la comprobación existente previa a mutar.
- [ ] Probar que no entran dos sugerencias, no se reutiliza aprobacion y ningun preview, ambiguedad, opcion invalida, validacion fallida o evidencia obsoleta cambia bytes humanos.
- [ ] Hacer que la cobertura de rollback construya una transacción mixta con la acción humana aceptada más acciones existentes de creación, append y reemplazo; forzar el fallo de una acción posterior; y probar restauración byte por byte de cada destino preexistente y eliminación de cada destino recién creado en toda la transacción.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py HumanOwnedSuggestionTests.test_accept_places_one_human_integration_action_in_the_existing_plan HumanOwnedSuggestionTests.test_changed_source_invalidates_selection_before_plan_construction HumanOwnedSuggestionTests.test_changed_rendered_proposal_invalidates_selection_before_plan_construction HumanOwnedSuggestionTests.test_mixed_plan_failure_rolls_back_human_and_existing_actions -v`. RED esperado: la evidencia de selección no se comprueba antes de `plan_changes()` y el rollback no cubre una transacción mixta humana/create/append/replace.

## Tarea 4: GREEN minimo, proyecciones y mantenimiento

- [ ] Implementar helpers deterministas junto a la clasificacion humana y extender solo el preview/reporte `render`. Conservar estados de `check()` e informes de generado, directivas, idioma y legacy.
- [ ] Extender `plan_changes()`, `plan_digest()`, `validate_write_preconditions()`, `write_outputs()` y `verify_outputs()` con exactamente una accion humana aceptada. Reutilizar contencion, aprobacion, digest, rollback y validacion. Sin escritura directa desde el reporte.
- [ ] Repetir Tareas 1-3 y ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py HumanOwnedSuggestionTests CheckTests AdditiveWriteTests ApprovalUxTests -v`. GREEN esperado: todo pasa.
- [ ] Inspeccionar y revisar primero los cambios canónicos en `scripts/harness_tool.py` y `tests/test_harness_tool.py`. Solo después de aceptar el comportamiento canónico, sincronizarlos hacia `.claude/`, `.opencode/` y `.pi/` y verificar esas copias mecánicamente por SHA-256/paridad, en vez de revisar la lógica duplicada de forma independiente. Luego usar preview de mantenimiento, apply ligado a digest autorizado por separado y segundo preview vacío. Este plan no autoriza apply, Git ni entrega.
- [ ] Tras implementacion verificada y autoridad separada, reconciliar plan, disenos, mapas, roadmap, estado, historiales y Engram `work/RH-W-0044`.

## Rollback

Retirar solo helpers RH-W-0044, entradas/campos de `render`, acción humana, pruebas, proyecciones y declaraciones de ciclo. Conservar reemplazo generado, directivas, localización, RH-W-0045, RH-W-0022, RH-W-0066, contexto de autoridad, digests y rollback existentes. Un apply mixto fallido restaura byte por byte cada archivo humano, agregado y reemplazado preexistente, y elimina cada archivo creado por esa transacción.

## Verificacion final

- [ ] Ejecutar los tres comandos enfocados en la ruta canónica de pruebas. Esperado: todo pasa; inspeccionar primero script/pruebas canónicos y luego verificar mecánicamente por SHA-256/paridad las copias sincronizadas de script/pruebas de runtime.
- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: salida 0.
- [ ] Verificar paridad SHA-256 de script y pruebas entre las cuatro raices. Esperado: sin diferencias.
- [ ] Ejecutar preview de mantenimiento; tras autoridad separada, apply por digest y segundo preview. Esperado: solo proyecciones previstas, `VALIDATED` y luego cero acciones mutantes.
- [ ] Ejecutar `python .repo-harness/check.py`. Esperado: `current`, salida 0, cero escrituras.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Esperado: cuatro pases.
- [ ] Ejecutar comprobaciones bilingues y de enlaces relativos, luego `git diff --check`. Esperado: sin hallazgos.

## Pronostico de revision

Cambio autoral de producto/pruebas: 260-380 lineas. Proyecciones mecanicas: 780-1,140. Riesgo alto para autoridad sobre bytes humanos y medio para exactitud. Revisar en tres unidades: comparacion/reporte, opciones/divergencia y accion ligada a contenido mas proyecciones/ciclo. Dividir antes de revision si una porcion autoral se acerca a 400 lineas.

## Aceptacion y decisiones restantes

- [ ] Sugerencias deterministas, localizadas, acotadas y solo de preview hasta aceptar una.
- [ ] Las tres opciones conservan autoridad una por una y evidencia fresca.
- [ ] Digest, contencion, aprobacion, rollback y verificacion existentes gobiernan la unica escritura.
- [ ] Sin aceptacion masiva, equivalencia semantica, persistencia, servicio, dependencia, comando nuevo ni propietario nuevo.

Decisión restante: se aceptó el plan corregido y el propietario autorizó después solo cuatro porciones parciales de vista previa. Completar R44-1 e implementar R44-2/R44-3 requieren autorización separada del propietario; sigue pendiente el informe final de toda la funcionalidad R44-4. Apply y la entrega del producto siguen siendo decisiones separadas. No queda ninguna decisión técnica de planificación abierta.
