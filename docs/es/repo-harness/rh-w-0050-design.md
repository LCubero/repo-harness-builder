# Diseno de reconciliacion acotada RH-W-0050

> **Estado: BORRADOR. Pendiente de revision externa y aprobacion del propietario.**
>
> Esta es una propuesta para revision, no un diseno final aprobado, aprobacion de implementacion, avance de ciclo ni entrega. RH-W-0050 permanece `proposed`.

## Contrato de revision

Revise este documento contra el [registro RH-W-0050](roadmap.md#rh-w-0050), las [decisiones](decisions.md), las [reglas de memoria](../../../.repo-harness/rules/memory.md) y el [contrato bilingue](i18n-maintenance.md). La evidencia del repositorio es canonica. El [documento ingles](../../repo-harness/rh-w-0050-design.md) posee los hechos; este archivo es una presentacion humana completa y semanticamente equivalente.

- **Restriccion aprobada:** ya autorizada en roadmap o decisiones y preservada aqui.
- **Decision recomendada:** propuesta por este borrador y pendiente de aprobacion.
- **Pregunta abierta:** genuinamente sin resolver y asignada a revision externa o al propietario.

## Problema y modos de fallo

Session start y session stop reconcilian evidencia pertinente, pero no definen cuando hace falta un ciclo completo. Despues de una entrega, hallazgos sin resolver, proyecciones obsoletas, evidencia incompleta, continuidad localizada duplicada o memoria contradictoria pueden producir routing desde estado falso.

Modos de fallo:

- Revisar todo portafolio y memoria en cada inicio o cierre, con costo y ruido altos.
- Ejecutar ciclo completo solo cuando una persona nota el problema, permitiendo estado falso persistente.
- Tratar cualquier archivo sucio, resumen viejo, keyword de commit o candidato Engram como drift probado.
- Tratar disponibilidad, `active`, `needs_review` o `judgment_required` como veredicto agregado de verdad.
- Colapsar fallos independientes Engram y perder exitos parciales validos.
- Recomendar siguiente unidad lista antes de acordar entrega y propietarios.
- Crear otro backlog, store de conflictos o recibos, cache de triggers, recibo de scan, framework, ledger o documento acumulativo.

## Objetivos

- Definir cuando inicio y cierre hacen scan acotado y cuando la evidencia exige ciclo completo.
- Clasificar `integrate`, `close`, `defer`, `reject` o `noise` con evidencia explicita.
- Preservar autoridad del repositorio, aprobacion humana, paridad localizada y resultados independientes.
- Reconciliar entrega antes de recomendacion autoritativa de siguiente unidad lista.
- Operar con y sin Git sin depender de internals de Git o Engram.
- Producir resultados observables y comprobables sin store durable nuevo.

## Fuera de alcance

- Implementar comportamiento generado, pruebas, codigo, almacenamiento o migracion.
- Avanzar RH-W-0050 mas alla de `proposed` o aprobar implementacion.
- Reemplazar roadmap, estado actual, decisiones, changelogs, pares o espejos concisos Engram.
- Agregar promocion, ciclo, juicio de conflictos o borrado historico automaticos.
- Exigir SDD, red, SQLite, daemon, scheduler u otro framework.
- Implementar RH-W-0011, RH-W-0048, RH-W-0051, RH-W-0052, RH-W-0053 o RH-W-0054.

## Autoridad y restricciones aprobadas

- Archivos, Git aplicable, fuente, pruebas y aprobaciones explicitas son canonicos. Engram solo aporta continuidad semantica.
- Toda operacion Engram usa proyecto explicito e informa su propio resultado.
- El harness investiga hechos verificables y pregunta solo decisiones humanas.
- Propietarios existentes siguen siendo roadmap, estado actual, decisiones, changelogs, pares y espejos concisos Engram.
- No se permite backlog, store de ciclo, conflictos o recibos, documento acumulativo, cache de triggers, recibo de scan, framework o ledger duplicado.
- Reconciliacion posterior a entrega ocurre antes de routing autoritativo.
- Comportamiento sigue liviano, organico y acotado.
- SQLite local-first es detalle actual de Engram, no dependencia.
- `active` y `needs_review` son etiquetas de antiguedad, no verdad.
- `judgment_required` descubre candidatos, no prueba contradiccion.
- Plugin, asociacion de prompt, captura pasiva, compactacion y sync exigen evidencia antes de afirmar exito.

## Base de evidencia

Contratos verificados establecen:

- Roadmap posee ciclo y RH-W-0050 sigue `proposed` sin dependencia.
- Session start lee propietarios, recupera Engram relevante cuando esta disponible, reconcilia memoria con repositorio y permite operacion local sin Git.
- Session stop verifica evidencia, reconcilia ciclo cambiado en una unidad, guarda continuidad integrada y separa continuidad requerida de Git opcional.
- Reglas de memoria exigen proyecto explicito y resultados separados para resolucion, lectura, escritura, prompt, conflictos, revision de ciclo, plugin y sync.
- Una escritura exitosa sigue exitosa si falla prompt o descubrimiento posterior de conflictos.
- `judgment_required` solo informa candidatos. `active` y `needs_review` se calculan por antiguedad y no otorgan autoridad.
- Engram usa actualmente SQLite local-first, pero el harness no depende de SQLite.

Son contratos soportados, no supuestos sobre internals privados. El diseno evita lineas fragiles y no afirma plugin, captura, compactacion o sync sin verificar.

## Enfoques

### A. Reconciliacion completa en cada inicio y cierre

Cada invocacion inventaria ciclo, proyecciones, entrega, Git, pares y memoria.

Tradeoffs: cobertura fuerte y triggers simples, pero costo, contexto y ruido altos, con presion para caches o recibos. Contradice operacion liviana.

### B. Reconciliacion solo por trigger visible

No hay scan acotado. Ciclo completo solo ante peticion humana o fallo visible.

Tradeoffs: costo minimo, pero no descubre routing falso, entrega incompleta o cambios de propietario. Delega deteccion a personas y estado obsoleto.

### C. Scan acotado hibrido y ciclo activado por evidencia

Cada inicio y cierre significativo hace scan pequeno consciente de propietarios. Trigger obligatorio activa ciclo. Senales consultivas solo acotan o se informan. Sin trigger, continua flujo normal.

Tradeoffs: agrega limite de clasificacion y pruebas enfocadas, pero evita scans completos rutinarios y detecta drift que afecta routing.

**Decision recomendada, no aprobada:** elegir C. Es el enfoque menor que satisface seguridad posterior a entrega, autoridad, no-Git y contexto acotado sin estado durable de triggers.

## Arquitectura y propietarios

| Propietario | Contrato |
|---|---|
| Session start | Resuelve repositorio, inspecciona propietarios relevantes, hace scan, reconcilia ante evidencia obligatoria y enruta conversacionalmente. No muta por defecto. |
| Session stop | Verifica trabajo, hace no-op o ciclo obligatorio, completa propietarios aprobados en una unidad, guarda continuidad concisa y despues trata Git opcional. |
| Habilidad de documentacion | Elige propietario menor, mantiene mapas y pares, rechaza stores y valida enlaces. |
| Reglas de memoria | Definen alcance, resultados por operacion, metadata consultiva, precedencia y evidencia para captura o sync. |
| Rulebook | Conserva solo compuertas esenciales, no procedimiento detallado. |
| Roadmap | Posee ciclo RH-W, avance, dependencias y disposiciones durables. |
| Estado actual | Proyecta actividad, bloqueos, completados recientes y siguiente condicion. |
| Decisiones | Poseen restricciones aprobadas, no recomendaciones borrador. |
| Changelogs | Registran trabajo completado. Un borrador no es entrega. |
| Propietario del repositorio | Aprueba semantica disputada, promociones, ciclo, limpieza destructiva, diseno final e implementacion. |

## Algoritmo de activacion acotada

### Entradas

- Repositorio seleccionado y accion solicitada.
- Rulebook y mapa aplicable.
- RH-W relevantes y unidad reportada como entregada o siguiente lista.
- Estado actual, decisiones, pares y changelog aplicable.
- Fuente, pruebas, checker y Git local disponibles.
- Engram relevante con proyecto explicito y resultados separados.
- Peticion o decision humana explicita.

### Resolucion de proyecto y propietario

1. Resolver una raiz y confirmar su rulebook.
2. Resolver propietario de cada afirmacion antes de comparar.
3. Ante proyecto ambiguo, detener Engram y pedir una sola eleccion humana. No adivinar.
4. Afirmacion sin propietario queda como hallazgo no clasificado y trigger obligatorio.

### Scan acotado

Inspeccionar solo:

- Unidad solicitada y registros directamente referidos.
- Estado actual y afirmacion de siguiente lista.
- Pares para afirmaciones cambiadas o conflictivas.
- Evidencia de trabajo representado como entregado desde estado verificado.
- Rutas propietarias relevantes y checker.
- Observaciones Engram de query estrecha con proyecto explicito.

No revisar todo backlog, commits, memoria o docs salvo trigger obligatorio. No persistir scan.

### Clasificacion de triggers

Clasificar con la tabla. Obligatorios activan ciclo. Consultivos solo acotan o aparecen en salida. Ruidosos se ignoran salvo evidencia obligatoria separada.

### Ruta sin trigger

Emitir `reconciliation: not_triggered`, nombrar evidencia, preservar resultados y continuar. Cierre sin cambio de ciclo o entrega es no-op y no escribe para registrar que nada cambio.

### Ruta completa

1. Inventariar hallazgos solo desde evidencia aplicable.
2. Resolver propietario y clase de afirmacion.
3. Comparar repositorio, localizacion, entrega y memoria.
4. Clasificar cada hallazgo.
5. Aplicar solo reparaciones no discrecionales ya autorizadas.
6. Detenerse en limites humanos.
7. Actualizar propietarios y pares en la misma unidad si hay aprobacion.
8. Actualizar solo espejo conciso Engram existente cuando aplique.
9. Repetir scan con evidencia fresca.

### Resultados independientes

Separar resolucion del repositorio, activacion, clasificacion, actualizaciones, paridad, entrega, routing, Git y cada operacion Engram. Un fallo no borra exito no relacionado.

### Routing posterior

- Con hallazgos obligatorios clasificados y propietarios verificados, routing puede continuar desde roadmap y estado actual.
- Con entrega incompleta o hallazgo pendiente, suprimir recomendacion autoritativa, informar dimension y una remediacion.
- Discusion puede continuar sin representar una unidad como lista ni autorizar trabajo.

## Tabla de triggers

| Clase | Senal | Tratamiento |
|---|---|---|
| Obligatorio | Posterior a entrega | Ciclo completo antes de routing autoritativo. |
| Obligatorio | Inconsistencia canonica de ciclo | Reconciliar roadmap y proyecciones. |
| Obligatorio | Propietario de ciclo cambiado | Reconciliar RH-W, estado, par y espejo conciso en una unidad. |
| Obligatorio | Evidencia de entrega incompleta | Clasificar brecha y suprimir routing. |
| Obligatorio | Riesgo de routing falso | Reconciliar status, dependencia, prioridad o condicion falsa. |
| Obligatorio | Peticion humana explicita | Ejecutar alcance solicitado sin expansion. |
| Obligatorio | Contradiccion repositorio-memoria verificada | Gana repositorio; clasificar memoria y preservar evidencia. |
| Obligatorio | Hallazgo retenido no clasificado | Ciclo completo porque routing no puede depender de hallazgo sin propietario. |
| Consultivo | `judgment_required` | Inspeccionar si es relevante; no prueba contradiccion. |
| Consultivo | `needs_review` | Solo antiguedad; verificar desde propietario. |
| Consultivo | Git sucio no relacionado | Informar y preservar, sin inferir drift. |
| Consultivo | Hints de commit no inspeccionados | Inspeccionar solo si afectan entrega o propietario. |
| Consultivo | Diferencia de wording sin conflicto | Permitir localizacion natural; reparar solo drift semantico. |
| Consultivo | Resumen obsoleto | Verificar claims; edad sola no activa. |
| Consultivo | Drift del checker sin efecto de ciclo | Enrutar al checker, sin forzar ciclo. |
| Consultivo | Git adelante sin entrega | Informar; no inferir entrega. |
| Rechazado o ruido | Cualquier archivo sucio | Suciedad sola no activa. |
| Rechazado o ruido | Tiempo transcurrido | Edad no prueba contradiccion. |
| Rechazado o ruido | Cada inicio o cierre | Invocacion sola no activa. |
| Rechazado o ruido | Volumen de memoria | Cantidad no es relevancia. |
| Rechazado o ruido | `active` o `needs_review` como verdad | Metadata no tiene autoridad. |
| Rechazado o ruido | Engram ausente como drift | Continuidad puede fallar con repositorio valido. |
| Rechazado o ruido | Keywords de commit | No prueban entrega o ciclo. |
| Rechazado o ruido | Red no disponible | Reconciliacion local continua. |
| Rechazado o ruido | Modo sin Git | Operacion soportada, no drift. |
| Rechazado o ruido | Search nulo no validado | Negativo exige control positivo o validacion equivalente. |
| Rechazado o ruido | Plugin, captura o sync no verificados | No afirmar exito sin evidencia. |

## Clasificacion de hallazgos

| Clase | Significado | Evidencia requerida |
|---|---|---|
| `integrate` | Hallazgo valido pertenece a propietario existente y afecta trabajo durable. | Relevancia, propietario, no duplicacion, clase de evidencia y aprobacion si es discrecional. |
| `close` | Satisfecho, superado en su alcance o no accionable. | Evidencia actual del propietario y entrega si aplica. Nunca solo memoria. |
| `defer` | Valido pero no debe avanzar ahora. | Bloqueo, prerrequisito o condicion humana y propietario roadmap existente o aprobado. |
| `reject` | Entendido y excluido deliberadamente. | Conflicto razonado con metas aprobadas o decision del propietario, preservado en propietario existente. |
| `noise` | Sin valor durable de producto, ciclo o continuidad. | Duplicado, irrelevante, no sustentado, consultivo o ya canonico. No crea registro. |

Cada hallazgo retenido tiene una clase actual. Clasificar no autoriza mutacion, avance, borrado o implementacion.

## Precedencia y clases de afirmacion

1. Decisiones humanas actuales poseen autoridad y criterios humanos.
2. Propietario canonico gobierna su dominio.
3. Fuente, pruebas, checker y Git aplicable corroboran implementacion y entrega.
4. Pares presentan hechos, no superan propietarios ingleses.
5. Engram aporta continuidad y candidatos.
6. Resumenes, hints, keywords y canales no disponibles no tienen autoridad.

Clases:

- `verified_fact`: evidencia aplicable, confiable y proporcional respalda directamente.
- `supported_inference`: evidencia y razonamiento nombrados, con condicion de verificacion.
- `unknown_or_unverifiable`: no hay mecanismo o evidencia valida.
- `human_decision_required`: autoridad o criterio pertenece a una persona.

No promediar conflictos. Resolver propietario, preservar conflicto y detener solo rama afectada.

## Git y no-Git

Con Git, usar estado, rutas, historia, ancestria y entrega local solo cuando sean relevantes y en lectura. Suciedad es consultiva. Ahead-only no prueba entrega. Red es opcional.

Sin Git, continuar con archivos, checker, fuente, pruebas, aprobaciones y continuidad disponible. Marcar claims de entrega Git `unknown_or_unverifiable` salvo otro mecanismo aprobado. No-Git no activa ciclo ni reduce autoridad.

## Matriz de degradacion Engram

| Operacion | Exito | Fallo o ausencia | Exito preservado |
|---|---|---|---|
| Resolucion de proyecto | Capacidad soportada resuelve proyecto explicito. | Detener lectura y escritura Engram bajo ambiguedad; continuar local. | Repositorio y Git siguen validos. |
| Lectura | Lectura acotada devuelve resultado valido, incluido vacio validado. | Marcar fallida o ausente; no afirmar continuidad recuperada. | Resolucion y repositorio siguen validos. |
| Escritura | Escritura acotada confirma observacion o resumen. | Marcar continuidad fallida; no afirmar guardado. | Repositorio y lecturas previas siguen validos. |
| Asociacion de prompt | Evidencia directa confirma asociacion. | Marcar desconocida o fallida; no degradar escritura. | Escritura sigue exitosa. |
| Descubrimiento de conflictos | Respuesta candidata valida. | Marcar fallido o ausente; no inventar no-conflict. | Lectura o escritura sigue exitosa. |
| Revision de ciclo | Operacion soportada devuelve antiguedad. | Marcar ausente y verificar desde propietario. | Otras operaciones siguen validas. |
| Captura de plugin | Evidencia confirma sesion y proyecto. | Marcar no verificada o fallida; no inferir por instalacion. | Operaciones manuales siguen validas. |
| Sync | Evidencia soportada confirma resultado acotado. | Marcar desconocido o fallido; no inferir continuidad remota. | Engram local y repositorio siguen validos. |

Ausencia Engram degrada continuidad solo salvo politica explicita. Nunca prueba drift ni crea store alterno.

## Aprobacion humana y paradas exactas

Requieren aprobacion:

- Diseno final e implementacion.
- Repositorio o proyecto ambiguo.
- Eleccion entre propietarios o criterios genuinamente conflictivos.
- Promocion discrecional a documentacion o ciclo.
- Cambio de status, prioridad, dependencia, condicion o alcance RH-W.
- Rechazo de direccion valida, borrado historico o limpieza destructiva.
- Excepcion localizada semantica en lugar de reparacion.

Detener solo rama afectada cuando proyecto o propietario es ambiguo, hallazgo obligatorio no se puede clasificar, entrega es incompleta, contradiccion requiere autoridad, pares chocan semanticamente sin resolucion o mutacion excede aprobacion. No preguntar hechos verificables por fuente, pruebas, docs, checker o Git.

## Destinos canonicos y misma unidad

- Ciclo y avance: `docs/repo-harness/roadmap.md`.
- Proyeccion activa: `docs/repo-harness/current-state.md` solo si cambia.
- Razon aprobada: `docs/repo-harness/decisions.md`.
- Cronologia completada: `CHANGELOG.md`.
- Pares espanoles: misma unidad y semantica equivalente.
- Continuidad significativa: actualizacion concisa al topic o resumen Engram existente.
- Contratos fuente: propietarios existentes del mapa de impacto.

Nunca crear log, cache, recibo, copia de backlog, store de conflicto, entrega o ciclo. Este borrador es el unico diseno RH-W-0050 y se actualiza en sitio.

## Secuencia posterior a entrega

1. Verificar entrega con evidencia aplicable.
2. Ejecutar ciclo completo siempre.
3. Clasificar hallazgos e incompletitud por separado.
4. Reconciliar roadmap, estado, decisiones o changelog cuando aplique, pares y espejo conciso en una unidad.
5. Verificar enlaces, paridad, pruebas, checker y alcance.
6. Repetir scan acotado.
7. Solo con entrega completa y sin hallazgo obligatorio permitir recomendacion autoritativa.

Con evidencia incompleta, emitir `next_ready: suppressed`, nombrar brecha y continuar solo discusion o remediacion. No inferir entrega por commit, ahead-only, memoria, issue o wording humano.

## Observabilidad y salida

```text
repository: <resultado y clase>
bounded_scan: <completed | failed> (<alcance>)
reconciliation: <not_triggered | completed | paused | blocked | failed>
trigger: <senales obligatorias o none>
findings: <conteos e IDs pendientes por clase>
canonical_updates: <none | rutas | paused | failed>
localized_parity: <not_applicable | verified | drift | paused | failed>
delivery_evidence: <not_applicable | complete | incomplete | unknown>
next_ready: <unidad | suppressed | not_requested>
git: <not_git | resultado local | rama opcional>
engram.project_resolution: <resultado>
engram.read: <resultado>
engram.write: <resultado>
engram.prompt_association: <resultado>
engram.conflict_discovery: <resultado>
engram.lifecycle_review: <resultado>
engram.plugin_capture: <resultado>
engram.sync: <resultado>
next_action: <una accion o decision>
```

Solo campos aplicables. Ningun estado agregado Engram reemplaza operaciones. Salida efimera, no recibo persistido.
Los valores entre angulos son metavariables del schema para revision de implementacion, no placeholders de diseno sin resolver.

## Escenarios RED/GREEN enfocados

Estos son criterios de implementacion propuestos. RED falla solo porque la obligacion nombrada esta ausente; GREEN usa el cambio de contrato mas pequeno.

| Escenario | RED | GREEN |
|---|---|---|
| Inicio normal sin trigger | No prueba comportamiento acotado. | Scan relevante, `not_triggered`, routing normal sin escrituras. |
| Cierre no-op | No define no-op. | Sin cambio o entrega, no hay escritura, changelog, memoria duplicada ni recibo. |
| Posterior a entrega | Puede enrutar directo. | Ciclo completo precede routing autoritativo. |
| Conflicto de propietario | Puede elegir silenciosamente. | Detiene rama con evidencia y una decision humana. |
| Conflicto localizado | Drift espanol puede pasar como wording. | Gana ingles; se repara par o pausa por excepcion genuina. |
| Sin Git | Ausencia bloquea o activa. | Continua local, entrega Git desconocida y ausencia no activa. |
| Senales Engram consultivas | Candidato o edad activa o prueba. | Siguen consultivas hasta verificacion canonica. |
| Conflicto memoria-repositorio | Memoria supera o se ignora. | Gana repositorio, se clasifica y solo se pregunta si aplica. |
| Fallos parciales Engram | Un fallo colapsa todo. | Lectura o escritura exitosa se preserva ante otros fallos. |
| Engram ausente | Crea drift o store. | Degrada continuidad segun politica y continua local sin store. |
| Sin store duplicado | Puede agregar cache, recibo o ledger. | Pruebas rechazan cada store durable nuevo. |

Casos adicionales: entrega incompleta, hallazgo no clasificado, peticion humana, suciedad no relacionada, ahead-only, search vacio validado y captura no verificada. Pertenecen al modulo de pruebas existente.

## Rollback y compatibilidad

Propuesta aditiva en contratos. Resultados actuales, seguridad Git, localizacion, standalone, integrated y propietarios siguen validos.

Rollback restaura wording y pruebas previas juntos y resincroniza proyecciones con renderer existente. Sin schema o store no hay migracion. Engram existente no cambia. Harnesses viejos operan sin estas garantias hasta regeneracion explicita aprobada.

## Mapa de impacto

No aprueba edicion:

| Simbolo o ruta | Impacto propuesto |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill` | Scan, triggers, resultados y routing posterior. |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_stop_skill` | No-op, ciclo posterior, misma unidad y supresion. |
| `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs` | Extender regla existente solo si hace falta; sin archivo nuevo por defecto. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render` y `render_with_skill_mapping` | Preservar paths y modos; sin input persistido. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Escenarios RED/GREEN en suite existente. |
| `skill/repo-harness-builder/references/architecture.md` | Limites y razon si se aprueba. |
| `skill/repo-harness-builder/references/validation.md` | Verificacion enfocada si se aprueba. |
| `.repo-harness/skills/repo-harness-session-start/SKILL.md` | Contrato generado de inicio. |
| `.repo-harness/skills/repo-harness-session-stop/SKILL.md` | Contrato generado de cierre. |
| `.repo-harness/rules/memory.md` | Memoria por operacion, solo si se aprueba aclaracion. |
| `.repo-harness/rules/working-agreements.md` | Trabajo acotado, solo si ownership lo exige. |
| `.opencode/skills/repo-harness-builder/**` y `.claude/skills/repo-harness-builder/**` | Proyecciones exactas de fuente cambiada. |
| Proyecciones de `render(...)` | Solo destinos existentes, sin path o store nuevo. |
| Roadmap, estado, decisiones, changelogs, pares y Engram | Propietarios existentes, solo cuando evidencia exige. |

## Decisiones recomendadas pendientes

- Adoptar C.
- Usar clasificacion obligatoria, consultiva y ruido de este documento.
- Hacer post-entrega trigger incondicional antes de routing.
- Mantener observabilidad efimera y por operacion, sin agregado o recibo.
- Implementar solo mediante propietarios existentes.

Ninguna queda aprobada por este borrador.

## Preguntas abiertas

1. Debe cambio de propietario expandir a dependientes directos? Recomendacion: solo si el campo cambia su routing, con limite determinista revisado.
2. Que valida search Engram vacio entre superficies soportadas? Recomendacion: recibo soportado o control positivo en la misma superficie, dejando prueba especifica a revision de implementacion.
3. Correccion documental de ciclo cuenta como post-entrega? Recomendacion: si cambia ciclo o siguiente unidad; no para reparacion de wording localizado.

No permiten avance. Revision externa debe resolverlas o acotarlas antes de aprobacion.

## Checklist de revision externa

- [ ] Estado es borrador pendiente de revision y aprobacion.
- [ ] RH-W-0050 sigue `proposed`; no implica implementacion o entrega.
- [ ] Autoridad del repositorio y continuidad Engram son consistentes.
- [ ] C se recomienda, no se afirma aprobado.
- [ ] Scan tiene limite determinista y no persiste cache o recibo.
- [ ] Triggers obligatorios son necesarios; consultivos y ruido no activan solos.
- [ ] Clasificaciones son claras y sustentadas.
- [ ] Supresion post-entrega evita routing falso.
- [ ] Git y no-Git son operables.
- [ ] Degradacion Engram preserva exito parcial y proyecto explicito.
- [ ] Paradas preguntan solo decisiones humanas.
- [ ] Destinos y paridad evitan verdad duplicada.
- [ ] Escenarios son enfocados y compatibles con RED/GREEN estricto.
- [ ] Mapa usa simbolos y paths estables sin lineas fragiles.
- [ ] Rollback no necesita schema, migracion o borrado.
- [ ] Preguntas abiertas son genuinas.
- [ ] Ingles y espanol son semanticamente equivalentes.
- [ ] No quedan U+2013, U+2014, placeholders, contradicciones de aprobacion ni autorizacion oculta.
