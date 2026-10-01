# Plan de implementacion RH-W-0051 de taxonomia de estado local

> **Estado: RH-W-0051 está `completed`.** El [registro de la hoja de ruta](roadmap.md#rh-w-0051) posee su resultado y evidencia de entrega; el plan siguiente se conserva como evidencia histórica de ejecución, no como instrucciones a seguir.

> **Estado: COMPLETADO EL 2026-08-25.** Las instrucciones siguientes se conservan como evidencia historica. Antes de ejecutar, este plan no otorgaba autoridad de implementacion, ciclo, Git, publicacion ni red; la implementacion procedio despues bajo autorizacion separada del propietario.

**Objetivo:** Agregar la taxonomia fija aprobada, cuatro directivas raiz exactas, una politica minima `localState` en el manifest y validacion de checker de solo lectura, sin agregar consumidores.

**Arquitectura:** Declarar cuatro constantes y reutilizar `guarded_target()` cuando un consumidor futuro autorizado resuelva una ruta declarada. Extender el plan aditivo existente de `.gitignore` y el reemplazo autenticado manifest/checker. No crear directorios, inspeccionar contenido local, migrar archivos ni introducir otra API de rutas.

**Stack:** Python 3.10+ estandar, `unittest`, render determinista de JSON y Markdown, y proyecciones canonica, OpenCode y Claude identicas por bytes.

**Propietario de ciclo:** [RH-W-0051](roadmap.md#rh-w-0051), estado `completed`. El [diseno](rh-w-0051-design.md) aprobado posee el comportamiento. El prerrequisito de almacenamiento de RH-W-0069 esta probado. Historicamente, planificar no cambiaba estado ni autorizaba implementar.

## Mapa de archivos

| Ruta o simbolo | Responsabilidad planeada |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` | Constantes, ignores, manifest, plan aditivo y generacion del checker. |
| `guarded_target()` | Proteccion existente de contencion y ancestros indirectos; no se agrega helper sustituto. |
| `render()`, `render_with_skill_mapping()`, `plan_changes()` y `check()` | Declaracion, plan y drift por mecanismos existentes. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | RED/GREEN enfocado para declaracion, directivas, reemplazo, ausencia y cero escrituras. |
| Proyecciones OpenCode y Claude | Copias mecanicas identicas despues del GREEN canonico. |
| `.repo-harness/manifest.json`, `.repo-harness/check.py`, `.gitignore` | Salidas autoalojadas solo por preview/apply de maintenance revisado. |

No se agrega comportamiento de sesion, creacion de directorios, parser, retencion, anulaciones, API generica, matriz de migracion, discovery de workspace, base de datos, servicio, daemon ni trabajo de fondo.

## Tarea 1: RED y GREEN de la declaracion fija

- [ ] Agregar `LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy`, exigiendo exactamente:

```python
{
    "tmp": {"path": ".repo-harness/tmp/", "class": "disposable"},
    "archive": {"path": ".repo-harness/archive/", "class": "recoverable"},
    "state": {"path": ".repo-harness/state/", "class": "operational"},
    "memory-bk": {"path": ".repo-harness/memory-bk/", "class": "continuity-backup"},
}
```

- [ ] Agregar `test_render_adds_exact_root_local_state_ignore_directives`, exigiendo una vez cada directiva `/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/` y `/.repo-harness/memory-bk/`, sin ignore amplio.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy LocalStateTaxonomyTests.test_render_adds_exact_root_local_state_ignore_directives -v`.
- [ ] RED esperado: `KeyError: 'localState'` y primera directiva ausente. Fallos de import, sintaxis o fixture no son RED valido.
- [ ] Agregar solo constantes, emision del campo y directivas. Mantener `guarded_target()` sin cambios.
- [ ] Repetir el comando. GREEN esperado: dos pruebas pasan.
- [ ] Commit futuro: `feat(harness): declare local-state taxonomy`. Rollback: primero revertir estos cambios canonicos de fuente y pruebas, luego usar maintenance de la Tarea 3 para regenerar los bytes anteriores.

## Tarea 2: RED y GREEN de checker y reemplazo

- [ ] Agregar `test_check_accepts_absent_local_state_directories_with_zero_writes`, instalando en una raiz temporal, exigiendo directorios ausentes, `status == "current"` y `writes == 0`.
- [ ] Agregar `test_check_reports_local_state_manifest_or_ignore_drift`, variando una clase y omitiendo una directiva en subtests, sin crear ni leer directorios declarados.
- [ ] Extender la fixture existente `test_manifest_and_checker_change_as_one_consistent_replacement_pair` con `localState`, sin duplicar la prueba de reemplazo.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_check_accepts_absent_local_state_directories_with_zero_writes LocalStateTaxonomyTests.test_check_reports_local_state_manifest_or_ignore_drift ApprovedGeneratedOutputReplacementTests.test_manifest_and_checker_change_as_one_consistent_replacement_pair -v`.
- [ ] RED esperado: las directivas o campo ausentes impiden `current` y la fixture deseada no contiene `localState`; la ausencia de directorios pasa como control negativo.
- [ ] Actualizar validacion exacta y constantes del checker generado. Reutilizar deteccion aditiva y reemplazo autenticado, sin migracion ni probes.
- [ ] Repetir el comando. GREEN esperado: las pruebas pasan, no existe directorio local y el reemplazo sigue atomico.
- [ ] Commit futuro: `feat(checker): validate local-state taxonomy`. Rollback: primero revertir cambios canonicos y luego usar maintenance de la Tarea 3 para regenerar bytes anteriores. Nunca borrar estado local.

## Tarea 3: GREEN canonico, proyecciones y apply de maintenance

- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: exit 0; detener antes de sincronizar si falla.
- [ ] Copiar solo `scripts/harness_tool.py` y `tests/test_harness_tool.py` a las rutas OpenCode y Claude correspondientes. Luego ejecutar `python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Desde la raiz, ejecutar este preview PowerShell exacto con evidencia de integracion verificada del host, nunca con contenido de memoria:

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

- [ ] Revisar el preview. Esperado: solo cuatro ignores y reemplazos autenticados requeridos; ninguna accion sobre directorios o archivos locales.
- [ ] Sin cambiar archivos ni argumentos, ejecutar:

```powershell
$applyJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $engramEvidence --apply --approved-plan-sha256 $approvedPlanSha256
if ($LASTEXITCODE -ne 0) { throw "maintenance apply failed" }
$apply = $applyJson | ConvertFrom-Json
if ($apply.status -ne "VALIDATED") { throw "maintenance apply was not VALIDATED" }
```

- [ ] Repetir el mismo preview y analizarlo con `ConvertFrom-Json`. Esperado: todas las listas de acciones vacias. Luego ejecutar `python -c "from pathlib import Path; import importlib.util; p=Path('skill/repo-harness-builder/scripts/harness_tool.py'); s=importlib.util.spec_from_file_location('harness_tool',p); m=importlib.util.module_from_spec(s); s.loader.exec_module(m); out=m.render('repo-harness-builder',['claude-code','opencode','pi'],license_id='MIT',report_language='english',documentation_languages=['en','es'],conversation_language='english'); paths=[m.MANIFEST_PATH,'.repo-harness/check.py']; bad=[x for x in paths if Path(x).read_text(encoding='utf-8') != out[x]]; assert not bad,bad; absent=[x['path'] for x in __import__('json').loads(out[m.MANIFEST_PATH])['localState'].values() if Path(x['path']).exists()]; assert not absent,absent"`. Esperado: exit 0 sin salida.
- [ ] Commit mecanico futuro: `chore(harness): synchronize local-state taxonomy`. Mantenerlo separado de RH-W-0069. Rollback: primero revertir cambios canonicos, luego usar esta ruta de maintenance para regenerar bytes anteriores.

## Tarea 4: verificacion completa, ciclo y entrega

- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`, `python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v` y `python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: cada comando sale con 0 y reporta el mismo total; skips de runtime pueden diferir.
- [ ] Ejecutar `python -B skill/repo-harness-builder/scripts/harness_tool.py self-test`; luego ejecutar los comandos exactos de paridad y bytes generados de la Tarea 3. Esperado: self-test imprime `pass`; ambos comandos salen con 0 sin salida.
- [ ] Ejecutar `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }`. Esperado: sin excepcion.
- [ ] Tratar los tres comandos de suite completa anteriores como evidencia requerida para rutas, no-Git, multiraiz, no-probe, migracion y preservacion. No agregar matriz negativa duplicada salvo que uno de esos comandos exponga un requisito sin prueba.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_documented_suite_count_matches_the_real_suite GeneratedCoreTests.test_checked_in_documents_do_not_mix_line_endings -v`. Esperado: cuatro pruebas pasan.
- [ ] Ejecutar `python -c "from pathlib import Path; import re; roots=[Path('README.md'),Path('README.es.md'),Path('REPO-HARNESS.md'),Path('REPO-HARNESS.es.md'),Path('CONTRIBUTING.md'),Path('docs')]; files=[p for r in roots for p in ([r] if r.is_file() else r.rglob('*.md'))]; bad=[]; pattern=re.compile(r'\[[^\]]+\]\(([^)]+)\)'); [(bad.append(f'{p}:{target}') if not (target.startswith(('#','http://','https://','mailto:')) or (p.parent/target.split('#',1)[0]).resolve().exists()) else None) for p in files for target in pattern.findall(p.read_text(encoding='utf-8'))]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Ejecutar `python -c "from pathlib import Path; files=[Path(p) for p in ['docs/repo-harness/rh-w-0051-implementation-plan.md','docs/es/repo-harness/rh-w-0051-implementation-plan.md','docs/repo-harness/rh-w-0069-implementation-plan.md','docs/es/repo-harness/rh-w-0069-implementation-plan.md']]; bad=[str(p) for p in files if any(c in p.read_text(encoding='utf-8') for c in (chr(0x2013),chr(0x2014)))]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Ejecutar `git diff --check`; si `$LASTEXITCODE -ne 0`, detener. Luego ejecutar `git status --short`, `git diff --stat` y `git diff --name-only`. Esperado: sin errores de whitespace, alcance ajeno nuevo ni cambios al trabajo previo.
- [ ] Solo despues de implementacion verificada y autoridad separada, reconciliar roadmap, current state, changelogs, pares, conteo y Engram `work/RH-W-0051`. GREEN local no cambia estado ni autoriza entrega.
- [ ] Issue, commit, PR, push, publicacion o red futuros siguen `CONTRIBUTING.md` con aprobacion separada.

## Aceptacion y pronostico

- [ ] Existen cuatro namespaces, un `localState` ordenado y cuatro directivas raiz exactas.
- [ ] `guarded_target()` sigue siendo la unica proteccion; no existe API especulativa.
- [ ] Checker de solo lectura acepta directorios ausentes, detecta drift y usa reemplazo autenticado.
- [ ] No hay consumidor, migracion, creacion, inspeccion local ni comportamiento ajeno.
- [ ] RH-W-0051 conserva ciclo y entrega separados antes de RH-W-0069.

Pronostico historico: 4 tareas, 140 a 190 lineas autorales cambiadas mas proyecciones mecanicas y bytes autoalojados. La implementacion reutilizo proteccion, ignore, checker y reemplazo existentes, y se completo bajo autorizacion separada.
