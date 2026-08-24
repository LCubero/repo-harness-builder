# Plan de implementacion RH-W-0034 para recuperacion de consumidores y correccion del modo solo en espanol

> **Aviso de ciclo de vida completado:** RH-W-0034 se completo el 2026-08-17. El [registro canonico de la hoja de ruta](roadmap.md#rh-w-0034) posee la evidencia de cierre y entrega. Las instrucciones historicas y las casillas sin marcar que siguen se conservan como evidencia de ejecucion, no como trabajo pendiente.

> **Para agentes de implementacion:** HABILIDADES REQUERIDAS: use `systematic-debugging`, `test-driven-development` y `verification-before-completion`. Marque cada casilla. Implemente solo despues de mover el registro del ciclo de vida a `active`.

**Objetivo:** Corregir los defectos del constructor expuestos por la transcripcion verificada del consumidor de `v3.1-rc.2`, de modo que las personas usuarias reciban preguntas concretas en espanol, el idioma del informe se resuelva solo desde evidencia humana valida, las instalaciones solo en espanol pasen ambos verificadores con enlaces locales validos y los fallos propiedad del constructor nunca entren al reporte de defectos del proveedor.

**Arquitectura:** Corregir cada defecto en su fuente canonica del constructor, sincronizar las proyecciones del paquete y demostrar el comportamiento en fixtures temporales de consumidor. Mantener independientes los idiomas del informe, conversacion, documentacion y artefactos. Clasificar la propiedad antes de cualquier transferencia a un proveedor. Tratar los archivos generados del consumidor como salidas, nunca como superficies para corregir el producto.

**Tecnologia:** Biblioteca estandar de Python 3.10+, `unittest`, contratos Markdown generados, manifest JSON, fixtures por subproceso y proyecciones incorporadas exactas de OpenCode y Claude.

**Propietario del ciclo de vida:** [RH-W-0034](roadmap.md#rh-w-0034), estado `completed`, prioridad `P1`. La hoja de ruta posee el estado y la evidencia de entrega; este archivo conserva la secuencia ejecutable historica.

## Causa raiz

El fallo del consumidor es un conjunto de defectos propiedad del constructor con cuatro limites:

1. El contrato de entrevista muestra vocabulario interno de arquitectura en vez de explicar la eleccion concreta. Esto afecta el mensaje cuando no existe un harness y la decision para preservar documentacion existente.
2. El contrato excluye la prosa del wrapper y la salida de herramientas como evidencia de idioma, pero la orquestacion acepto texto ingles generado por el wrapper como evidencia de la conversacion. Esto produjo `--report-language english` y `APPROVE` o `DENY` en ingles durante una conversacion humana en espanol.
3. `normalize_documentation_languages()` acepta salida solo en espanol y `docs_outputs()` la genera, pero `check_script()` rechaza por separado cualquier manifest cuyo primer locale documental no sea `en`. El mapa solo en espanol tambien publica enlaces bilingues y una ruta propietaria en ingles que no existe en una salida monolingue en espanol.
4. El manejo de fallos clasifico una incompatibilidad entre manifest y checker generados por el constructor como defecto de Gentle AI. Cruzo la propiedad antes de identificar el componente, introdujo consentimiento irrelevante para GitHub y termino sin salida porque no existia una ruta para rechazar el reporte al proveedor.

La correccion del producto pertenece a `repo-harness-builder`. Modificar directamente `.repo-harness/check.py` en un consumidor ocultaria la causa y dejaria vulnerables las instalaciones futuras.

## Requisitos

- **REQ-001:** Sustituir la explicacion sin harness por lenguaje concreto que comience `Este repositorio todavia no tiene un arnes configurado`, enumere los entornos disponibles y pregunte cuales incluir.
- **REQ-002:** Explicar concretamente la eleccion sobre documentacion existente: el harness necesita archivos nuevos de proyecto y continuidad; `Integrar y actualizar` conserva la documentacion actual y construye alrededor de ella; `Archivar y comenzar limpio` aparta la documentacion actual y genera una estructura nueva.
- **REQ-003:** Un argumento explicito del idioma del informe usa su valor validado exacto. Sin argumento, usar preferencia verificada del host y luego evidencia verificada de la conversacion humana. Si ninguna existe, preguntar una vez. La prosa del wrapper, contratos generados, texto del repositorio, prosa del asistente, memoria y salida de herramientas nunca son evidencia.
- **REQ-004:** Mantener independientes informe, conversacion, documentacion, guia y artefactos. La seleccion de idioma no cambia comandos, rutas, tokens, enums, digests, alcance de aprobacion, claves JSON ni otros bytes de maquina.
- **REQ-005:** Hacer que `parse_manifest()` generado acepte exactamente las declaraciones documentales aceptadas por `normalize_documentation_languages()`: `("en",)`, `("es",)` y `("en", "es")`, y rechace vacias, duplicadas, no soportadas o reordenadas.
- **REQ-006:** Una instalacion temporal solo en espanol debe pasar el check propietario y `.repo-harness/check.py`; ambos informan `current`, salida 0 y `writes: 0`.
- **REQ-007:** En modo solo en espanol, `docs/repo-harness/README.md` es el mapa canonico local. Todo enlace relativo emitido debe resolver dentro del fixture. No se emiten `../../../repo-harness/current-state.md`, `i18n-maintenance.md`, mapa ingles ni otro propietario exclusivo del modo bilingue.
- **REQ-008:** Clasificar al propietario antes de transferir al proveedor. Una incompatibilidad entre salida generada, manifest, checker o proyeccion del paquete pertenece a `repo-harness-builder`.
- **REQ-009:** La recuperacion de un fallo del constructor preserva al consumidor, entrega reproduccion y una ruta de actualizar el constructor y regenerar, y nunca entra en flujos de Gentle AI, issues de GitHub, publicacion, autenticacion o consentimiento del proveedor.
- **REQ-010:** Un consumidor sin Git y con RDD global desactivado no necesita un intento de desactivacion para el clone. El texto distingue reporte de defectos al proveedor y publicacion de un repositorio sin implicar ninguno.
- **REQ-011:** Corregir primero las fuentes canonicas, sincronizar proyecciones, probar el paquete sucesor y solo entonces permitir un plan nuevo de regeneracion. Nunca corregir manualmente el checker consumidor.
- **REQ-012:** el consumidor externo permanece como evidencia historica de solo lectura y nunca es raiz de prueba, destino de mutacion, destino de regeneracion ni espacio de release.
- **REQ-013:** No crear una entrada de changelog hasta completar la implementacion.

## Fuera de alcance

- Sin implementacion de codigo en esta unidad de planificacion.
- Sin editar, limpiar, inicializar Git, cambiar RDD ni reparar archivos generados en el consumidor externo.
- Sin cambiar Gentle AI salvo que evidencia posterior demuestre un defecto separado. La evidencia actual no lo demuestra.
- Sin autenticacion, issue de GitHub, publicacion, release, etiqueta, stage, commit, push ni servicio externo.
- Sin detector de lenguaje natural, traduccion, locale nuevo, version nueva del esquema, framework generico de incidentes ni dependencia nueva.
- Sin debilitar seguridad de rutas, autenticacion del digest, comparacion exacta ni comportamiento de cero escrituras.

## Decisiones de diseno

### Resolucion del idioma del informe

Precedencia exacta:

1. Argumento explicito validado.
2. Preferencia verificada del host para informes.
3. Evidencia verificada de la conversacion humana actual.
4. Preguntar una vez en el idioma disponible y detenerse.

No existe fallback desde texto del wrapper. Si el host no distingue conversacion humana de contexto, no puede afirmar evidencia verificada y debe preguntar una vez. La respuesta se conserva para la invocacion activa y se pasa explicitamente a preview y apply. Una respuesta separada para informe nunca se reemplaza con el idioma de conversacion.

### Propiedad del fallo

| Evidencia | Propietario | Ruta requerida |
|---|---|---|
| El checker generado rechaza un manifest emitido por el mismo constructor | `repo-harness-builder` | Preservar consumidor, reproducir, actualizar constructor y regenerar mediante plan nuevo aprobado |
| Proyecciones canonica y de paquete difieren | `repo-harness-builder` | Detener release y sincronizar proyecciones |
| El comando de Gentle AI falla despues de recibir salida valida en su limite documentado | Posible defecto de Gentle AI | Ofrecer reporte al proveedor separado de publicacion |
| El consumidor no es repositorio Git | Capacidad Git ausente | Informar localmente; no inicializar, clonar, publicar ni cambiar RDD |

La transferencia al proveedor es inalcanzable hasta identificarlo como propietario. Rechazar un reporte real al proveedor conserva una remediacion local completa, pero un fallo del constructor nunca formula esa pregunta.

### Sustitucion del release

`v3.1-rc.2` permanece como evidencia historica inmutable. Solo queda sustituido para recomendaciones de instalacion cuando un candidato sucesor contenga la correccion y toda la evidencia de aceptacion. No alterar ni borrar la etiqueta o release anterior. Este plan no publica un sucesor. Hasta completar la prueba, describir `v3.1-rc.2` como candidato publicado actual con defectos RH-W-0034 conocidos.

## Superficies exactas

| Ruta o simbolo | Responsabilidad planificada |
|---|---|
| `skill/repo-harness-builder/SKILL.md` | Entrevista, evidencia independiente, recuperacion por propietario y distincion de publicacion |
| `skill/repo-harness-builder/references/audit-and-interview.md` | Preguntas concretas, precedencia, una sola pregunta y exclusion de wrappers |
| `skill/repo-harness-builder/references/preservation-and-safety.md` | Integrar frente a archivar y limite de remediacion |
| `skill/repo-harness-builder/references/architecture.md` | Dimensiones independientes y clasificacion antes del proveedor |
| `skill/repo-harness-builder/references/validation.md` | Matrices, replay temporal y prueba de release |
| `skill/repo-harness-builder/scripts/harness_tool.py::normalize_documentation_languages` | Dominio unico de idiomas documentales |
| `skill/repo-harness-builder/scripts/harness_tool.py::docs_outputs` | Filas y enlaces locales del mapa solo en espanol |
| `skill/repo-harness-builder/scripts/harness_tool.py::check_script` | Paridad de `parse_manifest()` con las declaraciones aceptadas |
| `skill/repo-harness-builder/tests/test_harness_tool.py::ConversationLanguageBoundaryTests` | Evidencia humana y bytes de maquina |
| `skill/repo-harness-builder/tests/test_harness_tool.py::ProductWorkflowTests` | Mapa, entrevista y preservacion solo en espanol |
| `skill/repo-harness-builder/tests/test_harness_tool.py::ReportTemplateTests` | Evidencia explicita y exclusion del wrapper |
| Fixtures por subproceso en `test_harness_tool.py` | Instalacion, ambos checkers y clasificacion de propiedad |
| `.opencode/skills/repo-harness-builder/**` | Proyeccion exacta del paquete |
| `.claude/skills/repo-harness-builder/**` | Proyeccion exacta del paquete |
| `docs/repo-harness/**`, `docs/es/repo-harness/**` | Ciclo de vida, estado, mapas y evidencia final |

Si una clase cambia de nombre antes de implementar, extender la clase propietaria mas pequena. Validar cada simbolo antes de editar.

## Matriz RED/GREEN

| ID | Fixture RED | RED esperado | Invariante GREEN |
|---|---|---|---|
| T-001 | Entrevista en espanol sin harness | Frase tecnica o sin eleccion de entornos | Mensaje concreto, entornos listados y una pregunta |
| T-002 | Documentacion existente | Jerga o elecciones ambiguas | Efectos exactos de integrar y archivar |
| T-003 | Sin argumento, humano en espanol, wrapper ingles | Selecciona ingles | Selecciona evidencia humana verificada en espanol |
| T-004 | Sin argumento ni evidencia verificada | Fallback silencioso | Pregunta una vez y conserva valor explicito |
| T-005 | Informe ingles explicito con conversacion y docs en espanol | Otra dimension lo reemplaza | Solo informe ingles; las otras no cambian |
| T-006 | Valores de maquina en ambos idiomas | Cambia algun byte | Igualdad exacta de tokens, rutas, digests, claves y comandos |
| T-007 | Manifest solo en espanol de render normal | El checker generado lo rechaza | Ambos checkers `current`, salida 0, `writes: 0` |
| T-008 | Locales vacios, duplicados, no soportados o reordenados | Pasa una declaracion invalida | Ambos validadores rechazan igual |
| T-009 | Mapa solo en espanol | Publica destinos ausentes | Todos los enlaces resuelven y no hay filas bilingues |
| T-010 | Incompatibilidad del checker generado | Ofrece Gentle AI o GitHub | Propietario constructor y solo recuperacion local |
| T-011 | Fixture sin Git y RDD global apagado | Intenta RDD de clone o implica publicacion | Sin mutacion RDD, Git, clone ni publicacion |
| T-012 | Reporte real al proveedor rechazado | Flujo sin salida | Continua transferencia local reproducible y publicacion separada |

RED solo es valido si falla la asercion del comportamiento ausente. Errores de sintaxis, imports, fixtures, rutas o subprocesos no son evidencia RED valida.

## Fases atomicas

### Fase 1: Fijar texto y evidencia del idioma

- [ ] Agregar T-001 a T-006 antes de cambiar contratos.
- [ ] Usar la explicacion concreta aprobada y eliminar jerga de arquitectura.
- [ ] Codificar la precedencia con evidencia tipada, sin inspeccionar wrappers ni transcripciones arbitrarias.
- [ ] Probar una sola pregunta bloqueante y reanudacion inmediata, conservando RH-W-0033.
- [ ] Ejecutar pruebas enfocadas y registrar conteos RED y GREEN.

### Fase 2: Unificar manifest y mapa solo en espanol

- [ ] Agregar T-007 a T-009 antes de cambiar `check_script()` o `docs_outputs()`.
- [ ] Sustituir la condicion que exige ingles primero por el dominio exacto de `normalize_documentation_languages()`.
- [ ] Hacer local la propiedad del mapa en `docs/repo-harness/README.md` para el modo solo en espanol.
- [ ] Emitir mantenimiento bilingue y propietarios ingleses solo en modo bilingue.
- [ ] Auditar la existencia de cada enlace Markdown del render solo en espanol.
- [ ] Ejecutar pruebas de render directo y fixture instalado por subproceso.

### Fase 3: Exigir recuperacion por propietario

- [ ] Agregar T-010 a T-012 antes de cambiar contratos de recuperacion.
- [ ] Clasificar mediante procedencia y componente antes de transferir al proveedor.
- [ ] Para fallos del constructor, preservar bytes y devolver reproduccion, actualizacion y regeneracion nueva.
- [ ] Separar reporte de defecto al proveedor y publicacion de repositorio.
- [ ] Probar que sin Git y con RDD global apagado no existe accion de clone, RDD o publicacion.

### Fase 4: Sincronizar proyecciones

- [ ] Copiar solo archivos canonicos cambiados a OpenCode y Claude byte a byte.
- [ ] Comparar SHA-256 de cada trio.
- [ ] No editar salidas de consumidor ni usar el consumidor externo como destino.

### Fase 5: Verificacion completa y replay temporal

- [ ] Ejecutar suites completas:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py"
```

- [ ] Instalar en un directorio temporal fuera del consumidor externo con runtimes, licencia, `--report-language espanol`, `--documentation-language es`, `--conversation-language espanol` y modo de integracion explicitos.
- [ ] Ejecutar el check propietario con las mismas dimensiones. Exigir `current`, salida 0 y 0 escrituras.
- [ ] Ejecutar `.repo-harness/check.py` del fixture por subproceso con el mismo resultado.
- [ ] Resolver cada enlace relativo del mapa desde su directorio y comprobar ausencia de rutas bilingues.
- [ ] Reproducir en otro fixture: wrapper ingles, conversacion humana en espanol, sin argumento del informe, sin Git, RDD global apagado e incompatibilidad del checker. Exigir seleccion en espanol o una pregunta, propiedad del constructor, bytes sin cambios, sin proveedor ni GitHub, sin RDD de clone y con ruta nueva de regeneracion.
- [ ] Nunca ejecutar el replay en el consumidor externo.

### Fase 6: Verificacion autoalojada y cierre

- [ ] Ejecutar el check propietario autoalojado con dimensiones instaladas explicitas: runtimes `claude-code` y `opencode`, licencia MIT, idioma del informe, locales `en` y `es`, conversacion `espanol` y evidencia de integracion requerida. Exigir `current`, salida 0 y 0 escrituras.
- [ ] Ejecutar `python3 .repo-harness/check.py` (`python` en Windows) con el mismo resultado.
- [ ] Sincronizar hoja de ruta, estado, pares, mapas si cambia propiedad, decisiones aplicables y Engram `work/RH-W-0034`.
- [ ] Agregar changelog solo al completar.
- [ ] Registrar si la prueba completa permite que un candidato sucesor sustituya a `v3.1-rc.2` para instalaciones. Publicar sigue siendo otra unidad aprobada.
- [ ] Ejecutar parser del ciclo de vida, pruebas de enlaces, `git diff --check` y escaneo de U+2013/U+2014.
- [ ] Confirmar que no cambiaron consumidor, servicios, etiquetas, releases, stage, commits ni push.

## Criterios de aceptacion

- [ ] REQ-001 a REQ-013 tienen evidencia directa.
- [ ] La entrevista usa lenguaje concreto y una pregunta accionable por turno.
- [ ] Wrapper y herramientas no pueden elegir idioma del informe.
- [ ] Sin evidencia valida se pregunta exactamente una vez.
- [ ] Las dimensiones de idioma permanecen independientes.
- [ ] Los bytes de maquina son identicos entre idiomas.
- [ ] Ambos checkers solo en espanol informan `current`, salida 0 y 0 escrituras.
- [ ] Todos los enlaces del mapa solo en espanol resuelven y no anuncia archivos bilingues.
- [ ] Las incompatibilidades generadas permanecen como defectos del constructor.
- [ ] Sin Git y con RDD apagado no existe accion Git, RDD de clone ni publicacion.
- [ ] Paquetes y suites canonica, OpenCode y Claude estan sincronizados.
- [ ] El replay temporal pasa sin leer ni escribir el consumidor externo.
- [ ] El check propietario incluye runtimes, MIT, informe, locales, conversacion e integracion explicitos.
- [ ] Ciclo de vida, mapas, estado, pares, Engram, enlaces, whitespace y caracteres estan reconciliados.
- [ ] `v3.1-rc.2` permanece inmutable y solo se sustituye despues de prueba completa.

## Limite de rollback

El rollback se limita a archivos canonicos de RH-W-0034, proyecciones exactas de OpenCode y Claude, pruebas y documentacion del ciclo de vida. Revierta el patch o use el ejecutor acotado para proyecciones autoalojadas. Nunca use Git destructivo, altere un release o etiqueta, modifique el checker consumidor ni restaure desde el consumidor externo. Un fixture fallido se elimina solo de su ubicacion temporal despues de conservar evidencia acotada.

## Pronostico de revision

| Corte | Carga | Pregunta principal |
|---|---:|---|
| Entrevista e idioma del informe | Media | La evidencia humana tipada controla el informe sin acoplar dimensiones? |
| Checker y mapa solo en espanol | Media | Ambos validadores aceptan el mismo dominio y existen todos los enlaces? |
| Propiedad y remediacion | Media | Puede un fallo del constructor entrar aun al flujo de proveedor o publicacion? |
| Fixtures y proyecciones | Alta | Los subprocesos prueban comportamiento generado real en los tres paquetes? |
| Ciclo de vida y release | Baja | El estado planificado frente a completado es exacto y no hay afirmaciones prematuras? |

Pronostico: una revision enfocada de implementacion y una verificacion independiente. El mayor costo es el fixture por subproceso y el replay, no el delta de produccion.

## Riesgos

- Las pruebas de prosa demuestran distribucion del contrato, no procedencia del host. El resolvedor debe consumir evidencia tipada o preguntar una vez.
- La validacion duplicada puede volver a divergir. Las pruebas deben comparar dominios aceptados y rechazados entre ambos checkers.
- Pruebas sobre docs bilingues incorporados pueden omitir defectos monolingues. El fixture valida el arbol solo en espanol realmente renderizado.
- La copia de propiedad puede invadir Gentle AI. Mantener el manejo generico y no cambiar su fuente sin alcance separado.
- La presion de release puede impulsar un parche al consumidor o publicacion temprana. La compuerta exige correccion de fuente, paridad, replay, ambos checkers y evidencia explicita del sucesor.
