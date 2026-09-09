# Plan de implementación RH-W-0053 del checkpoint de operación interrumpida

> **Estado: registro de ejecución completado; RH-W-0053 está completada.** El plan fue aprobado por el propietario el 2026-09-07 y se ejecutó después bajo autoridad separada de implementación y ciclo. En la etapa del registro de implementación no se afirmaban entrega Git, staging, commit, push, publicación, acceso de red ni frescura de nombres registrados de runtime. La entrega posterior mediante issue #170 y PR #171 se fusionó por squash como `c7290fcf6c703df0839699a9cb8b77686bf18e74`.

**Evidencia de ejecución registrada:** la compuerta de control 4 pasó. RED A registró 27 fallos; RED B, 9 fallos y 1 error; RED C, 12 fallos; cada grupo GREEN enfocado pasó. `GeneratedCoreTests` pasó 142 pruebas con 5 omitidas, la suite canónica final pasó 523 pruebas con 9 omitidas y el checker informó `current`, salida 0 y 0 escrituras. La regresión de propiedad de Pi pasó de RED a GREEN: aceptación exigía ocho proyecciones generadas pero preview ofrecía seis hasta corregir la propiedad de skills generadas de Pi. El apply de mantenimiento ligado al digest `15d2f3fcfe3beead92559bf8dd57e55291967d178a8d5a55994fcdd62d91c683` aplicó las ocho proyecciones. La lista de tareas se conserva como registro histórico de instrucciones aprobadas; no se ejecutaron pasos de rollback.

Derivado del [diseño](rh-w-0053-design.md) aprobado por el propietario. Propietario canónico en inglés: [rh-w-0053-implementation-plan.md](../../repo-harness/rh-w-0053-implementation-plan.md).

## Resultado y límites

Implementar un único contrato de instrucciones para `.repo-harness/state/checkpoints/active.md`. Un agente en ejecución solo puede crear o reemplazar ese archivo después de una solicitud humana actual y explícita. El inicio puede inspeccionarlo sin mutación, pero nunca continúa el trabajo automáticamente. Una posible reanudación exige verificación actual del repositorio y autoridad nueva para cada acción que aún tenga compuerta.

Este plan no agrega una API Python de checkpoints. No agrega historial, subsistema de parsing, base de datos, daemon, scheduler, servicio, motor genérico de workflows, memoria duplicada, reutilización de aprobación, recibo, autoridad de entrega, campo de manifiesto, inspección de contenido por el checker, eliminación automática, migración ni limpieza.

## Propietarios comprobados y superficies exactas

| Superficie | Responsabilidad planificada |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py::session_stop_skill()` | Poseer creación o reemplazo solo por solicitud explícita, esquema exacto, contenido acotado, reemplazo seguro y relectura estructural. |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` | Poseer descubrimiento de ruta exacta, validación, expiración semántica, reconciliación con repositorio primero, propuesta de reanudación y autoridad nueva. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render_with_skill_mapping()` | Ruta existente que renderiza ambos contratos; sin nueva entrada, callback, estado ni salida. |
| `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` | Poseer aserciones enfocadas de contratos generados en modo integrado y standalone. |
| RH-W-0051 | Conserva `.repo-harness/state/`, contención, ignore, taxonomía del manifiesto y checker de solo lectura. RH-W-0053 no cambia manifiesto ni ignore. |
| RH-W-0050 | Conserva clases de evidencia, reconciliación acotada y autoridad del repositorio sobre memoria. |
| RH-W-0069 | Conserva `.repo-harness/memory-bk/`, contraste con Engram y continuidad ordinaria de pausa o cierre. El checkpoint no se copia allí. |

Los únicos archivos canónicos de implementación son el script y la prueba anteriores. Las proyecciones mecánicas son `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` y `.pi/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`. La sincronización generada se limita a las skills `repo-harness-session-start` y `repo-harness-session-stop` canónicas y de los tres runtimes.

## Tarea 1: Grupo RED/GREEN A - escritura autorizada y reemplazo seguro

- [ ] Ejecutar primero los controles RH-W-0051: `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy LocalStateTaxonomyTests.test_render_adds_exact_root_local_state_ignore_directives LocalStateTaxonomyTests.test_check_accepts_absent_local_state_directories_with_zero_writes LocalStateTaxonomyTests.test_check_reports_local_state_manifest_or_ignore_drift -v`. Detenerse si falla alguno.
- [ ] Agregar `GeneratedCoreTests.test_checkpoint_write_requires_explicit_current_human_request`, `test_checkpoint_schema_is_exact_and_bounded` y `test_checkpoint_replacement_is_safe_and_read_back`.
- [ ] Afirmar que interrupción, pausa, cierre, autorización de continuidad, aprobación anterior o autoridad de implementación nunca autorizan por sí mismas crear o reemplazar el checkpoint.
- [ ] Afirmar el frontmatter y encabezados exactos del diseño, `None.` para secciones vacías, rutas regulares relativas al repositorio, contenido acotado no sensible, `resume_authority: required` y `step_state` limitado a `not_started`, `started_unverified` o `failed`.
- [ ] Afirmar creación diferida solo de `.repo-harness/state/checkpoints/`, inspección de bytes del archivo regular actual antes de reemplazar, reemplazo seguro en el mismo directorio que conserva el archivo anterior ante fallo, relectura estructural antes de afirmar éxito y un máximo de un `active.md`. No hay historial ni segundo índice.
- [ ] Ejecutar las tres pruebas nuevas. RED esperado: solo faltan las cláusulas de escritura, esquema y reemplazo.
- [ ] Extender mínimamente `session_stop_skill()` con instrucciones Markdown imperativas, sin helper, callback, parser, máquina de estados ni API de persistencia.
- [ ] Repetir el comando. GREEN esperado: pasan tres pruebas.

## Tarea 2: Grupo RED/GREEN B - descubrimiento de solo lectura y expiración semántica

- [ ] Agregar `GeneratedCoreTests.test_checkpoint_discovery_is_exact_read_only_and_non_continuing`, `test_checkpoint_discovery_classifies_all_outcomes` y `test_checkpoint_expiry_is_semantic_and_never_deletes`.
- [ ] Afirmar que propietarios, fuente, pruebas, Git aplicable, checker, ciclo y decisiones se leen antes de consultar únicamente `active.md`. La ausencia es `not_found`; entradas no regulares, indirectas, fuera de raíz, malformadas o de otro proyecto se rechazan sin seguir enlaces.
- [ ] Afirmar resultados distintos `usable`, `stale`, `contradictory`, `expired` e `invalid`, con motivos acotados. `usable` solo puede informar una propuesta de reanudación.
- [ ] Afirmar expiración lógica por finalización, abandono, reemplazo, proyecto distinto, contradicción material, paso ya resuelto, prohibición del ciclo o ruta o estructura inválida. La antigüedad solo es consultiva. El descubrimiento nunca reescribe, borra, archiva, marca como consumido, actualiza timestamps, prepara Git ni continúa trabajo.
- [ ] Ejecutar las tres pruebas nuevas. RED esperado: faltan clasificación de ruta exacta y expiración semántica.
- [ ] Extender mínimamente `session_start_skill()` sin cambiar el orden RH-W-0069 ni los límites RH-W-0050.
- [ ] Repetir el comando. GREEN esperado: pasan tres pruebas.

## Tarea 3: Grupo RED/GREEN C - autoridad nueva y separación de propietarios

- [ ] Agregar `GeneratedCoreTests.test_checkpoint_resume_reverifies_current_evidence_and_authority`, `test_checkpoint_never_reuses_approval_or_grants_delivery` y `test_checkpoint_contract_stays_separate_from_continuity_and_subsystems`.
- [ ] Afirmar que reanudar reconstruye el objetivo actual mínimo, vuelve a verificar hechos completados y paso interrumpido, no repite una finalización actualmente verificada, reclasifica la intención restante y excluye afirmaciones obsoletas de la ruta propuesta.
- [ ] Afirmar que la ejecución solo comienza con autoridad nueva para la acción actual exacta. Nunca se reutilizan aprobación, consentimiento, digest, token ni intención de sesión; el checkpoint no concede implementación, mutación, Git, publicación, recibo ni entrega.
- [ ] Afirmar que backups RH-W-0069 y Engram siguen siendo continuidad separada y no autoritativa. Los renders integrado y standalone funcionan sin historial, parsing genérico, base de datos, daemon, scheduler, motor de workflows, memoria duplicada, almacén de conflictos, recibos ni entregas.
- [ ] Ejecutar las tres pruebas nuevas. RED esperado: faltan autoridad nueva y separación.
- [ ] Completar solo el texto mínimo de start/stop y ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. GREEN esperado: pasa la clase completa.
- [ ] El rollback elimina solo las cláusulas RH-W-0053 y sus nueve pruebas. No elimina ningún `active.md`, backup ni observación Engram existente.

## Tarea 4: verificación canónica y sincronización mecánica

- [ ] Ejecutar una vez `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v` después del GREEN enfocado. Resultado esperado: salida 0.
- [ ] Copiar bytes canónicos de script y prueba a las tres proyecciones de paquete y comprobar igualdad SHA-256 exacta entre las cuatro raíces.
- [ ] Con los valores del manifiesto instalado, ejecutar el preview normal de mantenimiento mediante `skill/repo-harness-builder/scripts/harness_tool.py render`. Revisar el digest y exigir reemplazos solo de skills start/stop y proyecciones previstas; sin acción sobre checkpoint o directorio, manifiesto/checker, eliminación, migración ni proyección ajena.
- [ ] Solo con autorización separada de implementación y apply, aplicar el digest revisado sin cambios. Repetir el preview y exigir listas de acciones mutantes vacías.
- [ ] Comparar las ocho skills autoalojadas - start/stop canónicas y pares Claude Code, OpenCode y Pi - contra `render()` para runtimes `claude-code`, `opencode` y `pi`, licencia `MIT`, idioma de reporte `english`, locales `en` y `es` e idioma de conversación `espanol`.
- [ ] Ejecutar `python .repo-harness/check.py`. Se espera `current`, salida 0 y cero escrituras. Repetir una suite proyectada solo si falla paridad o difieren bytes específicos del runtime.
- [ ] El rollback restaura primero script y pruebas canónicos, sincroniza copias exactas y vuelve a renderizar los contratos anteriores. Nunca elimina contenido local automáticamente.

## Tarea 5: documentación y cierre de ciclo

- [ ] Solo después de implementación verificada y autoridad de ciclo separada, reconciliar `docs/repo-harness/roadmap.md`, `docs/es/repo-harness/roadmap.md`, `docs/repo-harness/current-state.md`, `docs/es/repo-harness/current-state.md`, ambos pares de diseño y plan RH-W-0053, ambos mapas, `CHANGELOG.md`, `CHANGELOG.es.md` y el tema Engram `work/RH-W-0053`.
- [ ] Actualizar `docs/repo-harness/usage.md` y su par español solo si la ruta implementada requiere guía humana durable. No copiar el esquema a documentos ajenos.
- [ ] Mantener RH-W-0053 `proposed` hasta que el propietario del ciclo autorice otro estado. Aprobar el plan no autoriza implementación; verificar implementación no autoriza cierre, staging, commit, push, publicación ni red.
- [ ] Ejecutar los controles enfocados siguientes y después `git diff --check`.

## Comandos de verificación

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_current_state_portfolio_counts_are_derived_from_the_roadmap GeneratedCoreTests.test_current_state_action_plan_coverage_is_derived_from_the_roadmap GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v
python -c "from pathlib import Path; pairs=[('docs/repo-harness/rh-w-0053-implementation-plan.md','docs/es/repo-harness/rh-w-0053-implementation-plan.md'),('docs/repo-harness/README.md','docs/es/repo-harness/README.md'),('docs/repo-harness/roadmap.md','docs/es/repo-harness/roadmap.md'),('docs/repo-harness/current-state.md','docs/es/repo-harness/current-state.md')]; missing=[p for pair in pairs for p in pair if not Path(p).is_file()]; assert not missing,missing; assert all('RH-W-0053' in Path(p).read_text(encoding='utf-8') for pair in pairs for p in pair)"
python .repo-harness/check.py
git diff --check
```

Se espera que pasen las pruebas, existan los propietarios y enlaces bilingües, el checker informe `current` con cero escrituras y el diff no tenga errores. Las pruebas dinámicas existentes, no la prosa escrita, poseen los conteos de cartera y cobertura.

## Pronóstico de revisión y autoridad pendiente

Pronóstico: 180 a 260 líneas autorales de implementación y pruebas, más 120 a 220 líneas mecánicas de paquete/proyección y documentación enfocada de ciclo. Riesgo alto en semántica de autoridad, medio en reemplazo seguro y bajo en complejidad de runtime porque no se agrega subsistema ejecutable. Revisar en tres unidades: escritura/esquema, descubrimiento/reanudación y proyecciones/ciclo. Mantener cada RED junto a su GREEN; no se espera superar el objetivo autoral aproximado de 400 líneas.

Autoridad restante: la implementación está completada. Toda enmienda futura, apply de mantenimiento, avance de ciclo, entrega Git, commit, push, publicación, acceso de red o validación de nombres registrados de runtime conserva su propietario actual. Este plan histórico no afirmaba ninguno de esos resultados de entrega ni frescura de nombres registrados en su etapa de ejecución. Su entrega posterior mediante issue #170 y PR #171 se fusionó por squash como `c7290fcf6c703df0839699a9cb8b77686bf18e74`; la frescura de nombres registrados de runtime sigue sin verificar.
