# repo-harness-builder

Este documento es la presentacion humana en espanol de [`REPO-HARNESS.md`](REPO-HARNESS.md). El archivo en ingles sigue siendo el unico reglamento canonico y ejecutable.

## Proposito
<!-- doc-role:purpose -->

`repo-harness-builder` crea, audita, instala y evoluciona con seguridad sistemas acotados al repositorio de prompts, guardrails, reglas, documentacion durable, habilidades y comandos que asisten a agentes de programacion con IA. Las fuentes del producto viven en `skill/repo-harness-builder/`; `.claude/`, `.opencode/` y `.pi/` son proyecciones de los entornos seleccionados. Este repositorio se autoaloja: usa el harness generado aqui para mejorar el propio builder. Los harness de consumidores siguen acotados a sus repositorios y no heredan identidad de builder.

## Reglas obligatorias
<!-- doc-role:hard-rules -->

- Trabaje solo dentro del repositorio seleccionado.
- Pida aprobacion explicita antes de leer o modificar rutas fuera de el. Diga por que y para que.
- Aplique el cambio minimo suficiente.
- No introduzca alcance no relacionado.
- No reintente indefinidamente. Tras fallos repetidos, detengase con evidencia y solicite una decision.
- No continue automaticamente hacia tareas no solicitadas.
- Ejecute el trabajo autorizado que anuncio; no se detenga en el anuncio.
- Preserve archivos desconocidos y trabajo existente mediante una evolucion informada y aprobada explicitamente; nunca congele contenido por defecto.
- Contraste la evidencia propietaria aplicable; nunca dependa de una sola fuente. Clasifique cada afirmacion como hecho verificado, inferencia respaldada, desconocida o decision humana requerida, y nunca sustituya evidencia por confianza.
- Gentle AI y Engram son capacidades opcionales e independientes; la evidencia del repositorio sigue siendo autoritativa y solo bloquean el trabajo las fallas de capacidades que la operacion requiere.
- Mantenga el consentimiento Git acotado al resultado y conversacional. Una intencion combinada puede cubrir requisitos previos seguros y sin cambios; los cambios de proveedor, el respaldo, la publicacion, las acciones destructivas y los cambios materiales de riesgo requieren aprobacion separada.

## Compuertas de decision
<!-- doc-role:decision-gates -->

| Situacion | Lea o ejecute |
|---|---|
| Iniciar o reanudar trabajo | `.repo-harness/skills/repo-harness-session-start/SKILL.md` |
| Pausar o cerrar trabajo | `.repo-harness/skills/repo-harness-session-stop/SKILL.md` |
| Usar el harness como persona | [guia de uso](docs/es/repo-harness/usage.md) |
| Cambio una habilidad canonica o una copia de entorno puede estar desactualizada | `python3 .repo-harness/check.py` (`python` en Windows) |
| Dimensionar un cambio, decidir cuando parar, como se trabaja | `.repo-harness/rules/working-agreements.md` |
| Guardar o recuperar entre sesiones, memoria no disponible | `.repo-harness/rules/memory.md` |
| Una respuesta requiere aviso, accion pendiente, pregunta, aprobacion, siguiente accion visible o explicacion contextual | `.repo-harness/rules/response-contract.md` |
| Crear, mover o borrar un documento | [mapa de documentacion](docs/es/repo-harness/README.md) |
| Planificar, implementar, revisar o integrar una contribucion | [CONTRIBUTING.es.md](CONTRIBUTING.es.md) |
| Consultar el estado y entregas | [estado actual](docs/es/repo-harness/current-state.md) y [CHANGELOG.es.md](CHANGELOG.es.md) |
| Seguir o cambiar trabajo pendiente o resuelto no trivial | [hoja de ruta](docs/es/repo-harness/roadmap.md) |
| Consultar razones o restricciones | [decisiones](docs/es/repo-harness/decisions.md) |
| Componer o cambiar prompts o instrucciones del repositorio | [regla de prefijos estables](skill/repo-harness-builder/references/gentle-ai-style-guide.md#stable-prefixes) |

## Actualizacion de este archivo
<!-- doc-role:maintenance -->

Actualice este par cuando cambie una regla, una ruta o una afirmacion en `REPO-HARNESS.md`. Las reglas operativas siguen siendo propiedad exclusiva del archivo canonico en ingles. El presupuesto de 700 tokens aplica al archivo canonico en ingles; esta presentacion puede excederlo para seguir completa, sin agregar detalle que pertenece a los documentos enlazados.

No coloque estado fechado aqui. Los cambios completados pertenecen a `CHANGELOG.md`, el estado y siguiente paso a `docs/repo-harness/current-state.md`, y las razones durables a `docs/repo-harness/decisions.md`.
