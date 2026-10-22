# Plan de implementación RH-W-0055

> **Estado: RH-W-0055 está `completed`.** El [registro de la hoja de ruta](roadmap.md#rh-w-0055) posee su resultado y evidencia de entrega; el plan siguiente se conserva como evidencia histórica de ejecución, no como instrucciones a seguir.

**Estado: plan aprobado por el propietario.** El 2026-09-30 el propietario aprobó este plan y delegó la implementación. El [diseño aprobado](rh-w-0055-design.md) posee el contrato; el [roadmap](roadmap.md#rh-w-0055) posee el ciclo de vida y la evidencia de verificación.

Es un cambio solo de documentación. No toca código, pruebas, comportamiento generado, checker ni proyecciones de runtime.

1. **Ubicar la ruta donde el usuario ya mira.** Ampliar el paso 1 de inicio rápido en `README.md` y `README.es.md` con una subsección corta (`Install The Builder` / `Instalar el constructor`) dentro del rol `quick-start` existente. No crear documento, marcador doc-role ni arista de relación nuevos; el par README ya está declarado.
2. **Escribir la ruta en el orden del diseño, enlazando en vez de repetir.**
   - Origen: un checkout de fuente confiable o un paquete de release ya extraído. Fijar `ARTIFACT_ROOT` en su raíz. La guía nunca descarga, nombra un archivo ni afirma que un asset existe.
   - Forma: `skill/repo-harness-builder/` y `command/repo-harness-builder.md` deben existir bajo `ARTIFACT_ROOT`; si no, detenerse como artefacto inválido.
   - Requisitos: Python 3 y el runtime elegido ya instalados.
   - La tabla de destinos del diseño y luego un bloque POSIX shell para copiar y pegar por runtime (Git Bash en Windows), parametrizado solo por `ARTIFACT_ROOT` y la raíz del repositorio destino. Cada bloque se niega si algún destino ya existe y nunca sobrescribe.
   - Invocar `repo-harness-builder` en el repositorio destino; enlazar [Constructor y habilidades generadas](builder-guide.md#constructor-y-habilidades-generadas) para auditoría, vista previa y aprobación.
   - Después de aplicar, ejecutar `python3 .repo-harness/check.py` (`python` en Windows); solo salida 0 con `current` y cero escrituras indica congruencia. Un runtime puede necesitar un proceso nuevo para registrar skills nuevas.
   - Uso diario: enlazar [Flujo diario](usage.md#flujo-diario).
   - Una lista corta de paradas: artefacto inválido, destino ocupado, runtime sin registrar, rechazo del builder, deriva del checker. Sin red funciona una vez que el artefacto y los requisitos son locales.
3. **Verificar de verdad, dentro del repositorio.** Ejecutar cada bloque una vez con `ARTIFACT_ROOT` en este repositorio y un destino temporal bajo `.repo-harness/tmp/` (ignorado); confirmar los archivos esperados, repetir para confirmar el rechazo por destino ocupado y borrar el destino temporal. Confirmar títulos y enlaces EN/ES, `git diff --check` limpio y `python .repo-harness/check.py` en `current` con cero escrituras.
4. **Entregar y cerrar.** Un issue y un PR. Tras el merge, registrar `completed` en roadmap y estado actual (EN/ES), mapas de documentación, `CHANGELOG.md`/`CHANGELOG.es.md` y el tema Engram `work/RH-W-0055`, solo con evidencia observada. El rollback es revertir el único commit de documentación.

**No objetivos:** scripts instaladores, automatización con gestores de paquetes, variantes PowerShell, descargas, URLs, nombres de archivo, publicación, comandos o estado nuevos, cambios de comportamiento del builder e implementación sin autorización separada.
