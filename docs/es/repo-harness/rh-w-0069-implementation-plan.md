# Plan de implementacion RH-W-0069 de continuidad nativa de sesion

> **Para agentes ejecutores:** SUB-SKILLS OBLIGATORIAS: use `test-driven-development` para cada cambio, `work-unit-commits` para los limites planeados y `verification-before-completion` antes de entregar. Ejecute las tareas en orden y solo despues de probar RH-W-0051. Este plan no autoriza implementacion, transicion de ciclo, accion Git, publicacion ni red.

**Objetivo:** Extender los contratos generados de sesion para que el agente ejecutor escriba y lea backups Markdown nativos con stop local primero, start con repositorio primero, resultados Engram independientes, reconciliacion visible y privacidad acotada.

**Arquitectura:** `session_start_skill()` (definido actualmente cerca de la linea 1134) y `session_stop_skill()` (cerca de la linea 1266) renderizan contratos de instrucciones; no llaman APIs Python de continuidad durante runtime. Agregar solo texto conciso que ordene al agente usar operaciones existentes de archivos relativas al repositorio y protegidas. El renderer de la regla de memoria posee schema, autoridad, privacidad y vocabulario compartidos.

**Stack:** Python 3.10+ estandar, `unittest`, Markdown generado determinista, checks existentes de renderer y bytes, y proyecciones canonica, OpenCode y Claude identicas.

**Propietario de ciclo:** [RH-W-0069](roadmap.md#rh-w-0069), estado `proposed`. El [diseno](rh-w-0069-design.md) aprobado posee el comportamiento. Prerrequisito: RH-W-0051 implementado y probado. Planificar no cambia estado ni autoriza implementar.

## Call paths probados y mapa

| Ruta o simbolo | Responsabilidad y call path |
|---|---|
| `render()` -> `render_with_skill_mapping()` -> `session_start_skill()` | Renderiza instrucciones de start con repositorio primero. |
| `render()` -> `render_with_skill_mapping()` -> `session_stop_skill()` | Renderiza instrucciones de pause/close local primero. |
| Renderer existente de regla de memoria | Renderiza el unico schema, autoridad, privacidad y resultados compartidos. |
| `GeneratedCoreTests` | Afirma contratos integrated/standalone, exclusiones y no duplicacion. |
| Skills generadas y regla de memoria | Ordenan lecturas acotadas y writes exclusivos en `.repo-harness/memory-bk/`; no crean subsistema Python. |
| Proyecciones y archivos autoalojados | Sincronizacion mecanica exacta despues de GREEN canonico. |

El plan no agrega `render_continuity_backup`, `filter_continuity_summary`, `write_continuity_backup`, `read_continuity`, `reconcile_continuity` ni `persist_continuity`. Tampoco agrega callbacks, API generica, base de datos, cifrado, daemon, scheduler, servicio, motor de sincronizacion, storage pluggable, state machine, limpieza, reutilizacion de aprobacion, checkpoint, copia de ciclo, recibo, store de entrega ni store de conflictos.

## Tarea 1: gate predecesor y RED/GREEN de session-stop

- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy LocalStateTaxonomyTests.test_render_adds_exact_root_local_state_ignore_directives LocalStateTaxonomyTests.test_check_accepts_absent_local_state_directories_with_zero_writes LocalStateTaxonomyTests.test_check_reports_local_state_manifest_or_ignore_drift ApprovedGeneratedOutputReplacementTests.test_manifest_and_checker_change_as_one_consistent_replacement_pair -v`. Esperado: cinco pruebas pasan. Luego ejecutar `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }; $manifest = Get-Content -LiteralPath ".repo-harness/manifest.json" -Raw | ConvertFrom-Json; if ($manifest.localState.'memory-bk'.path -ne ".repo-harness/memory-bk/") { throw "manifest must reserve memory-bk" }`. Esperado: sin excepcion. Detener si cualquiera falla.
- [ ] Agregar `GeneratedCoreTests.test_session_stop_requires_native_local_first_pause_and_close_backup` y `test_session_stop_reports_local_engram_and_cross_check_independently`.
- [ ] Exigir: refrescar evidencia; construir el resumen estructurado requerido; aplicar filtro compartido; resolver `.repo-harness/memory-bk/` desde la raiz seleccionada con proteccion existente; crear solo ese directorio de forma diferida; crear exclusivamente `YYYYMMDDTHHMMSSZ-<pause|close>-<short-session-id>.md`; reabrir y validar; luego intentar Engram en integrated; cross-check solo si ambos writes pasan.
- [ ] Exigir resultados exactos: fallo local `failed`; local exitoso con Engram ausente o fallido `degraded`; ambos exitosos y de acuerdo `succeeded`; conflicto `degraded`; cada fuente visible.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_stop_requires_native_local_first_pause_and_close_backup GeneratedCoreTests.test_session_stop_reports_local_engram_and_cross_check_independently -v`.
- [ ] RED esperado: faltan clausulas local-first, creacion exclusiva, readback y resultados independientes; imports y renderer deben funcionar.
- [ ] Extender minimamente `session_stop_skill()` con instrucciones directas y referencia a memoria. No agregar helper callable, callback ni protocolo de retornos.
- [ ] Repetir comando. GREEN esperado: dos pruebas pasan.
- [ ] Commit futuro: `feat(harness): require local-first continuity on session stop`. Rollback: primero revertir renderer y pruebas canonicos, luego usar maintenance de Tarea 4 para regenerar bytes anteriores. Backups y Engram permanecen intactos.

## Tarea 2: RED/GREEN de session-start y reconciliacion

- [ ] Agregar `test_session_start_requires_repository_first_bounded_continuity_read`, `test_session_start_validates_newest_backup_before_use` y `test_session_start_reports_conflicts_without_merging`.
- [ ] Exigir secuencia: validar raiz; leer propietarios, fuente, pruebas, Git y checker aplicables; leer Engram con scope en integrated; inspeccionar solo `.md` regulares directos; aceptar el mas nuevo solo con acuerdo de nombre y frontmatter; leer maximo uno anterior por `newest_invalid`, `objective_names_backup`, `conflict_needs_chronology` o `newest_lacks_relevant_continuity`.
- [ ] Exigir `not_found` para directorio o historial ausente; candidatos malformados, inseguros, indirectos o contradictorios se rechazan y reportan sin reparar. Start no escribe, prepara, continua automaticamente ni sigue links.
- [ ] Exigir `accepted`, `stale`, `conflicting`, `unavailable` y `unverifiable`; conflictos resueltos usan repositorio visiblemente y los materiales no resueltos producen una pregunta humana, sin merge.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_start_requires_repository_first_bounded_continuity_read GeneratedCoreTests.test_session_start_validates_newest_backup_before_use GeneratedCoreTests.test_session_start_reports_conflicts_without_merging -v`.
- [ ] RED esperado: faltan orden, historia acotada, validacion y no-merge.
- [ ] Extender minimamente `session_start_skill()` con instrucciones directas. No agregar reader, reconciler, fixture de callbacks ni subsistema generico.
- [ ] Repetir comando. GREEN esperado: tres pruebas pasan.
- [ ] Commit futuro: `feat(harness): require bounded continuity on session start`. Rollback: primero revertir canon y luego usar maintenance de Tarea 4.

## Tarea 3: RED/GREEN de schema, privacidad y exclusiones compartidos

- [ ] Agregar `test_memory_rule_declares_native_backup_schema_authority_and_privacy`, `test_integrated_and_standalone_continuity_keep_source_outcomes_independent` y `test_generated_continuity_excludes_runtime_memory_and_stateful_overreach`.
- [ ] Exigir frontmatter `project`, `timestamp_utc`, `source`, `session_id`; `# Session Continuity`; secciones Goal, Instructions, Discoveries, Accomplished, Next Steps y Relevant Files; `None.` para vacias; rutas relativas; clasificacion de evidencia aplicable.
- [ ] Exigir omision de credenciales, valores de entorno, payloads, rutas externas absolutas, fuente copiada, diffs, logs, stacks, bytes generados, salida de pruebas, datos sensibles, aprobaciones, checkpoints, ciclo, recibos, entrega y conflictos. Reportar reduccion sin copiar valores.
- [ ] Exigir autoridad: fuente/pruebas, roadmap, current state, decisions, changelog, Git/checker, luego Engram/local no autoritativos. Memoria incorporada se ignora. Integrated conserva Engram independiente; standalone usa `not_applicable` y no lo emula.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_memory_rule_declares_native_backup_schema_authority_and_privacy GeneratedCoreTests.test_integrated_and_standalone_continuity_keep_source_outcomes_independent GeneratedCoreTests.test_generated_continuity_excludes_runtime_memory_and_stateful_overreach -v`.
- [ ] RED esperado: falta schema, privacidad, autoridad o exclusion; controles existentes pueden pasar.
- [ ] Extender solo renderer de memoria y referencias concisas. No duplicar pseudocodigo ni agregar parser o deteccion generica.
- [ ] Repetir el comando de pruebas seleccionadas y luego ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. GREEN esperado: pasan las pruebas seleccionadas y la clase `GeneratedCoreTests` completa.
- [ ] Commit futuro: `feat(harness): define native continuity contract`. Rollback: primero revertir canon y luego usar maintenance de Tarea 4.

## Tarea 4: GREEN canonico, proyecciones y apply de maintenance

- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: exit 0 antes de sincronizar.
- [ ] Copiar solo script y prueba canonicos a OpenCode y Claude. Luego ejecutar `python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Desde la raiz, ejecutar este preview PowerShell exacto con evidencia verificada del host, no contenido de memoria:

```powershell
$root = (Resolve-Path -LiteralPath ".").Path
if ([string]::IsNullOrWhiteSpace($env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON)) { throw "REPO_HARNESS_ENGRAM_EVIDENCE_JSON must contain host-verified Engram capability evidence" }
$engramEvidence = $env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON
$engramCapability = $engramEvidence | ConvertFrom-Json
if ($engramCapability.surface -notin @("mcp", "cli") -or $engramCapability.status -ne "verified" -or [string]::IsNullOrWhiteSpace([string]$engramCapability.evidence)) { throw "Engram evidence must have surface mcp|cli, status verified, and non-empty evidence" }
$previewJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $engramEvidence
if ($LASTEXITCODE -ne 0) { throw "maintenance preview failed" }
$preview = $previewJson | ConvertFrom-Json
$approvedPlanSha256 = [string]$preview.planSha256
if ($approvedPlanSha256 -notmatch '^[0-9a-f]{64}$') { throw "preview did not return a valid planSha256" }
```

El ejemplo de schema seguro para pruebas es `{"surface":"mcp","status":"verified","evidence":"test fixture capability"}`. Solo sirve para pruebas de parser o fixture y nunca se usa en preview o apply de produccion como prueba de capacidad viva.

- [ ] Revisar preview. Esperado: solo reemplazos de session/memory y proyecciones; sin campo RH-W-0069 en manifest/checker, directorio, backup, migracion ni borrado.
- [ ] Sin cambiar archivos ni argumentos, ejecutar:

```powershell
$applyJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $engramEvidence --apply --approved-plan-sha256 $approvedPlanSha256
if ($LASTEXITCODE -ne 0) { throw "maintenance apply failed" }
$apply = $applyJson | ConvertFrom-Json
if ($apply.status -ne "VALIDATED") { throw "maintenance apply was not VALIDATED" }
```

- [ ] Repetir el mismo preview y analizarlo con `ConvertFrom-Json`. Esperado: todas las listas de acciones vacias. Luego ejecutar `python -c "from pathlib import Path; import importlib.util; p=Path('skill/repo-harness-builder/scripts/harness_tool.py'); s=importlib.util.spec_from_file_location('harness_tool',p); m=importlib.util.module_from_spec(s); s.loader.exec_module(m); out=m.render('repo-harness-builder',['claude-code','opencode','pi'],license_id='MIT',report_language='english',documentation_languages=['en','es'],conversation_language='english'); paths=['.repo-harness/skills/repo-harness-session-start/SKILL.md','.repo-harness/skills/repo-harness-session-stop/SKILL.md','.repo-harness/rules/memory.md','.opencode/skills/repo-harness-session-start/SKILL.md','.opencode/skills/repo-harness-session-stop/SKILL.md','.claude/skills/repo-harness-session-start/SKILL.md','.claude/skills/repo-harness-session-stop/SKILL.md']; bad=[x for x in paths if Path(x).read_text(encoding='utf-8') != out[x]]; assert not bad,bad"`. Esperado: exit 0 sin salida. Finalmente ejecutar `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }`. Esperado: sin excepcion.
- [ ] Commit mecanico futuro: `chore(harness): synchronize native continuity contracts`. Separado de RH-W-0051. Rollback: primero revertir cambios canonicos, luego usar esta ruta para regenerar bytes anteriores. Nunca borrar backups ni Engram.

## Tarea 5: documentacion, verificacion, ciclo y entrega

- [ ] Actualizar uso, memoria y pares bilingues solo donde instrucciones visibles implementadas lo exijan. Un propietario por hecho; no copiar schema a docs ajenos.
- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`, `python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v` y `python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: cada comando sale con 0 y reporta el mismo total; skips de runtime pueden diferir.
- [ ] Ejecutar `python -B skill/repo-harness-builder/scripts/harness_tool.py self-test`; luego ejecutar los comandos exactos de paridad, bytes generados y checker de la Tarea 4. Esperado: self-test imprime `pass`; paridad y bytes salen con 0 sin salida; checker queda `current` con cero escrituras.
- [ ] Tratar los comandos exactos anteriores de suites, checker, paridad y bytes como evidencia requerida para seguridad de rutas, no-write, reemplazo y preservacion. No agregar matriz negativa duplicada salvo que uno de esos comandos exponga un requisito sin prueba.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_documented_suite_count_matches_the_real_suite GeneratedCoreTests.test_checked_in_documents_do_not_mix_line_endings -v`. Esperado: cuatro pruebas pasan.
- [ ] Ejecutar `python -c "from pathlib import Path; import re; roots=[Path('README.md'),Path('README.es.md'),Path('REPO-HARNESS.md'),Path('REPO-HARNESS.es.md'),Path('CONTRIBUTING.md'),Path('docs')]; files=[p for r in roots for p in ([r] if r.is_file() else r.rglob('*.md'))]; bad=[]; pattern=re.compile(r'\[[^\]]+\]\(([^)]+)\)'); [(bad.append(f'{p}:{target}') if not (target.startswith(('#','http://','https://','mailto:')) or (p.parent/target.split('#',1)[0]).resolve().exists()) else None) for p in files for target in pattern.findall(p.read_text(encoding='utf-8'))]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Ejecutar `python -c "from pathlib import Path; files=[Path(p) for p in ['docs/repo-harness/rh-w-0051-implementation-plan.md','docs/es/repo-harness/rh-w-0051-implementation-plan.md','docs/repo-harness/rh-w-0069-implementation-plan.md','docs/es/repo-harness/rh-w-0069-implementation-plan.md']]; bad=[str(p) for p in files if any(c in p.read_text(encoding='utf-8') for c in (chr(0x2013),chr(0x2014)))]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Ejecutar `python -c "from pathlib import Path; names=('render_continuity_backup','filter_continuity_summary','write_continuity_backup','read_continuity','reconcile_continuity','persist_continuity','continuity_orchestrator','continuity_callback','continuity_database','continuity_daemon','continuity_conflict_store'); roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder'),Path('.repo-harness/skills'),Path('.repo-harness/rules')]; files=[p for r in roots for p in r.rglob('*') if p.is_file()]; bad=[f'{p}:{name}' for p in files for name in names if name in p.read_text(encoding='utf-8',errors='ignore')]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Ejecutar `git diff --check`; si `$LASTEXITCODE -ne 0`, detener. Luego ejecutar `git status --short`, `git diff --stat` y `git diff --name-only`. Revisar esas rutas exactas por callbacks, bases, servicios, estado oculto, migracion, borrado, memoria incorporada, merge silencioso, aprobaciones, checkpoints, receipts, stores de entrega o cambios ajenos. Esperado: sin errores de whitespace, implementacion prohibida ni cambios al trabajo previo.
- [ ] Solo con implementacion verificada y autoridad separada, reconciliar roadmap, current state, changelogs, pares, conteo y Engram `work/RH-W-0069`. Continuidad no cierra ciclo sola.
- [ ] Issue, commit, PR, push, publicacion o red futuros siguen `CONTRIBUTING.md` con aprobacion separada. RH-W-0069 conserva unidad propia.

## Aceptacion y pronostico

- [ ] RH-W-0051 se prueba primero.
- [ ] Stop exige write local exclusivo y readback antes de Engram independiente.
- [ ] Start exige repositorio primero, reads acotados validos y reconciliacion visible sin merge.
- [ ] Una regla compartida posee schema, privacidad, autoridad, resultados y exclusiones.
- [ ] No se agrega API Python, callbacks, subsistema, campo manifest, inspeccion checker, migracion ni borrado.
- [ ] Canon, proyecciones, ciclo, rollback y entrega quedan separados de RH-W-0051.

Pronostico: 5 tareas, 280 a 380 lineas autorales cambiadas mas proyecciones mecanicas y bytes autoalojados. El rango cubre texto de renderer, pruebas enfocadas y documentacion visible requerida, sin subsistema runtime. Implementacion y avance de ciclo siguen sin autorizacion.
