# Plan de implementacion RH-W-0079 de capacidades opcionales

Estado: `ready`, completo y aprobado por el propietario el 2026-08-28 despues de que una revision final nueva devolviera PASS. El [diseno](rh-w-0079-design.md) aprobado es autoritativo. El `ok comitear entonces ?` del propietario y su seleccion del flujo normal de contribucion aprobaron commitear el plan revisado y, por tanto, aprobaron el plan, pero no autorizaron implementacion. La contribucion de documentacion, ciclo de vida, diseno y planificacion se entrego mediante el issue [#129](https://github.com/LCubero/repo-harness-builder/issues/129), ya cerrado, y el PR [#130](https://github.com/LCubero/repo-harness-builder/pull/130), ya fusionado, en `dedf74ec7fe3a2d3f2f7ead0fe6f98dc93eb0f55`. Este plan no autoriza implementacion, release, inicializacion Git, otro commit, push, issue ni pull request.

Par canonico: [RH-W-0079 Optional Capability Implementation Plan](../../repo-harness/rh-w-0079-implementation-plan.md).

## Contrato de ejecucion

- Ejecutar con pruebas primero en cortes revisables independientes, conservando bytes y cambios no commiteados.
- Usar repositorios temporales para matrices. Nunca mutar fixtures consumidores ni usar red o proveedores vivos.
- Cambiar fuente/pruebas canonicas solo despues de RED. Sincronizar `.opencode/` y `.claude/` con el renderer existente, nunca a mano.
- Serializar cada cambio canonico compartido y mantener cada RED en el mismo corte que su GREEN propietario.
- Mantener cerrados detectores RH-W-0025 y estado local RH-W-0051. Sin adapter universal, plugins, maquina de estados, store, ledger, registro de migracion ni segunda autoridad.
- Mantener `RH-W-0079` `ready` hasta que evidencia de implementacion autorizada por separado respalde otra transicion. Aprobar plan e implementacion son decisiones separadas.
- Los commits indicados son solo limites de planificacion. Descubrir la ruta real al ejecutar. GitHub, issues, PRs y `CONTRIBUTING.md` nunca son requisitos genericos.

## Trazabilidad del diseno

| Clausulas | Propietario y prueba |
|---|---|
| Proposito y propiedad, lineas 5-43 | Cortes 1 y 4: base, resultados independientes y release. |
| Deteccion, uso, mutacion y estados, lineas 45-64 | Cortes 1-3: deteccion, uso, gates, cinco resultados y deriva. |
| Git y recomendacion, lineas 66-79 | Corte 2: utilidad sin Git, una recomendacion, afirmaciones limitadas y aprobacion antes de `git init`. |
| Politica y proveedor, lineas 81-87 | Cortes 2 y 4 rechazan GitHub, issue-first, PR, proveedor y contribucion inferidos. |
| Seleccion y apply, lineas 89-101 | Cortes 1 y 3 reemplazan el par, conservan standalone, migran y ligan configuracion. |
| RH-W-0048/RH-W-0052, lineas 103-108 | Cortes 3 y 4 preservan propiedad separada. |
| Aprobacion, lineas 110-116 | Cortes 2, 3 y 5 conservan gates independientes. |
| RED, lineas 118-138 | Cortes 1-4 colocan los 15 fallos antes de su implementacion o fixture propietario dentro del mismo corte. |
| Multiplataforma/agentes, lineas 140-146 | Corte 4 registra solo evidencia disponible. |
| Release/rollback, lineas 148-159 | Cortes 4-5 cubren clasificacion, proyecciones, repeticion, rollback, docs y continuidad. |
| Sobreingenieria, lineas 161-169 | Cada corte incluye aserciones negativas y revision de alcance. |

## Base medida antes de RED

Registrar comandos, codigos, JSON, SHA-256, conteo enfocado, checker y rutas sucias antes de cambiar fuente. No usar totales obsoletos como gate.

1. En carpeta temporal, `render` sin `--standalone` ni integraciones: registrar salida 2, `FAILED_CLOSED` y error del par en `main()` 3707-3725.
2. Repetir con Gentle AI, Engram y ambos. Registrar que hoy solo ambos `verified` pasan. Conservar `not_found`, `found_on_path`, `verified`.
3. Preview/apply con `--standalone`: registrar salida de repositorio y apply por digest.
4. Session-start temporal sin Git: registrar utilidad, garantias no disponibles y recomendacion actual.
5. Registrar `expected_authority_builder_config()` y schema limitado a `integrated|standalone`.
6. Registrar paridad del script/pruebas canonicos y paquetes, checker y escrituras.

Superficies: script/pruebas canonicos, templates existentes, configuracion de autoridad, generation binding y copias de paquete solo para medir.

## Corte 1: Seleccion independiente de capacidades

**RED primero:** casos canonicos enfocados sin integraciones, cada una y ambas; preview util siempre y adiciones solo propias. Probar `available`, `unavailable`, `partial`, `disabled` y `policy-required` contextual; `partial` y `disabled` son estados efectivos de capacidad, nunca clasificaciones nuevas de los detectores RH-W-0025. Cubrir standalone. Fijar vocabulario RH-W-0025 y campos RH-W-0051; sin campo, namespace, migracion, store ni adapters. Ejecutar las pruebas nombradas antes de cambiar fuente, capturar sus fallos previstos contra el preflight emparejado y conservarlas en este corte hasta GREEN.

**Implementacion:** reemplazar minimamente `select_integration_mode()` y llamada de `main()` por proyeccion efectiva. Conservar detectores, `CAPABILITY_CLASSIFICATIONS` y evidencia. Mantener standalone y default util sin capacidades. Usar builders existentes, sin framework.

**Superficie:** script, templates y pruebas. **Verificacion:** combinaciones, detectores, standalone y rutas. **Rollback:** revertir corte coherente. **Unidad:** `feat(harness): compose optional capabilities independently`.

## Corte 2: Limites opcionales de Git y proveedor

**RED primero:** temporales para ejecutable ausente, no inicializado, sin commits, separado, sucio, limpio, sin upstream y divergente. Exigir salida util, afirmaciones permitidas, una recomendacion, sin persistencia ni init. Agregar Git sin proveedor, proveedor no GitHub, sin remoto y sin `CONTRIBUTING.md`. Probar aprobacion explicita antes de init y nunca como efecto lateral.

**Implementacion:** reusar subprocess y temporales multiplataforma; deteccion minima; proveedor/politica separados; sin contador.

**Superficie:** helpers Git/session-start, templates y pruebas. **Verificacion:** matriz enfocada, fakes, cero escrituras y scan. **Coordinacion:** serializar con RH-W-0060 todo cambio solapado de contratos diarios generados; RH-W-0060 conserva la propiedad del listado de ramas y la comparacion contra el blob commiteado. **Rollback:** revertir Git conservando Corte 1. **Unidad:** `feat(harness): bound optional git guidance`.

## Corte 3: Binding de preview, apply y migracion

**RED primero:** ausencia estable, combinaciones y deriva material; fixtures standalone/integrados con reemplazo ordinario. Las pruebas de limite deben fallar contra el comportamiento actual porque ejercitan la nueva conducta de migracion, no por agregar aserciones negativas que ya pasan. Los digests protegen apply/evidencia/objetivo/configuracion; sin recovery, archive, uninstall, reinstall, borrado ni restauracion.

**Implementacion:** intentar binding mediante `expected_authority_builder_config()`, igualdad, plan y `generationSha256`. Cambiar schema solo si RED prueba insuficiencia; si exige campo/ruta, detenerse por decision nueva. Reemplazar solo salida poseida por render/apply.

**Coordinacion:** RH-W-0048 conserva recovery y tracked/local; RH-W-0052 conserva archivo, uninstall, reinstall, borrado y rollback.

**Superficie:** configuracion/schema solo si probado, generation, apply, render y pruebas. **Verificacion:** autoridad, migracion, cero escrituras y propietarios cerrados. **Rollback:** restaurar seleccion/apply/pruebas/proyecciones como unidad, nunca estado consumidor ni Git. **Unidad:** `feat(harness): bind effective capabilities to apply`.

## Corte 4: Clasificacion de tres vias, sincronizacion por renderer y compatibilidad

**RED primero:** inventario del diff, cada conducta exactamente una vez como `builder-only scaffolding`, `repository-specific policy` o `generic consumer contract`. Fallar omision/duplicado. Rechazar en salida generica GitHub, issues, PRs, checks, `CONTRIBUTING.md`, autoalojamiento bilingue, Gentle AI, Engram, CodeGraph, issue-first, branch, commit, push y publicacion obligatorios. Probar politica local sin fuga.

**Implementacion:** fixture enfocado de prueba, no registro runtime. Tras el GREEN de clasificacion, sincronizar en este mismo corte el producto canonico resultante mediante el renderer:

1. Preview maintenance y rechazo de cambios ajenos/humanos.
2. Apply solo del digest aprobado, nunca edicion manual.
3. SHA-256 y checker `current`, `writes: 0`.
4. Pruebas enfocadas; suite completa solo si se prueba acoplamiento.
5. Semantica OpenCode, Claude, Pi y agente generico en superficies compatibles; evidencia ausente se reporta.
6. Host actual y CI por OS solo donde exista.

**Superficie:** pruebas canonicas, maximo un fixture y copias de paquete o contratos diarios solo si el renderer los planea. **Verificacion:** completitud, prohibiciones, revision manual, paridad, checker, compatibilidad enfocada y evidencia de plataforma disponible. Sin inventar fuente Pi. **Rollback:** retirar inventario y renderizar fuente revertida sin tocar desconocidos. **Unidad:** `chore(harness): classify and synchronize capability release`.

## Corte 5: Documentacion, continuidad y aceptacion de release

1. Reconciliar roadmap, estado, decisiones solo si nuevas, changelogs solo por conducta completada, mapas y pares.
2. **RED primero:** agregar una asercion documental enfocada que falle mientras la decision permanente de `decisions.md` diga que `integrated` usa Gentle AI y Engram juntos y `standalone` aplica cuando ninguno esta disponible. **GREEN documental:** reemplazar esa redaccion contradictoria por el modelo opcional independiente aprobado, conservando semantica de detectores RH-W-0025 y degradacion por operacion.
3. Actualizar Engram `work/RH-W-0079` tras evidencia, con repositorio autoritativo.
4. Pruebas de ciclo/docs, links, checker, paridad, `git diff --check` y scan U+2013/U+2014.
5. Repetir no Git, Git local sin proveedor y autoalojado read-only/preview. Sin init, inferencia, estado duplicado, garantia ni fuga.
6. Repetir clasificacion y confirmar RED/GREEN, nombrando limites.
7. Registrar rollback y descubrir entrega al ejecutar, sin incrustar proveedor.
8. Detenerse por decisiones separadas de release/entrega.

**Rollback:** docs/continuidad separables; producto restaura seleccion, apply, pruebas y proyecciones consistentemente. **Unidad:** `docs(harness): reconcile optional capability delivery`.

## Matriz de verificacion enfocada

| Evidencia | Metodo | Exito |
|---|---|---|
| RED/GREEN | Metodos `unittest` nombrados | Falla prevista antes; pasa tras cambio minimo. |
| Apply | Clases existentes mas deriva | Ausencia estable pasa; deriva rechaza antes de escribir. |
| Git | Temporales y mocks | Afirmaciones correctas; sin red, init ni persistencia. |
| Migracion | Instalaciones temporales | Render/apply reemplaza propiedad sin estado duplicado. |
| Proyecciones | SHA-256 y checker | Paridad; `current`, 0 escrituras. |
| Documentacion | Pruebas y links | Propietarios y pares concuerdan. |
| Texto | diff check y scan | Sin whitespace ni caracteres prohibidos. |
| Clasificacion | Inventario final | Cada conducta una vez, sin fuga. |

## Analisis de colisiones

| Propietario | Clasificacion | Regla |
|---|---|---|
| RH-W-0048 | Dependencia de secuencia mas serializacion | Posee recovery, tracked/local y restauracion. Puede compartir script/pruebas/checker/proyecciones. Serializar; RH-W-0079 no implementa recovery. RH-W-0052 espera su prueba. |
| RH-W-0052 | Dependencia de secuencia | Posee archivo, uninstall, reinstall, borrado, rollback y consecuencias no Git. No bloquea seleccion; luego consume contrato final. |
| RH-W-0046/RH-W-0018 | Coordinacion semantica | Poseen consentimiento de red y recordado. Conservar gates, sin store ni supresion. |
| RH-W-0053 | Ninguna | Checkpoints y resumption separados. |
| RH-W-0060 | Coordinacion semantica mas serializacion | Conserva la propiedad del listado de ramas y la comparacion contra el blob commiteado. El Corte 2 debe serializar cambios solapados de contratos diarios generados sin absorber ninguna conducta. |
| RH-W-0069 | Coordinacion semantica | Posee backup/Engram. Aqui Engram es opcional, sin schema. Serializar contratos solapados. |
| RH-W-0068 | Coordinacion mas serializacion | Posee cierre y defectos restantes. Conservar gates y serializar session-stop. |
| RH-W-0071 | Serializacion | Puede editar contratos, script/pruebas y proyecciones. Separar y regenerar desde canon combinado. |
| RH-W-0072 | Serializacion | Posee drift documental y prueba. No absorber licencia, Pi, localState ni inventario. |
| RH-W-0074 | Coordinacion mas serializacion | Posee warning/compliance y outcome. Recomendacion Git no anula consentimiento. |
| RH-W-0075 | Coordinacion semantica | Posee necesidad/justificacion externa. No redefinir external ni solicitudes. |
| RH-W-0076 | Serializacion | Puede cambiar docs builder y controles. Evitar regeneracion concurrente. |
| RH-W-0078 | Secuencia condicional mas serializacion | Puede cambiar topologia/manifiesto/checker/proyecciones. Si aterriza primero, usar su renderer; aqui no tocar topologia. |
| RH-W-0025 completada | Coordinacion semantica | Reusar vocabulario/evidencia sin reabrir. |
| RH-W-0051 completada | Ninguna | Sin namespace, campo, store ni checker nuevo. |

No hay blocker real probado para Cortes 1-4. Archivos compartidos exigen serializacion. RH-W-0048 sigue antes de RH-W-0052. Esta relacion amplia coherencia 11 sin fusionar ciclos.

## Pronostico de carga de revision

| Corte | Lineas estimadas | Superficies | Riesgo |
|---|---:|---:|---|
| 1 | 220-360 | 2 canonicos, proyecciones despues | Alto |
| 2 | 180-320 | 2 canonicos y texto generado | Alto |
| 3 | 160-280 | 2 canonicos y fixtures | Alto |
| 4 | 80-230 autoradas, 300-700 generadas | 1-2 pruebas y 4-10 proyecciones | Medio |
| 5 | 100-220 | 6-10 bilingues mas Engram | Medio |

Total: 740-1,410 lineas autoradas mas 300-700 de renderer en unos 10-18 archivos. Supera 400, por lo que NO se recomienda revision combinada. Usar cinco cortes, cada RED enfocado con su GREEN propietario dentro del mismo corte revisable, y mantener documentacion de ciclo separada del producto. Conteos estimados hasta baseline/previews.

## Compuerta de autorrevision independiente

- Cada requisito tiene tarea/prueba y RED primero.
- Sin adapters, maquina, store, ledger, registro, daemon ni proveedor universal.
- Git opcional; GitHub/issues/PR/proveedor/`CONTRIBUTING.md` no genericos; sin garantia Git sin Git; init con aprobacion.
- RH-W-0025/RH-W-0051 cerradas; RH-W-0048/RH-W-0052 separadas.
- Conteos estimados, sin total obsoleto, colisiones completas y limites de evidencia explicitos.
- Rollback no borra estado, cambia Git, fabrica restauracion ni publica el par obligatorio.
- Ambos pares tienen encabezados, cinco cortes, trazabilidad, colisiones, rangos y gate equivalentes.

## Decisiones humanas restantes

1. Autorizar por separado un alcance acotado de implementacion antes de iniciar cualquier corte.
2. Si RED del Corte 3 exige campo/ruta nueva, aprobar diseno revisado.
3. Al ejecutar/entregar, autorizar por separado Git, ruta, release, proveedor o publicacion tras verificar el repositorio.

## Compuerta del propietario

`next_recommended`: solicitar autorizacion separada del propietario para un alcance acotado de implementacion. La aprobacion del plan y la entrega documental completada mediante el issue #129 y el PR #130 no autorizan implementacion, release, push, otro issue ni otra entrega por pull request.
