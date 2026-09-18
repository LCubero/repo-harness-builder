# Plan de implementación RH-W-0091 de prioridad de inicio y reanudación primero

> **Estado: registro histórico de ejecución completado.** RH-W-0091 está `completed`. La aprobación del plan, la autorización de implementación y el cierre de ciclo fueron decisiones separadas del propietario; no se afirman commit, push, PR, merge, release ni publicación.

## Evidencia de ejecución completada

- El RED/GREEN de TDD estricto cubrió 2 + 3 + 2 pruebas enfocadas para forma/orden de backup, prioridad/disparadores de inicio y preservación de propietarios/capacidades.
- El digest de mantenimiento `5077cc89efdd1032a7dcd4e8374eb84bcd8925cee7110e4f8276a536aa5ca59e` aplicó exactamente ocho contratos de sesión generados. El RED de corrección midió 879; el 697 intermedio perdió dos frases exactas; el 697 final conserva `repository evidence remains authoritative` y `never claim successful continuity`.
- Suite completa final: 554 pruebas, 10 omitidas, salida 0. El checker estuvo `current` con cero escrituras; pasan cuatro pruebas de ciclo/enlaces; pasó `git diff --check`.
- Paridad exacta de paquetes: SHA del script `b08fa14b2e11e7b050371279c6837165112c07ce40374da38e35d7777dab0ac0`; SHA de pruebas `f1119d226386d5ac3144e34f515fd2038697b0d6fcd19691f56f0de982b94f1b`. Ocho contratos de sesión generados coinciden con los bytes del render vigente.
- `.repo-harness/rules/memory.md` instalado y de propiedad humana quedó idéntico a HEAD y no reemplazable; el preview no tiene mutaciones. El issue #190 está aprobado/abierto; la rama local es `feat/rh-w-0091-resume-first`.

Derivado estrictamente del [alcance](rh-w-0091-scope.md) y [diseño](rh-w-0091-design.md) aprobados por el propietario. Par canónico: [rh-w-0091-implementation-plan.md](../../repo-harness/rh-w-0091-implementation-plan.md).

## Objetivo y límite de autoridad

Implementar solo el contrato aprobado de instrucciones con reanudación primero: los backups nuevos de continuidad nativa colocan el bloque exacto `Where We Left Off` antes de la continuidad detallada existente; session-start consume carta, evidencia válida de idioma/preferencias, contenido de reanudación del backup válido más nuevo y luego propietarios más profundos solo por los cuatro disparadores aprobados. Los backups existentes siguen legibles sin campos sintetizados.

Este plan no autoriza cambios de fuente, pruebas, salida generada, proyección de runtime, backup, preferencias, Git, red, revisión ni entrega. La aprobación del propietario de este plan corregido autoriza solo el plan. Se requiere una autorización explícita posterior para implementar antes de la Tarea 1. Apply de mantenimiento, cierre de ciclo, entrega Git y operaciones de red conservan sus compuertas vigentes separadas.

## Superficies canónicas probadas y familias de proyección

| Superficie | Responsabilidad planificada |
| --- | --- |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_stop_skill()` | Exigir el bloque de reanudación en backups nuevos y conservar refresco, filtrado, escritura exclusiva, validación, orden local-antes-de-Engram y resultados independientes. |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` | Exigir prioridad exacta de inicio, lectura de reanudación válida más nueva, fallback para backups antiguos y exactamente cuatro disparadores de lectura profunda. |
| `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs()` / variable local `native_memory_contract` | Poseer la forma exacta compartida del backup, compatibilidad, autoridad, privacidad y exclusiones sin un propietario de esquema nuevo. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render_with_skill_mapping()` | Ruta existente que renderiza `session_start_skill()`, `session_stop_skill()` y `rule_outputs()`; sin cambiar firma, entrada, callback ni retorno. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render_repository()` | Ruta existente que proyecta archivos canónicos del paquete de producto a los runtimes seleccionados; sin cambiar el mecanismo de proyección. |
| `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` | Poseer aserciones enfocadas de contratos generados integrados y standalone. |

Los archivos canónicos de producto son exactamente:

- `skill/repo-harness-builder/scripts/harness_tool.py`
- `skill/repo-harness-builder/tests/test_harness_tool.py`

Las familias esperadas de proyecciones mecánicas del paquete de producto son:

- `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`
- `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`
- `.pi/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`

Las familias esperadas de contratos generados son:

- `.repo-harness/skills/repo-harness-session-{start,stop}/SKILL.md` autoalojados;
- `.claude/skills/repo-harness-session-{start,stop}/SKILL.md` de runtime;
- `.opencode/skills/repo-harness-session-{start,stop}/SKILL.md` de runtime;
- `.pi/skills/repo-harness-session-{start,stop}/SKILL.md` de runtime; y
- la salida compartida autoalojada `.repo-harness/rules/memory.md` asociada con `rule_outputs()`.

Estas son familias esperadas, no una lista preaprobada de reemplazos. El preview normal de mantenimiento debe determinar los reemplazos finales exactos según propiedad y bytes vigentes. En particular, el plan no supone que `.repo-harness/rules/memory.md`, de propiedad humana o evolucionado por humanos, sea reemplazable. Si el preview omite, bloquea o clasifica esa ruta como no reemplazable, se detiene sin forzar, sobrescribir, normalizar ni evitar la propiedad y se obtiene una decisión separada del propietario.

## Ejecución completada de TDD estricto

La implementación completada usó el runner canónico unittest. Cada tarea registró RED observado antes de cambiar texto de implementación canónico y luego el GREEN mínimo. Un control existente que pasa no constituía evidencia RED.

### Tarea 1: forma del backup y orden de session-stop

- [x] Agregar `GeneratedCoreTests.test_resume_first_backup_shape_and_stop_order_are_exact` y `test_resume_first_old_backups_remain_readable_without_synthesized_fields` en el archivo canónico de pruebas.
- [x] Exigir que un backup nuevo preserve el frontmatter obligatorio existente de RH-W-0069 como inicio del archivo, sin cambios; su cuerpo debe tener luego exactamente `# Session Continuity`, `## Where We Left Off`, `### Next Action`, `### Pending Work` y `### Current Blocker` en ese orden, seguido por las secciones de continuidad detallada existentes. Los valores vacíos son `None.` y las etiquetas de evidencia existentes permanecen sin cambios.
- [x] Exigir que `session_stop_skill()` refresque evidencia aplicable, determine esos tres valores en orden, escriba y valide ese orden exacto del cuerpo después del frontmatter obligatorio de RH-W-0069 sin cambios y antes de la continuidad detallada existente, y después conserve filtrado, creación exclusiva, orden local-antes-de-Engram, validación y resultados independientes de RH-W-0069.
- [x] Exigir que backups válidos existentes sin `Where We Left Off` sigan legibles mediante la continuidad detallada vigente y que ningún campo de reanudación ausente sea sintetizado, migrado, reescrito ni completado retroactivamente.
- [x] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_resume_first_backup_shape_and_stop_order_are_exact GeneratedCoreTests.test_resume_first_old_backups_remain_readable_without_synthesized_fields -v`. RED esperado: faltan el orden exacto del cuerpo que preserva frontmatter, el bloque de reanudación y las cláusulas de compatibilidad, mientras imports y controles existentes siguen funcionando; la expectativa no elimina ni modifica el frontmatter de RH-W-0069.
- [x] Actualizar mínimamente solo `session_stop_skill()` y el `native_memory_contract` renderizado por `rule_outputs()`. Agregar solo texto de instrucciones, sin API de escritura, parser, migración ni segundo almacén.
- [x] Repetir el comando exacto. GREEN esperado: ambas pruebas pasan.

### Tarea 2: prioridad de inicio, cuatro disparadores y aceptación negativa

- [x] Agregar `GeneratedCoreTests.test_session_start_uses_exact_resume_first_priority`, `test_session_start_deep_reads_for_exactly_four_triggers` y `test_session_start_does_not_deep_read_without_a_named_trigger`.
- [x] Exigir la prioridad exacta: puntero acotado a la carta en `decisions.md`; idioma de conversación verificado más preferencias estables activas o restricciones continuas desde propietarios existentes; `Where We Left Off` del backup RH-W-0069 válido más nuevo; después propietarios más profundos solo por disparador.
- [x] Exigir exactamente estos cuatro disparadores, sin alias ni cláusula abierta: objetivo explícito o `RH-W-####`; contradicción; evidencia de prioridad inválida o ausente; reconciliación obligatoria.
- [x] Exigir que cada disparador seleccione solo la evidencia necesaria de propietarios actuales y nunca autorice lectura exhaustiva, continuación automática, inferencia de preferencias ni un almacén nuevo.
- [x] Exigir aceptación negativa: con evidencia de prioridad válida y ninguno de los cuatro disparadores, las instrucciones renderizadas de session-start se detienen después de la lectura de reanudación y no realizan lectura de propietarios más profunda.
- [x] Exigir que un backup válido antiguo sin el bloque nuevo use la continuidad detallada RH-W-0069 sin sintetizar campos.
- [x] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_start_uses_exact_resume_first_priority GeneratedCoreTests.test_session_start_deep_reads_for_exactly_four_triggers GeneratedCoreTests.test_session_start_does_not_deep_read_without_a_named_trigger -v`. RED esperado: falta prioridad exacta, matriz acotada de disparadores o contrato negativo sin lectura profunda.
- [x] Actualizar mínimamente solo `session_start_skill()` y referencias concisas al contrato compartido de memoria. Sin callback de lectura, parser, cache ni subsistema de orquestación.
- [x] Repetir el comando exacto. GREEN esperado: pasan las tres pruebas.

### Tarea 3: preservación de capacidades y propietarios

- [x] Agregar `GeneratedCoreTests.test_resume_first_preserves_capability_and_owner_boundaries` y `test_resume_first_excludes_new_state_services_and_preflights`.
- [x] Exigir que la recuperación Engram con alcance siga siendo evidencia de disponibilidad por sí misma, sin prueba ni preflight Engram separado ni informe repetido de éxito sin cambios; fallos y discrepancias pertinentes siguen visibles.
- [x] Exigir que Gentle AI se alcance solo en un límite habilitado y aplicable de enrutamiento, revisión, recibo o corrección, sin prueba de inicio.
- [x] Exigir que RH-W-0069 conserve esquema, privacidad, escritura exclusiva, orden local-antes-de-Engram y resultados independientes; RH-W-0078 conserve división física de skills generadas y divulgación progresiva; RH-W-0031, RH-W-0071, RH-W-0082 y RH-W-0018 conserven semántica de idioma y preferencias; roadmap, estado actual y changelog conserven sus roles vigentes.
- [x] Exigir que no aparezca infraestructura nueva, parser/API, propietario de esquema, cache, daemon, servicio, scheduler, migración, limpieza, retención, checkpoint, framework de preferencias, prueba de Gentle, preflight de Engram ni segundo almacén de continuidad.
- [x] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_resume_first_preserves_capability_and_owner_boundaries GeneratedCoreTests.test_resume_first_excludes_new_state_services_and_preflights -v`. RED esperado: están incompletas las cláusulas de preservación o exclusión.
- [x] Agregar solo el texto compartido/inicio/cierre mínimo necesario y repetir el comando. GREEN esperado: ambas pruebas pasan.
- [x] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. Esperado: la clase canónica completa pasa antes de sincronizar proyecciones.

### Tarea 4: sincronización mecánica de proyecciones

- [x] Después de que `GeneratedCoreTests` canónico quede GREEN, sincronizar solo los bytes del script y prueba canónicos mediante `render_repository()` hacia las tres familias de paquete de producto indicadas. Verificar paridad SHA-256 exacta de `scripts/harness_tool.py` y `tests/test_harness_tool.py` entre las raíces canónica, Claude, OpenCode y Pi.
- [x] Ejecutar el preview normal de mantenimiento de solo lectura mediante el comando `render` del `harness_tool.py` canónico, con valores vigentes del manifest instalado y la evidencia de capacidad verificada requerida. El preview, no este plan, determina el conjunto exacto de reemplazos generados.
- [x] Revisar cada ruta y clasificación del preview. Exigir solo rutas candidatas de las familias nombradas de contratos generados, ningún archivo de backup o preferencias, ninguna migración ni eliminación y ningún cambio de esquema del manifest/checker. Tratar `.repo-harness/rules/memory.md` según la propiedad presentada por el preview; nunca asumir ni forzar reemplazo.
- [x] Solo después de autorización separada de implementación y apply exacto de mantenimiento, aplicar el digest revisado sin cambios. Repetir preview y exigir que no quede acción mutante. Un preview bloqueado, cambiado o ampliado invalida la autoridad de apply y detiene la tarea.
- [x] Comparar cada contrato de sesión generado y autorizado por preview con la salida vigente de `render_with_skill_mapping()` y ejecutar `python .repo-harness/check.py`. Esperado: checker `current`, salida 0 y cero escrituras.

### Tarea 5: verificación final y cierre documental

- [x] Ejecutar el comando enfocado que cubre las siete pruebas nuevas contra el archivo canónico y luego `GeneratedCoreTests` contra las rutas de prueba canónica, Claude, OpenCode y Pi. Esperado: resultados aprobados iguales.
- [x] Ejecutar una vez la suite canónica completa: `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Esperado: salida 0. No repetir suites proyectadas byte-idénticas salvo fallo de paridad o control específico de runtime.
- [x] Ejecutar `python .repo-harness/check.py`. Esperado: `current`, salida 0 y cero escrituras.
- [x] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Esperado: cuatro pasan.
- [x] Los controles enfocados de paridad bilingüe cubren los pares de alcance, diseño, plan, mapas, roadmap, estado actual y changelog; pasó `git diff --check`.
- [x] La implementación verificada y la autoridad separada de ciclo reconciliaron evidencia bilingüe de ejecución, roadmap, estado actual, mapas y changelogs. RH-W-0091 está `completed` con trabajo restante `none`.

## Rollback

El rollback elimina solo las cláusulas canónicas RH-W-0091 y pruebas enfocadas, restaura primero bytes canónicos de fuente y pruebas y luego usa preview normal y apply autorizado por separado para sincronizar solo las proyecciones anteriores exactas y elegibles. Preserva cada backup y observación Engram existente, nunca sintetiza campos en backups antiguos, nunca elimina continuidad y nunca sobrescribe `.repo-harness/rules/memory.md` de propiedad humana sin una ruta respaldada por preview y autorizada por separado.

## Pronóstico de carga de revisión

Cambio autoral esperado: 140 a 220 líneas de fuente y pruebas. Cambio mecánico esperado de paquetes y proyecciones generadas: 180 a 320 líneas, con conjunto exacto determinado por preview de mantenimiento. Riesgo medio para orden, compatibilidad y preservación de propietarios, y bajo para complejidad de runtime porque no se agrega un subsistema ejecutable de continuidad. Revisar en cuatro unidades acotadas: forma del backup/orden de cierre; prioridad/disparadores de inicio incluido el caso negativo; preservación de capacidades/propietarios; luego proyecciones mecánicas y ciclo. Mantener cada RED junto a su GREEN. No se espera superar el objetivo aproximado de 400 líneas autorales; si el preview amplía la superficie autoral o las familias de reemplazo, detener y reacotar antes de apply.

## Aceptación, exclusiones y decisiones

- [x] Los backups nuevos usan la forma y orden exactos de reanudación primero; los backups antiguos siguen legibles sin campos sintetizados.
- [x] El inicio usa la prioridad exacta aprobada y solo los cuatro disparadores de lectura profunda nombrados.
- [x] La aceptación negativa demuestra que no ocurre lectura profunda sin uno de esos disparadores.
- [x] Permanecen intactos los propietarios vigentes de capacidades, backups, idioma, preferencias, divulgación progresiva, ciclo, privacidad y autoridad.
- [x] El preview de mantenimiento determina el conjunto exacto de reemplazos y se conserva contenido de propiedad humana.
- [x] No se introduce infraestructura nueva, parser/API, propietario de esquema, cache, daemon, servicio, scheduler, migración, limpieza, retención, framework de preferencias, prueba de Gentle, preflight de Engram ni segundo almacén.

Decisiones técnicas o de producto sin resolver: ninguna. La ejecución completada preservó los límites declarados y el fallo histórico de verificación como evidencia de corrección. El cierre de ciclo está completo con trabajo restante `none`; la entrega Git, commit, push, PR, merge, release y publicación siguen sin afirmarse.
