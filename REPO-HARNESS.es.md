# repo-harness-builder

Este documento es la presentacion humana en espanol de [`REPO-HARNESS.md`](REPO-HARNESS.md). El archivo en ingles sigue siendo el unico reglamento canonico y ejecutable.

## Proposito
<!-- doc-role:purpose -->

`repo-harness-builder` crea, audita, instala y evoluciona con seguridad sistemas acotados al repositorio de prompts, guardrails, reglas, documentacion durable, habilidades y comandos que asisten a agentes de programacion con IA. Las fuentes del producto viven en `skill/repo-harness-builder/` y `command/repo-harness-builder.md`; `.claude/`, `.opencode/` y `.pi/` son proyecciones de los entornos seleccionados. Este repositorio se autoaloja: usa el harness generado aqui para mejorar el propio builder. Los harness de consumidores siguen acotados a sus repositorios y no heredan identidad de builder.

## Reglas obligatorias
<!-- doc-role:hard-rules -->

- Trabaje solo dentro del repositorio seleccionado.
- Preserve archivos desconocidos y trabajo existente.
- Clasifique cada afirmacion como hecho verificado, inferencia respaldada, desconocida o no verificable, o decision humana requerida. Un hecho verificado necesita evidencia aplicable, confiable y proporcional; una inferencia nombra su evidencia y razonamiento; lo desconocido aplica cuando no existe un mecanismo valido; una decision humana aplica cuando la autoridad o un criterio corresponde a una persona. Nunca sustituya evidencia faltante, memoria obsoleta, indicadores indirectos no relacionados, supuestos ni lenguaje de confianza. Verifique antes de preguntar; si no es posible o hay contradiccion, nombre la capacidad faltante y solicite solo la evidencia, el criterio, la autoridad o la decision minima.
- No ejecute operaciones Git o remotas sin aprobacion explicita.
- Use [la guia de uso](docs/es/repo-harness/usage.md), [el estado actual](docs/es/repo-harness/current-state.md) y [las decisiones](docs/es/repo-harness/decisions.md).
- Ejecute `python3 .repo-harness/check.py` (`python` en Windows) cuando cambien fuentes canonicas o proyecciones.

## Compuertas de decision
<!-- doc-role:decision-gates -->

| Situacion | Lea o ejecute |
|---|---|
| Iniciar o reanudar trabajo | `.repo-harness/skills/repo-harness-session-start/SKILL.md` |
| Pausar o cerrar trabajo | `.repo-harness/skills/repo-harness-session-stop/SKILL.md` |
| Usar el harness como persona | [guia de uso](docs/es/repo-harness/usage.md) |
| Cambio una habilidad canonica | `python3 .repo-harness/check.py` (`python` en Windows) |
| Una respuesta requiere aviso, accion pendiente, pregunta, aprobacion, siguiente accion visible o explicacion contextual | `.repo-harness/rules/response-contract.md` |
| Planificar, implementar, revisar o integrar una contribucion | [CONTRIBUTING.es.md](CONTRIBUTING.es.md) |
| Consultar el estado y entregas | [estado actual](docs/es/repo-harness/current-state.md) y [CHANGELOG.md](CHANGELOG.md) |
| Consultar razones o restricciones | [decisiones](docs/es/repo-harness/decisions.md) |

## Actualizacion de este archivo
<!-- doc-role:maintenance -->

Actualice este par cuando cambie una regla, una ruta o una afirmacion en `REPO-HARNESS.md`. Las reglas operativas siguen siendo propiedad exclusiva del archivo canonico en ingles. Mantenga esta presentacion por debajo del mismo presupuesto de 700 tokens y lleve el detalle a los documentos enlazados.

No coloque estado fechado aqui. Los cambios completados pertenecen a `CHANGELOG.md`, el estado y siguiente paso a `docs/repo-harness/current-state.md`, y las razones durables a `docs/repo-harness/decisions.md`.
