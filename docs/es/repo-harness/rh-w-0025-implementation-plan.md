# Plan de implementación de la integración consciente de capacidades RH-W-0025

> **Para agentes ejecutores:** SUBHABILIDAD OBLIGATORIA: usar `test-driven-development` para cada corte de comportamiento y `verification-before-completion` antes de la entrega. Dar seguimiento a cada paso con su casilla. Trabajar solo en la unidad aprobada RH-W-0025.

**Objetivo:** Generar un harness genérico que coordine la autoridad del repositorio, la continuidad de Engram y las capacidades opcionales de Gentle AI sin copiar estado de propietarios ni crear otro ciclo de vida.

**Arquitectura:** El repositorio conserva la verdad durable. Las funciones existentes de generación emiten contratos acotados para modo integrado e independiente; cada propietario habilitado emite sus propios estados, aprobaciones, recibos, conflictos y transiciones. La implementación no agrega servicio, dependencia, coordinador persistido ni tercer estado. `integrated` significa Gentle AI más Engram, `standalone` significa que ninguno está disponible y la disponibilidad parcial se informa como degradación limitada al componente bajo la política del repositorio.

**Tecnologías:** Python 3.10+ con biblioteca estándar, `unittest`, contratos Markdown generados, manifiesto JSON y proyecciones autoalojadas `.opencode/` y `.claude/`.

**Entradas canónicas:** [investigación de integración](integration-research.md), [RH-W-0025](roadmap.md#rh-w-0025) y [decisiones permanentes](decisions.md). El [plan en inglés](../../repo-harness/rh-w-0025-implementation-plan.md) es el propietario canónico.

**Prerrequisito de ejecución:** RH-W-0025 está bloqueado por RH-W-0008. No ejecutar este plan hasta que RH-W-0008 entregue el ejecutor aprobado de reemplazo e integración ligado al contenido para salidas generadas existentes.

**Evidencia del bloqueo:** `plan_changes()` clasifica salidas existentes no idénticas como `existing`, y `write_outputs()` escribe solo acciones create/append más la migración acotada RH-W-0024 de nombres heredados. Esa migración no puede regenerar propietarios consumidores existentes arbitrarios. Ampliar RH-W-0025 duplicaría RH-W-0008 y crearía un tercer mecanismo de reemplazo.

## Límites de alcance

- [ ] Mantener genérica la redacción para consumidores. Nunca mencionar mejorar `repo-harness-builder` en la salida generada.
- [ ] Preservar la propiedad del repositorio sobre verdad durable, fuente, pruebas, política, backlog y aprobaciones.
- [ ] Usar Gentle AI y Engram juntos solo en modo `integrated`. Usar `standalone` solo cuando ninguno existe.
- [ ] Tratar una integración ausente o fallida como degradación del componente, no como modo nuevo ni veredicto agregado.
- [ ] Promover conocimiento operativo reutilizable solo después de verificarlo en el repositorio y obtener aprobación humana.
- [ ] No agregar tercer estado, habilitación automática de revisión, aprobación fabricada, SDD sintético, copias de almacenes de recibos, conflictos o intentos, ni reintentos hasta quedar limpio.
- [ ] Mantener RH-W-0022 relacionado e independiente. No agregar dependencia salvo evidencia de implementación.
- [ ] Tratar RH-W-0008 como prerrequisito obligatorio. No iniciar RH-W-0025 hasta que RH-W-0008 entregue el ejecutor aprobado de reemplazo e integración ligado al contenido.
- [ ] No agregar dependencia de runtime, daemon, scheduler, cliente de red, mutación de proveedor ni detección basada en layout privado o presencia de ejecutable.
- [ ] Preservar archivos humanos y trabajo no confirmado ajeno. Preparar solo rutas del commit acotado actual.

## Mapa de archivos previsto

| Ruta | Responsabilidad |
|---|---|
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Pruebas canónicas RED/GREEN para modos, autoridad, promoción, degradación y ausencia de duplicación |
| `skill/repo-harness-builder/scripts/harness_tool.py` | Funciones canónicas de generación y selección de modo existente |
| `skill/repo-harness-builder/SKILL.md` | Entrevista del builder y compuerta de capacidades |
| `skill/repo-harness-builder/references/audit-and-interview.md` | Caracterización de solo lectura y compuerta incierta de Gentle AI |
| `skill/repo-harness-builder/references/architecture.md` | Límites de propietarios y arquitectura de modos |
| `skill/repo-harness-builder/references/preservation-and-safety.md` | Restricciones de aprobación, promoción, reintentos y no copia |
| `skill/repo-harness-builder/references/validation.md` | Validación del harness generado y estados esperados |
| `command/repo-harness-builder.md` | Proyección canónica del comando si cambia el enrutamiento |
| `.opencode/skills/repo-harness-builder/**` | Proyección autoalojada de OpenCode |
| `.claude/skills/repo-harness-builder/**` | Proyección autoalojada de Claude |
| `.repo-harness/**`, proyecciones de sesión, manifiesto, checker, reglamento y docs durables generadas | Excluidos de edición directa en RH-W-0025; regenerar solo en el seguimiento posterior a RH-W-0008 mediante su ejecutor aprobado |
| `docs/repo-harness/{roadmap,current-state}.md`, pares españoles y `CHANGELOG.md` | Reconciliación solo después de implementar y verificar |

El conjunto consumidor exacto debe provenir de `TOOL.render(...)` después de completar RH-W-0008. No mantener otra lista, borrar propietarios para forzar creación ni editar propietarios generados. RH-W-0025 permanece bloqueado hasta que exista ese ejecutor.

## Tarea 1: Caracterizar el comportamiento actual con pruebas fallidas

**Archivos:** modificar `skill/repo-harness-builder/tests/test_harness_tool.py`; inspeccionar `render()`, `render_with_skill_mapping()`, `session_start_skill()`, `session_stop_skill()`, `documentation_skill()`, `rule_outputs()` y `docs_outputs()`.

- [ ] Agregar `CapabilityAwareIntegrationContractTests(unittest.TestCase)` y `combined(self, standalone: bool = False) -> str`, uniendo los valores de `TOOL.render("Consumer", [], standalone=standalone)`.
- [ ] Agregar `test_integrated_contract_names_repository_engram_and_gentle_ai_owners`, `test_standalone_contract_uses_repository_evidence_without_integration_claims` y `test_generated_contract_forbids_copied_authority_and_unbounded_retry` con las mismas frases y exclusiones del plan canónico.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`.
- [ ] Esperar salida distinta de cero por afirmaciones que demuestren la ausencia actual de contratos RH-W-0025. Errores de importación o sintaxis no son RED válido.
- [ ] Registrar qué rutas renderizadas fallan y confirmar si el manifiesto ya usa `integrated` y `standalone`, sin cambiar comportamiento ni ciclo de vida.

## Tarea 2: Fijar la evidencia de Gentle AI e implementar la compuerta no resuelta

**Archivos:** `skill/repo-harness-builder/references/audit-and-interview.md`, opcionalmente `skill/repo-harness-builder/SKILL.md`, y pruebas canónicas.

- [ ] Agregar primero `test_unresolved_gentle_ai_probe_requires_explicit_human_confirmation`. Debe afirmar la revisión fijada `4004a0825d03a3d123a3c4f67bf375cb16fd6d8c`, `explicit human confirmation`, `advertised read-only capability` y `never infer from executable presence`, y rechazar comandos, flags, rutas o layouts no verificados.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests.test_unresolved_gentle_ai_probe_requires_explicit_human_confirmation -v`. Resultado esperado: FAIL por ausencia del contrato; errores de importación, sintaxis o fixture no son RED válido.
- [ ] Actualizar `audit-and-interview.md`: la revisión autoritativa es `4004a0825d03a3d123a3c4f67bf375cb16fd6d8c` y no define una sonda genérica estable. Exigir confirmación humana explícita y prohibir inferencia desde ejecutable, proceso o layout privado.
- [ ] Ejecutar el mismo comando. Resultado esperado: PASS sin interfaz inventada.
- [ ] Una sonda posterior solo puede reemplazar esta rama mediante actualización explícita de investigación que adopte y fije una revisión nueva, reemplace la evidencia anterior y haga otro ciclo RED/GREEN. No reinterpretar silenciosamente `current`, `installed` ni `available`.

## Tarea 3: Agregar reglas y contratos generados mínimos y genéricos

**Archivos:** `harness_tool.py`, `architecture.md`, `preservation-and-safety.md` y pruebas canónicas.

- [ ] Agregar RED `test_generated_contract_assigns_each_fact_to_one_owner` y `test_generated_contract_stays_consumer_generic`.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Resultado esperado: FAIL solo por lenguaje contractual ausente.
- [ ] Actualizar los cuerpos existentes de `rule_outputs()`, `docs_outputs()`, `session_start_skill()`, `session_stop_skill()` y `documentation_skill()` donde corresponda. Reutilizar `standalone: bool`; no crear enum de tercer modo ni coordinador persistido.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Las pruebas de autoridad y lenguaje genérico deben pasar.

## Tarea 4: Implementar ramas integradas, parciales e independientes

**Archivos:** `harness_tool.py`, `SKILL.md`, `audit-and-interview.md` y pruebas canónicas.

- [ ] Agregar `test_integrated_render_instructs_all_four_runtime_availability_outcomes` sobre una sola llamada `TOOL.render("Consumer", [], standalone=False)`. Afirmar instrucciones estáticas para ambos disponibles, solo Gentle AI, solo Engram y ninguno; no modelarlos como cuatro entradas del generador.
- [ ] Agregar `test_standalone_render_is_the_neither_available_generated_mode` sobre `TOOL.render("Consumer", [], standalone=True)`. Afirmar manifiesto `standalone`, continuidad solo del repositorio y ninguna promesa de ejecución de Gentle AI o Engram; el render integrado conserva `integrated`.
- [ ] Agregar `import inspect` con los imports estándar, las pruebas de precedencia y `test_render_accepts_no_runtime_capability_detection_state`, inspeccionando las firmas de `render()` y `render_with_skill_mapping()` para prohibir parámetros o estado persistido de detección.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Resultado esperado: RED por instrucciones generadas ausentes.
- [ ] Describir las cuatro ramas como instrucciones de runtime dentro del contrato integrado. Mantener `standalone: bool` como única entrada de modo y solo `integrated` y `standalone` en el manifiesto. No agregar tercer modo, entrada de disponibilidad, caché, campo de manifiesto ni estado persistido de detección.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Resultado esperado: PASS.

## Tarea 5: Implementar promoción selectiva de conocimiento

**Archivos:** `harness_tool.py`, `preservation-and-safety.md` y pruebas canónicas.

- [ ] Agregar `test_promotion_requires_verification_relevance_owner_and_human_approval` para `verified`, `still relevant`, `canonical owner` y `human approval`.
- [ ] Agregar `test_incidental_and_changing_external_facts_are_not_durable_truth` para `incidental`, `revalidated` y `current as of`.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Resultado esperado: RED.
- [ ] Actualizar `documentation_skill()` y las reglas de memoria/documentación emitidas por `rule_outputs()` o `docs_outputs()`: recuperar, etiquetar, verificar, identificar propietario, pedir aprobación y actualizar en la misma unidad. Descartar ruido incidental y no crear cola de promoción.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Resultado esperado: PASS.

## Tarea 6: Agregar degradación por componente y salida de estado

**Archivos:** `harness_tool.py`, `validation.md` y pruebas canónicas.

- [ ] Agregar `test_component_outcomes_remain_distinct_and_offer_one_next_action`, `test_component_failure_never_claims_false_success` y `test_malformed_owner_status_stops_only_that_optional_branch`.
- [ ] Agregar `test_generated_output_has_no_copied_owner_stores_or_endless_retries` para rechazar caché de recibos, almacén de conflictos, ledger de intentos, aprobación combinada y loops hasta quedar limpio.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Resultado esperado: RED.
- [ ] Actualizar contratos existentes con resultado del repositorio, resultados de Gentle AI y Engram, razones de degradación y una siguiente acción. No agregar esquema agregado salvo que un contrato propietario actual lo exija.
- [ ] Ejecutar `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Resultado esperado: PASS.

## Tarea 7: Probar estabilidad del contrato y manifiesto mínimo

**Archivos:** pruebas canónicas y `harness_tool.py` solo si las pruebas lo demuestran necesario.

- [ ] Agregar `test_manifest_keeps_only_integrated_and_standalone_modes`, `test_existing_runtime_locale_and_pointer_outputs_remain_stable` y `test_render_is_deterministic_for_each_mode`.
- [ ] Afirmar ausencia de almacenes copiados, promesas falsas en standalone, caracteres prohibidos y cambios de runtimes, idiomas o punteros.
- [ ] Ejecutar `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"`.
- [ ] Esperar salida 0, todas las pruebas aprobadas y solo omisiones soportadas. Detenerse ante fallos ajenos sin cambiar comportamiento no relacionado.

## Tarea 8: Sincronizar por copia exacta las proyecciones del paquete del builder

**Archivos:** siete rutas exactas: `SKILL.md`, `scripts/harness_tool.py`, `tests/test_harness_tool.py`, `references/architecture.md`, `references/audit-and-interview.md`, `references/preservation-and-safety.md` y `references/validation.md`; fuente `skill/repo-harness-builder/`, destinos equivalentes bajo `.opencode/skills/repo-harness-builder/` y `.claude/skills/repo-harness-builder/`.

- [ ] Antes de copiar, ejecutar la comprobación SHA-256 indicada abajo. Debe fallar en al menos una ruta RH-W-0025; salida 0 significa que no hay trabajo de proyección.
- [ ] Copiar desde la raíz con `$paths = @("SKILL.md", "scripts/harness_tool.py", "tests/test_harness_tool.py", "references/architecture.md", "references/audit-and-interview.md", "references/preservation-and-safety.md", "references/validation.md"); foreach ($path in $paths) { Copy-Item -LiteralPath "skill/repo-harness-builder/$path" -Destination ".opencode/skills/repo-harness-builder/$path" -Force; Copy-Item -LiteralPath "skill/repo-harness-builder/$path" -Destination ".claude/skills/repo-harness-builder/$path" -Force }`.
- [ ] Verificar bytes con `$paths = @("SKILL.md", "scripts/harness_tool.py", "tests/test_harness_tool.py", "references/architecture.md", "references/audit-and-interview.md", "references/preservation-and-safety.md", "references/validation.md"); foreach ($path in $paths) { if ((Get-FileHash -Algorithm SHA256 -LiteralPath "skill/repo-harness-builder/$path").Hash -ne (Get-FileHash -Algorithm SHA256 -LiteralPath ".opencode/skills/repo-harness-builder/$path").Hash) { throw "OpenCode projection mismatch: $path" }; if ((Get-FileHash -Algorithm SHA256 -LiteralPath "skill/repo-harness-builder/$path").Hash -ne (Get-FileHash -Algorithm SHA256 -LiteralPath ".claude/skills/repo-harness-builder/$path").Hash) { throw "Claude projection mismatch: $path" } }`. Esperar salida 0 sin texto.
- [ ] Ejecutar las tres suites con `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"`, y los comandos equivalentes para `.opencode/skills/repo-harness-builder/tests` y `.claude/skills/repo-harness-builder/tests`. Esperar salida 0 y totales coincidentes.
- [ ] Mantener separada la generación consumidora. No regenerar `.repo-harness/**`, proyecciones de sesión, manifiesto, checker, reglamento ni docs durables con el writer aditivo actual. Después de completar RH-W-0008, abrir un seguimiento acotado que use su interfaz real para preview ligado al contenido, aprobación humana, digest, apply sin cambios y verificación. No inventar ahora comandos futuros ni sustituirlos con `Copy-Item`, borrado o edición manual.

## Tarea 9: Reconciliar documentación bilingüe y ciclo de vida después de implementar

**Archivos:** `usage.md` y `decisions.md` con sus pares solo después del seguimiento de regeneración; `roadmap.md`, `current-state.md`, pares españoles y `CHANGELOG.md` al cierre verificado; preservar ambas investigaciones.

- [ ] No editar a mano propietarios durables generados. Después de RH-W-0008, el seguimiento aprobado debe regenerar selección, modos, degradación, promoción y siguiente acción, con inglés propietario y español espejo completo.
- [ ] Solo después de completar RH-W-0008, verificar RH-W-0025 y terminar el seguimiento de regeneración, cambiar RH-W-0025 de `blocked` a `completed`, usar trabajo restante `none`, agregar evidencia, actualizar estado y changelog. No cambiar RH-W-0022.
- [ ] Actualizar Engram `work/RH-W-0025` solo con implementación y regeneración verificadas. No marcar completo mientras RH-W-0008 siga incompleto.
- [ ] Ejecutar suite canónica y `python .repo-harness/check.py`. Esperar pruebas aprobadas, checker `current`, salida 0 y `writes: 0`.

## Tarea 10: Verificación completa, rollback y límites de entrega

**Archivos:** verificar todas las rutas cambiadas; no modificar GitHub, sistemas externos, registros ajenos ni consumidores no aprobados.

- [ ] Ejecutar las tres suites, `python .repo-harness/check.py` y `git diff --check`. Esperar suites aprobadas con totales iguales, checker actual y salida vacía de diff check.
- [ ] Validar enlaces relativos, estructura y semántica bilingüe, pruebas de documentación/roadmap y ausencia de U+2013/U+2014.
- [ ] Inspeccionar `git status --short`, `git diff --stat` y el diff limitado a rutas RH-W-0025. Rechazar dependencias, secretos, `.codegraph/`, proveedor, comportamiento RH-W-0022 o cambios ajenos.
- [ ] Ante fallo, restaurar solo ediciones RH-W-0025 desde el patch o snapshot del ejecutor. Nunca usar `git reset --hard` ni `git checkout --`. Usar rollback acotado del apply y detenerse tras fallo repetido.
- [ ] Límite preferido del commit de implementación: `feat: add capability-aware integration contracts`. Incluir fuente, pruebas y las siete proyecciones del paquete. Excluir propietarios consumidores y docs generadas hasta el seguimiento posterior a RH-W-0008, que mantiene regeneración, verificación, ciclo de vida bilingüe y changelog en su propia unidad. No hacer commit sin solicitud explícita.

## Pronóstico

- Archivos esperados antes de regeneración: 15 a 21 archivos canónicos, de pruebas, referencias y proyecciones del paquete.
- Líneas esperadas antes de regeneración: 350 a 650. El seguimiento posterior a RH-W-0008 pronostica sus propietarios desde el preview ligado al contenido de ese ejecutor.
- Riesgos principales: ausencia de sonda estable de Gentle AI, crecimiento contractual, copia accidental de autoridad, deriva de proyecciones y cierre prematuro del ciclo de vida.
- Unidad de rollback: patch RH-W-0025 completo, usando rollback del apply ante escritura generada parcial.

## Lista de aceptación del plan

- [ ] La Tarea 1 demuestra comportamiento actual con RED legítimo antes de editar producto.
- [ ] La sonda de Gentle AI es una compuerta verificada, nunca una interfaz inventada.
- [ ] Las cuatro combinaciones tienen cobertura sin tercer modo.
- [ ] Precedencia del repositorio, promoción selectiva, aprobación humana y degradación por componente están probadas.
- [ ] No hay aprobación fabricada, SDD sintético, almacenes copiados, veredicto agregado ni reintento infinito.
- [ ] Fuente, pruebas y siete proyecciones del paquete forman la unidad de implementación; propietarios consumidores y cierre de ciclo de vida permanecen juntos en el seguimiento habilitado por RH-W-0008.
- [ ] RH-W-0022 permanece relacionado e independiente.
