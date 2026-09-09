# Investigación de integración de Gentle AI y Engram

## Pregunta y alcance

¿Cómo debe un harness generado usar Gentle AI y Engram de forma orgánica sin crear otra autoridad ni otro ciclo de vida? Este documento registra únicamente orientación durable de investigación. No implementa comportamiento, promueve trabajo de ciclo de vida ni cambia el estado del producto.

Los harnesses generados para repositorios consumidores permanecen genéricos y nunca mencionan mejorar `repo-harness-builder`. Este repositorio autoalojado aplica la misma regla genérica a su propia fuente de producto.

## Revisiones fijadas

| Fuente | Revisión |
|---|---|
| `repo-harness-builder` | [`58f41a790556d9dbb2c9e3094459b85c206b8fbd`](https://github.com/LCubero/repo-harness-builder/tree/58f41a790556d9dbb2c9e3094459b85c206b8fbd) |
| Gentle AI | [`4004a0825d03a3d123a3c4f67bf375cb16fd6d8c`](https://github.com/Gentleman-Programming/gentle-ai/tree/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c) |
| Engram | [`1dafc0f63051b2214100f7bd801357e4aab61c26`](https://github.com/Gentleman-Programming/engram/tree/1dafc0f63051b2214100f7bd801357e4aab61c26) |

## Fuentes primarias

- Harness: [reglamento](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/REPO-HARNESS.md), [regla de memoria](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/.repo-harness/rules/memory.md) y [RH-W-0022](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/docs/repo-harness/roadmap.md#rh-w-0022).
- Gentle AI: [reglas de activación](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/docs/trigger-rules.md), [RDD orgánico](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/docs/architecture/organic-rdd.md), [contrato de estado](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/internal/assets/skills/_shared/sdd-status-contract.md) y [pruebas del runtime orgánico](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/e2e/organicruntime/organic_runtime_test.go).
- Engram: [uso previsto](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/intended-usage.md), [arquitectura](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/ARCHITECTURE.md), [configuración de agentes](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/AGENT-SETUP.md), [núcleo de memoria](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/codebase/memory-core.md) y [comportamiento del almacenamiento](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/internal/store/store.go).

## Matriz de autoridad

| Asunto | Autoridad | Responsabilidad del harness |
|---|---|---|
| Política durable, verdad actual, fuente, pruebas y backlog | Repositorio | Reconciliar evidencia y actualizar sus propietarios canónicos |
| Enrutamiento, revisión del candidato exacto, recibos y corrección acotada de la revisión | Gentle AI cuando está habilitado | Invocar y transmitir el estado emitido por el propietario sin copiarlo |
| Observaciones semánticas, relaciones, metadatos de obsolescencia y continuidad de sesión | Engram | Recuperar o guardar contexto y después reconciliar afirmaciones con evidencia del repositorio |
| Composición de capacidades | Harness | Seleccionar una rama válida sin persistir otro ciclo de vida |

## Matriz de capacidades y disponibilidad

| Gentle AI | Engram | Comportamiento requerido |
|---|---|---|
| Disponible | Disponible | La política del repositorio sigue siendo primaria y es responsable de commits, pushes, PR y releases; usar Engram para continuidad y Gentle AI para el enrutamiento, la revisión del candidato exacto, los recibos y la corrección acotada de la revisión aplicables bajo control del propietario |
| Disponible | No disponible | Usar evidencia del repositorio y contratos de Gentle AI; informar degradación de continuidad |
| No disponible | Disponible | Usar continuidad de Engram bajo la política del repositorio; nunca inferir aprobación de Gentle AI |
| No disponible | No disponible | Trabajar solo con evidencia del repositorio; informar capacidades de integración no disponibles sin bloquear trabajo ordinario salvo que la política local las requiera |

## Contrato genérico recomendado

1. Resolver primero la raíz y la política del repositorio.
2. Detectar Engram mediante disponibilidad de herramientas y detección del proyecto. Detectar Gentle AI mediante una superficie anunciada y de solo lectura para capacidad o estado, nunca mediante estructura privada o mera presencia de un ejecutable.
3. Reconciliar afirmaciones recuperadas y metadatos de obsolescencia con evidencia actual del repositorio antes de actuar.
4. Permitir que cada propietario habilitado emita sus propios estados y transiciones. No sintetizar un veredicto combinado de autoridad.
5. Devolver resultado del repositorio, resultados por componente, razones de degradación y una siguiente acción.

## Comportamiento orgánico y proporcional

Usar trabajo directo de forma predeterminada. Delegar o usar SDD solo cuando se solicite o sea materialmente útil y aceptado. La recuperación de memoria, las comprobaciones, la revisión y los reintentos deben estar limitados por la tarea. La disponibilidad de integraciones no debe causar lecturas amplias, artefactos sintéticos ni ceremonia innecesaria. El modo deshabilitado de Gentle AI significa política ordinaria del repositorio, no rechazo ni aprobación.

## Regla de promoción de conocimiento durable

Las observaciones y los resúmenes de sesión de Engram son entradas de continuidad, no verdad durable del equipo. Promover solo conocimiento verificado y todavía relevante al propietario canónico correcto del repositorio, en la misma unidad de trabajo cuando esa verdad deba cambiar. Mantener fuera de la política durable los inventarios de herramientas, opciones, esquemas, valores de caducidad y detalles de instalación ligados a una versión, salvo que el contrato del producto dependa de ellos.

## Antipatrones y objetivos explícitamente excluidos

- No crear un tercer ciclo de vida, registro de intentos, almacén de conflictos, caché de recibos ni copia de estado de autoridad.
- No permitir que la memoria posea backlog, estado actual, aprobación ni decisión de entrega.
- No fabricar aprobación ni automatizar la habilitación de revisión, la revisión de memoria obsoleta o la resolución de conflictos sensibles.
- No crear un protocolo de memoria propio cuando Engram esté disponible ni artefactos SDD sintéticos para trabajo directo.
- No incluir grafo de código, motor de dependencias, daemon, planificador, servicio externo ni promoción de ciclo de vida en este artefacto de investigación.
- No incluir lenguaje específico del producto en harnesses generados para consumidores. El autoalojamiento aplica comportamiento genérico a este repositorio; no cambia el lenguaje generado.

## Semántica de fallos y degradación

- La autoridad del repositorio ausente o contradictoria bloquea el trabajo afectado o requiere una decisión.
- Un fallo de Engram degrada solo la continuidad; nunca se debe afirmar que la recuperación o persistencia tuvo éxito.
- Gentle AI ausente o deshabilitado delega en la política del repositorio; solo sus capacidades de enrutamiento, revisión del candidato exacto, recibos y corrección acotada de la revisión dejan de estar disponibles o de administrarse, mientras la política del repositorio sigue siendo responsable de la entrega.
- Un estado malformado de un propietario detiene esa rama opcional con evidencia exacta; el harness no debe reconstruir autoridad.
- La memoria obsoleta debe etiquetarse y verificarse. Marcarla como revisada requiere confirmación explícita.
- Un conflicto de memoria pendiente permanece limitado a su componente y sigue las compuertas de juicio de Engram.

## Relación con RH-W-0022

Este contrato está relacionado con `RH-W-0022`, pero es independiente. Esa unidad de trabajo posee las relaciones documentales declaradas. Una futura unidad de integración poseería las ramas de capacidades en runtime y los límites de autoridad. Solo debería depender de `RH-W-0022` si la implementación necesita su mapa de relaciones para exigir promoción en la misma unidad de trabajo. Esta investigación no cambia ningún estado del ciclo de vida.

## Superficies canónicas candidatas del builder

- `skill/repo-harness-builder/scripts/harness_tool.py`: ramas y plantillas generadas.
- `skill/repo-harness-builder/SKILL.md` y `command/repo-harness-builder.md`: interacción del builder y proyección del comando.
- Documentos generados de memoria, inicio de sesión, cierre de sesión, uso y decisiones: autoridad, continuidad, degradación y orientación humana.
- `docs/repo-harness/roadmap.md`: solo una futura unidad aprobada y su relación con `RH-W-0022`.

## Pruebas requeridas

- Cobertura de contrato o golden para las cuatro combinaciones de disponibilidad.
- La evidencia del repositorio prevalece sobre memoria contradictoria u obsoleta.
- Un fallo de componente nunca produce falso éxito, aprobación, afirmación de continuidad ni rechazo de entrega.
- No se emite estado duplicado de ciclo de vida, intento, artefacto o recibo.
- Los conflictos sensibles y la revisión obsoleta usan la compuerta humana correcta.
- Los resultados por componente permanecen separados y presentan una siguiente acción ejecutable.
- La salida generada, los idiomas y las proyecciones de runtime existentes permanecen estables.

## Preguntas sin resolver y confianza

- ¿Debe Engram ser obligatorio solo cuando un repositorio generado seleccione explícitamente continuidad integrada? Confianza: media.
- ¿Qué sonda de capacidad de Gentle AI de solo lectura es estable e independiente de la estructura de instalación? Confianza: media.
- ¿Debe el resultado público exponer algún estado agregado? Recomendación: resultado del repositorio más resultados por componente. Confianza: alta.

La confianza es alta para límites de autoridad, comportamiento por disponibilidad, reglas de promoción, antipatrones y semántica de degradación. Es media para futuras interfaces de descubrimiento porque las fuentes fijadas definen contratos de propietarios, pero no una sonda genérica para el harness.
