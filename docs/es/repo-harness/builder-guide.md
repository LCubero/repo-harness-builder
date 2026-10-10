# Guia del constructor

Presentacion completa en espanol de la [guia canonica del constructor](../../repo-harness/builder-guide.md). Para instalar, comience con el [README](../../../README.es.md); para el trabajo diario, consulte la [guia de uso](usage.md).

## Posicionamiento del producto
<!-- doc-role:positioning -->

El harness conserva su utilidad por si solo. El modo integrado predeterminado compone de forma independiente las capacidades disponibles de Gentle AI y Engram, mientras CodeGraph sigue siendo opcional. Una capacidad no disponible degrada solo el comportamiento que posee, salvo que la operacion seleccionada o la politica del repositorio la exija. El modo `--standalone` explicito usa continuidad solo del repositorio.

| Componente | Limite |
|---|---|
| `repo-harness` | Guia operativa acotada al repositorio y verdad durable del proyecto para agentes de programacion con IA compatibles. |
| Gentle AI | Un ecosistema mas amplio para agentes de IA que puede proporcionar enrutamiento, revision del candidato exacto, recibos y correccion acotada de la revision; la politica del repositorio es responsable de commits, pushes, PR y releases. |
| Engram | Memoria semantica y continuidad entre sesiones. Nunca reemplaza la verdad del repositorio. |
| CodeGraph | El componente mas opcional: una ayuda local al repositorio para ubicar archivos, simbolos, relaciones e impacto. No es un almacen de autoridad. |

Hermes Agent, OpenClaw y Pi pueden aportar a la investigacion, pero recrearlos o copiarlos no es un objetivo del producto. `repo-harness` tiene limites distintos y acotados al repositorio.

## Inicio rapido
<!-- doc-role:quick-start -->

Despues de [instalar el constructor](../../../README.es.md#instalacion):

1. Abra el repositorio objetivo e invoque `/repo-harness-builder`.
2. Responda la entrevista: proyecciones de entorno, licencia, idioma de conversacion y locales de documentacion. La documentacion queda solo en ingles salvo que declare otro locale.
3. Mantenga el ignore predeterminado de `.codegraph/` en la raiz o use `--no-codegraph-ignore` para registrar el opt-out.
4. Revise el plan exacto y use la aprobacion vinculada al digest y al idioma: `APPROVE` o `DENY` en ingles, `APROBAR` o `DENEGAR` en espanol.
5. Inicie el trabajo normal con la habilidad generada `repo-harness-session-start`.

Sin operacion explicita, el constructor realiza una auditoria de solo lectura. No inicializa Git ni modifica remotos, crea commits, envia cambios o publica, salvo que esas acciones separadas se soliciten y aprueben despues.

El constructor posee la auditoria, vista previa, aprobacion y aplicacion. Si el entorno no lista la habilidad nueva, inicie una sesion nueva del entorno. Despues de aplicar, ejecute `python3 .repo-harness/check.py` (`python` en Windows) en el objetivo: solo la salida 0 con `"status": "current"` y `"writes": 0` indica que el harness es congruente. El trabajo diario sigue el [Flujo diario](usage.md#flujo-diario).

Detengase e inspeccione en vez de forzar cuando el artefacto sea invalido, un destino este ocupado, el entorno no liste la habilidad, el constructor rechace o el checker informe deriva. Con el artefacto y los requisitos ya locales, no se necesita red.

## Que genera
<!-- doc-role:outputs -->

- `REPO-HARNESS.md` en la raiz como entrada operativa concisa.
- `README.md` en la raiz cuando falta, con una seccion acotada administrada por el constructor.
- Documentacion durable completa en ingles bajo `docs/repo-harness/`.
- Habilidades de sesion y documentacion neutrales bajo `.repo-harness/skills/`, mas pares canonicos de comandos bajo `.repo-harness/commands/` y `.repo-harness/prompts/` para las familias de superficies emitidas.
- Proyecciones seleccionadas para Claude Code, OpenCode o Pi.
- `.repo-harness/check.py` para comprobaciones ordinarias de deriva sin escritura (`writes: 0`); solo `repair --apply --approved-plan-sha256 <digest>` explicito puede crear proyecciones locales faltantes.
- Directivas en `.gitignore` raiz: `.codegraph/` salvo opt-out explicito, mas las cuatro directivas de estado local del harness (`/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/`, `/.repo-harness/memory-bk/`).
- `.gitattributes` raiz con proteccion LF exacta para cada ruta generada comparada byte a byte.

Un harness es solo en ingles salvo que se declaren mas locales de documentacion. Consulte [Idioma de la documentacion](#idioma-de-la-documentacion).

| Parte | Aporte |
|---|---|
| `REPO-HARNESS.md` | Libro de reglas conciso que siempre se lee: proposito, reglas obligatorias y rutas hacia detalles. |
| `docs/repo-harness/` | Propietarios durables de uso, estado actual y decisiones, para que cada hecho tenga un solo hogar. |
| `CHANGELOG.md` | Registro fechado de cambios completados, separado del estado y trabajo futuro. |
| Continuidad de sesion | Backups locales estructurados obligatorios bajo `.repo-harness/memory-bk/`, mas continuidad Engram opcional cuando esta disponible; el repositorio conserva la autoridad. |
| Proyecciones | Superficies delgadas para Claude Code, OpenCode o Pi generadas desde habilidades canonicas y pares de comandos por familia. |
| `.repo-harness/check.py` | Comprobacion ordinaria de solo lectura con `writes: 0`; solo `repair --apply --approved-plan-sha256 <digest>` explicito puede crear proyecciones locales faltantes. |

## Constructor y habilidades generadas
<!-- doc-role:skills -->

El constructor instala o actualiza. Las tres habilidades del repositorio forman el flujo operativo diario despues de la instalacion.

| Habilidad | Cuando usarla | Resultado |
|---|---|---|
| `repo-harness-builder` | Crear, auditar o actualizar con seguridad un harness | Un harness planificado, aprobado y verificado |
| `repo-harness-session-start` | Iniciar o reanudar trabajo | Evidencia del repositorio primero, backup local valido mas nuevo, contexto Engram acotado opcional y reconciliacion visible |
| `repo-harness-documentation` | Cambiaron hechos durables o estado visible | Documentos canonicos actualizados sin duplicacion ni propiedad obsoleta |
| `repo-harness-session-stop` | Pausar o terminar | Backup local exclusivo verificado, resultado Engram independiente, unidades de trabajo revisables y cualquier pregunta Git requerida |

## Ciclo de vida
<!-- doc-role:lifecycle -->

```text
Primera ejecucion del constructor
  -> auditoria de solo lectura
  -> elecciones de entorno, integracion y licencia
  -> plan exacto y digest
  -> aprobacion explicita para aplicar
  -> harness generado y verificado

Sesion normal
  -> session-start lee evidencia del repositorio, luego backup local y despues Engram opcional
  -> el trabajo sigue REPO-HARNESS.md
  -> documentation actualiza propietarios durables cuando hace falta
  -> session-stop verifica y escribe la transferencia local primero
  -> el siguiente session-start continua desde los archivos actuales y la continuidad reconciliada
```

Ambos modos usan el backup local nativo. El modo integrado compone de forma independiente las capacidades disponibles de Gentle AI y Engram. Engram conecta sesiones cuando esta disponible, pero nunca reemplaza los archivos actuales; standalone no usa componentes externos de continuidad.

## Flujo de ejemplo
<!-- doc-role:example -->

```text
/repo-harness-builder
# Revise la auditoria, responda una eleccion a la vez, inspeccione el plan y apruebe.

Cargue repo-harness-session-start y continue la migracion de API.
# Trabaje, pruebe y verifique.

Cargue repo-harness-documentation y actualice el estado y el historial.

Cargue repo-harness-session-stop y prepare la transferencia.
```

El comando del constructor sigue siendo `/repo-harness-builder`; las habilidades diarias usan los nombres registrados fijos mostrados arriba. La sintaxis de comandos depende del entorno, asi que las habilidades se invocan mediante el mecanismo de habilidades del entorno. La ayuda generada lee `docs/repo-harness/usage.md`, y lee `docs/es/repo-harness/usage.md` solo cuando el espanol es un locale de documentacion declarado y el idioma de conversacion es espanol. Nunca nombra una guia que ese mismo render no creo.

Los resultados de sesion son `ready`, `paused`, `blocked`, `failed`, `completed` y `degraded`. Tienen alcance acotado: un fallo Git opcional puede degradar una sesion local exitosa sin declarar falsamente fallida la continuidad. El consentimiento explicito `commit and push` cubre la secuencia segura sin cambios hasta un push; consentir solo commit nunca implica push, y los cambios materiales requieren renovar el consentimiento.

## Comportamiento del repositorio
<!-- doc-role:repository-behavior -->

### Repositorios nuevos

Un repositorio nuevo o practicamente vacio recibe una base util completa despues de aprobar el plan: README raiz, libro de reglas, documentacion durable, habilidades diarias, proyecciones seleccionadas, manifiesto y checker.

### Repositorios existentes

El constructor analiza el contenido existente relevante y propone una integracion informada. Preserva bytes desconocidos y nunca entiende la preservacion como congelar el repositorio. Si el contenido esta obsoleto o es estructuralmente incompatible, archivar y partir de una base limpia es una eleccion distinta que igual requiere aprobacion explicita.

Un repositorio existente nunca recibe un locale de documentacion que no tenga ya. Un harness heredado cuyo manifiesto es anterior a los metadatos de idioma conserva los arboles de locale realmente instalados, de modo que un repositorio bilingue antiguo no pierde en silencio su comprobacion en espanol y un repositorio solo en ingles no gana en silencio archivos en espanol.

### Idioma de la documentacion

La documentacion humana del repositorio puede quedar solo en ingles, solo en espanol o bilingue. Omita `--documentation-language` para ingles, use `--documentation-language es` para espanol o repita primero `en` y luego `es` para salida bilingue.

Los modos monolingues usan rutas canonicas como `README.md`, `CHANGELOG.md` y `docs/repo-harness/`. El modo bilingue conserva el ingles en rutas canonicas y agrega pares completos en espanol. Las habilidades, contratos, wrappers, verificadores, instrucciones y esquema del manifest, codigo, comentarios, instrucciones de runtime y artefactos tecnicos permanecen en ingles en todos los modos.

Cuando se declara un segundo locale, se genera completo, no parcial. Sus documentos son espejos humanos completos que presentan los mismos hechos y enlazan a los propietarios en ingles. Nunca se convierten en propietarios factuales independientes.

El idioma de conversacion es una eleccion aparte. Define el idioma en que las habilidades de sesion generadas le hablan a usted. No agrega ni quita locales de documentacion.

Puede conservar a mano el idioma de conversacion y los modos de respuesta ADHD-friendly o plain-language en un [archivo local de preferencias](usage.md#preferencias-de-respuesta).

Los selectores reciprocos de README son una sola integracion atomica. Si el repositorio ya tiene `README.md` y el plan ejecutable exacto no puede actualizarlo, `README.es.md` se clasifica como bloqueado en vez de crearse. Nunca se produce un selector unilateral que afirme un repositorio bilingue.

### Actualizaciones seguras

Los harness anteriores se auditan, no se reemplazan a ciegas. Las rutas administradas se clasifican como `current`, `stale`, `missing`, `human-evolved` o `legacy`; reemplazo, integracion, archivo y eliminacion siguen siendo propuestas hasta recibir aprobacion explicita.

El README es propiedad humana despues de su creacion. Su seccion acotada del constructor identifica las entradas iniciales del harness, mientras quienes mantienen el repositorio pueden evolucionar la guia completa del producto. Los archivos existentes `CLAUDE.md` y `AGENTS.md` conservan sus bytes: el constructor solo agrega una linea exacta de enlace faltante y las ejecuciones repetidas son idempotentes.

El `.gitignore` raiz sigue siendo propiedad humana. El plan aprobado crea el archivo si falta o agrega directivas exactas faltantes, preservando BOM, bytes, comentarios, lineas vacias, finales de linea y reglas ajenas. Incluye `.codegraph/` por defecto y siempre incluye `/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/` y `/.repo-harness/memory-bk/`. `--no-codegraph-ignore` registra la exclusion en el manifiesto y omite solo la directiva `.codegraph/`; las cuatro directivas de estado local permanecen y nunca se eliminan lineas de ignorado existentes.

El `.gitattributes` raiz tambien sigue siendo propiedad humana. El constructor crea o agrega una regla exacta `<path> text eol=lf` por cada ruta generada comparada byte a byte, preservando reglas ajenas y finales de linea existentes. Una regla coincidente que asigne comportamiento incompatible de texto o EOL bloquea el plan en vez de crear semantica ambigua de checkout.

Los hechos operativos tienen un solo propietario. `REPO-HARNESS.md`, `CHANGELOG.md` y los propietarios durables de estado en ingles son canonicos. Cuando se declara otro locale, sus pares son presentaciones completas que enlazan a esos propietarios. Consulte el [contrato de mantenimiento bilingue](i18n-maintenance.md), que aplica solo cuando se declara mas de un locale.

## Desarrollo
<!-- doc-role:development -->

Las fuentes editables son:

- `skill/repo-harness-builder/`
- `command/repo-harness-builder.md`

`.claude/`, `.opencode/` y `.pi/` son proyecciones de ejecucion y prueba, no fuentes canonicas. Este repositorio usa su propio harness para mejorar el constructor que genera harness de repositorio. Consulte [CONTRIBUTING.es.md](../../../CONTRIBUTING.es.md) para el flujo de contribucion.

Tras editar la fuente canonica, sincronice las salidas administradas afectadas en `.repo-harness/`, `.claude/`, `.opencode/` y `.pi/` mediante preview de mantenimiento y apply con el digest vigente, y verifique la paridad en disco. La validacion por nombre registrado puede requerir un proceso nuevo o una recarga explicita soportada por el entorno; una cache anterior del proceso no invalida los archivos verificados y no debe suponerse un comando de recarga.

Ejecute la suite canonica desde la raiz del repositorio:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

Compruebe las proyecciones del repositorio desde su raiz:

```powershell
python .repo-harness/check.py
```
