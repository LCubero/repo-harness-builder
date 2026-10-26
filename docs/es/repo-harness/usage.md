# Uso del harness del repositorio

La fuente canónica es [`../../repo-harness/usage.md`](../../repo-harness/usage.md).

<!-- doc-role:overview -->

Use las tres skills generadas como un ciclo: recupere contexto, mantenga vigente la documentación durable y deje una transferencia verificada.

## Flujo diario
<!-- doc-role:daily-workflow -->

1. Inicie o reanude con `repo-harness-session-start` y declare una funcionalidad, bloqueo, issue u objetivo nombrado coherente; un issue no tiene que ser de un proveedor remoto. Lee primero evidencia del repositorio, luego el backup local protegido válido más nuevo y después Engram acotado cuando está disponible. Inspecciona Git local en solo lectura y conserva la clase de evidencia de cada afirmación.
2. Use `repo-harness-documentation` cuando cambien hechos durables o el estado visible. Complete la documentación, verificación, revisión y entrega autorizada aplicables al objetivo antes de cerrarlo.
3. Pause o cierre con `repo-harness-session-stop`. Verifica evidencia y documentación requeridas, escribe y reabre un backup Markdown estructurado y exclusivo bajo `.repo-harness/memory-bk/`, luego escribe en Engram cuando está disponible e informa cada resultado por separado. Solo el trabajo Git solicitado activa revisión de rutas y propuestas exactas. Inicie el trabajo adyacente en una sesión nueva del agente mediante `repo-harness-session-start`.

La continuidad no necesita commit, push ni remoto: también funciona en repositorios privados y solo locales. Las fechas y nombres de respaldos usan su zona horaria elegida o, en su defecto, la hora local del host. Los respaldos antiguos con nombres UTC siguen siendo legibles; no los renombre.

## Preferencias de respuesta
<!-- doc-role:response-preferences -->

Para conservar sus elecciones, cree o edite `.repo-harness/state/preferences/user-preferences.json` a mano, o pida explícitamente al agente que las guarde. El agente nunca guarda preferencias por iniciativa propia. Este archivo local queda ignorado por Git mediante `/.repo-harness/state/`. Use un objeto JSON con al menos una de estas claves opcionales:

- `conversationLanguage`: `english` o `espanol`
- `responseStructure`: `standard` o `adhd-friendly`
- `explanationStyle`: `standard` o `plain-language`

Ejemplo:

```json
{"conversationLanguage": "espanol", "responseStructure": "adhd-friendly"}
```

`repo-harness-session-start` lo lee una vez al iniciar mediante el lector fijo (`harness_tool.py read-preference`). Una clave o valor desconocido, clave duplicada o JSON ilegible invalida el archivo. El inicio informa un diagnóstico breve y continúa sin preferencias (modos standard); nunca bloquea.

Para `responseStructure` y `explanationStyle`, prevalece una instrucción explícita de la sesión actual, luego el archivo y luego `standard`. Nunca se infieren modos del estilo de escritura o comportamiento. Ambos modos pueden combinarse; solo moldean respuestas y no hacen afirmaciones clínicas.

El idioma de conversación se resuelve aparte: elección humana explícita, preferencia persistida, idioma de sesión elegible, manifiesto y, por último, inglés.

Propietarios de las reglas: [contrato de respuesta](../../../.repo-harness/rules/response-contract.md), [ADHD-friendly](../../../.repo-harness/rules/response-adhd-friendly.md), y [plain-language](../../../.repo-harness/rules/response-plain-language.md).

## Disponibilidad integrada
<!-- doc-role:availability -->

El backup local es obligatorio en modos integrado y standalone. El modo integrado además compone Gentle AI y Engram de forma independiente; cualquiera puede seguir siendo útil sin el otro. El inicio consulta Engram solo después de la evidencia del repositorio y de continuidad local, y comprueba Gentle AI solo en límites de coordinación habilitados. Informe fallos sin prometer garantías ausentes. Standalone no usa componentes externos. Los propietarios del repositorio conservan la autoridad, y commit, push y PR siguen bajo política del repositorio.

## Congruencia y propiedad
<!-- doc-role:ownership -->

Ejecute `python3 .repo-harness/check.py` (`python` en Windows) después de cambiar fuentes canónicas o proyecciones. El archivo raíz `.gitattributes` preserva bytes LF mediante una regla exacta `<path> text eol=lf` por cada ruta generada comparada byte a byte, incluidos checker, manifiesto, habilidades y familias de comandos canónicos, proyecciones de runtime, wrappers y proyecciones exactas opcionales del producto. Documentos humanos y archivos comprobados por directivas quedan fuera de ese contrato de rutas generadas. El control demuestra paridad en disco, no la vigencia de contenido registrado en el proceso activo. El proceso activo puede conservar contenido anterior en caché: use `/reload` en Pi o una sesión nueva; otros entornos necesitan su recarga soportada o una sesión nueva. Engram aporta contexto temporal entre sesiones; los archivos del repositorio siguen siendo autoritativos.

## Límites de aprobación
<!-- doc-role:approval-boundaries -->

Las habilidades diarias inspeccionan en solo lectura por defecto y usan aprobación conversacional. Los resultados son `ready`, `paused`, `blocked`, `failed`, `completed` o `degraded`, y `paused` significa que realmente se necesita intervención humana. Las escrituras documentales requieren aprobación conversacional explícita. El consentimiento explícito de sincronización segura cubre un fetch acotado más una integración solo de avance rápido (fast-forward). El consentimiento explícito combinado de commit y push cubre el staging exacto, el commit, las comprobaciones, una verificación previa con fetch y un push al upstream identificado. Solo commit y solo push siguen siendo estrechos. Un cambio de alcance, archivos sensibles o no relacionados, divergencia, cambios de destino o credencial, integración destructiva, mutación por hooks o una verificación fallida invalidan el consentimiento.

La aplicación del constructor es distinta y estricta únicamente durante una ejecución explícitamente activa de la habilidad `repo-harness-builder`, declarada con `--activation builder`: en inglés exige exactamente `APPROVE` o `DENY`; en español exige exactamente `APROBAR` o `DENEGAR`. La CLI recibe explícitamente el idioma resuelto y exige tanto el digest vigente del plan como la aprobación exacta del dominio activo. Con el valor predeterminado `--activation maintenance`, el dominio estricto no aparece en la salida, `--approval` se rechaza y aplicar solo requiere el digest coincidente. Las llamadas directas a la CLI usan inglés de forma predeterminada. La identidad del repositorio, el autoalojamiento, el mantenimiento ordinario del repositorio, la propiedad de archivos generados, las habilidades de sesión y las preguntas directas no activan esta interfaz. Fuera de una ejecución activa de la habilidad del constructor, use reglas ordinarias de aprobación conversacional.

Cuando el constructor lleva contexto de autoridad, revise sus referencias de evidencia y digests antes de aprobar; los valores de premisa permanecen redactados. Cambiar premisas o destinos exige renovar el contexto y la vista previa; un plan idéntico por sí solo no permite reutilizar una aprobación anterior. Consulte los detalles de vinculación en el [contrato de corrección de autoridad](../../../skill/repo-harness-builder/references/authority-correction.md) y las actualizaciones en la [guía del constructor](builder-guide.md#actualizaciones-seguras).

Pida explícitamente modo batch para una tarea delimitada: agrupa las preguntas de requisitos pendientes tras inspeccionar y continúa sin confirmación por archivo. Nunca es automático ni persistente, y termina al pausar, cerrar, completar, abandonar o cambiar de tarea. No hay una frase definida para apagarlo a mitad de una tarea. Las respuestas en batch nunca son aprobaciones: el acceso externo, los commits, push, PR, la publicación, las acciones destructivas, los secretos, la aplicación del constructor (`APPROVE`/`DENY`, digests) y las decisiones materiales de producto se siguen preguntando una a la vez. Propietarios: [`repo-harness-session-start`](../../../.repo-harness/skills/repo-harness-session-start/SKILL.md) (activación), [`repo-harness-session-stop`](../../../.repo-harness/skills/repo-harness-session-stop/SKILL.md) (expiración) y [límites de interacción por lotes](../../../.repo-harness/rules/batch-interaction.md).

## Retirar el harness manualmente
<!-- doc-role:manual-removal -->

No existe un desinstalador automático. El comando de ayuda explica esta guía en modo de solo lectura; nunca elimina archivos.

1. Revise `.repo-harness/manifest.json` y los archivos reales para listar las skills, comandos y copias de los entornos instalados. El manifiesto es un inventario, no permiso para borrar; preserve el contenido modificado o desconocido. Revise también la carpeta del builder si se instaló por separado. No elimine los directorios completos `.claude/`, `.opencode/` ni `.pi/`.
2. Conserve la documentación del proyecto (README, CHANGELOG y `docs/`) y la continuidad, preferencias y respaldos útiles por defecto. Preserve lo necesario antes de retirar el almacenamiento del harness.
3. En los archivos compartidos `AGENTS.md`, `CLAUDE.md`, `.gitignore` y `.gitattributes`, retire solo las referencias o reglas confirmadas del harness que ya no sean necesarias. Conserve el contenido ajeno, incluidas las reglas de exclusión usadas por otras herramientas. Revise los documentos conservados y actualice solo sus referencias a rutas retiradas.
4. Muestre las eliminaciones y ediciones exactas propuestas y obtenga aprobación explícita. Retire solo los archivos exclusivos del harness aprobados; retire `.repo-harness/` al final y solo tras revisar si contiene información que conservar. Nunca elimine el repositorio ni archivos de otras herramientas.
5. Revise el diff si hay Git y compruebe que no queden entradas del harness ni enlaces rotos. Inicie una sesión nueva del agente: una sesión existente puede conservar instrucciones cargadas. No ejecute el checker retirado ni reinstale el harness para comprobar su retirada.
