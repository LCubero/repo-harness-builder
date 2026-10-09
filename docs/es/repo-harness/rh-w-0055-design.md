# Diseño RH-W-0055 de instalación e incorporación

> **Estado: RH-W-0055 está `completed`.** El [registro de la hoja de ruta](roadmap.md#rh-w-0055) posee su resultado; el texto siguiente se conserva como evidencia histórica.

Estado: `proposed`; diseño aprobado por el propietario el 2026-09-06. Esta aprobación no autoriza planificación de implementación, implementación, publicación ni acceso de red.

## Problema y límite

**Hecho verificado:** el producto admite `claude-code`, `opencode` y `pi`; el repositorio ya posee la invocación del builder y el uso diario en [README.es.md](../../../README.es.md) y [usage.md](usage.md), mientras la [hoja de ruta](roadmap.md#rh-w-0055) registra que no existe una ruta única desde un artefacto seleccionado hasta esos propietarios. Un asset de release existente es evidencia histórica, no una promesa de disponibilidad futura.

Este diseño agrega una sola secuencia LLM-first independiente del nombre del artefacto: seleccionar y adquirir localmente un artefacto; asignar a `ARTIFACT_ROOT` su raíz extraída; validar su estructura fuente; seleccionar explícitamente un runtime; copiar el paquete y la entrada correspondiente; invocar el builder en el repositorio objetivo bajo su contrato actual; ejecutar el checker generado de solo lectura; y usar el flujo existente de inicio y cierre de sesión.

No agrega comandos, API, estado, stores, automatización de gestores de paquetes, URL, comportamiento generado ni acciones de release.

## Enfoques considerados

1. **Un instalador universal.** Reduce la orden humana, pero crea comportamiento de instalación, manejo por plataforma y otra superficie de autoridad. Rechazado por desproporcionado.
2. **Guías independientes por runtime.** Son fáciles de escribir, pero duplican adquisición, verificación, builder y uso diario, y facilitan divergencias. Rechazado.
3. **Una ruta compartida con un paso de copia por runtime.** Reutiliza propietarios actuales y solo varía los dos destinos exigidos por cada runtime. **Seleccionado y aprobado por el propietario como diseño coherente mínimo.**

## Contrato seleccionado

La futura guía acepta un checkout fuente confiable o un paquete de release ya adquirido y extraído. Nunca descarga, autentica, publica ni afirma que existe un artefacto. La raíz local debe contener `skill/repo-harness-builder/` y `command/repo-harness-builder.md`; si falta alguno, la instalación se detiene por artefacto inválido.

La persona selecciona explícitamente uno o más runtimes. Para cada runtime, la guía proporciona un par de copias literal, parametrizado solo por `ARTIFACT_ROOT` y la raíz del repositorio objetivo:

| Runtime | Destino del paquete | Destino de entrada |
|---|---|---|
| Claude Code | `.claude/skills/repo-harness-builder/` | `.claude/commands/repo-harness-builder.md` |
| OpenCode | `.opencode/skills/repo-harness-builder/` | `.opencode/commands/repo-harness-builder.md` |
| Pi | `.pi/skills/repo-harness-builder/` | `.pi/prompts/repo-harness-builder.md` |

La copia es aditiva: un destino ocupado detiene el flujo para inspección y nunca se sobrescribe. Después se abre el repositorio objetivo y se invoca la entrada `repo-harness-builder` instalada. El builder conserva la propiedad exclusiva de auditoría, selección de proyecciones, preview, aprobación, apply y `check`; la guía enlaza [Constructor y habilidades generadas](../../../README.es.md#constructor-y-habilidades-generadas) sin repetir esos contratos.

Después de un apply exitoso se ejecuta `python3 .repo-harness/check.py` (`python` en Windows). Solo salida 0 con `current` y cero escrituras establece congruencia en disco. El registro del runtime puede requerir un proceso nuevo si el actual conserva contenido anterior; no se inventa una orden de recarga. El trabajo diario sigue [Flujo diario](usage.md#flujo-diario): inicio de sesión, documentación cuando cambia estado durable y cierre de sesión.

Los fallos se detienen en su límite propietario: artefacto inválido antes de copiar; destino ocupado antes de sobrescribir; registro no disponible antes de invocar; rechazo del builder en su límite actual; y drift antes de declarar preparación diaria. El modo offline solo aplica cuando el artefacto ya está local y los prerrequisitos existen.

## Identidad offline

**Hecho verificado:** los bytes actuales incluyen versiones de generador y paquete, y los releases publicados pueden aportar un digest externo. **Inferencia respaldada:** insertar otra identidad no aporta prueba offline confiable, porque puede copiarse junto con el payload; solo un digest obtenido independientemente puede autenticarlo. Una identidad firmada y verificable de manera independiente cambiaría la conclusión.

Por tanto, no se encuentra una identidad viable para RH-W-0062 más allá de la frescura del manifest y un digest conservado independientemente. [RH-W-0062](roadmap.md#rh-w-0062) se cierra sin residuo bajo su condición de avance existente; esa reconciliación no aprueba este diseño ni avanza RH-W-0055.

## Aceptación y exclusiones

El diseño aprobado exige una ruta EN/ES coherente, enlaces válidos, destinos exactos, límites offline y de fallo explícitos, y ausencia de detalle duplicado. Se excluyen publicación, red, nombrado de archivos, renombres reales, cambios generados, API o estado nuevos, stores, comandos, assets, URL, automatización de gestores de paquetes, planes e implementación.

## Compuerta humana

Decisión del propietario registrada: el diseño bilingüe reducido está aprobado. RH-W-0055 sigue `proposed`; planificación de implementación, implementación, publicación, acceso de red y cualquier fase posterior permanecen sin autorización.
