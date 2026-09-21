# Plan de implementación RH-W-0052 solo de archivo

> **Estado de ejecución: porción solo de archivo completa, verificada y entregada mediante issue #177 y PR #178. Ciclo de la unidad: `deferred`.** El [registro de la hoja de ruta](roadmap.md#rh-w-0052) posee el aplazamiento residual y la reentrada. Las instrucciones históricas de las tareas ejecutadas permanecen intactas; este registro no concede autoridad nueva de implementación, entrega, red, limpieza ni operación destructiva.

Derivado del [diseño solo de archivo](rh-w-0052-design.md) aprobado por el propietario. Propietario canónico en inglés: [rh-w-0052-implementation-plan.md](../../repo-harness/rh-w-0052-implementation-plan.md). El [registro de la hoja de ruta](roadmap.md#rh-w-0052) conserva la propiedad del ciclo de vida.

## Resultado y límites

Implementar solo una operación explícita de copia de archivo: una fuente regular directa, relativa al repositorio y nombrada puede producir una copia nueva bajo el namespace existente `.repo-harness/archive/` cuando un manifest actual autenticado pruebe que esa fuente exacta es propiedad del harness. Inspección y vista previa no escriben. Una aprobación explícita actual cubre solo el mapeo fuente-a-destino-nuevo mostrado; un cambio material de fuente, destino, propiedad o evidencia la invalida. Apply crea solo la copia nueva aprobada y preserva la fuente.

Toda prueba de propiedad sin resolver, ruta insegura o indirecta, fuente fuera de la raíz o dentro del namespace de archivo, colisión de destino, evidencia de aprobación cambiada o padre de destino ausente/indirecto/no-directorio rechaza sin escribir. La cadena exacta de padres debe preexistir como directorios reales directos; apply no ejecuta `mkdir`, borrado de rollback ni limpieza. Este mínimo seleccionado por el propietario evita la combinación imposible de padres ausentes, colisión de carrera, rechazo con cero escrituras y ausencia de borrado de rollback. La operación no supone Git y preserva todas las rutas ajenas, desconocidas, de estado local, humanas y de herramientas, incluidas `.atl/`, `.codegraph/` y `.engram/`.

Este plan no agrega desinstalación, reinstalación, borrado, movimiento, sobrescritura, limpieza, migración, base de datos, ledger, daemon, scheduler, framework genérico de archivo, schema de manifest, formato de digest, subsistema transaccional ni contrato de comando generado. No cambia la propiedad del namespace de RH-W-0051 ni la conducta de recuperación de RH-W-0048.

## Propietarios inspeccionados y superficies probables

| Superficie | Responsabilidad planificada |
| --- | --- |
| `skill/repo-harness-builder/scripts/harness_tool.py::guarded_target()` | Reutilizar la contención de raíz y el rechazo de ancestros indirectos existentes; no crear una guarda paralela de rutas. |
| `skill/repo-harness-builder/scripts/harness_tool.py::authentic_installed_checker()`, `manifest_owned_generated_paths()` y `generated_skill_paths()` | Primero probar actualidad del checker por igualdad exacta con `check_script(digest_bytes(manifest_bytes))`; después usar los helpers existentes para autenticidad y cobertura exacta del manifest. Evidencia auténtica pero obsoleta es `ownership-unresolved`; evidencia sin resolver sigue siendo inelegible. |
| Vecindad de `skill/repo-harness-builder/scripts/harness_tool.py::plan_changes()` | Alojar una ruta acotada de inspección/vista previa/apply solo de archivo, sin cambiar las rutas existentes de migración de salidas generadas ni de recuperación. La invocación pública exacta sigue siendo un detalle de implementación que se seleccionará solo bajo autoridad de implementación. |
| `skill/repo-harness-builder/tests/test_harness_tool.py::ArchiveOnlyTests` | Agregar las pruebas enfocadas de contrato a nivel de comportamiento como una clase nueva, manteniendo fuera de alcance las pruebas de migración heredada y su conducta de eliminación. |
| `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.pi/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` | Proyecciones mecánicas idénticas byte a byte solo después de GREEN canónico. No se planifica cambio de skill de sesión generada, wrapper, manifest ni checker salvo que la vista previa autorizada pruebe que una proyección administrada existente debe cambiar. |

El repositorio inspeccionado ya tiene `safe_relative()`, `guarded_target()`, igualdad exacta de checker actual mediante `check_script(digest_bytes(manifest_bytes))`, autenticación mediante `authentic_installed_checker()`, rutas generadas exactas derivadas del manifest, pruebas de vista previa/apply ligadas a digest y regresiones de rutas directas/indirectas. Son evidencia para reutilización, no autoridad para reutilizar la eliminación de migración heredada, reparación ni conducta Git alguna.

## Tarea 1: RED/GREEN A — elegibilidad exacta e inspección/vista previa con cero escrituras

- [ ] Agregar `ArchiveOnlyTests.test_archive_inspect_accepts_only_one_explicit_direct_regular_manifest_owned_source`, `test_archive_inspect_reports_ownership_unresolved_without_writing` y `test_archive_preview_emits_one_exact_mapping_with_zero_writes`.
- [ ] Establecer RED con un fixture de manifest/checker instalado y autenticado. Afirmar que solo es elegible una fuente regular directa nombrada explícitamente y cubierta por el manifest autenticado actual; evidencia ausente, malformada, auténtica pero obsoleta, obsoleta, sin cobertura o no autenticada informa `ownership-unresolved` y deja sin cambios cada byte del fixture.
- [ ] Afirmar que inspección y vista previa rechazan fuentes internas al archivo, que escapan, indirectas, no regulares o descubiertas implícitamente; la inspección solo lee el candidato nombrado y evidencia aplicable de propiedad/destino.
- [ ] Afirmar que la vista previa devuelve exactamente un mapeo de fuente a destino-nuevo-de-archivo y `writes: 0`, sin crear directorio, copiar, mover, editar manifest/checker, operar Git ni escanear rutas ajenas o de herramientas.
- [ ] Implementar el camino acotado mínimo usando las guardas y evidencia de propiedad inspeccionadas. No alterar la conducta de migración de `plan_changes()` ni agregar un campo al manifest.
- [ ] Repetir la selección enfocada. GREEN exige que solo pasen estas aserciones de elegibilidad y cero escrituras.

## Tarea 2: RED/GREEN B — límite exacto de aprobación y rechazo

- [ ] Agregar `ArchiveOnlyTests.test_archive_preview_refuses_collision_and_unsafe_destination_without_writing` y `test_archive_approval_is_invalidated_by_material_mapping_or_ownership_change`.
- [ ] Establecer RED para colisión de destino, destino inseguro, cambio de bytes de fuente, cambio de destino y cambio de evidencia de propiedad después de la vista previa. Cada caso debe rechazar; no debe sobrescribir, seleccionar otro destino ni heredar una aprobación anterior.
- [ ] Afirmar que la aprobación es actual, explícita y ligada solo al mapeo exacto previsualizado. Ninguna sesión, Git, manifest, reparación ni aprobación previa de archivo aporta autoridad.
- [ ] Implementar la comparación mínima necesaria para restablecer los hechos previsualizados inmediatamente antes de apply. No definir formato nuevo de aprobación, formato de digest ni subsistema transaccional.
- [ ] Repetir la selección enfocada. GREEN exige que pasen todos los casos de rechazo e invalidación con cero destinos de archivo creados.

## Tarea 3: RED/GREEN C — preservación de apply y evidencia de fallo

- [ ] Agregar `ArchiveOnlyTests.test_archive_apply_creates_only_the_approved_new_copy_and_preserves_source` y `test_archive_apply_refusal_preserves_local_unknown_and_tool_owned_paths`.
- [ ] Establecer RED con instantáneas de bytes de la fuente, rutas ajenas, cada ruta de estado local preexistente y rutas representativas `.atl/`, `.codegraph/` y `.engram/`. En éxito, afirmar una copia nueva completa de archivo y preservación byte a byte de cada instantánea; en todo rechazo, afirmar ningún destino nuevo y ninguna instantánea cambiada.
- [ ] Afirmar que apply vuelve a comprobar tipo de fuente regular directa, contención, cobertura por manifest actual autenticado, ausencia de destino y aprobación actual exacta inmediatamente antes de la única creación.
- [ ] Implementar solo el camino exitoso de una copia y el informe acotado de rechazo. No mover, borrar, sobrescribir, normalizar, reparar ni limpiar la fuente o rutas ajenas, y no introducir borrado como rollback.
- [ ] Repetir la selección enfocada y después ejecutar `ArchiveOnlyTests -v`. GREEN exige que pase la clase.

## Tarea 4: verificación enfocada y condiciones de proyección mecánica

- [ ] Después de GREEN enfocado, ejecutar la suite estándar de biblioteca estándar canónica exigida por `CONTRIBUTING.md` para cambios de producto: `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"`.
- [ ] Copiar solo los bytes canónicos de script y prueba a los tres pares exactos de proyección de paquete nombrados arriba; verificar igualdad de bytes en las cuatro raíces de paquete. No sincronizar skills de sesión generadas, wrappers, manifests ni checkers salvo que la vista previa de mantenimiento autorizada identifique una consecuencia administrada existente.
- [ ] Bajo autoridad separada de implementación y mantenimiento, inspeccionar la vista previa normal de mantenimiento antes de cualquier apply. No debe mostrar acción sobre fuente de archivo, estado local, ruta de herramientas, borrado, migración ni ruta ajena. Aplicar solo una vista previa sin cambios y aprobada por separado; repetir vista previa y exigir que no quede acción administrada.
- [ ] Ejecutar `python .repo-harness/check.py`; exigir `current`, salida 0 y `writes: 0`. Ejecutar `git diff --check` y detenerse ante errores de espacios. Ninguna evidencia de repositorio Git, índice, historia o red es prerrequisito ni entrada de prueba.
- [ ] Si falla la suite completa, paridad de proyección, vista previa autorizada o condición de checker, detenerse antes de reconciliar ciclo. No ampliar la implementación para reparar deriva ajena.

## Tarea 5: límite de documentación, ciclo y revisión

- [ ] Solo después de implementación verificada y autoridad de ciclo separada, reconciliar la hoja de ruta bilingüe, proyección de estado actual, mapas de documentación, pares de este plan y diseño, ambos changelogs y el tema de continuidad `work/RH-W-0052` si su capacidad está disponible. Mantener el registro de la hoja de ruta `proposed` hasta que se autorice por separado su avance.
- [ ] Registrar solo recibos de implementación verificados en el changelog. Aprobación del plan, implementación, avance de ciclo, trabajo de issue/PR, commit, push, publicación y red siguen siendo decisiones distintas del propietario.
- [ ] Revisar Tarea 1, Tarea 2 y Tarea 3 como unidades de comportamiento separadas. Mantener cada RED junto a su GREEN y cada unidad canónica autoral por debajo del objetivo aproximado de 400 líneas cambiadas del repositorio cuando sea práctico. Dividir una implementación futura solo si el diff enfocado observado supera ese límite revisable o una consecuencia de proyección es material de forma independiente.

## Evidencia verificada de ejecución con TDD estricto

Las tareas anteriores se conservan como el plan histórico aprobado. La implementación separada solo de archivo completó sus tres grupos RED/GREEN originales. La revisión independiente informó primero un hallazgo alto y uno bajo. La corrección pasó después por RED y GREEN; un caso de directorio en carrera produjo un hallazgo medio. El propietario seleccionó la corrección de precondición de padres descrita arriba, seguida de corrección final y PASS independiente.

Recibos finales: `ArchiveOnlyTests` 9 passed/1 skipped; pruebas vecinas 132 passed/2 skipped; suite canónica completa 545 passed/10 skipped. El checker informó `current`, salida 0, `writes: 0` y 63 proyecciones. Los bytes de paquetes canónico, Claude, OpenCode y Pi tienen paridad exacta; `git diff --check` quedó limpio. El caso Windows de enlace simbólico de fuente se omitió por falta de privilegio, mientras el rechazo de padre junction se ejerció correctamente. La revisión nativa se detuvo antes de START porque los assets administrados de Pi estaban desactualizados; no produjo linaje ni veredicto.

## Preservación, rollback y decisiones restantes

La evidencia de preservación es la comparación byte a byte antes/después de la fuente explícita y todas las rutas protegidas del fixture. Un fallo o rechazo prueba preservación dejando sin cambios esos bytes y todo destino de archivo preexistente; este plan no autoriza limpieza ni borrado. Un apply exitoso prueba preservación agregando solo el destino de archivo aprobado y reteniendo la fuente.

No queda abierta ninguna decisión de diseño para la porción solo de archivo, ni trabajo de implementación para esa porción. El propietario seleccionó la precondición de padres reales directos después del hallazgo de directorio en carrera. Los detalles de implementación no seleccionados por el diseño —incluidos la forma de invocación pública y la representación de nombres de destino de archivo— siguen mínimos, deterministas y revisables sin convertirse en schema ni framework. RH-W-0052 está `deferred`, no completada ni rechazada: desinstalación, reinstalación y borrado residuales esperan la [condición de reentrada de la hoja de ruta](roadmap.md#rh-w-0052). El diseño y la implementación futuros requieren autoridad separada; la porción solo de archivo entregada no necesita más trabajo.
