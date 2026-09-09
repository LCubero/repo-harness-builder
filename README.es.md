# repo-harness-builder
<!-- doc-role:repository-harness -->

<!-- README-I18N:START -->

[English](./README.md) | **Español**

<!-- README-I18N:END -->

`repo-harness` es un sistema acotado al repositorio compuesto por prompts, guardrails, reglas del harness, documentacion durable, habilidades y comandos que fortalece y asiste a agentes de programacion con IA como OpenCode, Claude Code y Pi. Mantiene visibles las reglas, vigente el estado del proyecto, portables las instrucciones de runtime y verificables las transferencias, sin quitar propiedad a quienes mantienen el repositorio.

`repo-harness-builder` es la habilidad y herramienta que crea, audita, instala y evoluciona con seguridad `repo-harness` dentro de un repositorio seleccionado.

## Posicionamiento del producto
<!-- doc-role:positioning -->

El harness conserva su utilidad por si solo. El modo integrado predeterminado compone de forma independiente las capacidades disponibles de Gentle AI y Engram, mientras CodeGraph sigue siendo opcional. Una capacidad no disponible degrada solo el comportamiento que posee, salvo que la operacion seleccionada o la politica del repositorio la exija. El modo `--standalone` explicito usa continuidad solo del repositorio.

| Componente | Limite |
|---|---|
| `repo-harness` | Guia operativa acotada al repositorio y verdad durable del proyecto para agentes de programacion con IA compatibles. |
| Gentle AI | Un ecosistema más amplio para agentes de IA que puede proporcionar enrutamiento, revisión del candidato exacto, recibos y corrección acotada de la revisión; la política del repositorio es responsable de commits, pushes, PR y releases. |
| Engram | Memoria semantica y continuidad entre sesiones. Nunca reemplaza la verdad del repositorio. |
| CodeGraph | El componente mas opcional: una ayuda local al repositorio para ubicar archivos, simbolos, relaciones e impacto. No es un almacen de autoridad. |

Hermes Agent, OpenClaw y Pi pueden aportar a la investigacion, pero recrearlos o copiarlos no es un objetivo del producto. `repo-harness` tiene limites distintos y acotados al repositorio.

## Inicio rapido
<!-- doc-role:quick-start -->

1. Proyecte la habilidad y el comando del constructor en un entorno compatible.
2. Abra el repositorio objetivo e invoque `/repo-harness-builder`.
3. Responda la entrevista: proyecciones de entorno, licencia, idioma de conversacion y locales de documentacion. La documentacion queda solo en ingles salvo que declare otro locale.
4. Mantenga el ignore predeterminado de `.codegraph/` en la raiz o use `--no-codegraph-ignore` para registrar el opt-out.
5. Revise el plan exacto. En espanol, la aprobacion estricta usa `APROBAR` o `DENEGAR`.
6. Inicie el trabajo normal con la habilidad generada `repo-harness-session-start`.

Sin operacion explicita, el constructor realiza una auditoria de solo lectura. No inicializa Git ni modifica remotos, crea commits, publica o envia cambios.

## Que genera
<!-- doc-role:outputs -->

- `REPO-HARNESS.md` como entrada operativa canonica.
- `README.md` en la raiz cuando falta, con una seccion acotada administrada por el constructor.
- Documentacion durable completa en ingles bajo `docs/repo-harness/`.
- Habilidades neutrales bajo `.repo-harness/skills/`, pares canonicos de comandos bajo `.repo-harness/commands/` y `.repo-harness/prompts/`, y proyecciones para Claude Code, OpenCode o Pi.
- `.repo-harness/check.py` para comprobaciones de congruencia sin escritura.
- Una unica directiva `.codegraph/` en `.gitignore` raiz, salvo opt-out explicito.
- `.gitattributes` raiz con proteccion LF exacta para cada ruta generada comparada byte a byte.

Un harness es solo en ingles salvo que se declaren mas locales de documentacion. Consulte [Idioma de la documentacion](#idioma-de-la-documentacion).

| Parte | Aporte |
|---|---|
| `REPO-HARNESS.md` | Libro de reglas conciso que siempre se lee: proposito, reglas obligatorias y rutas hacia detalles. |
| `docs/repo-harness/` | Propietarios durables de uso, estado actual y decisiones. |
| `CHANGELOG.md` | Registro fechado de cambios completados, separado del estado y trabajo futuro. |
| Continuidad de sesion | Backups locales estructurados obligatorios bajo `.repo-harness/memory-bk/`, mas continuidad Engram opcional cuando esta disponible; el repositorio conserva la autoridad. |
| Proyecciones | Superficies delgadas para Claude Code, OpenCode o Pi generadas desde habilidades canonicas y pares de comandos por familia. |
| `.repo-harness/check.py` | Comprobacion de solo lectura que detecta proyecciones de habilidades, comandos y prompts faltantes u obsoletas. |

## Constructor y habilidades generadas
<!-- doc-role:skills -->

El constructor instala o actualiza. Las tres habilidades del repositorio forman el flujo operativo diario.

| Habilidad | Cuando usarla | Resultado |
|---|---|---|
| `repo-harness-builder` | Crear, auditar o actualizar con seguridad un harness | Un harness planificado, aprobado y verificado |
| `repo-harness-session-start` | Iniciar o reanudar trabajo | Evidencia del repositorio primero, backup local valido mas nuevo, contexto Engram acotado opcional y reconciliacion visible |
| `repo-harness-documentation` | Cambiaron hechos durables o estado visible | Documentos canonicos actualizados sin duplicacion |
| `repo-harness-session-stop` | Pausar o terminar | Backup local exclusivo verificado, resultado Engram independiente y unidades de trabajo revisables |

## Ciclo de vida
<!-- doc-role:lifecycle -->

```text
Primera ejecucion del constructor
  -> auditoria de solo lectura
  -> elecciones de entorno, integracion y licencia
  -> plan exacto y digest
  -> aprobacion explicita
  -> harness generado y verificado

Sesion normal
  -> session-start lee evidencia del repositorio, luego backup local y despues Engram opcional
  -> el trabajo sigue REPO-HARNESS.md
  -> documentation actualiza propietarios durables
  -> session-stop verifica y escribe la transferencia local primero
```

Ambos modos usan el backup local nativo. El modo integrado compone de forma independiente las capacidades disponibles de Gentle AI y Engram. Engram conecta sesiones cuando esta disponible, pero nunca reemplaza los archivos actuales; standalone no usa componentes externos de continuidad.

## Flujo de ejemplo
<!-- doc-role:example -->

```text
/repo-harness-builder
# Revise la auditoria y el plan, luego apruebe.

Cargue repo-harness-session-start y continue la migracion de API.
# Trabaje, pruebe y verifique.

Cargue repo-harness-documentation y actualice el estado y el historial.

Cargue repo-harness-session-stop y prepare la transferencia.
```

El comando del constructor sigue siendo `/repo-harness-builder`; las habilidades diarias usan los nombres registrados fijos mostrados arriba. La sintaxis de comandos depende del entorno, asi que las habilidades se invocan mediante el mecanismo de habilidades del entorno. La ayuda generada lee `docs/repo-harness/usage.md`, y lee `docs/es/repo-harness/usage.md` solo cuando el espanol es un locale de documentacion declarado y el idioma de conversacion es espanol. Nunca nombra una guia que ese mismo render no creo. Los resultados son `ready`, `paused`, `blocked`, `failed`, `completed` y `degraded`.

## Comportamiento del repositorio
<!-- doc-role:repository-behavior -->

### Repositorios nuevos

Un repositorio nuevo o practicamente vacio recibe una base util completa despues de aprobar el plan: README raiz, libro de reglas, documentacion durable, habilidades diarias, proyecciones seleccionadas, manifiesto y checker.

### Repositorios existentes

El constructor analiza el contenido existente relevante y propone una integracion informada. Preserva bytes desconocidos y nunca entiende la preservacion como congelar el repositorio. Si el contenido esta obsoleto o es estructuralmente incompatible, archivar y partir de una base limpia es una eleccion distinta que igual requiere aprobacion explicita.

Un repositorio existente nunca recibe un locale de documentacion que no tenga ya. Un harness heredado cuyo manifiesto es anterior a los metadatos de idioma conserva los arboles de locale realmente instalados, de modo que un repositorio bilingue antiguo no pierde en silencio su comprobacion en espanol y un repositorio solo en ingles no gana en silencio archivos en espanol.

### Idioma de la documentacion

La documentacion humana del repositorio puede quedar solo en ingles, solo en espanol o bilingue. Omita `--documentation-language` para ingles, use `--documentation-language es` para espanol o repita primero `en` y luego `es` para salida bilingue.

Los modos monolingues usan rutas canonicas como `README.md`, `CHANGELOG.md` y `docs/repo-harness/`. El modo bilingue conserva el ingles en rutas canonicas y agrega pares completos en espanol. Las habilidades, contratos, wrappers, verificadores, instrucciones y esquema del manifest, codigo, comentarios, instrucciones de runtime y artefactos tecnicos permanecen en ingles.

Cuando se declara un segundo locale, se genera completo, no parcial. Sus documentos son espejos humanos completos que presentan los mismos hechos y enlazan a los propietarios en ingles. Nunca se convierten en propietarios factuales independientes.

El idioma de conversacion es una eleccion aparte. Define el idioma en que las habilidades de sesion generadas le hablan a usted. No agrega ni quita locales de documentacion.

Los selectores reciprocos de README son una sola integracion atomica. Si el repositorio ya tiene `README.md` y el plan ejecutable exacto no puede actualizarlo, `README.es.md` se clasifica como bloqueado en vez de crearse. Nunca se produce un selector unilateral que afirme un repositorio bilingue.

### Actualizaciones seguras

Los harness anteriores se auditan, no se reemplazan a ciegas. Las rutas se clasifican como `current`, `stale`, `missing`, `human-evolved` o `legacy`; reemplazo, integracion, archivo y eliminacion requieren propuesta y aprobacion explicitas.

El `.gitignore` raiz sigue siendo propiedad humana. El plan aprobado crea el archivo si falta o agrega directivas exactas faltantes, preservando BOM, bytes, comentarios, lineas vacias, finales de linea y reglas ajenas. Incluye `.codegraph/` por defecto y siempre incluye `/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/` y `/.repo-harness/memory-bk/`. `--no-codegraph-ignore` registra la exclusion en el manifiesto y omite solo la directiva `.codegraph/`; las cuatro directivas de estado local permanecen y nunca se eliminan lineas de ignorado existentes.

El `.gitattributes` raiz tambien sigue siendo propiedad humana. El constructor crea o agrega una regla exacta `<path> text eol=lf` por cada ruta generada comparada byte a byte, preservando reglas ajenas y finales de linea existentes. Una regla coincidente que asigne comportamiento incompatible de texto o EOL bloquea el plan en vez de crear semantica ambigua de checkout.

En modo bilingue, los propietarios canonicos en ingles conservan la verdad operativa unica y los pares en espanol son presentaciones completas. Consulte el [contrato de mantenimiento bilingue](docs/es/repo-harness/i18n-maintenance.md), que aplica solo cuando se declaran ambos locales.

## Desarrollo
<!-- doc-role:development -->

Las fuentes editables son `skill/repo-harness-builder/` y `command/repo-harness-builder.md`. `.claude/`, `.opencode/` y `.pi/` son proyecciones de ejecucion y prueba, no fuentes canonicas. Este repositorio usa su propio harness para mejorar el constructor. Tras editar la fuente canonica, sincronice las salidas administradas afectadas en `.repo-harness/`, `.claude/`, `.opencode/` y `.pi/` mediante preview de mantenimiento y apply con el digest vigente, y verifique la paridad en disco. La validacion por nombre registrado puede requerir un proceso nuevo o una recarga explicita soportada por el entorno; una cache anterior del proceso no invalida los archivos verificados y no debe suponerse un comando de recarga.

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
python .repo-harness/check.py
```

## Licencia
<!-- doc-role:license -->

Licenciado bajo la [licencia MIT](LICENSE).
