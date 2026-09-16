# Plan de implementacion RH-W-0046 de consentimiento para fetch

> **Estado: registro histórico de ejecución completada.** RH-W-0046 está `completed`; RH-W-0018 sigue `deferred` y fuera de alcance. La reconciliación de ciclo, la verificación final y la aprobación nativa están completas. No ocurrieron acciones Git, de red, commit, push, PR, entrega, release ni publicación.

Derivado del [diseno](rh-w-0046-rh-w-0018-design.md) aprobado por el propietario. Par canonico: [rh-w-0046-implementation-plan.md](../../repo-harness/rh-w-0046-implementation-plan.md).

## Propietarios exactos y limite

`skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` es el unico propietario de comportamiento. `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` posee las regresiones del contrato renderizado; `render()` y `render_with_skill_mapping()` proyectan cambios canonicos autorizados. Se conserva la aprobacion de red predeterminada. Solo se agrega la concesion aprobada, visible y revocable de `git-fetch`, para un repositorio, un remoto y refs exactas, en `.repo-harness/state/preferences/fetch-consent.md`.

No incluye comportamiento RH-W-0018, framework general de preferencias, servicio de parser, daemon, base de datos, cache, expiracion programada, limpieza automatica, autoridad de entrega Git ni autoridad de proveedor.

## Tarea 1: prueba enfocada consolidada (ejecución histórica completada)

- [x] Se implementó y pasó `GeneratedCoreTests.test_session_start_fetch_consent_schema_is_exact_and_fail_closed`.
- [x] La prueba enfocada consolidada verifica el esquema exacto del archivo fijo, los campos base64url canónicos, el manejo de fallo cerrado para datos inválidos/diferentes/ambiguos y el fallback interactivo.
- [x] Los cuatro nombres de métodos planificados arriba se consolidaron en esta única prueba enfocada; no se crearon como cuatro métodos separados.
- [x] Se ejecutó correctamente `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_start_fetch_consent_schema_is_exact_and_fail_closed -v`.

## Tarea 2: implementación mínima y límite de rollback (ejecución histórica completada)

- [x] Se extendió solo `session_start_skill()` con el límite de consentimiento del archivo fijo, incluida creación o reemplazo solo por instrucción humana actual y explícita, invalidación, uso visible, revocación y fetch exacto.
- [x] Se preservó el límite de que leer o usar una concesión nunca la crea, renueva, reescribe, extiende, borra ni transfiere; el reemplazo nunca se infiere ni es automático.
- [x] El rollback permanece histórico y acotado: revertir juntas las cláusulas exactas de `session_start_skill()` y su prueba correspondiente; nunca borrar automáticamente una concesión local ni alterar configuración Git, historial, remotos o credenciales.

## Tarea 3: proyecciones establecidas y reconciliación de ciclo (completada)

- [x] Se sincronizó el candidato autorizado por separado mediante las proyecciones de runtime establecidas.
- [x] Se completaron verificación final, aprobación nativa y reconciliación de roadmap, estado actual, mapas, pares en español, changelog y tema de continuidad `work/RH-W-0046`.
- [x] RH-W-0046 está `completed`; RH-W-0018 sigue `deferred` con su implementación y condición de avance separadas.

## Verificación acotada

- [x] Pasó la prueba enfocada consolidada real: `GeneratedCoreTests.test_session_start_fetch_consent_schema_is_exact_and_fail_closed`.
- [x] Evidencia independiente de suite completa: 547 passed/10 skipped; checker `current` con 0 escrituras; paridad exacta de proyecciones y `git diff --check` limpio.
- [x] El linaje de aprobación nativa `review-9d5afc68be68d0be` fue reconocido con autoridad consumida para el objetivo `sha256:628449dd9144825559da6617e237b3d09a80dbd851c39522e6f57ab54b3c5e23`.
- [x] No se seleccionaron acceso de red, fetch real, entrega Git ni comportamiento RH-W-0018.

## Carga de revisión y decisiones

La revisión completada permaneció acotada al límite de consentimiento: primero invalidación y exclusiones, luego la prueba enfocada consolidada y la verificación independiente. Se retienen la aprobación histórica del plan y la autoridad separada de implementación. RH-W-0046 está completada; reabrir solo ante una regresión de este límite exacto de consentimiento. RH-W-0018 sigue `deferred` y fuera de alcance. No ocurrieron acciones Git, de red, commit, push, PR, entrega, release ni publicación.
