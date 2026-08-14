# Hoja de ruta del harness adaptativo

La fuente factual canonica de esta hoja de ruta es [`../../repo-harness/roadmap.md`](../../repo-harness/roadmap.md). Esta presentacion completa ordena las unidades revisables mas pequenas para hacer adaptable `repo-harness-builder` sin comprometer prematuramente una arquitectura de integracion. Cada unidad usa su propio issue, rama, PR, pruebas, revision y actualizacion documental.

## Resultado y principios

- Reconciliar la evidencia del repositorio antes de cambiar planes. Ningun pendiente se agrega, conserva o elimina desde una sola fuente.
- Mantener un propietario factual claro, usar revelacion progresiva y preferir unidades cortas revisables de forma independiente.
- Actualizar el estado actual y cada espejo localizado afectado en la misma unidad que cambia la realidad.
- La guia dirigida a IA lee solo el ingles canonico para evitar contexto duplicado. Los documentos localizados son presentaciones humanas completas mantenidas por la skill de documentacion para los idiomas declarados por la persona usuaria en el repositorio.
- Responder en el idioma indicado por la persona usuaria o, en su defecto, en el idioma que esta usando.
- Adaptarse a la evidencia del repositorio, preguntar solo ante decisiones reales y advertir contradicciones en vez de imponer estructura rigidamente.
- Limitar la mejora propia a una friccion verificada y un cambio minimo revisable por vez. Nunca iniciar mejora recursiva ni cambiar silenciosamente politicas de comportamiento; las reglas de aprobacion existentes del repositorio mantienen la autoridad.
- Evitar sobreingenieria, desviacion de alcance y acciones conocidas como no permitidas.
- Usar Engram cuando este disponible para continuidad durable y luego revalidarlo contra la evidencia actual del repositorio.
- Colocar reglas criticas concisas en el `REPO-HARNESS.md` generado, detalle operativo en skills, continuidad en Engram y explicaciones humanas en la documentacion.

## Autoridad y ubicacion

| Informacion | Propietario |
|---|---|
| Comportamiento actual | Codigo fuente y pruebas, reconciliados con documentos propietarios e historial Git |
| Estado actual y siguiente paso | [current-state.md](current-state.md) |
| Trabajo futuro ordenado | Esta hoja de ruta |
| Justificacion y restricciones permanentes | [decisions.md](decisions.md) |
| Cronologia completada | [CHANGELOG.md](../../../CHANGELOG.md) |
| Reglas operativas generadas criticas | `REPO-HARNESS.md` generado |
| Operaciones detalladas | Skills generadas y canonicas |
| Continuidad entre sesiones | Engram, reconciliado antes de promover hechos |
| Presentacion humana en espanol | Espejos completos bajo [`docs/es/`](README.md), enlazados a propietarios factuales en ingles |

## Base verificada

- `python .repo-harness/check.py` informa `current`, salida 0 y 0 escrituras.
- El PR #2 se integro en `main` como `ec56b1b` y enlazo el issue #1.
- La ampliacion del conjunto de runtimes esta completa: `runtime_set_report()` informa runtimes `unrecorded` y `staleManifest`, con cobertura de `RecordedRuntimeSetTests`.
- La ronda 2 de Judgment Day permanece incompleta segun la evidencia disponible.
- Cinco hallazgos de la ronda 1 siguen abiertos: los manifiestos heredados pueden omitir comprobaciones en espanol; `check` acepta pero ignora flags de idioma; las declaraciones de idioma instaladas invalidas no se normalizan al leer; dos pruebas documentales suponen archivos en espanol; y falta cobertura de compatibilidad entre roles prometidos y cabeceras reales.
- Siguen abiertos la ejecucion de reemplazo e integracion aprobados, la integracion de selectores en README existentes, la migracion de la revision historica, la deteccion de un builder instalado obsoleto y el diseno del harness de contribucion para consumidores.
- Las pruebas y la validacion actuales prohiben gestionar `.gitignore`, por lo que ignorar `.codegraph/` por defecto exige un cambio explicito de politica y su propia unidad TDD.

## Unidades de trabajo ordenadas

| # | Problema y evidencia | Resultado acotado | Verificacion | No objetivos explicitos |
|---|---|---|---|---|
| 1 | Los hechos pendientes pueden desviarse cuando se confia en una sola fuente. | Definir la regla de reconciliacion entre documentos propietarios, codigo, pruebas, historial, Git y Engram segun corresponda. | Pruebas de contrato y actualizaciones de estado en ingles y espanol en la misma unidad. | No cambiar el backlog sin evidencia reconciliada. |
| 2 | `.codegraph/` esta sin seguimiento, mientras las pruebas y validacion prohiben gestionar `.gitignore`. | Desarrollar con TDD una politica explicita para ignorarlo por defecto con opt-out persistente y comprobable. | Pruebas rojo-verde para valor por defecto, opt-out, idempotencia y preservacion. | Ninguna edicion silenciosa de `.gitignore` ni politica general de ignorados. |
| 3 | Los manifiestos heredados sin idioma pueden omitir comprobaciones en espanol. | Conservar y comprobar arboles de idiomas instalados conocidos cuando falta el campo de idioma. | Prueba de regresion con fixture bilingue heredado. | Ningun framework de migracion de manifiestos. |
| 4 | `check` anuncia flags de idioma pero los ignora. | Eliminarlos o respetarlos mediante un unico contrato de solo lectura sin ambiguedad. | Pruebas de comportamiento y ayuda CLI. | Ningun rediseno amplio del CLI. |
| 5 | Las declaraciones de idioma instaladas invalidas fallan sin normalizacion de lectura. | Normalizarlas o rechazarlas de manera uniforme con diagnosticos del campo correcto. | Matriz de regresion de manifiestos invalidos. | Ningun idioma nuevo. |
| 6 | Dos pruebas documentales incluidas suponen que existen archivos en espanol. | Hacer que respeten los idiomas de documentacion declarados. | Ejecuciones solo en ingles y bilingues. | No debilitar comprobaciones bilingues cuando se declara espanol. |
| 7 | `missing_sections` no cubre compatibilidad entre roles prometidos y cabeceras reales. | Especificar y probar esa direccion y aplicar la correccion compatible mas pequena. | Pruebas unitarias enfocadas en ambas direcciones de vocabulario. | Ninguna reescritura del parser documental. |
| 8 | Las opciones aprobadas de reemplazo e integracion son descriptivas, no ejecutables. | Ejecutar una accion aprobada ligada al contenido, preservando bytes desconocidos y cierre seguro. | TDD para digest de aprobacion, bytes cambiados, limite de rollback y vista previa sin escrituras. | Ningun motor masivo de actualizacion. |
| 9 | Los selectores de idioma en README existentes siguen cerrandose de forma segura sin ejecutor. | Agregar el caso reciproco acotado mediante el ejecutor de integracion aprobado. | Pruebas LF, CRLF, reciprocidad, duplicados y cambio de plan. | Ninguna reescritura general de README. |
| 10 | Los consumidores no distinguen un builder instalado obsoleto de deriva del harness generado. | Definir y detectar por separado la vigencia del paquete del builder. | Fixtures vigente, obsoleto, ausente y no verificable. | Ninguna promesa de recarga del runtime. |
| 11 | El informe de revision de `skills-pack-ld9` no se traslado de forma durable. | Reconciliar y migrar hallazgos aun relevantes con evidencia publica del repositorio. | Trazar cada hallazgo conservado o cerrado a evidencia actual. | Ninguna importacion ciega de notas privadas u obsoletas. |
| 12 | La investigacion externa necesita un limite seguro y reproducible. | Documentar investigacion primero con fuentes primarias; permitir clones temporales solo en un area ignorada tras revisar licencia y revision, sin ejecutar codigo. Los documentos publicos usan redaccion independiente y citas publicas, y la persona usuaria decide la retencion. | Comprobacion de enlaces, revision de citas y pruebas del limite de clones cuando pueda imponerse. | Ninguna dependencia publicada de rutas privadas, ignoradas o locales. |
| 13 | Los contratos siempre cargados pueden crecer sin limites medidos. | Agregar presupuestos medidos, advertencias, indices, referencias y revelacion progresiva para `REPO-HARNESS.md`, skills y contratos operativos, con excepciones pequenas justificadas cuando dividir cueste mas. La documentacion humana general usa indice y lectura selectiva, no un limite universal. | Pruebas de limites y mediciones documentadas. | Ningun limite arbitrario para toda documentacion humana. |
| 14 | El soporte de contribucion para consumidores carece de un diseno consciente de capacidades. | Disenar el descubrimiento de politica del repositorio y capacidades reales del proveedor antes de proponer `CONTRIBUTING`, issues, ramas, PR, templates, labels o checks. | Escenarios de diseno revisables y criterios neutrales al proveedor. | Ninguna mutacion de proveedor ni implementacion. |
| 15 | MCP, CLI, binarios e integraciones de terceros podrian ser utiles despues. | Reconsiderarlos solo cuando las unidades anteriores aporten evidencia y limites estables. | Registro de decision que compare necesidades medidas y compromisos. | Ningun compromiso arquitectonico actual. |

## Registro de decisiones

- Se evito suponer que Engram, un documento, una prueba o un informe historico pueden establecer por si solos la verdad actual.
- Se evito tratar el ignore de `.codegraph/` como correccion solo documental porque contradice la politica ejecutable actual.
- Se evito elegir arquitectura MCP, CLI, binaria o de terceros antes de contar con evidencia fundamentada de consumidores.
- Sin resolver: representacion exacta del opt-out de `.codegraph/`, regla de inferencia de idiomas heredados, destino de los flags de `check`, esquema de acciones de integracion aprobadas, umbrales de presupuesto de contexto y forma futura de integracion.
