# RH-W-0078 Contratos de entrada ligeros — Diseño aprobado

La fuente canónica es el [diseño en inglés](../../repo-harness/rh-w-0078-design.md).

**Hecho verificado:** el 2026-10-04 el propietario aprobó explícitamente este diseño bilingüe acotado y el procedimiento de medición reproducible al responder «dale» a la pregunta de aprobación del diseño que mantenía separados planificación de implementación, cambios de producto, experimentos de runtime y entrega. Esto sigue a la preparación del diseño autorizada por separado; aprobar el método no es evidencia de runtime ni de seguridad. [RH-W-0078](roadmap.md#rh-w-0078) está ahora `active`, `P2`: el propietario autorizó explícitamente implementar RH-W-0078 y se confirmó el issue #266. El propietario autorizó por separado preparar el [plan de implementación](rh-w-0078-implementation-plan.md) y lo aprobó el 2026-10-05 solo para registro documental; esas aprobaciones históricas no constituyen evidencia de runtime. Se eliminó la antigua compuerta de base antes de empezar. La evidencia de runtime sigue desconocida y es necesaria antes de afirmar ahorros medidos o aceptar efectividad. Los cambios en herramientas externas, experimentos de runtime y entrega quedan fuera de la autoridad de esta tarea. Este diseño no es un plan de implementación.

## Recomendación y límite

**Inferencia respaldada:** se prefieren contratos de entrada ligeros con compuertas explícitas de referencias condicionales, cuya lectura sea obligatoria antes de la acción, no una reubicación indiscriminada. El [alcance aprobado](rh-w-0078-scope.md) y la [investigación local](rh-w-0078-gentle-ai-binary-research.md) respaldan esta dirección, pero no prueban ahorros efectivos. Se incluyen la compresión del builder y la redacción del alcance del techo en la guía empaquetada, transferidas desde RH-W-0076 completada. La documentación se mantiene sin cambios como control de preservación, salvo que las mediciones prueben necesaria una modificación compartida.

La entrada conserva la activación deliberada, la seguridad que nunca debe omitirse, la autoridad humana, los predicados de las compuertas, el orden de lectura obligatorio y el comportamiento ante fallos. El detalle específico de cada fase se mueve solo detrás de su compuerta nombrada; cada compuerta nombra una referencia local al repositorio y exige cargarla antes de la acción pertinente. Se conserva un recordatorio conciso en el punto de acción cuando importe la distancia. Una referencia obligatoria ausente o insegura detiene la acción afectada; nunca exime silenciosamente de una obligación. No se proponen almacén, dependencia, binario, runtime ni autoridad automática nuevos.

### Selecciones de diseño aprobadas para compuertas de referencias exactas propuestas

**Hecho verificado:** las siguientes rutas y predicados son selecciones de diseño aprobadas, no comportamiento instalado. En esta tabla `B` significa `skill/repo-harness-builder/references/`, `S` significa `.repo-harness/skills/repo-harness-session-start/references/` y `T` significa `.repo-harness/skills/repo-harness-session-stop/references/`; se concatena cada prefijo con el nombre de archivo indicado. Los nombres del builder existen salvo `authority-correction.md`; toda referencia `S`/`T` es **PROPUESTA**, no un archivo existente.

| Referencia canónica | Predicado y orden obligatorio antes de la acción |
| --- | --- |
| `B` + `audit-and-interview.md` | Inspección con el builder activo: antes de la auditoría del repositorio y de la entrevista de idioma/perfil según los pasos de ejecución actuales 1–2; la excepción candidata de solo comprobación indicada abajo no es la base. |
| `B` + `architecture.md` | Planificación de instalación/actualización: antes de componer capacidades y seleccionar salidas. |
| `B` + `license-selection.md` | Resolución de licencia de instalación/actualización: antes de formar el plan; se preserva la compuerta de decisión existente para una licencia sin resolver. |
| `B` + `preservation-and-safety.md` | Instalación/actualización: antes de auditar propiedad, previsualizar reemplazos o aplicar. La entrada conserva la contención para toda inspección. |
| `B` + `validation.md` | Verificación del harness aplicado: antes de validar e informar preparación. |
| `B` + `authority-correction.md` (**PROPUESTA**) | Contexto de autoridad del builder de activación opcional: antes de construir/previsualizar el contexto y de nuevo antes de la aplicación ligada a él; nunca se aplica a la autoridad de mantenimiento. |
| `S` + `reconciliation.md` | Uno de los cuatro disparadores nombrados de lectura profunda exige reconciliación: antes de comparar evidencia acotada o enrutar con autoridad. |
| `S` + `checkpoint.md` | Un checkpoint descubierto directamente requiere procesamiento: antes de clasificar/proponer reanudación; cualquier lectura más profunda de un propietario debe satisfacer uno de los cuatro disparadores existentes. |
| `S` + `synchronization.md` | La sincronización solicitada es pertinente: antes de inspeccionar upstream/transporte, usar una autorización recordada o consentir/actuar en red. |
| `T` + `checkpoint.md` | Solicitud explícita y nueva de checkpoint: antes de la escritura protegida y su lectura de comprobación. |
| `T` + `git-commit.md` | Staging/commit solicitado: antes de inspeccionar alcance preparado, aprobar mensaje, preparar archivos o crear el commit. |
| `T` + `git-transport.md` | Push solicitado: después de la elegibilidad local y antes de la comprobación previa, consentimiento o transporte del push. Solo commit no la carga. |

Cada compuerta obligatoria conserva su predicado, orden y recordatorio de fallo en la entrada: referencias obligatorias ausentes, sin propiedad acreditada, inseguras o ilegibles detienen la acción afectada con evidencia; nunca eximen de consentimiento ni obligaciones. Stop conserva incondicionalmente verificación → documentación aplicable → continuidad nativa → elegibilidad, con resultado independiente del Engram opcional. La documentación permanece sin cambios, con sus compuertas de propiedad, paridad, disposiciones de hallazgos y contrato de respuesta intactas.

Start conserva en la entrada el descubrimiento incondicional de checkpoint en la ruta exacta **después de recuperar continuidad nativa**, incluido `not_found` cuando no existe. Solo el procesamiento y la carga de referencias son condicionales. Sus únicos disparadores de lectura más profunda de propietarios siguen siendo: objetivo explícito o `RH-W-####`, contradicción, evidencia de prioridad inválida o ausente, reconciliación obligatoria. La presencia de checkpoint no es un quinto disparador; se clasifica desde la entrada acotada del descubrimiento y se exige un disparador existente antes de obtener evidencia adicional de propietarios. La evidencia obligatoria que no pueda resolverse detiene la propuesta, no el descubrimiento.

Para cada ruta de la tabla, la proyección exacta de runtime reemplaza `skill/repo-harness-builder/` o `.repo-harness/skills/<name>/` por `<base>/skills/<name>/`, donde `<base>` es `.claude` para `claude-code`, `.opencode` para `opencode` o `.pi` para `pi`, y el nombre del builder es `repo-harness-builder`. Solo los perfiles de runtime seleccionados aportan rutas. **Requisito de propiedad propuesto:** el renderer existente deriva una lista finita de rutas exactas desde estos nombres de archivo enumerados, los nombres canónicos de skills y los perfiles seleccionados validados; el manifest autenticado declara esa lista y el checker verifica su esquema, igualdad exacta con la lista derivada, procedencia y bytes canónicos/de proyección. Se extienden `generated_skill_paths()` y los enlaces de reemplazo/actualización a esos archivos exactos, con aprobación del digest actual, protección de bytes desconocidos/humanos y sin propiedad recursiva de directorios. La misma lista derivada alimenta las proyecciones y una regla exacta de atributo LF por ruta comparada byte a byte; los manifests heredados nunca autentican por proximidad las referencias recién propuestas. Aquí no se autoriza un almacén nuevo de manifest ni cambio de producto.

## Matriz de preservación

**Hecho verificado:** los siguientes propietarios locales contienen las obligaciones actuales. **Requisito de diseño:** preservarlas antes de su acción en cada runtime seleccionado; una reducción del cuerpo no compensa su pérdida.

| Propietario | Preservación obligatoria |
| --- | --- |
| [Builder](../../../skill/repo-harness-builder/SKILL.md), [seguridad](../../../skill/repo-harness-builder/references/preservation-and-safety.md), [arquitectura](../../../skill/repo-harness-builder/references/architecture.md) | Activación deliberada y aprobación estricta del digest actual frente al consentimiento conversacional de mantenimiento; indicadores independientes de idioma/runtime/licencia de entrevista; contexto de autoridad sin estado, enlaces a destino/plan/generación y rechazo que exige una vista previa nueva; raíz/ancestros protegidos y límites honestos de contención; bytes humanos/desconocidos protegidos. |
| [Start](../../../.repo-harness/skills/repo-harness-session-start/SKILL.md) | Carta del proyecto, idioma/preferencias, orden de reanudación primero desde el backup válido más reciente; exactamente cuatro disparadores de lectura profunda; evidencia/cronología/dependientes acotados; resolución de idioma según procedencia; capacidades opcionales independientes; sin trabajo automático, mutación ni reutilización de consentimiento. |
| [Stop](../../../.repo-harness/skills/repo-harness-session-stop/SKILL.md) | Verificación exitosa antes de documentación aplicable, continuidad y elegibilidad; backup local primero con filtro de privacidad y resultado de Engram independiente; checkpoint exige autoridad explícita nueva; alcance preparado exacto y separación del índice preexistente; sin atribución de IA ni reescritura silenciosa del mensaje aprobado; solo commit nunca aprueba push. |
| [Arquitectura de runtime](../../../skill/repo-harness-builder/references/architecture.md) | Fuente canónica de producto frente a skills neutrales generadas y proyecciones; paridad en disco frente a frescura del cache del proceso activo; entradas ligeras de comandos/prompts por nombre registrado; sin recarga inventada ni reinicio en vivo obligatorio. |
| [Propietario de renderer/checker](../../../skill/repo-harness-builder/scripts/harness_tool.py), [política LF](../../../.gitattributes) | La propiedad autenticada exacta de referencias generadas, la cobertura de esquema/rutas propias del checker, la transacción y procedencia de reemplazo/actualización, todos los destinos de runtime seleccionados y las reglas LF explícitas deben evolucionar juntos bajo autoridad posterior. Hoy `generated_skill_paths()` solo posee archivos operativos `SKILL.md`: nunca confiar en directorios recursivos arbitrarios ni inferir propiedad desde un enlace/nombre/prefijo. Las referencias humanas permanecen protegidas. |
| [Control de documentación](../../../.repo-harness/skills/repo-harness-documentation/SKILL.md), [reglamento](../../../REPO-HARNESS.md) | Un propietario canónico, pares localizados completos, cronología factual, clases de evidencia, aprobación humana y obligación inmediatamente antes de responder permanecen alcanzables y oportunos. |

## Cuatro mediciones, no una sola afirmación de tokens

| Cantidad | Definición y limitación |
| --- | --- |
| Indicador aproximado del cuerpo de entrada | Estimación existente `round(len(text.split()) * 1.33)` tras aplicar la regla de frontmatter indicada abajo; no es un tokenizer ni un costo de runtime observado. |
| Alcance local estático | Archivos seguros únicos alcanzables desde referencias locales explícitas y rutas nombradas por instrucciones, obligatorias o condicionales. Un enlace por sí solo no prueba lectura obligatoria ni lectura de runtime. |
| Cierre ordenado obligatorio/por fase | Eventos de lectura obligatoria en el orden del contrato para un escenario fijado, con evidencia de compuertas; informar por separado totales por fase, volumen único acumulado y volumen de relectura. Las referencias incondicionales cuentan completas, así que reubicarlas no obtiene ahorros. |
| Consumo real de runtime | Desconocido sin trazas reales de runtime/herramientas: wrappers/metadata inyectados, resultados completos o parciales de herramientas, recursos implícitos, relecturas y truncamiento pueden diferir del cierre estático. Efectos de cache y totales de tokenizer requieren evidencia explícita de runtime. |

Contar por separado bytes fuente UTF-8, palabras delimitadas por espacios en blanco y estimación proxy. Solo para medir el cuerpo, excluir frontmatter si y solo si la primera línea es exactamente `---` y una línea posterior es exactamente `---`; retirar ambos delimitadores y todas las líneas intermedias; en otro caso no excluir nada y señalar frontmatter de apertura mal formado. Sin parseo YAML ni heurística de encabezados. Los cierres cuentan archivos completos, incluidos frontmatter, comentarios y código, por defecto. Un enlace a un encabezado nunca implica lecturas solo de sección. La evidencia de runtime solo de sección debe especificar los bytes exactos devueltos y no debe cambiar la base estática de archivo completo.

Estimaciones históricas conservadas en la [hoja de ruta](roadmap.md#rh-w-0078): builder 3042, start 2707, stop 2822, documentación 878. Son evidencia histórica, no mediciones de este borrador ni del consumo actual de runtime.

## Matriz de escenarios fijados

Ejecutar las mismas filas por separado para `claude-code`, `opencode` y `pi`, y para cada locale/configuración declarada que se compare. Fijar la integración por fila; standalone/integrated no son intercambiables.

| Familia | Escenarios y compuertas que los distinguen |
| --- | --- |
| Builder | La intención explícita de inspeccionar/comprobar selecciona una rama de solo lectura del builder activo que usa únicamente el check existente y la configuración declarada validada; no infiere runtime, perfiles, idioma ni capacidades. Si falta evidencia necesaria o no es confiable, informa la limitación y se detiene. El check CLI directo sigue siendo una comparación renderizada aparte; instalación/actualización y corrección de autoridad conservan sus compuertas vigentes. |
| Start | Solo reanudación con prioridad válida y sin disparador de lectura profunda; objetivo nombrado (solo su propietario/evidencia); checkpoint (descubrimiento después de recuperar continuidad nativa, evidencia actual y luego autoridad nueva para reanudar); sincronización (upstream exacto actual y consentimiento de red separado, invalidación ante cambio material). |
| Stop | Handoff local (verificación/documentación/continuidad nativa/elegibilidad, sin Git); checkpoint solicitado (solicitud explícita nueva, escritura protegida y lectura de comprobación); solo commit (consentimiento de alcance/mensaje y comprobación de lo preparado, sin push); commit+push (mismo consentimiento acotado, se conservan comprobación previa renovada y fallos de transporte). |
| Documentación | Control solo de preservación con propietarios canónicos/localizados aplicables y compuerta de respuesta; sin objetivo de ahorro impuesto a un cuerpo conforme que no cambia. |

**Hecho verificado:** Product Identity del builder indica comprobación de solo lectura cuando no se indica operación, pero los pasos activos 1–4 todavía exigen auditoría/entrevista, preguntas de idioma/perfil y referencias de instalación. **Decisión humana (2026-10-06):** la intención explícita de inspeccionar/comprobar selecciona una rama de solo lectura del builder activo que usa el check existente y solo la configuración declarada validada; no infiere runtime, perfiles, idioma ni capacidades. Si falta evidencia necesaria o no es confiable, informa la limitación y se detiene. Esta rama no autoriza apply, repair, sincronización ni acciones Git. Las compuertas de instalación, actualización y corrección permanecen sin cambios. Esta aprobación es solo de diseño: no autoriza implementación, experimentos de runtime, veredicto nativo, afirmaciones de ahorro, commit ni entrega. El `check()` CLI existente sigue siendo una comparación renderizada de solo lectura aparte.

Para propietarios dinámicos, backups, checkpoint y evidencia Git, usar fixtures fijos no sensibles en memoria con ruta, bytes, SHA256 y resultado de compuerta declarados; o declarar por separado un grupo de variables excluidas. Nunca tratar silenciosamente contenido dinámico ausente como cero, leer estado local privado para medir tamaño ni afirmar que las variables excluidas forman parte del contexto total. Fijar resultados de capacidades opcionales y forma del repositorio como entradas del escenario.

## Procedimiento reproducible de base estática

Este pseudocódigo explícito de biblioteca estándar y solo lectura es un procedimiento, no un script/herramienta nuevo ni permiso para ejecutar experimentos. La base versionada es el commit `d30f8b442288eeb180257a46ccc4bb1b64688f5c`; registrar por separado el delta documental local. Un candidato futuro de renderer autorizado es un mapa de salidas separado en memoria, nunca una aplicación generada ni un reemplazo de esta base.

```text
pin = {commit, runtime, documentation_locales, conversation_language,
       report_language, integration_mode, license, project_identity,
       scenario, capability_outcomes, repository_shape, count_rule_version}
assert baseline HEAD == pin.commit; record any scoped diff separately
select exact skill and wrapper paths from authenticated manifest/config:
  builder source skill/repo-harness-builder/SKILL.md; its runtime package peer
  daily canonical .repo-harness/skills/<name>/SKILL.md; runtime skill peer
  Claude/OpenCode command families; Pi prompt family; required root pointer
pin renderer, manifest, checker, entry, wrapper and each read-source SHA256
classify raw Markdown destinations before deriving read locators:
  split fragment from path; same-file anchors are section edges, not new files;
  validate anchors against the target, never treat them as section-only reads
  queries are unsupported/unresolved local edges, never stripped silently;
  external http/https links are non-read edges (no network); other schemes
  are unsupported; reject drive-qualified, UNC and root-relative paths
  normalize decoded relative path against containing file's repository-relative
  parent (instruction-named paths use their declared base); allow ../../ only
  when lexical normalization remains inside the validated repository root;
  reject malformed/ambiguous encoding and encoded separators; no alias following
safe_read(normalized_locator): require contained repository-relative components
  with no remaining parent segments, drive, UNC or absolute prefix;
  guard root and every ancestor before reading, rejecting symlink/reparse,
  indirect, non-directory ancestors and non-regular/escaping final targets;
  read UTF-8 strictly; return bytes and hashlib.sha256(bytes).hexdigest()
  never recurse a directory; cooperative filesystem checks retain race limits,
  not a hostile-filesystem security subsystem
inventory explicit Markdown links plus instruction-named local paths;
  classify examples and placeholders as non-read edges unless required;
  flag unresolved edges; inventory membership itself creates no read event
visit reachable safe local files once (visited set); record owner for each;
  this graph is reachability, not automatic mandatory traversal
for each scenario: manually derive ordered read events from exact clauses:
  (phase, ordinal, path, source_clause, gate_predicate, gate_input, gate_outcome)
  include wrapper, entry, root instructions, unconditional reads and triggered
  dependencies only where exact scenario clauses require them; canonical and
  projection copies count together only when those clauses require both
  expand recursive obligations to a finite ordered sequence with clause-defined
  stopping conditions; retain repeated events, never silently deduplicate reads
  ambiguity or unbounded cycles makes that scenario unmeasured, not a broken cycle
  reject ambiguous order/gates rather than guessing; record skipped gates too
for each phase: volume = sum(full_file_words(event.path) for event in phase)
  cumulative_unique = sum(full_file_words(path) for path read so far once)
  cumulative_repeats = cumulative_event_words - cumulative_unique
  report bytes/words/proxy per file and phase; sum per-event rounded proxies
  separately from round(total_words * 1.33), since rounding may differ
compare baseline and future candidate using identical pin scenarios/count rules;
  attach exact required-read sequence, gate evidence, ownership/link failures
```

Usar `pathlib`, `hashlib` y conteo ordinario de texto con biblioteca estándar en memoria; emitir evidencia en la sesión, sin almacén persistente nuevo de mediciones. Los hashes fuente identifican entradas, no autoridad humana. Una instalación integrada versionada no aporta salidas standalone nuevas: marcar las configuraciones no disponibles como sin medir hasta que exista una base de renderer en memoria autorizada por separado. Aquí no se inventa resultado numérico.

## Aceptación y compuerta pendiente

- Nominar start de solo reanudación y stop de handoff local como rutas comunes; el cierre estático obligatorio candidato debe ser estrictamente menor que la misma base fijada para esas filas en cada runtime/configuración que se afirme cubrir. Exigir también un cierre menor de inspección/solo comprobación del builder activo una vez resuelta su rama propuesta; mantener separada la comprobación CLI directa e informar rutas alternativas de instalación/corrección sin ocultar aumentos.
- Los cuerpos de entrada del builder y de las skills generadas deben ser como máximo el máximo proxy existente de 1000; el presupuesto del reglamento sigue en 700 con su semántica actual de aserciones. Los presupuestos son aserciones de desarrollo, nunca rechazos de render/check. La redacción empaquetada debe vincular explícitamente al builder y a toda skill generada sin subir ninguno de los techos.
- Toda obligación de preservación está presente antes de su acción; sin referencia colgante, que escape o sin propiedad acreditada, ni consentimiento humano/seguridad cambiados. Recursos obligatorios ausentes impiden la acción afectada. Todas las proyecciones canónicas/de runtime deben ser equivalentes, con cobertura autenticada de rutas exactas y LF, no confianza en directorios.
- La aceptación estática es necesaria, no suficiente: aprobar runtime requiere una reducción real medida del consumo en las rutas comunes nombradas que se afirme cubrir (start de solo reanudación, stop de handoff local e inspección/solo comprobación del builder activo resuelta), comparando escenarios idénticos de base/candidato en cada runtime/configuración afirmada. Fijar versión/configuración, estado de proceso nuevo frente a cache, trazas exactas de prompts/lecturas de herramientas y cumplimiento ordenado de compuertas; comparar bytes reales devueltos/inyectados y estimaciones por palabras delimitadas por espacios en blanco, o conteos de tokenizer solo con evidencia explícita del tokenizer. Sin porcentaje arbitrario, sustituto estático ni ahorros inferidos del tamaño del cuerpo. Observaciones comparables sin reducción hacen fallar la ruta afirmada; observaciones no disponibles, ambiguas o incomparables quedan sin medir, nunca aprobadas. Este requisito sigue sin cumplirse; no se afirman observaciones de runtime ni ahorros, y no se requieren sesiones en vivo solo para redactar este diseño.

**Compensación:** la recuperación condicional puede reducir contexto obligatorio, pero arriesga obligaciones omitidas o distantes; las compuertas explícitas antes de la acción y las futuras pruebas negativas/de referencias ausentes protegen el contrato. La reubicación pura es más simple, pero no obtiene ahorros si todas las referencias siguen siendo obligatorias. Las comprobaciones mecánicas orientadas al modelo son candidatas solo para un cambio de producto autorizado posteriormente, no salvaguardas implementadas.

**Autoridad verificada:** el propietario autorizó explícitamente implementar RH-W-0078; se confirmó el issue #266. Ya no aplica la antigua compuerta de base de runtime antes de empezar. Las observaciones de runtime siguen desconocidas y son necesarias antes de afirmar ahorros medidos o aceptar efectividad; esta actualización documental no afirma ninguno. Esta tarea no autoriza cambios de herramientas externas, experimentos de runtime ni entrega.

**Actualización de autorización vigente:** el «si dale» explícito de la persona usuaria autoriza la implementación acotada I1 de la rama aprobada de inspección/comprobación del builder activo y solo de lectura, sus pruebas canónicas enfocadas y la reconciliación bilingüe mínima de autoridad/estado. No autoriza el trabajo posterior de propiedad de referencias/proyecciones generadas, sincronización de proyecciones, afirmar suite completa, experimentar con runtime, emitir veredicto nativo, afirmar ahorros, usar Git, hacer commit ni entregar. La rama debe usar el checker instalado existente y configuración declarada autenticada, detenerse ante evidencia ausente o no confiable y dejar intactos los flujos de instalación/actualización/corrección.
