# Plan de implementacion RH-W-0080 de interaccion efimera por lotes

> **Estado del plan: aprobado por el propietario el 2026-10-06 ("si") solo para registro documental. Despues se otorgo autoridad separada de implementacion; la implementacion acotada y las comprobaciones listadas estan verificadas en el [registro de tarea](../../../odd/tasks/rh-w-0080-bounded-intake.md). La lista siguiente es contenido historico del plan, no la lista vigente de trabajo restante. No consta commit de implementacion ni publicacion; el ciclo sigue `proposed` a la espera de aceptacion separada.**

Derivado del [diseno](rh-w-0080-design.md) aprobado. Par canonico: [rh-w-0080-implementation-plan.md](../../repo-harness/rh-w-0080-implementation-plan.md).

## Superficie y limites de autoridad

La propiedad esta resuelta. `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` posee activacion, intake despues de inspeccionar, aclaracion no autorizante agrupada y continuacion de un objetivo. `::session_stop_skill()` posee vencimiento al pausar, cerrar, completar, abandonar o reemplazarlo. `::rule_outputs()` posee exclusiones compartidas en working agreements. `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` posee aserciones del contrato generado. `render_with_skill_mapping()` renderiza los tres propietarios. RH-W-0074 y RH-W-0075 son referencias a conductas ya completadas, no dependencias inconclusas. Los demas propietarios y la autoridad por digest no cambian.

Agrupar solo preguntas de requisitos no autorizantes; excluir decisiones humanas/materiales y toda aprobacion. Mantener pausas por evidencia, seguridad y autoridad; hacer una pregunta de decision bloqueante a la vez. La activacion y las respuestas son solo de sesion, vencen al pausar o reemplazar el objetivo (ademas de las otras condiciones de cierre anteriores), no confieren autoridad y nunca se reutilizan automaticamente. Las pruebas del contrato generado verifican estructura, no cumplimiento de agentes en vivo.

## Tarea 1: RED de activacion, consulta y expiracion

- [ ] Agregar `GeneratedCoreTests.test_batch_interaction_requires_explicit_one_objective_activation` y `test_batch_intake_groups_only_non_authorizing_clarifications`.
- [ ] Exigir activacion explicita para un objetivo acotado, inspeccion antes de consultar y agrupacion solo de aclaraciones no autorizantes descubiertas por esa inspeccion.
- [ ] Resolver sin preguntar huecos de evidencia internos y ejecutar sin confirmacion archivo por archivo solo el trabajo ya autorizado.
- [ ] Exigir que activacion y respuestas agrupadas sean solo de sesion; venzan al pausar, reemplazar el objetivo y en las demas condiciones de cierre; nunca se reutilicen automaticamente ni se escriban en manifest, estado local, backups, Engram, checkpoints, recibos o stores.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_batch_interaction_requires_explicit_one_objective_activation GeneratedCoreTests.test_batch_intake_groups_only_non_authorizing_clarifications -v`. RED esperado: faltan solo clausulas de contrato.

## Tarea 2: RED de compuertas separadas

- [ ] Agregar `GeneratedCoreTests.test_batch_interaction_keeps_authority_gates_separate_and_resumable` y `test_batch_answers_never_satisfy_builder_digest_bound_authority`.
- [ ] Exigir preguntas separadas para acceso externo, entrega Git, publicacion, destruccion, secretos, preview, apply, digest o token exacto y nuevas decisiones materiales.
- [ ] Exigir que respuestas agrupadas nunca persistan, se reutilicen, amplien, normalicen, suministren, infieran ni sustituyan `APPROVE`, `DENY`, `APROBAR`, `DENEGAR`, `planSha256`, digest de generacion, evidencia refrescada, bytes aprobados, apply ni dominio activo.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_batch_interaction_keeps_authority_gates_separate_and_resumable GeneratedCoreTests.test_batch_answers_never_satisfy_builder_digest_bound_authority -v`. RED esperado: faltan separacion y exclusiones; `ApprovalUxTests` sigue como control verde.

## Tarea 3: GREEN minimo y colisiones

- [ ] Agregar activacion e intake solo a `session_start_skill()`, vencimiento solo a `session_stop_skill()` y exclusiones compartidas solo al working agreement de `rule_outputs()`. Sin parser, objeto de modo, flag, campo manifest, callback, maquina de estados, servicio ni persistencia.
- [ ] Referenciar RH-W-0067 para presentacion de una pregunta, sin copiar sintaxis.
- [ ] Referenciar RH-W-0075, RH-W-0074 y otros propietarios, sin copiar conducta.
- [ ] Repetir ambos comandos y luego ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py ApprovalUxTests GeneratedCoreTests -v`. GREEN esperado: todo pasa.
- [ ] Rollback retira solo clausulas y pruebas RH-W-0080, conservando propietarios y ediciones concurrentes.

## Tarea 4: proyecciones, documentacion y ciclo

- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v` tras GREEN enfocado. Esperado: exit 0.
- [ ] Sincronizar script y prueba de paquetes y usar el renderer normal para contratos de sesion. Revisar preview y digest antes de apply autorizado. Segundo preview esperado: sin acciones.
- [ ] Tras implementacion verificada y autoridad separada, reconciliar exactamente `docs/repo-harness/roadmap.md`, `docs/es/repo-harness/roadmap.md`, `docs/repo-harness/current-state.md`, `docs/es/repo-harness/current-state.md`, `docs/repo-harness/README.md`, `docs/es/repo-harness/README.md`, `docs/repo-harness/rh-w-0080-design.md`, `docs/es/repo-harness/rh-w-0080-design.md`, `docs/repo-harness/rh-w-0080-implementation-plan.md`, `docs/es/repo-harness/rh-w-0080-implementation-plan.md`, `CHANGELOG.md`, `CHANGELOG.es.md` y Engram `work/RH-W-0080`, sin guardar autoridad de lote.
- [ ] Conservar issue, commit, revision, rollback y entrega independientes. No se autoriza Git ahora.

## Verificacion

- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_batch_interaction_requires_explicit_one_objective_activation GeneratedCoreTests.test_batch_intake_groups_only_non_authorizing_clarifications GeneratedCoreTests.test_batch_interaction_keeps_authority_gates_separate_and_resumable GeneratedCoreTests.test_batch_answers_never_satisfy_builder_digest_bound_authority ApprovalUxTests -v`; repetir con las rutas OpenCode y Claude. Luego ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v` en las tres rutas. Esperado: pases iguales.
- [ ] Ejecutar `python -c "from pathlib import Path; import hashlib; rel=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in rel if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Ejecutar `$root=(Resolve-Path -LiteralPath '.').Path; $e=$env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON; if ([string]::IsNullOrWhiteSpace($e)) { throw 'verified Engram evidence required' }; $j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'preview failed' }; $p=$j|ConvertFrom-Json; $d=[string]$p.planSha256; if ($d -notmatch '^[0-9a-f]{64}$') { throw 'invalid plan digest' }`. Esperado: digest valido y solo reemplazos previstos de reglas o sesiones.
- [ ] Tras autorizacion separada y sin cambiar inputs, ejecutar `$j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e --apply --approved-plan-sha256 $d; if ($LASTEXITCODE -ne 0) { throw 'apply failed' }; $a=$j|ConvertFrom-Json; if ($a.status -ne 'VALIDATED') { throw 'apply not validated' }; $j2=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'second preview failed' }; $p2=$j2|ConvertFrom-Json; if (@($p2.create).Count + @($p2.append).Count + @($p2.replace).Count + @($p2.blocked).Count + @($p2.migrate).Count -ne 0) { throw 'second preview is not empty' }`. Esperado: `VALIDATED` y segundo preview mutante vacio.
- [ ] Ejecutar `python .repo-harness/check.py`. Esperado: `current`, exit 0, cero escrituras.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Esperado: cuatro pasan.
- [ ] Ejecutar `python -c "from pathlib import Path; roots=[Path('.repo-harness/manifest.json'),Path('.repo-harness/rules'),Path('.repo-harness/skills')]; terms=('batchMode','batch_mode','groupedAnswers','grouped_answers','batchApproval','batch_authority'); files=[p for r in roots for p in ([r] if r.is_file() else r.rglob('*')) if p.is_file()]; bad=[f'{p}:{t}' for p in files for t in terms if t in p.read_text(encoding='utf-8',errors='ignore')]; assert not bad,bad"`. Esperado: exit 0 sin estado o autoridad persistidos.
- [ ] Ejecutar `git diff --check` y escaneo U+2013/U+2014. Esperado: sin hallazgos.

## Pronostico de carga de revision

Cambio autoral: 150 a 230 lineas. Proyecciones mecanicas: 90 a 180. Riesgo alto para semantica de autoridad y bajo para complejidad runtime. Unidades: activacion/consulta, exclusiones de autoridad, luego proyecciones/ciclo. El presupuesto autoral de 400 lineas no esta en riesgo.

## Aceptacion y decisiones

- [ ] Consulta agrupada solo aclara, despues de inspeccionar y para un objetivo.
- [ ] Cada compuerta sigue separada, reanudable, exacta y de una pregunta.
- [ ] Ninguna autoridad persiste ni afecta aprobacion por digest.
- [ ] No se fusiona propietario ni se agrega estado, dependencia, daemon, store, servicio o framework.

Decision historica de planificacion: no quedaban decisiones pendientes para el plan. La aprobacion del 2026-10-06 por si sola no autorizo implementar; la autoridad separada se otorgo despues. Entrega y aceptacion del ciclo siguen como compuertas separadas pendientes.
