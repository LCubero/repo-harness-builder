# Plan de implementación RH-W-0048 de recuperación de proyecciones de runtime

> **Estado: RH-W-0048 está `completed`.** El [registro de la hoja de ruta](roadmap.md#rh-w-0048) posee su resultado y evidencia de entrega; el plan siguiente se conserva como evidencia histórica de ejecución, no como instrucciones a seguir.

> **Estado histórico antes de completarse: propuesto (reabierto); plan aprobado por el propietario.** El propietario aprobó explícitamente este plan bilingüe, y su implementación fue autorizada por separado por el propietario, completada localmente y verificada. El único trabajo restante es reconciliar el contrato de mutabilidad y volver a verificar de forma independiente. La revisión nativa no está disponible porque falta el binario local del paquete, por lo que no existe linaje; eso no es un veredicto. La entrega—incluso entrega Git, red, commit, push, PR, release y publicación—permanece separada y sin realizar.

Derivado del [diseño](rh-w-0048-design.md) aprobado por el propietario. Par canónico: [rh-w-0048-implementation-plan.md](../../repo-harness/rh-w-0048-implementation-plan.md). El [registro del roadmap](../../repo-harness/roadmap.md#rh-w-0048) conserva la propiedad del ciclo de vida.

## Arquitectura exacta y mapa de archivos

| Propietario | Responsabilidad planificada |
| --- | --- |
| `skill/repo-harness-builder/scripts/harness_tool.py::check_script()` | Ampliar solo el checker instalado que emite. Reutilizar `parse_manifest()`, comprobaciones de ruta protegida, mapeos canónicos y `check_projections()`. Agregar aquí el parser generado de `repair`, preflight de solo lectura, digest determinista de preview, creación exclusiva, rollback y reporte post-check. No enrutar repair por el `main()` de la CLI del builder. |
| `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCheckScriptTests` | Poseer el comportamiento RED/GREEN aislado del checker instalado con raíces temporales, incluido un índice Git temporal para evidencia exacta de ruta rastreada. |
| `.repo-harness/check.py` | Checker autoalojado regenerado solo después de GREEN canónico y apply de mantenimiento autorizado por separado. |
| `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.pi/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` | Proyecciones mecánicas de los dos archivos canónicos; los tres paquetes de runtime conservan igualdad de bytes con sus pares canónicos. |

No son destinos de implementación el esquema del manifiesto, `canonicalProjections`, `.gitignore`, `.gitattributes`, la autoridad de update del renderer, la recarga de runtime ni documentación humana. `repair` se emite en el checker instalado porque el harness debe recuperarse sin una instalación externa del builder.

## Ejecución con TDD estricto (Evidencia histórica ejecutada)

Las casillas sin marcar preservan solo la notación del plan previa a la ejecución. El trabajo RED, GREEN, de sincronización y de verificación de abajo se ejecutó y verificó después; el roadmap conserva los recibos autoritativos y el registro de ciclo completado.

### RED A: check simple y clasificación dinámica

- [ ] Agregar `GeneratedCheckScriptTests.test_plain_check_remains_read_only_when_repair_is_available` y `test_repair_preview_derives_missing_local_targets_from_authenticated_manifest`.
- [ ] Afirmar que `python .repo-harness/check.py` simple conserva `writes: 0`, no activa repair implícito y mantiene la semántica ordinaria de deriva.
- [ ] Derivar destinos esperados desde `canonicalSkills`, `canonicalCommands` y `canonicalProjections` aplicable del manifiesto autenticado; calcular el conjunto en el fixture. Afirmar una clasificación de preview para cada destino derivado, con cada exclusión acompañada por una razón acotada; no afirmar las seis proyecciones faltantes históricas como conteo actual.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests.test_plain_check_remains_read_only_when_repair_is_available GeneratedCheckScriptTests.test_repair_preview_derives_missing_local_targets_from_authenticated_manifest -v`. RED esperado: solo fallos por contrato repair ausente.

### RED B: preservación y autoridad

- [ ] Agregar `test_repair_reports_missing_tracked_and_preserves_stale_targets` y `test_repair_apply_requires_current_preview_digest_and_rechecks_preflight`.
- [ ] En un repositorio Git temporal inicializado, agregar el destino exacto al índice, quitarlo del árbol y exigir `missing-tracked`, sin creación y con deriva preservada. En un fixture sin Git o sin rastrear, exigir elegibilidad `missing-local`.
- [ ] Afirmar que los casos obsoleto, desconocido, indirecto, inseguro, ilegible y canónico ausente no escriben; `stale` significa bytes existentes distintos y no se confunde con vocabulario del reporte repair como `skipped`, `refused`, `created`, `rollback` y `post-check`.
- [ ] Afirmar que `planSha256` vincula el digest autenticado del manifiesto junto con las rutas relativas ordenadas de creación y sus digest de contenido esperado. Por separado, afirmar que la respuesta de preview informa el digest del manifiesto autenticado, cada clasificación de destino, las exclusiones con razones acotadas y `writes: 0`, y que tiene éxito explícito cuando el conjunto elegible está vacío; apply acepta solo el digest sin cambios y repite preflight antes de escribir.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests.test_repair_reports_missing_tracked_and_preserves_stale_targets GeneratedCheckScriptTests.test_repair_apply_requires_current_preview_digest_and_rechecks_preflight -v`. RED esperado: solo fallos de clasificación y autoridad ausentes.

### RED C: transacción y resultado honesto

- [ ] Agregar `test_repair_creates_only_dynamic_missing_local_targets_and_post_checks` y `test_repair_rolls_back_only_unchanged_files_created_by_that_invocation`.
- [ ] Cubrir clon fresco y árbol parcial para cada familia de destino Claude Code, OpenCode y Pi seleccionada; los conteos esperados de creación se derivan del manifiesto.
- [ ] Forzar un fallo posterior de creación o verificación. Afirmar que rollback elimina solo archivos de la invocación con bytes aún iguales, puede quitar solo padres vacíos de la invocación y preserva bytes preexistentes o cambiados.
- [ ] Afirmar que obsoletos restantes, rastreados ausentes, fallo canónico, rollback parcial y carga de runtime sin verificar se informan por separado y nunca se resumen como recuperados.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests.test_repair_creates_only_dynamic_missing_local_targets_and_post_checks GeneratedCheckScriptTests.test_repair_rolls_back_only_unchanged_files_created_by_that_invocation -v`. RED esperado: solo fallos de transacción y resultado ausentes.

### GREEN: implementación mínima y sincronización

- [ ] Implementar solo el comportamiento del checker generado nombrado arriba. Conservar `check_projections()` como dueño del check simple; clasificar cada destino e informar exclusiones con razones acotadas antes de decidir repair; mantener preview de solo lectura (`writes: 0`) y con éxito explícito para un conjunto elegible vacío; y vincular mediante `planSha256` el digest autenticado del manifiesto junto con rutas relativas ordenadas de creación y digest de contenido esperado.
- [ ] Repetir cada comando RED y luego `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests -v`. GREEN esperado: salida 0.
- [ ] Ejecutar el preview de mantenimiento existente, revisar su digest y, solo con apply autorizado por separado, regenerar `.repo-harness/check.py` y los tres paquetes completos del builder. No editar proyecciones a mano.
- [ ] Verificar bytes idénticos de manifiesto y `.gitattributes`; la recuperación local no agrega regla de atributos. Un destino obsoleto recibe recomendación de update, no sobreescritura por repair.

## Verificación enfocada

- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests -v`, y repetirlo desde `.claude/skills/repo-harness-builder/tests/test_harness_tool.py`, `.opencode/skills/repo-harness-builder/tests/test_harness_tool.py` y `.pi/skills/repo-harness-builder/tests/test_harness_tool.py`. Esperado: resultados iguales y correctos.
- [ ] Ejecutar `python -B .repo-harness/check.py`. Esperado: `current`, salida 0 y `writes: 0` tras sincronizar proyecciones.
- [ ] Ejecutar `python -c "from pathlib import Path; import hashlib; rel=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.claude/skills/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.pi/skills/repo-harness-builder')]; bad=[p for p in rel if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Esperado: salida 0 y sin salida.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Esperado: cuatro pruebas correctas después del cierre documental/de ciclo autorizado por separado.
- [ ] Ejecutar `git diff --check`. Esperado: salida 0.

## Preservación, cierre y separación de entrega

Repair nunca sobrescribe, normaliza, prepara, hace commit, push, borra, inicializa Git, recarga un runtime ni accede a red. Su límite de rollback abarca solo archivos sin cambios y padres vacíos creados por esa invocación. Update conserva autoridad separada; RH-W-0052 conserva archivo, desinstalación, reinstalación y borrado.

La implementación autorizada por separado por el propietario está completada localmente y es evidencia histórica verificada. Este plan, ambos mapas, roadmap, current state, pares en español y cronología conservan esa evidencia mientras RH-W-0048 se reabre solo para reconciliar el contrato de mutabilidad y volver a verificar de forma independiente. La aprobación del plan y la autoridad de implementación permanecen separadas. La revisión nativa no está disponible porque falta el binario local del paquete, por lo que no existe linaje; eso no es un veredicto. Commit, push, PR, entrega, publicación, release y red permanecen independientes, sin autorización y sin realizar.

## Pronóstico de revisión y no objetivos

Pronóstico histórico (reemplazado): 220 a 360 líneas autorales entre script y pruebas canónicos, con un presupuesto de revisión autoral menor de 400 líneas. La rotación real de fuente/pruebas canónicas es de 648 adiciones y 7 eliminaciones, y reemplaza ese pronóstico y presupuesto. La verificación independiente no encontró hallazgos. La revisión nativa no estuvo disponible porque faltaba el binario local del paquete, por lo que no se creó linaje; no se afirma revisión nativa. Sincronización mecánica: siete archivos generados. El riesgo sigue medio-alto porque el checker instalado muta solo bajo autoridad repair explícita; los cortes históricos son RED A/B, RED C más GREEN, proyecciones y cierre de ciclo autorizado por el propietario.

No objetivos: segundo manifiesto o inventario, modo tracked/local en manifiesto, almacén de plantillas copiadas, caché, ledger, base de datos, servicio, daemon, scheduler, API de runtime, migración, reinstalación, archivo, desinstalación, borrado, rediseño de `.gitattributes`, dependencia externa del builder ni implementación de producto fuera del contrato de recuperación aprobado.

Decisión resuelta del propietario: aprobó este plan; la implementación fue autorizada por separado, completada localmente y verificada. RH-W-0048 está `proposed`, reabierta solo para reconciliar el contrato de mutabilidad y volver a verificar de forma independiente. La verificación independiente histórica no encontró hallazgos. La revisión nativa no está disponible porque falta el binario local del paquete, por lo que no existe linaje; eso no es un veredicto. La entrega no se afirma ni realizó.
