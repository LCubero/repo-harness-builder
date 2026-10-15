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

## Actualización actual (2026-09-17)

**Hecho verificado:** esta actualización revisó los snapshots locales seleccionados de referencia de `gentle-shell`, Gentle AI y Engram en estos commits Git locales exactos: `gentle-shell` [`2b579c80824e83442b8ae9f7bfad629a52f9f711`](https://github.com/Gentleman-Programming/gentle-pi/tree/2b579c80824e83442b8ae9f7bfad629a52f9f711), Gentle AI [`eae8fadd5ece2622cb072c836fa49cef520afc8e`](https://github.com/Gentleman-Programming/gentle-ai/tree/eae8fadd5ece2622cb072c836fa49cef520afc8e) y Engram [`f6b22f89d024fb6aaa207606d893f47b3f4e23a9`](https://github.com/Gentleman-Programming/engram/tree/f6b22f89d024fb6aaa207606d893f47b3f4e23a9). Son evidencia local actual de la actualización, no pines de revisión de reemplazo. El material fijado anterior conserva su carácter de evidencia histórica separada y no se reescribió como una observación nueva.

**Hecho verificado:** `gentle-shell` es el nombre actual del producto/repositorio orientado a Pi, mientras que las identidades de npm, runtime, paquete y repositorio fuente pueden seguir usando `gentle-pi`. Es una transición de nombres mixta en curso, no un cambio de nombre de paquete ya completado. Las fuentes estables para actualizar son el [repositorio de gentle-shell](https://github.com/Gentleman-Programming/gentle-pi), el [repositorio de Gentle AI](https://github.com/Gentleman-Programming/gentle-ai) y el [repositorio de Engram](https://github.com/Gentleman-Programming/engram); actualizar cuando cambien el nombre documentado del producto, el límite de propiedad, la configuración de Pi o la semántica del flujo de trabajo.

**Hecho verificado:** `gentle-shell` posee la orquestación nativa de Pi, subagentes acotados, artefactos de tareas ODD y su reflejo con alcance de proyecto mediante Engram, SDD opcional, contratos de transporte/espejo de revisión y composición opcional con Engram. Su flujo ODD cierra cada tarea sustancial con un commit de unidad de trabajo; los candidatos de revisión nativa son commits de unidad de trabajo o cortes de PR, nunca una casilla ni una rama de funcionalidad acumulada; y la estrategia de entrega considera cortes encadenados alrededor de 400 líneas modificadas de autoría. Cuando la política de subagentes en segundo plano está activa y `subagent_run` está disponible, las delegaciones ordinarias se ejecutan en segundo plano de forma predeterminada. El espejo de Engram es específico de la coordinación ODD y no limita la capacidad más amplia de continuidad de Engram. La semántica de revisión nativa sigue siendo propiedad de Gentle AI/proveedor. Gentle AI documenta ODD como flujo predeterminado, SDD como selección explícita y RDD como propiedad de la persona usuaria y opcional; archivar SDD no es aprobación de revisión ni entrega. Engram `f6b22f89d024fb6aaa207606d893f47b3f4e23a9` hace que el formato de backup directo `0.2.0` conserve pines locales y metadatos completos de relaciones de memoria, a la vez que acepta backups heredados `0.1.0` que omiten esos campos.

## Comparación con planning-with-files (2026-09-18)

**Hecho verificado:** el repositorio upstream inspeccionado `OthmanAdi/planning-with-files`, en el commit exacto `2fbbd77ba9a74cddb9504285935ef9ae0837cdec`, se revisó mediante evidencia local acotada: su [skill canónica](https://github.com/OthmanAdi/planning-with-files/blob/2fbbd77ba9a74cddb9504285935ef9ae0837cdec/skills/planning-with-files/SKILL.md), [flujo de trabajo](https://github.com/OthmanAdi/planning-with-files/blob/2fbbd77ba9a74cddb9504285935ef9ae0837cdec/docs/workflow.md), [modos de larga duración](https://github.com/OthmanAdi/planning-with-files/blob/2fbbd77ba9a74cddb9504285935ef9ae0837cdec/docs/long-running-agent-tasks.md), [adaptador de Pi](https://github.com/OthmanAdi/planning-with-files/blob/2fbbd77ba9a74cddb9504285935ef9ae0837cdec/docs/pi-agent.md), [plantillas](https://github.com/OthmanAdi/planning-with-files/tree/2fbbd77ba9a74cddb9504285935ef9ae0837cdec/templates), límite de seguridad en la skill canónica y [limitaciones de evaluación](https://github.com/OthmanAdi/planning-with-files/blob/2fbbd77ba9a74cddb9504285935ef9ae0837cdec/docs/evals.md).

**Solapamiento verificado:** esas fuentes documentan continuidad persistente de la tarea activa, recuperación tras pérdida de contexto, selección exacta de tarea/plan, actualización acotada de contexto y estado compartido bajo control del orquestador; los propietarios actuales de ODD documentan las responsabilidades correspondientes del lado del repositorio.

**Diferencia de propiedad verificada:** ODD de repo-harness/Gentle Shell ya posee un único `odd/tasks/<feature-name>.md`, su espejo de Engram con alcance de proyecto y la proyección visible de `todo`, bajo autoridad del repositorio y humana.

**Dirección futura respaldada, requiere aprobación del propietario:** considerar solo estas cinco ideas acotadas: vinculación exacta de la tarea activa; una proyección estructurada y compacta de contexto; sincronización previa a la compactación mediante los propietarios existentes; diagnósticos de continuidad de solo lectura; y un benchmark de recuperación medido. No adoptar directamente `task_plan.md`, `findings.md` ni `progress.md`; registros JSONL por agente; recitación por herramienta; compuertas automáticas de finalización; hash como aprobación; adivinación del plan más reciente; ni ningún tercer almacén de ciclo de vida o autoridad.

Esta investigación no autoriza implementación ni cambia el roadmap, estado actual, comportamiento del producto ni comportamiento del runtime.

## Evaluación de impacto acotada (2026-09-17)

**Hecho verificado:** la auditoría cubrió 11/12 rutas de evidencia del repositorio, 8/8 registros dependientes directos y 0/20 commits; el límite no se agotó. No se encontró una superficie afectada sin propietario. Todos los registros directos son actualmente de **sin impacto**:

### Límite de reproducibilidad

`11/12` significa que se inspeccionaron las once rutas exactas del repositorio siguientes y que quedó sin usar un cupo de capacidad; no que se omitió una ruta conocida. La auditoría no inspeccionó commits (`0/20`) y no afirma una prueba ilimitada.

Rutas de evidencia del repositorio inspeccionadas:

- `REPO-HARNESS.md`
- `.pi/skills/repo-harness-documentation/SKILL.md`
- `odd/tasks/gentle-ecosystem-reconciliation.md` (registro histórico local de tareas; ya no está versionado en Git)
- `docs/repo-harness/roadmap.md`
- `docs/repo-harness/current-state.md`
- `docs/repo-harness/decisions.md`
- `docs/repo-harness/integration-research.md`
- `docs/repo-harness/README.md`
- `.repo-harness/rules/response-contract.md`
- `skill/repo-harness-builder/scripts/harness_tool.py`
- `skill/repo-harness-builder/tests/test_harness_tool.py`

Los siguientes localizadores de fuente relativos al repositorio oficial se inspeccionaron desde snapshots locales seleccionados por el propietario. Su ubicación de almacenamiento se omite intencionalmente de la documentación canónica; sus identidades de commit Git locales exactas se registran en la actualización actual anterior:

- [gentle-shell](https://github.com/Gentleman-Programming/gentle-pi)
  - `assets/orchestrator-delegation.md`
  - `assets/orchestrator-memory.md`
  - `assets/sdd-orchestrator-workflow.md`
  - `contracts/review-provider-contract-mirror/v1.2.0/bundle/orchestration/pi.md`
- [Gentle AI](https://github.com/Gentleman-Programming/gentle-ai)
  - `docs/intended-usage.md`
  - `docs/trigger-rules.md`
  - `docs/review-integration.md`
- [Engram](https://github.com/Gentleman-Programming/engram)
  - `docs/AGENT-SETUP.md`
  - `docs/intended-usage.md`
  - `docs/codebase/memory-core.md`

`RH-W-0077` no es un dependiente directo del contrato del ecosistema Gentle: su asunto de contención de almacenamiento personal no cambia porque ningún propietario canónico adopta ni depende de la ubicación del snapshot. El conjunto directo se mantiene en 8/8; las entradas de auditoría se promueven como identidades de fuente y conclusiones, no como dependencia del repositorio.

| Registro | Clasificación y fundamento actual | Condición para reabrir o avanzar |
|---|---|---|
| `RH-W-0025` | **sin impacto** — la evidencia actual de nombres y capacidades preserva su contrato completado de dos componentes y con autoridad delimitada. | Reabrir solo si la clasificación de capacidades obtiene otro estado, la selección integrada omite alguna verificación, se copia estado de propietario, la pérdida posterior a la instalación afecta el componente equivocado, la promoción omite verificación del repositorio o aprobación humana, o la seguridad de reemplazo de RH-W-0008 se degrada. |
| `RH-W-0048` | **sin impacto** — ninguna evidencia actual contradice el contrato completado de recuperación acotada. | Ninguna. |
| `RH-W-0050` | **sin impacto** — la evidencia actual no altera sus límites completados de reconciliación ni continuidad. | Reabrir solo si se degrada la reconciliación acotada, la disposición de hallazgos, los resultados de continuidad por operación, la degradación autónoma, la paridad de proyecciones o la convergencia posterior a la entrega. |
| `RH-W-0053` | **sin impacto** — ninguna contradicción afecta el contrato completado de checkpoints. | Ninguna. |
| `RH-W-0069` | **sin impacto** — la evidencia actual deja intacto el límite completado de respaldo local. | Terminal; reabrir solo mediante un nuevo registro de propietario inmutable. |
| `RH-W-0072` | **sin impacto** — no se encontró una ruta faltante concreta ni una operación afectada. | Una ruta faltante concreta y una operación afectada, seguidas de un alcance aprobado por el propietario, son requeridas para cualquier corrección posterior. |
| `RH-W-0078` | **sin impacto** — la evidencia upstream respalda, sin cambiar, su dirección medida de divulgación progresiva ni su compuerta de alcance. | Avanzar solo después de que `RH-W-0076` entregue la frase corregida de la guía y el propietario apruebe un alcance bilingüe que valore y verifique la dirección recomendada. La implementación sigue sin autorización separada. |
| `RH-W-0079` | **sin impacto** — la evidencia actual de capacidades opcionales no contradice su clasificación completada de límite de release. | Ninguna. Cualquier release real permanece separado. |

**Hecho verificado:** los pines, versiones, recibos y fallos antiguos permanecen como evidencia histórica; no deben reescribirse como defectos actuales. El crecimiento actual de capacidades upstream o cambios de nombres por sí solos no reabren trabajo completado. Esta auditoría no requiere cambios de estado, prioridad, dependencia ni alcance del roadmap, ni de estado actual, mapa, changelog, producto, pruebas, contrato generado o proyección de runtime.

**Evidencia actual, no afirmación permanente:** actualizar esta evaluación si cambian los contratos de propietario, aparece una ruta faltante concreta, falla el comportamiento de una capacidad o se viola una condición de reapertura listada.

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

Usar trabajo directo de forma predeterminada. Gentle AI usa ODD como flujo predeterminado; entrar en SDD solo cuando se seleccione explícitamente. La recuperación de memoria, las comprobaciones, la revisión y los reintentos deben estar limitados por la tarea. La disponibilidad de integraciones no debe causar lecturas amplias, artefactos sintéticos ni ceremonia innecesaria. El modo deshabilitado de Gentle AI significa política ordinaria del repositorio, no rechazo ni aprobación.

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
- ¿Qué sonda de capacidad de Gentle AI de solo lectura es estable e independiente de la estructura de instalación? Resuelta el 2026-08-15: `gentle-ai version` devuelve `gentle-ai <version>`, se resuelve mediante PATH en vez de un directorio conocido y retorna antes de la detección del sistema, las lecturas de configuración o las comprobaciones de actualización. Tratar un código de salida validado y la forma de salida como `verified`; la mera presencia en PATH permanece `found_on_path` y nunca autoriza una capacidad, porque este documento prohíbe usar la presencia del ejecutable como base de detección. Confianza: alta.
- ¿Debe el resultado público exponer algún estado agregado? Recomendación: resultado del repositorio más resultados por componente. Confianza: alta.

La confianza es alta para límites de autoridad, comportamiento por disponibilidad, reglas de promoción, antipatrones y semántica de degradación. Es media para futuras interfaces de descubrimiento porque las fuentes fijadas definen contratos de propietarios, pero no una sonda genérica para el harness.
