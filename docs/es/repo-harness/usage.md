# Uso del harness del repositorio

La fuente canonica es [`../../repo-harness/usage.md`](../../repo-harness/usage.md).

<!-- doc-role:overview -->

Use las tres habilidades generadas como un ciclo: recupere contexto, mantenga vigente la documentacion durable y deje una transferencia verificada.

## Flujo diario
<!-- doc-role:daily-workflow -->

1. Inicie o reanude con `repo-harness-session-start` y declare una funcionalidad, bloqueo, issue u objetivo nombrado coherente; un issue no tiene que ser de un proveedor remoto. Lee primero evidencia del repositorio, luego el backup local protegido valido mas nuevo y despues Engram acotado cuando esta disponible. Inspecciona Git local en solo lectura y conserva la clase de evidencia de cada afirmacion.
2. Use `repo-harness-documentation` cuando cambien hechos durables o el estado visible. Complete la documentacion, verificacion, revision y entrega autorizada aplicables al objetivo antes de cerrarlo.
3. Pause o cierre con `repo-harness-session-stop`. Verifica evidencia y documentacion requeridas, escribe y reabre un backup Markdown estructurado y exclusivo bajo `.repo-harness/memory-bk/`, luego escribe en Engram cuando esta disponible e informa cada resultado por separado. Solo el trabajo Git solicitado activa revision de rutas y propuestas exactas. Inicie el trabajo adyacente en una sesion nueva de Pi mediante `repo-harness-session-start`.

## Preferencias de respuesta
<!-- doc-role:response-preferences -->

Para conservar sus elecciones, cree o edite `.repo-harness/state/preferences/user-preferences.json` a mano, o pida explicitamente al agente que las guarde. El agente nunca guarda preferencias por iniciativa propia. Este archivo local queda ignorado por Git mediante `/.repo-harness/state/`. Use un objeto JSON con al menos una de estas claves opcionales:

- `conversationLanguage`: `english` o `espanol`
- `responseStructure`: `standard` o `adhd-friendly`
- `explanationStyle`: `standard` o `plain-language`

Ejemplo:

```json
{"conversationLanguage": "espanol", "responseStructure": "adhd-friendly"}
```

`repo-harness-session-start` lo lee una vez al iniciar mediante el lector fijo (`harness_tool.py read-preference`). Una clave o valor desconocido, clave duplicada o JSON ilegible invalida el archivo. El inicio informa un diagnostico breve y continua sin preferencias (modos standard); nunca bloquea.

Para cada eje de respuesta, prevalece una instruccion explicita de la sesion actual, luego el archivo y luego `standard`. Nunca se infieren modos del estilo de escritura o comportamiento. Ambos modos pueden combinarse; solo moldean respuestas y no hacen afirmaciones clinicas.

Propietarios de las reglas: [contrato de respuesta](../../../.repo-harness/rules/response-contract.md), [ADHD-friendly](../../../.repo-harness/rules/response-adhd-friendly.md), y [plain-language](../../../.repo-harness/rules/response-plain-language.md).

## Disponibilidad integrada
<!-- doc-role:availability -->

El backup local es obligatorio en modos integrado y standalone. El modo integrado ademas compone Gentle AI y Engram de forma independiente; cualquiera puede seguir siendo util sin el otro. El inicio consulta Engram solo despues de la evidencia del repositorio y de continuidad local, y comprueba Gentle AI solo en limites de coordinacion habilitados. Informe fallos sin prometer garantias ausentes. Standalone no usa componentes externos. Los propietarios del repositorio conservan la autoridad, y commit, push y PR siguen bajo politica del repositorio.

## Congruencia y propiedad
<!-- doc-role:ownership -->

Ejecute `python3 .repo-harness/check.py` (`python` en Windows) despues de cambiar fuentes canonicas o proyecciones. El archivo raiz `.gitattributes` preserva bytes LF mediante una regla exacta `<path> text eol=lf` por cada ruta generada comparada byte a byte, incluidos checker, manifiesto, habilidades y familias de comandos canonicos, proyecciones de runtime, wrappers y proyecciones exactas opcionales del producto. Documentos humanos y archivos comprobados por directivas quedan fuera de ese contrato de rutas generadas. El control demuestra paridad en disco, no la vigencia de contenido registrado en el proceso activo.

## Limites de aprobacion
<!-- doc-role:approval-boundaries -->

Las habilidades diarias inspeccionan en solo lectura por defecto y usan aprobacion conversacional. Los resultados son `ready`, `paused`, `blocked`, `failed`, `completed` o `degraded`, y `paused` significa que realmente se necesita intervencion humana. El consentimiento explicito de sincronizacion segura cubre un fetch acotado mas una integracion solo de avance rapido (fast-forward). El consentimiento explicito combinado de commit y push cubre el staging exacto, el commit, las comprobaciones, una verificacion previa con fetch y un push al upstream identificado. Solo commit y solo push siguen siendo estrechos. Un cambio de alcance, archivos sensibles o no relacionados, divergencia, cambios de destino o credencial, integracion destructiva, mutacion por hooks o una verificacion fallida invalidan el consentimiento.

La aplicacion del constructor es distinta y estricta unicamente durante una ejecucion explicitamente activa de la habilidad `repo-harness-builder`, declarada con `--activation builder`: en ingles exige exactamente `APPROVE` o `DENY`; en espanol exige exactamente `APROBAR` o `DENEGAR`. La CLI recibe explicitamente el idioma resuelto y exige tanto el digest vigente del plan como la aprobacion exacta del dominio activo. Con el valor predeterminado `--activation maintenance`, el dominio estricto no aparece en la salida, `--approval` se rechaza y aplicar solo requiere el digest coincidente. Las llamadas directas a la CLI usan ingles de forma predeterminada. La identidad del repositorio, el autoalojamiento, el mantenimiento ordinario del repositorio, la propiedad de archivos generados, las habilidades de sesion y las preguntas directas no activan esta interfaz. Fuera de una ejecucion activa de la habilidad del constructor, use reglas ordinarias de aprobacion conversacional.

Una corrida del constructor que lleva un contexto de autoridad tambien imprime digests que puede revisar sin leer nada privado: `authorityContextSha256` para saber de que trataba la aprobacion, `targetBindingSha256` para que destino nombro, `planSha256` para los cambios, y `generationSha256` que los liga a todos. Los valores de premisa y los enunciados de decision aparecen como marcadores explicitos de redaccion, y cada evidencia aparece como un locator relativo al repositorio y un digest. Verifique que esos locators sean los archivos que espera y recien entonces apruebe. Si algo estaba mal, el host debe refrescar el contexto con una revision material nueva y volver a previsualizar, porque una respuesta corregida con un plan identico conserva el mismo `planSha256` y obtiene un `generationSha256` distinto, de modo que la aprobacion anterior no se puede reutilizar.

El modo batch permite que el agente agrupe sus preguntas sobre una tarea en vez de hacerlas una por una. Pidalo de forma explicita, nombrando el modo batch y una sola tarea delimitada, por ejemplo "modo batch: actualizar la documentacion de RH-W-0107"; el agente nunca lo activa por su cuenta. Primero inspecciona el repositorio, resuelve sin preguntar lo que el repositorio puede responder, hace juntas las preguntas de requisitos que quedan, como una lista numerada en un solo bloque de pregunta, y luego continua sin confirmacion archivo por archivo. Termina solo cuando usted pausa o cierra la sesion, la tarea se completa o se abandona, o cambia a otra tarea, y nunca se guarda para una sesion posterior. No hay una frase definida para apagarlo a mitad de una tarea. Las respuestas en batch nunca son aprobaciones: el acceso externo, los commits, push, PR, la publicacion, las acciones destructivas, los secretos, la aplicacion del constructor (`APPROVE`/`DENY`, digests) y las decisiones materiales de producto se siguen preguntando una a la vez. Propietarios: [`repo-harness-session-start`](../../../.repo-harness/skills/repo-harness-session-start/SKILL.md) (activacion), [`repo-harness-session-stop`](../../../.repo-harness/skills/repo-harness-session-stop/SKILL.md) (expiracion) y [limites de interaccion por lotes](../../../.repo-harness/rules/batch-interaction.md).

## Retirar el harness manualmente
<!-- doc-role:manual-removal -->

No existe un desinstalador automático. El comando de ayuda explica esta guía en modo de solo lectura; nunca elimina archivos.

1. Revise `.repo-harness/manifest.json` y los archivos reales para listar las skills, comandos y copias de los entornos instalados. El manifiesto es un inventario, no permiso para borrar; preserve el contenido modificado o desconocido. Revise también la carpeta del builder si se instaló por separado. No elimine los directorios completos `.claude/`, `.opencode/` ni `.pi/`.
2. Conserve la documentación del proyecto (README, CHANGELOG y `docs/`) y la continuidad, preferencias y respaldos útiles por defecto. Preserve lo necesario antes de retirar el almacenamiento del harness.
3. En los archivos compartidos `AGENTS.md`, `CLAUDE.md`, `.gitignore` y `.gitattributes`, retire solo las referencias o reglas confirmadas del harness que ya no sean necesarias. Conserve el contenido ajeno, incluidas las reglas de exclusión usadas por otras herramientas. Revise los documentos conservados y actualice solo sus referencias a rutas retiradas.
4. Muestre las eliminaciones y ediciones exactas propuestas y obtenga aprobación explícita. Retire solo los archivos exclusivos del harness aprobados; retire `.repo-harness/` al final y solo tras revisar si contiene información que conservar. Nunca elimine el repositorio ni archivos de otras herramientas.
5. Revise el diff si hay Git y compruebe que no queden entradas del harness ni enlaces rotos. Inicie una sesión nueva del agente: una sesión existente puede conservar instrucciones cargadas. No ejecute el checker retirado ni reinstale el harness para comprobar su retirada.
