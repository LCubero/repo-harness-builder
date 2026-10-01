# Plan de implementacion RH-W-0075 de necesidad y justificacion externa

> **Estado: reemplazado el 2026-10-01** por la implementación mínima registrada en el [diseño](rh-w-0075-design.md#implementación-mínima-2026-10-01). Se conserva como historia.

Derivado del [diseno](rh-w-0075-design.md) aprobado. Par canonico: [rh-w-0075-implementation-plan.md](../../repo-harness/rh-w-0075-implementation-plan.md).

## Superficie probada y limites

La propiedad canonica es exacta: `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs()` posee necesidad y los cuatro campos completos. `::session_start_skill()` y `::session_stop_skill()` reciben solo referencias condicionales donde ya exponen compuertas externas o de red. `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` posee las aserciones. `render()` llama `render_with_skill_mapping()`. RH-W-0074 puede editar `.repo-harness/rules/working-agreements.md`, por lo que las clausulas se serializan o rebasean. RH-W-0075 posee solo necesidad y justificacion antes de pedir; las demas autoridades no cambian.

## Tarea 1: RED enfocado

- [ ] Agregar `GeneratedCoreTests.test_external_request_requires_strict_necessity_and_four_field_justification`, `test_repository_local_solution_suppresses_external_request` y `test_batch_activation_and_grouped_answers_cannot_bypass_necessity_or_supply_external_approval` en `skill/repo-harness-builder/tests/test_harness_tool.py`.
- [ ] Afirmar rutas externas, repositorios hermanos, red y tooling de proveedor, siempre con prueba de que el objetivo no se resuelve dentro del repositorio.
- [ ] Afirmar objetivo exacto, razon necesaria, uso previsto y consecuencia de declinar. Necesidad no concede acceso.
- [ ] Afirmar que una solucion local suprime la peticion externa.
- [ ] Afirmar que activacion y respuestas agrupadas RH-W-0080 no satisfacen necesidad ni suministran, infieren, reutilizan o amplian aprobacion externa.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_external_request_requires_strict_necessity_and_four_field_justification GeneratedCoreTests.test_repository_local_solution_suppresses_external_request GeneratedCoreTests.test_batch_activation_and_grouped_answers_cannot_bypass_necessity_or_supply_external_approval -v`. RED esperado: tres fallos de contrato, con import del renderer exitoso.

## Tarea 2: GREEN minimo y rollback

- [ ] Extender solo working agreements en `rule_outputs()` con la disciplina completa.
- [ ] Agregar referencias concisas en `session_start_skill()` y `session_stop_skill()` donde ya piden autoridad externa o de red, sin duplicar los cuatro campos.
- [ ] Conservar la compuerta separada de aprobacion. Declinar no concede acceso ni crea autoridad recordada.
- [ ] Repetir Tarea 1 y luego `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. GREEN esperado: exit 0.
- [ ] Revertir juntas las pruebas y clausulas exactas de este propietario, conservando clausulas RH-W-0074 independientes.

## Tarea 3: proyecciones, documentacion y ciclo

- [ ] Tras GREEN enfocado, ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: exit 0.
- [ ] Sincronizar script y prueba en `.opencode/skills/repo-harness-builder/` y `.claude/skills/repo-harness-builder/`, y usar el renderer normal de mantenimiento. Revisar digest antes de apply autorizado. Segundo preview esperado: sin acciones.
- [ ] Tras implementacion verificada y autoridad de ciclo, reconciliar guia aplicable, roadmap, current state, mapas, pares, disposicion RH-W-0068 y Engram `work/RH-W-0075`.
- [ ] Conservar issue, commit, revision, rollback y entrega independientes de RH-W-0074. Este plan no autoriza Git ni entrega.

## Verificacion

- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_external_request_requires_strict_necessity_and_four_field_justification GeneratedCoreTests.test_repository_local_solution_suppresses_external_request GeneratedCoreTests.test_batch_activation_and_grouped_answers_cannot_bypass_necessity_or_supply_external_approval -v` y repetir con `.opencode/skills/repo-harness-builder/tests/test_harness_tool.py` y `.claude/skills/repo-harness-builder/tests/test_harness_tool.py`. Esperado: tres pases iguales por paquete.
- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: exit 0 antes de sincronizar.
- [ ] Ejecutar `python -c "from pathlib import Path; import hashlib; rel=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in rel if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Preview exacto: `$root=(Resolve-Path -LiteralPath '.').Path; $e=$env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON; if ([string]::IsNullOrWhiteSpace($e)) { throw 'verified Engram evidence required' }; $j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'preview failed' }; $p=$j|ConvertFrom-Json; $d=[string]$p.planSha256; if ($d -notmatch '^[0-9a-f]{64}$') { throw 'invalid plan digest' }`. Esperado: digest valido y solo reglas o sesiones previstas.
- [ ] Tras autorizacion separada, ejecutar `$j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e --apply --approved-plan-sha256 $d; if ($LASTEXITCODE -ne 0) { throw 'apply failed' }; $a=$j|ConvertFrom-Json; if ($a.status -ne 'VALIDATED') { throw 'apply not validated' }; $j2=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'second preview failed' }; $p2=$j2|ConvertFrom-Json; if (@($p2.create).Count + @($p2.append).Count + @($p2.replace).Count + @($p2.blocked).Count + @($p2.migrate).Count -ne 0) { throw 'second preview is not empty' }`. Esperado: apply validado y segundo preview mutante vacio.
- [ ] Ejecutar `python .repo-harness/check.py`. Esperado: `current`, exit 0 y cero escrituras.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Esperado: cuatro pasan.
- [ ] Ejecutar `git diff --check` y escanear Markdown cambiado por U+2013 y U+2014. Esperado: sin hallazgos.

## Pronostico de carga de revision

Cambio autoral: 90 a 150 lineas. Proyecciones mecanicas: 60 a 120. Riesgo medio por colision con RH-W-0074, con conducta aditiva solo antes de pedir. Unidades recomendadas: RED y GREEN canonico, luego proyecciones y ciclo. El presupuesto autoral de 400 lineas no esta en riesgo.

## Aceptacion y decisiones

- [ ] Las cuatro clases usan necesidad estricta y cuatro campos.
- [ ] Solucion local suprime la peticion, aprobacion queda separada y declinar no concede nada.
- [ ] No se agrega store, broker, telemetria, persistencia, servicio, dependencia ni framework.
- [ ] RH-W-0074 y autoridad por digest quedan intactos.

Decisiones pendientes: ninguna de planificacion. Aprobar el plan e implementar siguen separados.
