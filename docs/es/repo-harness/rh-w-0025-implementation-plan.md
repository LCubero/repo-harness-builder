# Plan de implementación de integración consciente de capacidades RH-W-0025

> **Estado: RH-W-0025 está `completed`.** El [registro de la hoja de ruta](roadmap.md#rh-w-0025) posee su resultado y evidencia de entrega; el plan siguiente se conserva como evidencia histórica de ejecución, no como instrucciones a seguir.

> **Para agentes ejecutores:** usar `test-driven-development` para cada corte de comportamiento y `verification-before-completion` antes de entregar. Trabajar solo en la unidad aprobada RH-W-0025.

**Objetivo:** Generar un harness genérico que coordine la autoridad del repositorio, la continuidad de Engram y las capacidades opcionales de Gentle AI sin copiar estado de propietarios ni crear otro ciclo de vida.

**Arquitectura:** El repositorio conserva la verdad durable. La generación `integrated` selecciona Gentle AI más Engram; `standalone` no selecciona ninguno. El preflight del builder durante la instalación valida ambas capacidades antes de seleccionar o aplicar generación integrada. Después de instalar, la pérdida o falla degrada solo el componente afectado, salvo que la política del repositorio lo haga obligatorio. La disponibilidad parcial no es un tercer modo ni un veredicto agregado de autoridad.

**Tecnologías:** Python 3.10+ con biblioteca estándar, `unittest`, Markdown generado, manifiesto JSON y proyecciones incorporadas del paquete en `.opencode/` y `.claude/`.

**Entradas canónicas:** [investigación de integración](integration-research.md), [RH-W-0025](roadmap.md#rh-w-0025) y [decisiones permanentes](decisions.md). El [plan en inglés](../../repo-harness/rh-w-0025-implementation-plan.md) es el propietario canónico; este archivo es su espejo humano completo.

**Resultado de ejecución:** completado. RH-W-0008 entregó el ejecutor de reemplazo ligado al contenido y aplicó el plan consumidor verificado de RH-W-0025 el 2026-08-16.

## Contrato de detección

Ejecutar el comando anunciado de solo lectura `gentle-ai version` mediante PATH y con timeout acotado.

| Clasificación | Evidencia | Autorización |
|---|---|---|
| `not_found` | La creación del proceso informa que no se encuentra el ejecutable. | Gentle AI no está disponible. |
| `found_on_path` | El proceso inicia, pero agota el tiempo, termina con salida distinta de cero o devuelve texto fuera de la forma requerida. | Gentle AI no está verificado y no se puede usar. Conservar evidencia exacta del fallo. |
| `verified` | El proceso termina con salida 0 dentro del límite y stdout normalizado coincide con `gentle-ai <version>` con un token de versión no vacío. | Se puede seleccionar Gentle AI, sujeto a validar Engram y la política del repositorio. |

La mera presencia del ejecutable nunca produce `verified`. La confirmación humana no es la ruta normal de detección y no puede sustituir evidencia del proceso. No inspeccionar layouts privados de instalación, tablas de procesos ni archivos supuestos.

La detección de Engram debe usar su superficie local soportada y conservar evidencia exacta. Definir un adaptador concreto solo después de que la fuente y las pruebas actuales identifiquen el límite existente. No inferir Engram desde archivos no relacionados ni desde contenido de memoria.

Seleccionar y aplicar integrado exige Gentle AI `verified` y Engram verificado. Cualquier otra combinación rechaza generación integrada y puede ofrecer `standalone` solo mediante el límite de selección aprobado existente.

Después de instalar integrado, la pérdida posterior de Gentle AI degrada solo revisión y entrega de Gentle AI. La pérdida posterior de Engram degrada solo continuidad. La política del repositorio puede hacer obligatoria cualquiera de las capacidades y bloquear su flujo propietario.

## Límites de alcance

- [ ] Mantener genérica y centrada en el repositorio la redacción para consumidores.
- [ ] Preservar la propiedad del repositorio sobre verdad durable, fuente, pruebas, política, backlog y aprobaciones.
- [ ] Mantener solo los modos de generación `integrated` y `standalone`.
- [ ] Validar ambas capacidades antes de seleccionar o aplicar generación integrada.
- [ ] Tratar la pérdida posterior a la instalación como degradación del componente, salvo que la política lo haga obligatorio.
- [ ] Promover conocimiento reutilizable solo después de verificarlo, elegir propietario canónico y obtener aprobación humana.
- [ ] No agregar servicio, daemon, scheduler, cliente de red, mutación de proveedor, detección de layout privado, almacén copiado, SDD sintético, aprobación fabricada ni reintento ilimitado.
- [ ] Mantener RH-W-0022 relacionado e independiente.
- [ ] Usar el ejecutor de reemplazo completado en RH-W-0008 para propietarios generados existentes y autenticados. No restaurar una limitación de writer aditivo ni inventar otra ruta de reemplazo.
- [ ] Preservar archivos de propiedad humana y trabajo no relacionado.

## Mapa de archivos previsto

| Ruta | Responsabilidad |
|---|---|
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Pruebas canónicas RED/GREEN de comportamiento y contratos generados |
| `skill/repo-harness-builder/scripts/harness_tool.py` | Ejecución, clasificación, selección y contratos generados de capacidades |
| `skill/repo-harness-builder/SKILL.md` | Contrato de preflight y selección del builder |
| `skill/repo-harness-builder/references/audit-and-interview.md` | Detección de solo lectura y reglas de evidencia |
| `skill/repo-harness-builder/references/architecture.md` | Límites de propietarios, preflight y degradación en runtime |
| `skill/repo-harness-builder/references/preservation-and-safety.md` | Restricciones de aprobación, promoción, reintentos y no copia |
| `skill/repo-harness-builder/references/validation.md` | Validación de capacidades y del harness generado |
| `.opencode/skills/repo-harness-builder/**`, `.claude/skills/repo-harness-builder/**` | Proyecciones exactas de los archivos canónicos modificados |
| Propietarios consumidores devueltos por `TOOL.render(...)` | Preview y apply mediante el ejecutor RH-W-0008 existente |
| Roadmap, estado actual, pares españoles y changelog | Reconciliación del ciclo de vida solo después de implementar y aplicar al consumidor con verificación |

Los nombres de producción siguientes no son obligatorios salvo que se marquen como **objetivos de diseño**. Reutilizar límites existentes cuando correspondan.

## Tarea 1: Caracterizar contratos existentes

- [ ] Agregar una clase enfocada de pruebas conscientes de capacidades junto a las pruebas existentes de contratos generados.
- [ ] Probar la redacción ausente o incompleta sobre repositorio, Engram, Gentle AI, modos, promoción, degradación y prohibición de duplicación.
- [ ] Confirmar el comportamiento existente de `integrationMode` sin agregar entradas de capacidades a `render()` solo para las pruebas.
- [ ] Ejecutar la clase enfocada. Solo los fallos de afirmación son evidencia RED válida.

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v
```

## Tarea 2: Implementar detección y clasificación

Este es un solo corte conductual RED/GREEN. Inyectar o sustituir la ejecución de procesos, el tiempo y la evidencia de Engram. Las pruebas no deben depender de la máquina, el contenido de PATH, esperas reales, red ni instalaciones reales.

- [ ] RED: la ejecución usa exactamente `gentle-ai version`, timeout acotado, captura de stdout y stderr, y ninguna expansión mediante shell.
- [ ] RED: ejecutable ausente clasifica exactamente como `not_found`.
- [ ] RED: timeout clasifica exactamente como `found_on_path` y conserva su evidencia.
- [ ] RED: salida distinta de cero clasifica exactamente como `found_on_path` y conserva código y salida acotada.
- [ ] RED: salida 0 con stdout vacío, malformado, multilínea o con prefijo incorrecto clasifica exactamente como `found_on_path`.
- [ ] RED: salida 0 con `gentle-ai <version>` normalizado y token no vacío clasifica exactamente como `verified`.
- [ ] RED: cobertura parametrizada demuestra que el conjunto exacto es `not_found`, `found_on_path` y `verified`, sin atajo booleano ni cuarto estado.
- [ ] RED: la detección de Engram cubre evidencia verificada, ausente, fallida y malformada mediante su límite local soportado.
- [ ] RED: la clasificación combinada selecciona `integrated` solo cuando ambas capacidades se verifican; en otro caso rechaza seleccionar o aplicar integrado y conserva ambos resultados.
- [ ] RED: los dobles terminan de inmediato, los fixtures de salida son acotados y las pruebas de timeout simulan la excepción sin esperar.
- [ ] GREEN: implementar el comportamiento mínimo en los límites existentes de procesos y selección.
- [ ] GREEN: cualquier nombre propuesto para una costura es un **objetivo de diseño**, no una API aprobada. Elegirlo solo tras inspeccionar el código actual y mantenerlo privado salvo exigencia de un contrato público existente.
- [ ] GREEN: volver a ejecutar el corte enfocado y confirmar resultados deterministas.

## Tarea 3: Agregar contratos genéricos de propietarios

- [ ] Agregar pruebas RED que asignen verdad durable al repositorio, continuidad a Engram y evidencia opcional de revisión y entrega a Gentle AI.
- [ ] Rechazar redacción específica del producto, almacenes copiados, aprobación fabricada, SDD sintético y reintentos hasta quedar limpio.
- [ ] Actualizar solo cuerpos existentes del generador y referencias donde pertenezca cada asunto.
- [ ] Mantener las razones en referencias y no en punteros raíz siempre cargados.
- [ ] Ejecutar la clase enfocada en GREEN.

## Tarea 4: Separar preflight y degradación en runtime

- [ ] Probar que seleccionar y aplicar integrado exige ambas capacidades durante el preflight del builder.
- [ ] Cubrir todas las combinaciones posteriores a la instalación: ambas disponibles, solo Gentle AI, solo Engram y ninguna.
- [ ] Probar que la pérdida posterior afecta solo su componente y nunca fabrica el resultado de otro propietario.
- [ ] Probar que la política del repositorio puede elevar cualquier componente de degradación opcional a requisito bloqueante.
- [ ] Mantener exactamente `integrated` y `standalone` como modos del manifiesto; no persistir detección de runtime ni agregar modo parcial.
- [ ] Ejecutar la clase enfocada en GREEN.

## Tarea 5: Implementar promoción selectiva

- [ ] Exigir relevancia verificada, propietario canónico del repositorio y aprobación humana antes de promover.
- [ ] Fechar y revalidar hechos externos cambiantes antes de reutilizarlos.
- [ ] Rechazar ruido incidental y cualquier backlog o cola de promoción propiedad de memoria.
- [ ] Implementar cambios mínimos en contratos generados de documentación y memoria, y ejecutar pruebas enfocadas en GREEN.

## Tarea 6: Agregar estado por componente y guía de fallos acotados

- [ ] Probar resultados distintos del repositorio, Gentle AI y Engram, evidencia exacta de degradación y una siguiente acción.
- [ ] Probar que salida malformada de un propietario detiene solo esa rama opcional y nunca se reconstruye como éxito.
- [ ] Rechazar cachés de recibos, almacenes de conflictos, ledgers de intentos, estado combinado de aprobación y reintentos interminables.
- [ ] Implementar guía en prosa sin nuevo esquema agregado, salvo exigencia de un contrato propietario actual.

## Tarea 7: Probar estabilidad

- [ ] Afirmar que solo se emiten `integrated` y `standalone`.
- [ ] Preservar comportamiento de runtimes, locales, punteros, manifiesto y propiedad.
- [ ] Afirmar renders repetidos idénticos byte a byte y ausencia de U+2013 y U+2014 en texto modificado.
- [ ] Ejecutar la suite canónica completa y detenerse ante fallos no relacionados.

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

## Tarea 8: Sincronizar proyecciones del paquete

- [ ] Determinar las rutas canónicas modificadas del paquete desde el diff final, no desde una lista fija especulativa.
- [ ] Probar que al menos una proyección correspondiente difiere antes de copiar.
- [ ] Copiar cada archivo canónico modificado a las rutas equivalentes de `.opencode/` y `.claude/`.
- [ ] Verificar igualdad SHA-256 y ejecutar las tres suites con totales iguales y omisiones soportadas.

## Tarea 9: Aplicar reemplazos consumidores

RH-W-0008 está completo y aplicado. Su ejecutor de reemplazo es la única ruta de mutación para propietarios generados existentes y autenticados.

- [ ] Renderizar la salida consumidora exacta con todas las dimensiones instaladas explícitas.
- [ ] Generar un preview nuevo ligado al contenido desde los bytes actuales del repositorio.
- [ ] Revisar acciones exactas create, append, replace, migrate y blocked. No reutilizar listas ni digests históricos.
- [ ] Obtener la aprobación exigida por el límite activo del builder y aplicar el plan sin cambios mediante el ejecutor existente ligado al digest.
- [ ] Verificar que un segundo preview idéntico no tenga acciones y que ambos checkers informen `current` con 0 escrituras.
- [ ] Nunca sustituirlo por ediciones manuales, borrado, copia del paquete ni un writer aditivo nuevo.

## Tarea 10: Reconciliar y entregar

- [ ] Solo después de verificar implementación y apply consumidor, cambiar RH-W-0025 de `ready` a `completed`, usar trabajo restante `none` y reconciliar estado actual, pares españoles, changelog y Engram.
- [ ] No cambiar estado ni dependencia de RH-W-0022.
- [ ] Validar enlaces, paridad semántica bilingüe, caracteres prohibidos y rutas exactas modificadas.
- [ ] Ejecutar todas las suites del paquete, `python .repo-harness/check.py` y `git diff --check`.
- [ ] Inspeccionar estado, estadística y diff enfocado antes de cualquier acción Git aprobada por separado.
- [ ] Ante un fallo, restaurar solo esta unidad mediante su patch o rollback del ejecutor. Nunca usar recuperación destructiva de Git.

## Pronóstico

- Alcance: fuente canónica, pruebas enfocadas, referencias, proyecciones exactas del paquete y propietarios generados seleccionados por un preview nuevo de RH-W-0008.
- Riesgos: validación débil de salida, pruebas de procesos no deterministas, confundir preflight de instalación con degradación en runtime, copiar autoridad y completar prematuramente el ciclo de vida.
- Rollback: patch RH-W-0025 completo más rollback acotado del ejecutor para escrituras generadas.

## Lista de aceptación

- [ ] `gentle-ai version` es la única sonda de Gentle AI; `verified` exige salida 0 y `gentle-ai <version>`.
- [ ] La detección tiene exactamente `not_found`, `found_on_path` y `verified`.
- [ ] La Tarea 2 cubre ejecución, ejecutable ausente, timeout, salida distinta de cero, salida malformada, Engram y clasificación combinada mediante pruebas deterministas y acotadas.
- [ ] Seleccionar y aplicar integrado valida ambas capacidades durante la instalación.
- [ ] La pérdida posterior a la instalación degrada solo su componente, salvo que la política lo haga obligatorio.
- [ ] El ejecutor de reemplazo completado en RH-W-0008 maneja propietarios generados existentes.
- [ ] Se prueban precedencia del repositorio, promoción selectiva, aprobación humana y las cuatro combinaciones de runtime sin tercer modo.
- [ ] Los planes en inglés y español permanecen como espejos semánticos completos.
- [ ] RH-W-0022 permanece relacionado e independiente.
