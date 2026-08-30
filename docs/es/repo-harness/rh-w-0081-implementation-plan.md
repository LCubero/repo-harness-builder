# Plan de implementacion RH-W-0081 de cronologia local del changelog

> **Estado: propuesto.** Requiere aprobacion del propietario y autorizacion separada para implementar el producto. La correccion documental se completo por separado y se conserva abajo como trabajo historico.

Derivado del [diseno](rh-w-0081-design.md) aprobado. Par canonico: [rh-w-0081-implementation-plan.md](../../repo-harness/rh-w-0081-implementation-plan.md).

## Superficie y propiedad

`session_stop_skill()` renderiza guia humana del changelog mediante `render_with_skill_mapping()`. `changelog()` crea archivos iniciales, pero no deriva fechas. `DATE_HEADING` reconoce encabezados. `GeneratedCoreTests.test_changelog_is_simple_completed_history` y `test_session_skills_treat_changelog_as_completed_history_only` cubren limites. RH-W-0068 decide si se necesita entrada. RH-W-0081 deriva fecha local. Identidad y orden UTC de RH-W-0069 son independientes en ambos sentidos.

La evidencia exacta para la correccion completada es `1e5759e0` a `2026-08-28T22:33:40-06:00` y `c892cfb` a `2026-08-28T22:40:57-06:00`. El commit `fac022c6` movio ambas entradas a las secciones existentes `2026-08-28`. El commit `b39e1ba` actualizo solo la proyeccion del Plan 30.

## Corte 1: RED enfocado y contrato minimo

- [ ] Agregar `GeneratedCoreTests.test_session_stop_derives_changelog_date_from_exact_host_local_evidence` y `test_changelog_chronology_is_independent_from_continuity_utc_identity`.
- [ ] Usar el reloj local observado del host para trabajo completado sin Git. Para trabajo Git completado, preferir evidencia Git local verificada ligada al cambio exacto; conservar segundos y offset al mostrar timestamp, o fecha local con offset.
- [ ] Exigir un encabezado por fecha, sin UTC silencioso ni minuto inventado, y una pregunta separada reanudable si falta la observacion del reloj para trabajo sin Git o si Git local falta, se contradice o es ambiguo. Nunca derivar cronologia de timestamps de creacion, modificacion o acceso del filesystem.
- [ ] Exigir que filename y `timestamp_utc` RH-W-0069 no deriven ni reciban cronologia del changelog.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_stop_derives_changelog_date_from_exact_host_local_evidence GeneratedCoreTests.test_changelog_chronology_is_independent_from_continuity_utc_identity -v`. RED esperado: faltan solo clausulas.
- [ ] Extender minimamente `session_stop_skill()`. Sin servicio de reloj, base timezone, daemon locale, estado, campo manifest, estado checker, dependencia ni API generica.
- [ ] Repetir ese comando y luego ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_changelog_is_simple_completed_history GeneratedCoreTests.test_session_skills_treat_changelog_as_completed_history_only -v`. GREEN esperado: todo pasa.
- [ ] Rollback retira solo pruebas y clausulas RH-W-0081.

## Corte 2: correccion documental bilingue historica y completada

- [x] Se verifico solo con Git local: `git show -s --format=%H%n%cI 1e5759e0` y `git show -s --format=%H%n%cI c892cfb` devolvieron `2026-08-28T22:33:40-06:00` y `2026-08-28T22:40:57-06:00`.
- [x] Se movio la entrada inglesa desde el encabezado falso `2026-08-29` a la seccion existente `2026-08-28`.
- [x] Se hizo el movimiento equivalente en `CHANGELOG.es.md`, conservando orden y paridad.
- [x] No se cambio contenido de las entradas fuera de la fusion de encabezados ligada a evidencia.
- [x] La correccion quedo independiente del comportamiento generado de producto en el commit `fac022c6`.

## Corte 3: proyecciones, ciclo y entrega

- [ ] Ejecutar suite canonica completa tras GREEN del Corte 1. Esperado: exit 0.
- [ ] Sincronizar script y prueba de paquetes y usar renderer normal para session-stop. Revisar preview y digest antes de apply autorizado. Segundo preview esperado: sin acciones.
- [ ] Tras implementacion verificada y autoridad separada, reconciliar exactamente `docs/repo-harness/roadmap.md`, `docs/es/repo-harness/roadmap.md`, `docs/repo-harness/current-state.md`, `docs/es/repo-harness/current-state.md`, `docs/repo-harness/README.md`, `docs/es/repo-harness/README.md`, `docs/repo-harness/rh-w-0081-design.md`, `docs/es/repo-harness/rh-w-0081-design.md`, `docs/repo-harness/rh-w-0081-implementation-plan.md`, `docs/es/repo-harness/rh-w-0081-implementation-plan.md`, `CHANGELOG.md`, `CHANGELOG.es.md`, disposicion RH-W-0068 y Engram `work/RH-W-0081`.
- [ ] Mantener Corte 2 como correccion documental revisable por separado. Mantener issue, commits, revision, rollback y entrega separados de RH-W-0068 y RH-W-0069. No se autoriza Git ahora.

## Verificacion

- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_stop_derives_changelog_date_from_exact_host_local_evidence GeneratedCoreTests.test_changelog_chronology_is_independent_from_continuity_utc_identity GeneratedCoreTests.test_changelog_is_simple_completed_history GeneratedCoreTests.test_session_skills_treat_changelog_as_completed_history_only -v` y repetir con las rutas OpenCode y Claude. Esperado: cuatro pases iguales por paquete.
- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: exit 0. Ejecutar `python -c "from pathlib import Path; import hashlib; rel=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in rel if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Esperado: exit 0 sin salida.
- [ ] Ejecutar `$root=(Resolve-Path -LiteralPath '.').Path; $e=$env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON; if ([string]::IsNullOrWhiteSpace($e)) { throw 'verified Engram evidence required' }; $j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'preview failed' }; $p=$j|ConvertFrom-Json; $d=[string]$p.planSha256; if ($d -notmatch '^[0-9a-f]{64}$') { throw 'invalid plan digest' }`. Esperado: digest valido y solo reemplazos previstos de session-stop.
- [ ] Tras autorizacion separada y sin cambiar inputs, ejecutar `$j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e --apply --approved-plan-sha256 $d; if ($LASTEXITCODE -ne 0) { throw 'apply failed' }; $a=$j|ConvertFrom-Json; if ($a.status -ne 'VALIDATED') { throw 'apply not validated' }; $j2=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'second preview failed' }; $p2=$j2|ConvertFrom-Json; if (@($p2.create).Count + @($p2.append).Count + @($p2.replace).Count + @($p2.blocked).Count + @($p2.migrate).Count -ne 0) { throw 'second preview is not empty' }`. Esperado: `VALIDATED` y segundo preview mutante vacio.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Esperado: cuatro pasan.
- [ ] Ejecutar `python -c "from pathlib import Path; files=[Path('CHANGELOG.md'),Path('CHANGELOG.es.md')]; bad=[(str(p),p.read_text(encoding='utf-8').splitlines().count('## 2026-08-29'),p.read_text(encoding='utf-8').splitlines().count('## 2026-08-28')) for p in files if p.read_text(encoding='utf-8').splitlines().count('## 2026-08-29') != 0 or p.read_text(encoding='utf-8').splitlines().count('## 2026-08-28') != 1]; assert not bad,bad"` despues del Corte 2. Esperado: exit 0 sin salida.
- [ ] Ejecutar `python .repo-harness/check.py`. Esperado: `current`, exit 0, cero escrituras.
- [ ] Ejecutar `git diff --check` y escaneo U+2013/U+2014. Esperado: sin hallazgos.

## Pronostico de carga de revision

Cambio autoral: 90 a 150 lineas, incluidos movimientos de encabezado. Proyecciones mecanicas: 30 a 80. Riesgo medio porque la cronologia es visible, con implementacion limitada a guia generada. Unidades: Corte 1 contrato, Corte 2 correccion, Corte 3 proyecciones/ciclo. El presupuesto autoral de 400 lineas no esta en riesgo.

## Aceptacion y decisiones

- [ ] Evidencia local deriva cronologia sin UTC silencioso ni redondeo.
- [ ] Ambiguedad produce una pregunta separada reanudable.
- [ ] Disposicion RH-W-0068 e identidad UTC RH-W-0069 quedan intactas.
- [ ] Correccion inglesa y espanola es exacta, ligada a evidencia y reversible.

Decisiones pendientes: ninguna de planificacion. Aprobar plan, implementar y entregar siguen separados.
