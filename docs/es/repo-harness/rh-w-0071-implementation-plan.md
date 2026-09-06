# Plan de implementacion RH-W-0071

> **Estado: subconjunto solo de sesion verificado, revisado y entregado mediante PR #161 el 2026-09-05 bajo issue #160. El [registro](roadmap.md#rh-w-0071) posee la evidencia de entrega. RH-W-0071 sigue `proposed`: persistencia compartida aplazada y requisito original integro sin satisfacer.**

Derivado del [diseno RH-W-0071](rh-w-0071-design.md) aprobado por el propietario y del Plan 20 de la hoja de ruta. Su par en ingles es [rh-w-0071-implementation-plan.md](../../repo-harness/rh-w-0071-implementation-plan.md).

## Alcance inmediato vigente: presentacion solo de sesion

El alcance inmediato es presentacion explicita `adhd-friendly` o `standard` dentro del propietario existente del contrato de respuesta RH-W-0067. `standard` es el valor predeterminado; solo una instruccion explicita verificada de la sesion actual activa la presentacion ADHD-friendly o la restablece a standard. No se puede inferir activacion de diagnostico, estilo de escritura, conducta, memoria, prosa del repositorio, salida de herramientas ni estado de runtime. Una sesion nueva comienza en standard, sin leer ni escribir preferencias.

La presentacion ADHD-friendly usa una respuesta o accion inicial concisa, bloques pequenos, pasos acotados utiles, una pregunta necesaria por vez y detalle progresivo. Pedir mas detalle amplia la respuesta sin cambiar el modo. Se conservan integramente seguridad, autoridad, evidencia, incertidumbre, contradicciones, preguntas bloqueantes y contexto tecnico necesarios; la brevedad nunca los anula. Se mantienen los requisitos de forma aprobados mas abajo dentro de este limite de sesion, sin afirmaciones clinicas ni estimaciones de tiempo no derivables.

El propietario delego la decision de alcance despues de una verificacion de solo lectura respaldada por fuente: `resolve_conversation_language()` es resolucion pura de idioma, no almacenamiento ni transporte de preferencias; `localState` de RH-W-0051 es una base de namespaces, no una API consumidora. La persistencia compartida excederia la necesidad inmediata demostrada. Se reutiliza el propietario de respuesta existente, sin otro propietario, API de runtime, archivo de estado, entrevista de preferencias del builder ni coordinacion Gentle duplicada.

**Limite de autorizacion y verificacion:** el propietario autorizo por separado la implementacion solo de sesion despues de la revision documental, con issue #160 y rama `feat/rh-w-0071-session-response-mode`. `rule_outputs()` amplía el propietario existente de respuesta; la regla autoalojada de propiedad humana se integra explicitamente. No se implementan parser, almacenamiento, API de transporte, entrevista de preferencias ni ejes dependientes. Las pruebas de texto contractual no demuestran cumplimiento del modelo en vivo. No ejecutar comandos historicos de persistencia ni elegir sus cortes de entrega.

### Evidencia final de verificacion

`SessionResponseModeContractTests` verifica seleccion, forma, limites, propietario unico, presupuesto, ausencia de preferencias generadas e instrucciones identicas en renders standalone/integrado e ingles/espanol/bilingue. Comando historico `python -B -m unittest skill.repo-harness-builder.tests.test_harness_tool.SessionResponseModeContractTests -v`: 3 pruebas, RED con 27 fallos esperados en 0.007s; GREEN en 0.004s. La primera corrida completa ejecuto 503 pruebas en 154.092s con 9 omisiones y un fallo por conteo documental obsoleto (500); ambos idiomas se corrigieron antes de la verificacion final.

Recibos finales del informe de implementacion, despues de normalizar: `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"` ejecuto 503 pruebas en 154.714s, OK (skipped=9), salida 0; regresion enfocada de 20 pruebas en 0.763s, salida 0; `python .repo-harness/check.py`: `current`, 63 proyecciones, 0 escrituras, salida 0; `git diff --check`: salida 0. Corresponden a la implementacion entregada, no a una repeticion para este seguimiento documental. Las pruebas contractuales no demuestran transiciones ni cumplimiento del modelo en vivo. El roadmap registra revision nativa del arbol exacto y entrega del agente padre; esa aprobacion no cubre ediciones documentales posteriores.

## Persistencia compartida aplazada: acuerdo conservado

La persistencia compartida de tres ejes queda aplazada, no cancelada ni satisfecha por la revision solo de sesion. Se conserva el contrato Markdown estructurado aprobado en `.repo-harness/state/preferences/user-preferences.md` que aparece abajo; el escalar JSON anterior nunca se implemento. RH-W-0071 conserva persistencia compartida y `response-structure`, RH-W-0082 conserva `explanation-style` y RH-W-0018 conserva `interaction-behavior`, con sus dependencias intactas. Esta revision no implementa los ejes dependientes ni absorbe consentimiento o procedencia de idioma.

La propiedad y API del lector o transporte de runtime nombrado y la agrupacion original de entrega siguen sin resolver solo para la persistencia aplazada. No bloquean la implementacion solo de sesion autorizada por separado. Reabrir persistencia exige autorizacion explicita del propietario, evidencia actual de fuente y reconciliar esas compuertas y propietarios dependientes antes de implementar. Aqui no se selecciona arquitectura de transporte ni agrupacion de entrega nueva.

## Plan historico aprobado de persistencia (no son instrucciones vigentes de ejecucion)

Todo lo que sigue conserva el plan mas amplio aprobado el 2026-08-30, incluidas compuertas, cortes, comandos de pruebas, criterios de aceptacion y rollback. Es guia historica y aplazada, no un plan ejecutable para el alcance inmediato. Las secciones vigentes anteriores reemplazan, para el trabajo solo de sesion, sus afirmaciones de que toda implementacion espera las compuertas de persistencia. No se afirma completar el requisito original integro.

## Evidencia del repositorio confirmada antes de planear

Reconfirmar estas afirmaciones al implementar en vez de arrastrar la instantanea historica `c4387e4`:

| Afirmacion | Evidencia | Estado |
|---|---|---|
| `state/` es un espacio de estado local reservado | `manifest.json` tiene `localState.state` como `{"class": "operational", "path": ".repo-harness/state/"}` | verificado |
| El espacio esta ignorado por Git | el `.gitignore` raiz contiene `/.repo-harness/state/` | verificado |
| El checker valida el conjunto de espacios | `check()` compara la declaracion de estado local del manifest con `LOCAL_STATE` | verificado |
| `state/preferences/` existe como ruta reservada | ninguna implementacion de persistencia vigente posee esa subruta | **no verificado** |
| RH-W-0067 posee el contrato de respuesta | `render_with_skill_mapping()` produce `.repo-harness/rules/response-contract.md`; su presupuesto debe medirse de nuevo al comenzar la implementacion | verificado |
| Existe fila de compuerta hacia el contrato de respuesta | `REPO-HARNESS.md` enruta a `.repo-harness/rules/response-contract.md` | verificado |

### Una correccion que lleva este plan

La redaccion anterior del Plan 20 y del registro RH-W-0071 exageraba la reserva como `state/preferences/`. Se conserva aqui la distincion corregida: RH-W-0051 reservo `state/`, no `state/preferences/`.

Nada se rompe: `/.repo-harness/state/` en `.gitignore` ya cubre toda subruta. RH-W-0051 posee ruta, contencion, ignore Git y mecanica de estado local; RH-W-0071 posee persistencia compartida y semantica de estructura de respuesta. `state/preferences/` sigue dentro de un namespace reservado e ignorado.

## Fundamento externo

El diseno declara los requisitos de forma ADHD-friendly sin citar fuente. No estan inventados: siete de los nueve corresponden a patrones nombrados de W3C COGA. Queda registrado aqui, en documentacion del repositorio, y deliberadamente fuera de todo artefacto de producto, porque el diseno prohibe referencias externas alli.

| Requisito del diseno | Patron W3C COGA |
|---|---|
| capa inicial concisa | 4.4.5 Keep Text Succinct |
| trozos pequenos | 4.4.10 Use White Spacing |
| divulgacion progresiva | 4.9.3 Support Simplification |
| siguiente accion visible, pasos numerados acotados | 4.2.4 Make Each Step Clear |
| suprimir tangentes y relleno | 4.6.1 Limit Interruptions |
| oferta de mas detalle | 4.8.2 Provide Alternative Content for Complex Information |
| una pregunta por vez | 4.5.4 Design Forms to Prevent Mistakes |

La regla de una pregunta por vez ademas coincide con el patron "one thing per page" de GOV.UK, cuya justificacion declarada es reducir la carga cognitiva.

**Lo que la evidencia no respalda, dicho sin adornos.** El troceado como acomodacion especifica para TDAH tiene respaldo empirico mixto; al menos un estudio controlado de trabajo troceado no encontro diferencia significativa de productividad. La base de evidencia de la divulgacion progresiva es en gran medida de casos practicos y no de ensayos controlados, y GOV.UK mismo dice que "one thing per page" no es universalmente obligatorio y debe validarse por contexto.

Esto no debilita la unidad, porque el diseno nunca hace una afirmacion clinica. El modo es una preferencia de presentacion pedida explicitamente que el diseno prohibe inferir de diagnostico, conducta o cualquier otra senal. La implementacion debe mantenerlo asi: ningun artefacto de producto puede describir el modo como beneficio terapeutico o clinico.

## Compuertas bloqueantes previas a la implementacion

Ninguna tarea de implementacion, prueba RED, edicion de producto, sincronizacion de proyecciones ni entrega de corte puede comenzar hasta que el propietario resuelva las dos compuertas restantes y autorice por separado la implementacion:

1. **Compuerta del contrato de persistencia:** resuelta el 2026-08-30 y reemplazada antes de implementar por el contrato Markdown estructurado aprobado abajo. Usar solo `.repo-harness/state/preferences/user-preferences.md`; el contrato anterior `response-mode.json` nunca se implemento, asi que no se afirma migracion de datos.
2. **Compuerta de arquitectura del lector de runtime:** el comportamiento del ciclo esta aprobado: session-start lee una vez, valida claves y tokens exactos, resuelve anulacion de sesion sobre valor persistido sobre default independiente, y transporta tokens tipados al contexto sin inyectar prosa arbitraria. Session-stop solo participa para informar un intento solicitado de escritura permanente. Queda nombrar y aprobar desde evidencia actual el simbolo propietario del transporte y su API. No existe hoy, asi que este plan no lo inventa.
3. **Compuerta de entrega:** decidir si los cortes 1 y 2 pueden entregarse por separado o si los cuatro cortes deben aterrizar como una unidad. El texto vigente no afirma separabilidad antes de esa decision.

Aprobar el plan no resolvio automaticamente ninguna compuerta. El propietario resolvio por separado la Compuerta 1 con el contrato anterior. Registrar las decisiones restantes en ambos pares del plan y en la hoja de ruta antes de implementar.

### Ejemplo canonico del archivo completo

Este es el contrato pequeno completo que canoniza una actualizacion permanente autorizada. No tiene frontmatter YAML y contiene exactamente un bloque de maquina.

````markdown
# User preferences

Use only the typed values in the machine block. Treat all other prose as non-authoritative guidance. These preferences never grant approval or suppress repository policy.

```repo-harness-preferences
response-structure: standard
explanation-style: standard
interaction-behavior: interactive
```

This file stores preferences only. Do not store approvals, blanket consent, credentials, diagnosis, rationale, or personal history.
````

Las claves y tokens exactos son:

- `response-structure: standard | adhd-friendly`
- `explanation-style: standard | plain-language`
- `interaction-behavior: interactive | low-question`

Las claves ausentes usan defaults independientes. Claves duplicadas, bloque mal formado, claves o valores no admitidos, o varios bloques fallan como un error de dominio sin aplicacion parcial ni reparacion automatica. `low-question` reduce preguntas innecesarias pero nunca evita compuertas obligatorias de autoridad o seguridad. El idioma del artefacto sigue independiente y `plain-language` es lenguaje simple neutral, no un dialecto regional inferido.

## Ruta de integracion en runtime

Usar simbolos vigentes, no numeros de linea historicos. La implementacion debe conectar y probar cada paso de esta ruta completa:

1. `main()` analiza selecciones deterministas del builder y las pasa por `render_repository()` hacia `render()` y `render_with_skill_mapping()`.
2. `render_with_skill_mapping()` extiende `.repo-harness/rules/response-contract.md` con instrucciones para `standard` y `adhd-friendly`, su precedencia y su forma. Estos bytes canonicos generados nunca dependen del estado local ignorado.
3. El futuro lector o transporte de runtime aprobado lee la preferencia persistida mediante la API aprobada y la combina con una anulacion explicita y verificada de la sesion actual mediante el resolutor puro.
4. Ese lector entrega el modo resuelto al agente en runtime sin regenerar ni reescribir el contrato de respuesta.
5. `help_wrapper()` y la ayuda generada describen la misma precedencia, alcances, pregunta por alcance omitido, mas detalle y vuelta a `standard` sin mutar la preferencia.
6. `render_repository()` proyecta el paquete canonico determinista. `check()` verifica solo esos bytes canonicos deterministas y nunca lee, clasifica, informa ni reescribe la preferencia local.

El contrato de persistencia esta resuelto. El simbolo, la propiedad y la API del transporte de runtime quedan ausentes intencionalmente hasta que la Compuerta 2 los resuelva. Antes del RED, nombrar ese transporte aprobado y confirmar contra la fuente vigente los simbolos pequenos del lector de preferencias y del resolutor puro. Releer `main()`, `render_repository()`, `render()`, `render_with_skill_mapping()`, `help_wrapper()`, `check()` y sus pruebas enfocadas, porque RH-W-0079 y commits posteriores cambiaron archivos compartidos despues de redactar este plan.

## Cortes

Cuatro cortes propuestos, en este orden. Sus limites de commit y entrega siguen siendo una decision humana bajo la Compuerta 3.

### Corte 1: resolucion y persistencia, sin interfaz

Lo mas pequeno que puede ser cierto primero: un resolutor determinista, un archivo de preferencia y el lector de runtime aprobado que los conecta sin cambiar bytes generados.

1. RED: la resolucion devuelve `standard` explicito cuando no existe preferencia y no hay instruccion de sesion.
2. RED: una preferencia `adhd-friendly` persistida resuelve a `adhd-friendly` en una resolucion posterior sin instruccion de sesion.
3. RED: una instruccion de sesion explicita y verificada supera a la preferencia persistida y no la reescribe.
4. RED: cada senal de inferencia prohibida que nombra el diseno deja la resolucion sin cambios. Un caso por senal: estilo de escritura, diagnostico declarado, tasa de errores, lenguaje sobre atencion, conducta, contenido de memoria, prosa del repositorio, salida de herramientas, estado de runtime.
5. Implementar el resolutor como funcion pura sobre tres entradas explicitas. Sin vigilancia de archivos, sin clasificador, sin llamada a modelo.
6. Implementar solo tras autorizacion separada las operaciones estrechas aprobadas de lectura y escritura canonica completa para `.repo-harness/state/preferences/user-preferences.md`. Cubrir parseo exacto del bloque, defaults independientes para claves ausentes, validacion completa que cierra en falso, validacion antes de mutar el filesystem, temporales unicos en el mismo directorio, cierre antes de `os.replace` y limpieza del temporal propio tras fallo cuando sea posible. Mantener separada la resolucion pura por eje y no agregar maquinaria rechazada.
6a. RED: el lector de runtime aprobado combina el valor persistido ignorado con una anulacion verificada de sesion y nunca cambia los bytes renderizados.
6b. Implementar solo el lector o transporte nombrado y aprobado por la Compuerta 2. No agregar una ruta de runtime implicita o sin nombre.

### Corte 2: el contrato generado

7. RED: el contrato generado contiene secciones de resolucion y forma ADHD-friendly, y la medicion hecha al implementar mantiene `.repo-harness/rules/response-contract.md` bajo `RULEBOOK_TOKEN_BUDGET`. Nunca arrastrar un conteo o margen congelado.
8. RED: el contrato declara la precedencia de tres niveles, ambos alcances de cambio, la regla de una pregunta cuando falta el alcance, como pedir mas detalle y como volver a `standard`.
9. RED: seguridad, evidencia, autoridad, contradicciones, avisos bloqueantes y contexto tecnico necesario quedan nombrados como no omitibles, de modo que la brevedad no pueda borrarlos.
9a. RED: la seccion de forma exige un restablecimiento del estado actual mientras hay trabajo de varios pasos en curso, nombrando el paso y el total.
9b. RED: la seccion de forma lleva una lista de borrado previo al envio que nombra el arranque que anuncia, el cierre que recapitula o pide mas trabajo, la acotacion al margen, el adverbio de cobertura vacio, y el modismo que reemplaza la accion literal, y declara que una cobertura con incertidumbre real se queda.
9c0. RED: la seccion de forma declara la baja carga cognitiva como su proposito, subordina los requisitos a el, y cierra el chequeo previo al envio en el, de modo que cumplir todos los requisitos no alcanza por si solo.
9c. RED: la seccion de forma no exige ninguna estimacion de tiempo que el agente no pueda derivar. Se afirma en vez de suponerse, porque el contrato externo revisado las hace obligatorias y el reglamento prohibe sustituir evidencia por lenguaje de confianza.
10. Extender el contrato RH-W-0067 existente. No agregar segundo propietario de respuesta, ni archivo de reglas, ni fila de compuerta.
11. Sincronizar las tres proyecciones de runtime y confirmar que el checker informa `current` con cero escrituras.

### Corte 3: entrevista del constructor y ayuda

12. RED: la entrevista presenta exactamente dos opciones y registra `standard` de forma explicita cuando la persona lo elige, nunca por omision.
13. RED: la entrevista nunca preselecciona `adhd-friendly` desde ningun contexto.
14. RED: la ayuda explica el orden de resolucion, ambos alcances, la regla de una pregunta, el comportamiento de mas detalle y la ruta de vuelta a `standard`.
15. Implementar ambas, en ambos locales de documentacion.

### Corte 4: pregunta de alcance, migracion, rollback

16. RED: una instruccion de cambio que omite alcance produce exactamente una pregunta permanente-o-solo-esta-sesion y no muta nada antes de responderse.
17. RED: un pedido de mas detalle expande una sola respuesta y deja el modo y la preferencia persistida sin cambios.
18. RED: una instalacion existente sin preferencia resuelve a `standard` explicito, sin migracion y sin inferencia.
19. RED: el rollback deja de leer la preferencia y devuelve la resolucion a `standard` explicito sin borrar estado de preferencias no relacionado.
20. RED: standalone e integrado se comportan igual en seleccion, persistencia, precedencia y forma.

### Matriz de requisito a RED

Cada uno de los 11 requisitos aprobados obtiene una asercion fallida distinta antes de implementar producto:

| Requisito | Prueba RED distinta |
|---|---|
| respuesta o accion primero | rechaza un arranque anunciador antes de la respuesta o accion |
| capa inicial concisa | exige una capa inicial acotada antes del detalle revelado |
| trozos pequenos | rechaza una forma densa sin division |
| pasos numerados y acotados cuando sirvan | exige pasos ordenados y acotados para un fixture de varios pasos |
| una pregunta por vez | rechaza dos preguntas simultaneas |
| divulgacion progresiva | exige que el detalle quede tras una ruta explicita de expansion |
| siguiente accion visible | exige una accion siguiente visible cuando queda accion |
| suprimir tangentes y relleno | rechaza las formas de la lista de borrado y conserva coberturas con incertidumbre real |
| ofrecer o dar mas detalle | prueba que expandir cambia solo la respuesta actual |
| restablecer estado durante trabajo de varios pasos | exige paso actual y total sin volver a narrar |
| chequeo de borrado previo al envio | exige lista completa y comprobacion final de baja carga cognitiva |

Agregar una asercion RED separada que rechace las **estimaciones de tiempo obligatorias no derivables**. Es un candidato rechazado, no un duodecimo requisito.

## Contrato ejecutable de verificacion

Despues de resolver compuertas y autorizacion, crear primero los metodos nombrados de `ResponseModeContractTests` y ejecutar:

```powershell
python -B -m unittest skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_resolution_persistence_and_session_override skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_each_prohibited_inference_signal_is_ignored skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_each_approved_shaping_requirement_and_time_estimate_rejection skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_builder_help_scope_migration_rollback_and_runtime_parity -v
python -B -m unittest skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_resolution_persistence_and_session_override skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_each_prohibited_inference_signal_is_ignored skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_each_approved_shaping_requirement_and_time_estimate_rejection skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_builder_help_scope_migration_rollback_and_runtime_parity -v

# El JSON de Engram siguiente es un ejemplo de marcador. Reemplazarlo con evidencia actual, veraz y saneada del host antes de ejecutar el comando.
python skill/repo-harness-builder/scripts/harness_tool.py render --root . --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --documentation-language en --documentation-language es --conversation-language espanol --activation maintenance --engram-evidence-json '{"surface":"mcp","status":"verified","evidence":"SYNTHETIC_PLACEHOLDER_REPLACE_WITH_SANITIZED_HOST_EVIDENCE"}'
# Repetir el comando impreso con --apply y su --approved-plan-sha256 solo tras autorizar ese preview.

python -B -m unittest skill.repo-harness-builder.tests.test_harness_tool.GeneratedCoreTests.test_runtime_projections_use_the_current_canonical_contract skill.repo-harness-builder.tests.test_harness_tool.GeneratedCoreTests.test_self_hosted_render_projects_the_complete_builder_package_to_every_runtime skill.repo-harness-builder.tests.test_harness_tool.RuleFileTests.test_rulebook_gates_route_to_every_rule_file -v
python .repo-harness/check.py
git diff --check
```

El primer comando prueba RED enfocado y debe fallar solo por RH-W-0071; el segundo es GREEN enfocado identico.

```powershell
python -c "from pathlib import Path; e=Path('docs/repo-harness/rh-w-0071-implementation-plan.md').read_text(encoding='utf-8'); s=Path('docs/es/repo-harness/rh-w-0071-implementation-plan.md').read_text(encoding='utf-8'); ep=e.split('## Executable Verification Contract')[0]; sp=s.split('## Contrato ejecutable de verificacion')[0]; er=['answer or action first','concise initial layer','small chunks','numbered bounded steps where useful','one question at a time','progressive disclosure','visible next action','suppress tangents and filler','offer or provide more detail','restate current state during multi-step work','pre-send removal check']; sr=['respuesta o accion primero','capa inicial concisa','trozos pequenos','pasos numerados y acotados cuando sirvan','una pregunta por vez','divulgacion progresiva','siguiente accion visible','suprimir tangentes y relleno','ofrecer o dar mas detalle','restablecer estado durante trabajo de varios pasos','chequeo de borrado previo al envio']; assert ep.count('### Slice ')==4 and sp.count('### Corte ')==4; assert all(x in ep for x in er) and all(x in sp for x in sr); assert ep.count('**Persistence contract gate:**')==1 and ep.count('**Runtime reader architecture gate:**')==1 and ep.count('**Delivery gate:**')==1; assert sp.count('**Compuerta del contrato de persistencia:**')==1 and sp.count('**Compuerta de arquitectura del lector de runtime:**')==1 and sp.count('**Compuerta de entrega:**')==1; assert 'Status: approved by the owner' in e and 'Estado: aprobado por el propietario' in s; assert not any(chr(c) in e+s for c in (0x2013,0x2014))"
```

Luego ejecutar pruebas RH-W-0071 y las tres regresiones anteriores. No ejecutar la suite amplia no relacionada. Registrar comando, salida, conteo, `current` y `writes: 0`.

## Controles que no deben agregarse

Nombrados aqui para que un lector posterior no los reabra:

- Sin campo de manifiesto para la preferencia activa. Es estado local revocable, no configuracion rastreada.
- Sin lectura, clasificacion, reescritura ni reporte del valor de la preferencia por parte del checker. El chequeo sigue de solo lectura.
- Sin entrada de preferencia local en los bytes generados. El contrato siempre describe ambos modos; solo el lector de runtime aprobado resuelve el modo activo.
- Sin clasificador, llamada a modelo, subagente, medidor de legibilidad, parser, demonio, monitor, telemetria ni fan-out. La resolucion es una consulta mas prosa determinista.
- Sin segundo propietario del contrato de respuesta. RH-W-0067 conserva ese rol.
- Sin ninguna ruta de inferencia, y sin via por la que pudiera agregarse despues sin reabrir esta unidad.
- Sin frontmatter YAML, notas autoritativas libres, framework de schema, registro generico de preferencias, `fsync`, locks, watchers, reintentos, backups, journaling, maquina de estados ni reparacion automatica.

## Criterios de aceptacion

- La resolucion es determinista en los tres niveles y comprobablemente inmutable ante cada senal prohibida, afirmado por senal y no en agregado.
- Una instruccion de solo sesion nunca reescribe la preferencia del repositorio, y un pedido de mas detalle nunca cambia el modo.
- Un alcance omitido produce exactamente una pregunta y ninguna mutacion.
- El contrato generado lleva resolucion de modo y forma ADHD-friendly, declara la baja carga cognitiva como el proposito al que sirven los requisitos, nombra las categorias no omitibles, lleva el restablecimiento de estado y la lista de borrado previo al envio, no exige ninguna estimacion de tiempo no derivable, y `response-contract.md` sigue bajo `RULEBOOK_TOKEN_BUDGET`.
- El constructor registra una eleccion `standard` explicita por defecto y nunca preselecciona `adhd-friendly`.
- No aparece campo de manifiesto, estado de checker ni cambio de esquema. `render()` y `check()` quedan identicos byte a byte para toda entrada que no seleccione el modo.
- Las tres proyecciones de runtime quedan identicas byte a byte a la canonica y el checker informa `current` con cero escrituras.
- Ambos locales de documentacion se reconcilian en la misma unidad de trabajo.
- Ningun artefacto de producto describe el modo como beneficio clinico o terapeutico, y ninguno referencia una skill, repositorio o dependencia externa de TDAH.

## No objetivos

Sin diagnostico, clasificacion, telemetria ni monitoreo conductual. Sin arquitectura de consentimiento, que queda en RH-W-0046 y la diferida RH-W-0018. Sin enrutamiento de memoria, que queda en RH-W-0066. Sin trabajo general de seguridad de recursos, que queda en RH-W-0070. Sin cambio a la precedencia de idioma de RH-W-0031 mas alla de reutilizar su regla de procedencia. Sin evaluacion con modelo en vivo y sin suite de verificacion costosa.

## Rollback por corte

Revertir en orden inverso de dependencia y verificar despues de cada limite:

1. **Corte 4:** retirar alcance, migracion, orquestacion standalone/integrada y pruebas. Verificar seleccion, ayuda, contrato, resolutor y persistencia.
2. **Corte 3:** retirar entrevista y ayuda/estado con sus pruebas. Verificar contrato, resolutor y persistencia sin exposicion seleccionable.
3. **Corte 2:** retirar solo adiciones RH-W-0071 de `render_with_skill_mapping()` y proyecciones. Verificar RH-W-0067, paridad, checker `current` con cero escrituras y Corte 1.
4. **Corte 1:** dejar de llamar y retirar resolutor y API aprobada. Volver a `standard`, conservar estado no relacionado bajo `preferences/`, y verificar preferencia ausente, checker, `git diff --check` y ausencia de simbolos RH-W-0071.

Si la Compuerta 3 exige una entrega, tratar estos como limites internos en el mismo orden inverso. Nunca borrar todo el directorio de preferencias.

## Riesgos

- **La seccion de forma compite por un presupuesto compartido.** Medir de nuevo la regla generada al implementar. Si no entra, baja detalle; el presupuesto no sube.
- **Las pruebas de senales prohibidas son toda la garantia de no inferencia.** Afirmadas por senal y no en agregado, porque una asercion agregada pasa mientras una senal se filtra.
- **El corte 3 toca la entrevista del constructor**, que es la superficie de la que depende una instalacion. Va despues de los cortes 1 y 2 para que el comportamiento que expone ya exista y ya este afirmado.

## Decisiones humanas requeridas antes de implementar

1. Completado el 2026-08-30: el propietario aprobo este plan corregido. La aprobacion no autoriza producto.
2. Completado el 2026-08-30: el propietario resolvio por completo la compuerta de persistencia.
3. Nombrar y aprobar el lector o transporte de runtime y su API bajo la compuerta de arquitectura.
4. Resolver la compuerta de entrega por cortes.
5. Autorizar implementacion por separado solo despues de registrar 1 a 4. Hasta entonces no puede ejecutarse ninguna tarea de implementacion.
