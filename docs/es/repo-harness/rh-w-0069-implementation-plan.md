# Plan de implementacion RH-W-0069 de continuidad nativa de sesion

> **Estado: entregado.** El propietario autorizo la implementacion directa acotada el 2026-09-07; no autorizo ni se realizo entrega Git, publicacion o acceso de red.

La implementacion siguio los tres grupos RED/GREEN enfocados, sincronizo proyecciones canonicas y de runtime, y reconcilio los propietarios bilingues del ciclo despues de verificar. Los recibos de comandos permanecen en el informe de la sesion de implementacion; este plan conserva el contrato historico de ejecucion.

## Reconciliacion de preparacion para ejecutar

El diseno esta aprobado y RH-W-0051 ya entrego soporte de almacenamiento. En la conversacion del 2026-09-06, el propietario aprobo explicitamente este plan simplificado reconciliado con `dale`, despues de la recomendacion del asistente. Esa aprobacion fue evidencia documental solo para el limite del plan. El propietario autorizo despues la implementacion por separado el 2026-09-07; RH-W-0069 ahora esta completado y entregado segun la convencion de ciclo del roadmap.

Durante la ejecucion, los comandos siguientes usaron los valores instalados autenticados: runtimes `claude-code`, `opencode` y `pi`; licencia `MIT`; idioma del informe `english`; locales documentales `en` y `es`; idioma de conversacion `espanol`; y activacion de mantenimiento. La instruccion de volver a leer el manifest se conserva como guia historica de ejecucion. Un ejemplo nunca debe cambiar preferencias instaladas.

La carga de verificacion es proporcional y ya no constituye un bloqueo separado de aprobacion. La Tarea 1 es una sola unidad TDD canonica con tres grupos de comportamiento RED/GREEN ordenados. La Tarea 2 ejecuta una vez la suite canonica completa y luego prueba las copias mediante paridad SHA exacta y las salidas generadas mediante render/checker. La Tarea 3 posee las comprobaciones enfocadas de documentacion y cierre de ciclo. Solo se repite una suite proyectada si falla la paridad, difiere fuente especifica de runtime o una comprobacion enfocada expone un requisito sin probar.

**Objetivo:** Extender los contratos generados de sesion para que el agente ejecutor escriba y lea backups Markdown nativos con stop local primero, start con repositorio primero, resultados Engram independientes, reconciliacion visible y privacidad acotada.

**Arquitectura:** `render()` llama `render_with_skill_mapping()`, que renderiza `session_start_skill()`, `session_stop_skill()` y la regla compartida de memoria desde `rule_outputs()`. Son contratos de instrucciones, no llamadas runtime a APIs Python. Agregar solo texto conciso que ordene usar operaciones protegidas y relativas al repositorio. La regla de memoria posee schema, autoridad, privacidad y vocabulario compartidos.

**Stack:** Python 3.10+ estandar, `unittest`, Markdown generado determinista, checks existentes de renderer y bytes, y proyecciones canonica, OpenCode y Claude identicas.

**Propietario de ciclo:** [RH-W-0069](roadmap.md#rh-w-0069), estado `completed`. El [diseno](rh-w-0069-design.md) aprobado posee el comportamiento y este plan aprobado por el propietario se conserva como evidencia historica de ejecucion. Prerrequisito: RH-W-0051 implementado y probado. Aprobar el plan no concedio autoridad para implementar; esa autorizacion separada se otorgo el 2026-09-07.

## Call paths probados y mapa

| Ruta o simbolo | Responsabilidad ejecutada y call path |
|---|---|
| `render()` -> `render_with_skill_mapping()` -> `session_start_skill()` | Renderiza instrucciones de start con repositorio primero. |
| `render()` -> `render_with_skill_mapping()` -> `session_stop_skill()` | Renderiza instrucciones de pause/close local primero. |
| Regla de memoria en `rule_outputs()` | Renderiza el unico schema, autoridad, privacidad y resultados compartidos. |
| `GeneratedCoreTests` | Afirma contratos integrated/standalone, exclusiones y no duplicacion. |
| Skills generadas y regla de memoria | Ordenan lecturas acotadas y writes exclusivos en `.repo-harness/memory-bk/`; no crean subsistema Python. |
| Proyecciones y archivos autoalojados | Sincronizacion mecanica exacta despues de GREEN canonico. |

El plan no agrego `render_continuity_backup`, `filter_continuity_summary`, `write_continuity_backup`, `read_continuity`, `reconcile_continuity` ni `persist_continuity`. Tampoco agrego callbacks, API generica, base de datos, cifrado, daemon, scheduler, servicio, motor de sincronizacion, storage pluggable, state machine, limpieza, reutilizacion de aprobacion, checkpoint, copia de ciclo, recibo, store de entrega ni store de conflictos.

Todos los elementos de las listas de las Tareas 1 a 3 y de la seccion de aceptacion se ejecutaron y cumplieron. Sus casillas originales sin marcar se conservan solo como instrucciones historicas de ejecucion, no como trabajo pendiente.

## Tarea 1: TDD canonico de continuidad nativa

- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy LocalStateTaxonomyTests.test_render_adds_exact_root_local_state_ignore_directives LocalStateTaxonomyTests.test_check_accepts_absent_local_state_directories_with_zero_writes LocalStateTaxonomyTests.test_check_reports_local_state_manifest_or_ignore_drift ApprovedGeneratedOutputReplacementTests.test_manifest_and_checker_change_as_one_consistent_replacement_pair -v`. Esperado: cinco pruebas pasan. Luego ejecutar `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }; $manifest = Get-Content -LiteralPath ".repo-harness/manifest.json" -Raw | ConvertFrom-Json; if ($manifest.localState.'memory-bk'.path -ne ".repo-harness/memory-bk/") { throw "manifest must reserve memory-bk" }`. Esperado: sin excepcion. Detener si cualquiera falla.
### Grupo RED/GREEN 1: backup de session-stop

- [ ] Agregar `GeneratedCoreTests.test_session_stop_requires_native_local_first_pause_and_close_backup` y `test_session_stop_reports_local_engram_and_cross_check_independently`.
- [ ] Exigir siempre el backup Markdown local: refrescar evidencia; construir el resumen estructurado; aplicar filtro compartido; resolver `.repo-harness/memory-bk/` desde la raiz seleccionada; crear solo ese directorio de forma diferida; crear exclusivamente `YYYYMMDDTHHMMSSZ-<pause|close>-<short-session-id>.md`; reabrir y validar; luego intentar Engram cuando este disponible; cross-check solo si ambos writes pasan.
- [ ] Exigir resultados exactos: fallo local `failed`; local exitoso con Engram ausente o fallido `degraded`; ambos exitosos y de acuerdo `succeeded`; conflicto `degraded`; cada fuente visible.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_stop_requires_native_local_first_pause_and_close_backup GeneratedCoreTests.test_session_stop_reports_local_engram_and_cross_check_independently -v`.
- [ ] RED esperado: faltan clausulas local-first, creacion exclusiva, readback y resultados independientes; imports y renderer deben funcionar.
- [ ] Extender minimamente `session_stop_skill()` con instrucciones directas y referencia a memoria. No agregar helper callable, callback ni protocolo de retornos.
- [ ] Repetir comando. GREEN esperado: dos pruebas pasan.
### Grupo RED/GREEN 2: recuperacion de session-start

- [ ] Agregar `test_session_start_requires_repository_first_bounded_continuity_read`, `test_session_start_validates_newest_backup_before_use` y `test_session_start_reports_conflicts_without_merging`.
- [ ] Exigir secuencia: validar raiz; leer propietarios, fuente, pruebas, Git y checker aplicables; inspeccionar solo `.md` regulares directos y aceptar el archivo valido mas nuevo solo con acuerdo de nombre y frontmatter; despues leer Engram con scope en integrated; leer maximo uno anterior por `newest_invalid`, `objective_names_backup`, `conflict_needs_chronology` o `newest_lacks_relevant_continuity`.
- [ ] Exigir `not_found` para directorio o historial ausente; candidatos malformados, inseguros, indirectos o contradictorios se rechazan y reportan sin reparar. Start no escribe, prepara, continua automaticamente ni sigue links.
- [ ] Exigir `accepted`, `stale`, `conflicting`, `unavailable` y `unverifiable`; conflictos resueltos usan repositorio visiblemente y los materiales no resueltos producen una pregunta humana, sin merge.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_start_requires_repository_first_bounded_continuity_read GeneratedCoreTests.test_session_start_validates_newest_backup_before_use GeneratedCoreTests.test_session_start_reports_conflicts_without_merging -v`.
- [ ] RED esperado: faltan orden, historia acotada, validacion y no-merge.
- [ ] Extender minimamente `session_start_skill()` con instrucciones directas. No agregar reader, reconciler, fixture de callbacks ni subsistema generico.
- [ ] Repetir comando. GREEN esperado: tres pruebas pasan.
### Grupo RED/GREEN 3: regla compartida de memoria

- [ ] Agregar `test_memory_rule_declares_native_backup_schema_authority_and_privacy`, `test_memory_rule_omits_external_private_repository_names_and_locations_without_approved_aliases`, `test_integrated_and_standalone_continuity_keep_source_outcomes_independent` y `test_generated_continuity_excludes_runtime_memory_and_stateful_overreach`.
- [ ] Exigir frontmatter `project`, `timestamp_utc`, `source`, `session_id`; `# Session Continuity`; secciones Goal, Instructions, Discoveries, Accomplished, Next Steps y Relevant Files; `None.` para vacias; rutas relativas; clasificacion de evidencia aplicable.
- [ ] Exigir omision de credenciales, valores de entorno, payloads, rutas externas absolutas, fuente copiada, diffs, logs, stacks, bytes generados, salida de pruebas, datos sensibles, aprobaciones, checkpoints, ciclo, recibos, entrega y conflictos. Reportar reduccion sin copiar valores.
- [ ] Exigir omision de nombres y ubicaciones de repositorios privados externos salvo que un propietario aplicable registre un alias estable aprobado. Puede conservarse el alias, pero no el nombre privado, ubicacion absoluta, URL remota ni ruta hermana.
- [ ] Exigir autoridad: fuente/pruebas, roadmap, current state, decisions, changelog, Git/checker, luego Engram/local no autoritativos. Memoria incorporada se ignora. El backup local es obligatorio en todo modo; Engram es un destino adicional independiente solo cuando evidencia de capacidad lo deja disponible y nunca se emula.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_memory_rule_declares_native_backup_schema_authority_and_privacy GeneratedCoreTests.test_memory_rule_omits_external_private_repository_names_and_locations_without_approved_aliases GeneratedCoreTests.test_integrated_and_standalone_continuity_keep_source_outcomes_independent GeneratedCoreTests.test_generated_continuity_excludes_runtime_memory_and_stateful_overreach -v`.
- [ ] RED esperado: falta schema, privacidad, autoridad o exclusion; controles existentes pueden pasar.
- [ ] Extender solo renderer de memoria y referencias concisas. No duplicar pseudocodigo ni agregar parser o deteccion generica.
- [ ] Repetir el comando de pruebas seleccionadas. GREEN esperado: pasan cuatro pruebas.
- [ ] Despues del GREEN de los tres grupos, ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. Esperado: pasa la clase completa.
- [ ] Limite funcional recomendado para un commit futuro despues de autorizacion separada: `feat(harness): add native session continuity contracts`. Es una recomendacion, no autoridad para preparar ni crear un commit. Rollback: primero revertir renderer y pruebas canonicos, luego usar maintenance de Tarea 2; backups y Engram permanecen intactos.

## Tarea 2: GREEN canonico, proyecciones y apply de maintenance

- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: exit 0 antes de sincronizar.
- [ ] Copiar solo script y prueba canonicos a OpenCode y Claude. Luego ejecutar `python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Desde la raiz, ejecutar este preview PowerShell exacto con evidencia verificada del host, no contenido de memoria:

```powershell
$root = (Resolve-Path -LiteralPath ".").Path
if ([string]::IsNullOrWhiteSpace($env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON)) { throw "REPO_HARNESS_ENGRAM_EVIDENCE_JSON must contain host-verified Engram capability evidence" }
$engramEvidence = $env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON
$engramCapability = $engramEvidence | ConvertFrom-Json
if ($engramCapability.surface -notin @("mcp", "cli") -or $engramCapability.status -ne "verified" -or [string]::IsNullOrWhiteSpace([string]$engramCapability.evidence)) { throw "Engram evidence must have surface mcp|cli, status verified, and non-empty evidence" }
$previewJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language espanol --activation maintenance --engram-evidence-json $engramEvidence
if ($LASTEXITCODE -ne 0) { throw "maintenance preview failed" }
$preview = $previewJson | ConvertFrom-Json
$approvedPlanSha256 = [string]$preview.planSha256
if ($approvedPlanSha256 -notmatch '^[0-9a-f]{64}$') { throw "preview did not return a valid planSha256" }
```

El ejemplo de schema seguro para pruebas es `{"surface":"mcp","status":"verified","evidence":"test fixture capability"}`. Solo sirve para pruebas de parser o fixture y nunca se usa en preview o apply de produccion como prueba de capacidad viva.

- [ ] Revisar preview. Esperado: solo reemplazos de session/memory y proyecciones; sin campo RH-W-0069 en manifest/checker, directorio, backup, migracion ni borrado.
- [ ] Sin cambiar archivos ni argumentos, ejecutar:

```powershell
$applyJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language espanol --activation maintenance --engram-evidence-json $engramEvidence --apply --approved-plan-sha256 $approvedPlanSha256
if ($LASTEXITCODE -ne 0) { throw "maintenance apply failed" }
$apply = $applyJson | ConvertFrom-Json
if ($apply.status -ne "VALIDATED") { throw "maintenance apply was not VALIDATED" }
```

- [ ] Repetir el mismo preview y analizarlo con `ConvertFrom-Json`. Esperado: todas las listas de acciones vacias. Luego ejecutar `python -c "from pathlib import Path; import importlib.util; p=Path('skill/repo-harness-builder/scripts/harness_tool.py'); s=importlib.util.spec_from_file_location('harness_tool',p); m=importlib.util.module_from_spec(s); s.loader.exec_module(m); out=m.render('repo-harness-builder',['claude-code','opencode','pi'],license_id='MIT',report_language='english',documentation_languages=['en','es'],conversation_language='espanol'); paths=['.repo-harness/skills/repo-harness-session-start/SKILL.md','.repo-harness/skills/repo-harness-session-stop/SKILL.md','.repo-harness/rules/memory.md','.opencode/skills/repo-harness-session-start/SKILL.md','.opencode/skills/repo-harness-session-stop/SKILL.md','.claude/skills/repo-harness-session-start/SKILL.md','.claude/skills/repo-harness-session-stop/SKILL.md']; bad=[x for x in paths if Path(x).read_text(encoding='utf-8') != out[x]]; assert not bad,bad"`. Esperado: exit 0 sin salida. Finalmente ejecutar `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }`. Esperado: sin excepcion.
- [ ] Limite mecanico recomendado para un commit futuro: `chore(harness): synchronize native continuity contracts`. Es una recomendacion, no autoridad para preparar ni crear un commit. Mantenerlo separado de RH-W-0051. Rollback: primero revertir cambios canonicos, luego usar esta ruta para regenerar bytes anteriores. Nunca borrar backups ni Engram.

## Tarea 3: documentacion, ciclo y entrega

- [ ] Documentacion condicional, solo si la conducta visible implementada lo exige: `docs/repo-harness/usage.md`, `docs/es/repo-harness/usage.md`, `.repo-harness/rules/memory.md`, `README.md`, `README.es.md`, `docs/repo-harness/README.md` y `docs/es/repo-harness/README.md`. Mantener un propietario por hecho y no copiar schema.
- [ ] Rutas obligatorias de cierre de ciclo, solo despues de implementacion verificada y autoridad separada: `docs/repo-harness/roadmap.md`, `docs/es/repo-harness/roadmap.md`, `docs/repo-harness/current-state.md`, `docs/es/repo-harness/current-state.md`, `CHANGELOG.md`, `CHANGELOG.es.md` y Engram `work/RH-W-0069`. Que la guia sea condicional no vuelve opcionales estas rutas tras completar con autorizacion.
- [ ] No repetir la suite canonica completa ya ejecutada en la Tarea 2 ni ejecutar por defecto suites proyectadas identicas byte a byte. La suite canonica, la paridad SHA exacta, la comparacion de bytes generados y el checker de la Tarea 2 son la evidencia de regresion salvo que un fallo identifique una comprobacion menor ausente.
- [ ] Ejecutar una vez `python -B skill/repo-harness-builder/scripts/harness_tool.py self-test`. Esperado: `pass`.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_documented_suite_count_matches_the_real_suite GeneratedCoreTests.test_checked_in_documents_do_not_mix_line_endings GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Esperado: pasan seis pruebas canonicas enfocadas de ciclo y enlaces. La paridad SHA exacta de la Tarea 2 cubre las copias proyectadas.
- [ ] Ejecutar `python -c "from pathlib import Path; owner_pairs=[('docs/repo-harness/roadmap.md','docs/es/repo-harness/roadmap.md'),('docs/repo-harness/current-state.md','docs/es/repo-harness/current-state.md'),('CHANGELOG.md','CHANGELOG.es.md')]; plan_pair=('docs/repo-harness/rh-w-0069-implementation-plan.md','docs/es/repo-harness/rh-w-0069-implementation-plan.md'); map_pair=('docs/repo-harness/README.md','docs/es/repo-harness/README.md'); paths=[p for pair in (*owner_pairs,plan_pair,map_pair) for p in pair]; missing=[p for p in paths if not Path(p).is_file()]; assert not missing,missing; bad=[(a,b) for a,b in owner_pairs if (('RH-W-0069' in Path(a).read_text(encoding='utf-8')) != ('RH-W-0069' in Path(b).read_text(encoding='utf-8')))]; assert not bad,bad; assert all('RH-W-0069' in Path(p).read_text(encoding='utf-8') for p in plan_pair); assert all('rh-w-0069-implementation-plan.md' in Path(p).read_text(encoding='utf-8') for p in map_pair)"`. Esperado: exit 0 sin salida; es el selector exacto bilingue de ciclo, mapa y proyeccion del plan, incluida la proyeccion espanola.
- [ ] Ejecutar `git diff --check`; si `$LASTEXITCODE -ne 0`, detener. Luego ejecutar `git status --short`, `git diff --stat` y `git diff --name-only`. Revisar esas rutas exactas por callbacks, bases, servicios, estado oculto, migracion, borrado, memoria incorporada, merge silencioso, aprobaciones, checkpoints, receipts, stores de entrega o cambios ajenos. Esperado: sin errores de whitespace, implementacion prohibida ni cambios al trabajo previo.
- [ ] Solo con implementacion verificada y autoridad separada, reconciliar roadmap, current state, changelogs, pares, conteo y Engram `work/RH-W-0069`. Continuidad no cierra ciclo sola.
- [ ] Issue, commit, PR, push, publicacion o red futuros siguen `CONTRIBUTING.md` con aprobacion separada. RH-W-0069 conserva unidad propia.
- [ ] Limite documental de ciclo recomendado para un commit futuro: `docs(harness): close RH-W-0069 lifecycle`. Es solo una recomendacion posterior a implementacion verificada y autoridad de ciclo separada, no autoridad para preparar ni crear un commit ahora.

## Aceptacion historica y pronostico de carga de revision

- [ ] RH-W-0051 permanece probado como propietario completado del namespace; el plan fue aprobado por el propietario y la autorizacion separada de implementacion se otorgo el 2026-09-07.
- [ ] Stop exige write local exclusivo y readback antes de Engram independiente.
- [ ] Start exige repositorio primero, reads acotados validos y reconciliacion visible sin merge.
- [ ] Una regla compartida posee schema, privacidad, autoridad, resultados y exclusiones.
- [ ] No se agrega API Python, callbacks, subsistema, campo manifest, inspeccion checker, migracion ni borrado.
- [ ] Canon, proyecciones, ciclo, rollback y entrega quedan separados de RH-W-0051.

Pronostico historico: 3 limites coherentes: TDD funcional canonico, proyecciones mecanicas y cierre documental de ciclo. El volumen esperado era de 280 a 380 lineas autorales y 180 a 300 lineas mecanicas de proyecciones o salida generada. El riesgo se evaluo como medio por la interseccion entre inicio, cierre, privacidad, orden y resultados parciales, sin planear un subsistema runtime. No se esperaba superar el presupuesto autoral de 400 lineas. Los limites de commit siguen siendo recomendaciones; esta implementacion completada no autoriza entrega Git, staging, commit, push, publicacion ni acciones de red.
