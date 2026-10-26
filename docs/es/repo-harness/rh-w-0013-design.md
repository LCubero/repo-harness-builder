# Diseno minimo RH-W-0013 de presupuesto de contexto

## Contrato de revision

Revisar este diseno contra el [registro canonico RH-W-0013](../../repo-harness/roadmap.md#rh-w-0013), el [Plan 16](../../repo-harness/roadmap.md#plan-16-rh-w-0013-measured-context-budgets), la [guia de estilo de skills](../../../skill/repo-harness-builder/references/skill-style-guide.md), que permanece inmutable, y el comportamiento actual del generador y del checker. Debe conservar equivalencia semantica con el [diseno en ingles](../../repo-harness/rh-w-0013-design.md).

El estado es `ready`. Este documento no aprueba implementacion, otra transicion de ciclo de vida, commit, publicacion ni acceso externo.

## Hallazgo de arquitectura

Solo `REPO-HARNESS.md` se carga siempre. Los archivos bajo `.repo-harness/rules/` y las tres skills diarias son rutas condicionales. RH-W-0013 no debe imponer informes de presupuesto en ejecucion sobre esas superficies ni refactorizarlas como si se cargaran siempre.

El `token_proxy()` existente y `RULEBOOK_TOKEN_BUDGET = 700` siguen siendo el mecanismo completo de medicion. Las pruebas de plantilla existentes conservan el reglamento generado y los archivos de reglas por debajo de 700 durante el desarrollo.

## Decision

Los tramos aprobados por el propietario permanecen como guia documental:

| Tokens estimados | Guia |
|---:|---|
| 180 a 450 | Objetivo |
| 451 a 700 | Aceptable |
| 701 a 1000 | Warning |
| 1001 a 1200 | Atencion prioritaria |
| Mas de 1200 | Consejo de revision inmediata |

Ningun tramo bloquea render, cambia el estado del checker o del archivo, ni crea metadata en ejecucion. RH-W-0067 posee la ubicacion de reglas de respuesta, su fila de gate y el margen del reglamento.

## Limite de implementacion

La implementacion cambia exactamente dos efectos en `skill/repo-harness-builder/scripts/harness_tool.py`:

1. Retirar de `render_with_skill_mapping()` el loop con el `require()` especifico del presupuesto, para que una salida de exactamente 700 o mas pueda renderizar.
2. Retirar de `check()` la rama `over-budget`, para que un archivo humano estructuralmente valido permanezca `human-evolved` cuando sus bytes difieren, o `current` cuando coinciden.

Retirar `budgeted()` solo si la implementacion demuestra que no quedan callers despues de ambos cambios. Conservar `token_proxy()`, `RULEBOOK_TOKEN_BUDGET` y las pruebas de presupuesto de desarrollo para el reglamento y los archivos de reglas.

## Exclusiones explicitas

- Sin informe ni schema en ejecucion.
- Sin `contextBudget` ni campo de umbral.
- Sin parser Markdown, ranking de secciones ni identificadores de recomendacion.
- Sin propagacion por preview o apply.
- Sin referencias generadas ni refactor de skills diarias.
- Sin cambios de manifest, checker generado, comandos, prompts ni wrappers.
- Sin cambios a la guia de estilo vendida.
- Sin trabajo de ubicacion de reglas de respuesta ni margen de RH-W-0067.

## Evidencia requerida

RED debe probar los dos defectos actuales:

- Un reglamento renderizado cuyo `token_proxy()` es exactamente 700 falla en `render_with_skill_mapping()` con el `ContractError` actual de presupuesto.
- Un archivo humano presupuestado y evolucionado de 700 o mas queda `over-budget` y hace que `check()` informe `drift`, en vez de conservar `human-evolved` y estado general `current`.

GREEN debe probar solo los cambios previstos:

- Fixtures de 700 exactos y 701 exactos renderizan sin rechazo de presupuesto.
- El mismo fixture humano conserva `human-evolved`, estado general `current` y `writes: 0`.
- Los fallos reales de secciones, locale, bytes generados, atributos, archivos ausentes y rutas indirectas conservan su comportamiento.
- Las pruebas de plantilla existentes para reglamento y reglas siguen pasando.
- Script y pruebas canonicas son byte a byte identicos a sus proyecciones de OpenCode y Claude.

## Decisiones humanas restantes

No queda ninguna decision de producto. Implementar y avanzar el ciclo de vida todavia exige autorizacion y evidencia separadas.
