# RH-W-0078 Plan de implementación aprobado por el propietario

**Estado: `completed` por aceptación explícita del propietario (2026-10-06); trabajo restante: `none`. La medición de efectividad de runtime se dispensó como condición de finalización; la efectividad y los ahorros siguen sin medir y no se afirman. El issue #266 está verificado como `CLOSED` (GitHub `closedAt` 2026-10-06T23:10:19Z).** El propietario aprobó el plan el 2026-10-05 solo para registro documental. La compuerta de medición detallada abajo conserva el criterio aprobado anteriormente; la dispensa posterior del propietario la reemplaza como condición de cierre del ciclo. El 2026-10-04 autorizó por separado preparar esta propuesta bilingüe acotada («si dale») tras aprobar el [diseño](rh-w-0078-design.md). **Autoridad verificada:** con la autorización actual, el propietario autorizó explícitamente implementar RH-W-0078 y se confirmó el issue #266. La [hoja de ruta](roadmap.md#rh-w-0078) posee el ciclo de vida; el diseño aprobado posee las compuertas exactas, mediciones y aceptación. La aprobación histórica del plan sigue siendo histórica y no es evidencia de runtime. Fuente canónica: [plan en inglés](../../repo-harness/rh-w-0078-implementation-plan.md).

## Autoridad y compuertas de medición

- El propietario autorizó explícitamente implementar RH-W-0078; se elimina la antigua compuerta de base/viabilidad antes de empezar. Una autorización anterior del propietario también cubrió staging y commit local acotados; no se ejecutó ninguna de esas acciones. Esa autoridad se conserva como intención histórica, no se renueva ni es ejecutable bajo la continuación documental vigente, que excluye Git. No autorizó cambios de herramientas externas, experimentos de runtime, push, PR, merge, release, publicación ni detención automática.
- Preservar el [Plan 27](roadmap.md#plan-27-alcance-de-skills-generadas-rh-w-0078): medir contexto efectivo cargado y alcance de referencias en Claude Code, OpenCode y Pi como compuerta de efectividad/aceptación, no como requisito para empezar a implementar. Los conteos estáticos no prueban ahorros de runtime.
- Toda observación/experimento de runtime sigue sujeto a autorización separada. Si no está disponible, informar `unmeasured`; nunca tratar conteos estáticos como evidencia de runtime ni afirmar ahorros sin resultados comparables medidos.
- La comparación con el candidato futuro ocurre **después de existir su código** y antes de aceptar efectividad/ahorros; no se exige probar un candidato futuro antes de que pueda existir. Se preserva la compuerta de reducción real del diseño para cada ruta común/runtime/configuración afirmada.
- La decisión del 2026-10-06 aprobó inicialmente la rama explícita de inspección/comprobación solo como diseño. El «si dale» posterior autorizó implementar I1: despacho de solo lectura en la skill canónica del builder, pruebas contractuales canónicas enfocadas y documentación bilingüe mínima de autoridad/estado. Usar únicamente el checker instalado existente y configuración declarada autenticada; no inferir runtime/perfiles/idioma/capacidades; detenerse ante evidencia ausente o no confiable. No cambiar CLI/runtime ni flujos de instalación/actualización/corrección, sincronizar proyecciones, reparar/aplicar, usar Git/red, ejecutar experimentos de runtime, hacer commit ni entregar. La propiedad exacta de referencias/proyecciones sigue siendo una etapa separada. Esta continuación documental no autoriza implementación ni sincronización de proyecciones.
- **Desconocido:** bases numéricas de runtime, resultados candidatos y ahorros efectivos. El fallo enfocado anterior, recibo T1–T5 de 623 pruebas y timeout antiguo de 300 segundos son históricos, no describen la corrección más reciente. Véase la [evidencia de verificación más reciente](#evidencia-de-verificacion-mas-reciente) para recibos técnicos actuales, aprobación nativa sin resolver y evidencia histórica.

## Evidencia de verificacion mas reciente

La verificación técnica independiente más reciente observó:

- Última verificación independiente mux: 625 pruebas en 334.094 segundos, `OK (skipped=11)`: 614 no omitidas, cero fallos/errores. No se dispuso de las identidades omitidas ni del conteo de procesos hijos.
- El checker permaneció `current` con 0 escrituras; comprobación global de diferencias limpia. Pasaron seis pares de herramienta/pruebas runtime y cuatro comprobaciones de render propio de inicio/UTF-8-LF. Cinco anclas conocidas y el hash agregado de archivo/índice/estado no cambiaron durante la verificación.
- El recibo de mantenimiento de diez rutas devolvió `VALIDATED`. Un recibo posterior de copia de tres pruebas también devolvió `VALIDATED`, pero no se retuvieron la extracción del digest devuelto ni el hash previo al apply; no se puede afirmar ni reconstruir preservación completa previa.
- La corrección de seguridad agrega guardas antes de leer backups y reutiliza el oráculo de checkpoint existente, acotado a `NativeContinuityRecovery` en `.index`; no se debilitaron controles. Los recibos anteriores de suite de 623 y 624 pruebas siguen siendo históricos.
- Instantánea histórica de verificación previa a la entrega: la revisión nativa y la entrega seguían sin resolver entonces. Los recibos de entrega y aceptación posteriores reemplazan aquella instantánea de ciclo. La efectividad y los ahorros siguen sin medir y no se afirman.

**Recibo histórico de entrega posterior al merge (2026-10-06):** la implementación se entregó mediante el [PR #267](https://github.com/LCubero/repo-harness-builder/pull/267), fusionado por squash como `814f44f376e9546f6441b7060958857e43a8c5ad`; se verificó el árbol posterior al merge `798bc79a46326bc4dc113fd714c8b1b00b1881ec`. Se mantienen los recibos existentes de 625 pruebas/11 omitidas y checker `current`/cero escrituras; GitHub CI no está configurado y no se afirma release de paquete. El propietario declinó la revisión nativa solo para este candidato; no hay aprobación ni desactivación global de RDD. En ese recibo, el issue #266 seguía abierto y aún no se había registrado la aceptación del ciclo. Después, el propietario aceptó completar el ciclo y dispensó la medición de efectividad de runtime como condición. La efectividad y los ahorros siguen sin medir y no se afirman. El issue #266 seguía abierto al momento del recibo de entrega y después se verificó como `CLOSED` (GitHub `closedAt` 2026-10-06T23:10:19Z).

## Propietarios futuros y orden

Los localizadores describen superficies futuras propuestas, no autoridad de escritura actual. `H` = `skill/repo-harness-builder/scripts/harness_tool.py`; `T` = `skill/repo-harness-builder/tests/test_harness_tool.py`. Las líneas remiten a la base `d30f8b442288eeb180257a46ccc4bb1b64688f5c`, con este delta documental separado. Se inspeccionaron localmente las clases/métodos existentes nombrados abajo; los casos negativos son **pruebas propuestas**, no métodos existentes ni recibos.

### 1. Fijar semántica y procedimiento de base

Propietarios: helpers existentes de conteo/contención en `H` y `GeneratedCoreTests` en `T`; usar el procedimiento aprobado de biblioteca estándar/en memoria, sin script ni almacén nuevo de medición.

- Fijar runtime, locale, integración, resultados de capacidades, forma del repositorio, cláusulas del escenario, bytes de fixtures fijos no sensibles y SHA256. Los hashes fuente identifican entradas, nunca autoridad. Configuraciones no disponibles quedan `unmeasured`.
- Distinguir exclusión de frontmatter solo del cuerpo frente al cierre de archivos completos; inventario de alcance seguro único frente a eventos ordenados de lectura con compuertas; volumen único frente a relecturas. Reubicar lecturas incondicionales no obtiene ahorros.
- Mantener aristas sin resolver, compuertas/orden ambiguos y ciclos sin límite como `unmeasured`; no omitirlos, romper ciclos ni leer estado privado silenciosamente. Usar guardas de contención existentes conservando límites de carreras cooperativas.
- RED propuesto: frontmatter mal formado, rutas que escapan/sin resolver, eventos repetidos y reubicación incondicional contada falsamente como ahorro; GREEN solo tras cambios mínimos aplicables, luego alternativos/refactor.

### 2. Evolucionar atómicamente la propiedad exacta de referencias generadas

Propietarios: esquemas `H` (435, 2760, 2927), renderer (3861), checker (2975), `generated_skill_paths()` (4575), propiedad instalada (4018), reemplazo (4274), legado/migración (4594–4770), apply (4871), derivación LF (1197–1212).

- Derivar archivos/proyecciones exactos y finitos enumerados por el diseño aprobado desde perfiles seleccionados validados y nombres canónicos. Declaración de manifest, esquema/igualdad exacta/procedencia/bytes del checker, reemplazo/actualización, `.repo-harness/manifest.json`, `.repo-harness/check.py` y `.gitattributes` deben evolucionar coherentemente. Nunca confiar en directorio, enlace o prefijo; manifests heredados no otorgan propiedad nueva por proximidad.
- En esta unidad inspeccionar regex de migración en 4723–4724 (hoy excluye Pi y referencias) y atajo de reemplazo `canonicalProjections` en 4293–4374 (pertenecer a proyecciones evita la comprobación de procedencia). Reconciliarlos con propiedad exacta autenticada y protección ante colisiones humanas, no limpieza ajena.
- Pruebas existentes: `GeneratedCheckScriptTests`, `ApprovedGeneratedOutputReplacementTests`, `GeneratedOutputMigrationTests`, `CrossVersionUpdateTests`, `RepositoryLineEndingContractTests`.
- RED propuesto: declaraciones ausentes/extra/duplicadas/que escapan, perfiles/procedencia incorrectos, digest obsoleto, colisión con referencia humana y restauración por rollback. Preservar atomicidad manifest/checker y consentimiento ligado a bytes durante GREEN y triangulación.

### 3. Aligerar entradas start/stop detrás de compuertas obligatorias

Depende de la unidad 2. Propietarios: `H` `session_start_skill()` (1446), `session_stop_skill()` (1654), `.repo-harness/skills/repo-harness-session-{start,stop}/SKILL.md` canónicos generados y solo pares/referencias de runtimes seleccionados.

- Usar exactamente start `reconciliation.md`, `checkpoint.md`, `synchronization.md`; stop `checkpoint.md`, `git-commit.md`, `git-transport.md`, bajo `references/` de cada skill. Mapeo de runtime y predicados siguen la tabla exacta del diseño, no ramas nuevas.
- Conservar en cada entrada activación, seguridad, predicados/orden, cargas obligatorias antes de actuar y detención ante referencias ausentes/inseguras/sin propiedad/ilegibles. Mantener cuatro disparadores de lectura profunda; descubrimiento incondicional de checkpoint en ruta exacta después de recuperación nativa, ausencia `not_found`, sin quinto disparador ni consentimiento reutilizado.
- Mantener stop verificación → documentación aplicable → continuidad nativa → elegibilidad incondicional, resultado de Engram opcional independiente y autoridad de solo commit distinta de push.
- RED propuesto de `GeneratedCoreTests`: compuerta/orden, referencias ausentes y toda obligación de preservación. Reorientar aserciones inline a entrada más cierre obligatorio con compuertas; no borrar protecciones para aprobar compresión. Documentación generada sigue como control sin cambios.

### 4. Comprimir cuerpo del builder y aclarar techo de la guía

Propietarios: `skill/repo-harness-builder/SKILL.md`, sus `references/` existentes, `references/authority-correction.md` propuesta, `references/gentle-ai-style-guide.md` empaquetada (línea 5), aserciones de presupuesto en `T` (5428–5494) y proyecciones exactas seleccionadas del paquete builder.

- Mantener proxy de cuerpo ≤1000 para builder/toda skill generada y aserción del reglamento `<700` sin cambios; nunca restaurar rechazo de presupuesto en render/check ni subir techos. Reutilizar aserciones de desarrollo.
- Preservar `GeneratedCoreTests.test_documented_render_command_parses_and_keeps_every_interview_answer`, `ApprovalUxTests`, flags independientes de entrevista, consentimiento estricto del builder frente al conversacional de mantenimiento y enlaces de autoridad sin estado a destino/plan/generación.
- I1 implementa únicamente la rama explícita de inspección/comprobación autorizada por separado. Preservar las compuertas vigentes de instalación/actualización/corrección; no conceder autoridad para apply/repair/sincronización/Git ni ejecutar experimentos de runtime. El [diseño aprobado](rh-w-0078-design.md) define los límites de evidencia y la detención ante fallos. Los ahorros de runtime siguen desconocidos.
- RED propuesto: cuerpo/alcance del techo y obligaciones de autoridad/punto de acción perdidas; probar entrada más cierre obligatorio. Se pueden secuenciar unidades fuente seguras sin afirmar éxito de runtime; check CLI directo sigue siendo otro escenario.

### 5. Verificar preservación y efectividad antes de aceptar

Depende de unidades 1–4 y autoridades separadas anteriores. Para comportamiento determinista aplicable: RED mínimo enfocado → GREEN mínimo → triangulación negativa/alternativa → refactor solo en verde. Detenerse ante referencias inseguras, discrepancia de propiedad/esquema, colisión humana, digest cambiado, obligación perdida, escenario ambiguo o deriva candidata; preservar evidencia y solicitar la decisión del propietario.

Comandos de validación de implementación (ejecutados para T1–T5; los resultados observados de suite/checker constan en el [estado actual](current-state.md)). La comparación de efectividad de runtime y los experimentos con autorización separada siguen pendientes:

```text
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
python .repo-harness/check.py
```

- Exigir checker `current`, salida 0, escrituras 0; paridad de bytes canónicos/proyecciones de runtimes seleccionados y cobertura LF exacta; bytes humanos sin cambios. Preview/apply de mantenimiento ligado a contenido exige autoridad explícita futura; segunda preview debe ofrecer cero acciones.
- Comparar escenarios estáticos idénticos fijados, entrada/cierre de archivos completos, compuertas ordenadas y relecturas; exigir reducciones aprobadas de rutas comunes, informar aumentos alternativos, nunca inferir ahorros desde tamaño del cuerpo.
- Bajo experimentos aprobados por separado, medir consumo real base/candidato para start de solo reanudación, stop de handoff local e inspección activa resuelta del builder en cada runtime/configuración afirmada. Fijar versiones, estado nuevo/cache y trazas de prompts/herramientas; observaciones no disponibles/incomparables son `unmeasured`, no reducir hace fallar la afirmación. No inventar garantía de recarga, cumplimiento en vivo ni portabilidad.

## Carga de revisión y próxima compuerta

**Inferencia respaldada:** carga documental acumulada esperada de unas 506–576 líneas cambiadas, incluidas las 336 del candidato de diseño preexistente; el handoff final informa conteos reales incluidos archivos sin rastrear. Se recomiendan dos cortes conceptuales: diseño/preparación aprobados (336 líneas), luego esta propuesta bilingüe más metadata indispensable. Es solo recomendación, no estrategia Git/PR seleccionada ni autoridad de entrega. Mantener coherente la transacción de propiedad de unidad 2 aunque su cambio eventual supere el objetivo consultivo de 400 líneas; dividir unidades por comportamiento y rutas propietarias, no pronósticos de líneas sin respaldo.

**Hecho verificado:** los recibos técnicos más recientes se resumen en [Evidencia de verificación más reciente](#evidencia-de-verificacion-mas-reciente), y pasó la reconciliación bilingüe independiente final B4 (`mux4fck6-r-csef`). RH-W-0078 está `completed` por aceptación explícita del propietario; el trabajo restante del ciclo es `none`. La medición de efectividad de runtime se dispensó como condición de finalización; la efectividad y los ahorros siguen sin medir y no se afirman. El issue #266 está verificado como `CLOSED` (GitHub `closedAt` 2026-10-06T23:10:19Z). La revisión nativa se declinó solo para este candidato, sin aprobación ni desactivación global de RDD. Instantánea histórica previa a la entrega: el INSPECT nativo de entonces devolvió `intended_untracked_selection_required` con cero linajes; no hubo START ni reintento. La selección aceptada de 32 archivos, el fallo histórico del presupuesto de lentes de 75 rutas y las brechas del apply-3 siguen siendo antecedentes históricos sin reconstruir. La autoridad previa de staging/commit fue histórica, no se ejerció y no se renueva.
