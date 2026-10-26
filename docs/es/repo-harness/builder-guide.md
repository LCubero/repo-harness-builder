# Guía del constructor

Presentación completa en español de la [guía canónica del constructor](../../repo-harness/builder-guide.md). Para instalar, comience con el [README](../../../README.es.md); para el trabajo diario, consulte la [guía de uso](usage.md).

## Posicionamiento del producto
<!-- doc-role:positioning -->

El harness conserva su utilidad por sí solo. El modo integrado predeterminado compone de forma independiente las capacidades disponibles de Gentle AI y Engram, mientras CodeGraph sigue siendo opcional. Una capacidad no disponible degrada solo el comportamiento que posee, salvo que la operación seleccionada o la política del repositorio la exija. El modo `--standalone` explícito usa continuidad solo del repositorio.

La detección no exige una instalación global: las herramientas nativas de Engram en Pi pueden aportar evidencia con `surface: "pi-tools"` después de una respuesta pertinente satisfactoria. La detección de Gentle AI solo consulta PATH; «no encontrado en PATH» no demuestra que falte la integración privada del host. Gentle Shell, por ejemplo, mantiene a propósito su copia privada de Gentle AI fuera del PATH del shell del agente; los informes no deben afirmar que su revisión o coordinación no están disponibles. No hace falta buscar carpetas privadas ni reparar instalaciones. El diagnóstico heredado `applyAllowed` no es una condición para instalar; los avisos de capacidades opcionales no invalidan una instalación correcta del harness.

| Componente | Límite |
|---|---|
| `repo-harness` | Guía operativa acotada al repositorio y verdad durable del proyecto para agentes de programación con IA compatibles. |
| Gentle AI | Un ecosistema más amplio para agentes de IA que puede proporcionar enrutamiento, revisión del candidato exacto, recibos y corrección acotada de la revisión; la política del repositorio es responsable de commits, pushes, PR y releases. |
| Engram | Memoria semántica y continuidad entre sesiones. Nunca reemplaza la verdad del repositorio. |
| CodeGraph | El componente más opcional: una ayuda local al repositorio para ubicar archivos, símbolos, relaciones e impacto. No es un almacen de autoridad. |

Hermes Agent, OpenClaw y Pi pueden aportar a la investigación, pero recrearlos o copiarlos no es un objetivo del producto. `repo-harness` tiene límites distintos y acotados al repositorio.

## Inicio rápido
<!-- doc-role:quick-start -->

Después de [instalar el constructor](../../../README.es.md#instalación):

1. Abra el repositorio objetivo e invoque la skill: Claude Code `/repo-harness-builder`, Pi `/skill:repo-harness-builder`, o pida a OpenCode que use la skill `repo-harness-builder` mediante su herramienta `skill`.
2. Responda la entrevista: proyecciones de entorno, licencia, idioma de conversación y locales de documentación. La documentación queda solo en inglés salvo que declare otro locale.
3. Mantenga el ignore predeterminado de `.codegraph/` en la raíz o use `--no-codegraph-ignore` para registrar el opt-out.
4. Revise el plan exacto y use la aprobación vinculada al digest y al idioma: `APPROVE` o `DENY` en inglés, `APROBAR` o `DENEGAR` en español.
5. Inicie el trabajo normal con la habilidad generada `repo-harness-session-start`.

Al actualizar, el constructor reutiliza los idiomas registrados en el manifiesto instalado en lugar de volver a preguntarlos, y cuando el plan no tiene nada que crear, anexar, reemplazar ni migrar, no pide aprobación, y solo informa que el harness esta actualizado si nada queda bloqueado. Sus consultas Git de solo lectura forman parte de la auditoría y no requieren permiso aparte.

Una solicitud explícita de inspección/check termina después de la auditoría de solo lectura. La activación normal de la skill continúa con la entrevista y el plan, y espera aprobación antes de escribir. No exige Git, remoto ni publicación; esas acciones requieren autorización aparte.

El constructor posee la auditoría, vista previa, aprobación y aplicación. Después de instalar o reemplazar una skill, use `/reload` en Pi o inicie una sesión nueva; en otros entornos, use su recarga soportada o una sesión nueva. Después de aplicar, ejecute `python3 .repo-harness/check.py` (`python` en Windows) en el objetivo: solo la salida 0 con `"status": "current"` y `"writes": 0` indica que el harness es congruente. El trabajo diario sigue el [Flujo diario](usage.md#flujo-diario).

Deténgase e inspeccione en vez de forzar cuando el artefacto sea inválido, un destino este ocupado, el entorno no liste la habilidad, el constructor rechace o el checker informe deriva. Con el artefacto y los requisitos ya locales, no se necesita red.

## Que genera
<!-- doc-role:outputs -->

- `REPO-HARNESS.md` en la raíz como entrada operativa concisa.
- `README.md` en la raíz cuando falta, con una sección acotada administrada por el constructor.
- Documentación durable completa en los idiomas seleccionados, en las rutas descritas en [Idioma de la documentación](#idioma-de-la-documentacion).
- Habilidades de sesión y documentación neutrales bajo `.repo-harness/skills/`, más pares canónicos de comandos bajo `.repo-harness/commands/` y `.repo-harness/prompts/` para las familias de superficies emitidas.
- Proyecciones seleccionadas para Claude Code, OpenCode o Pi.
- `.repo-harness/check.py` para comprobaciones ordinarias de deriva sin escritura (`writes: 0`); solo `repair --apply --approved-plan-sha256 <digest>` explícito puede crear proyecciones locales faltantes.
- Directivas en `.gitignore` raíz: `.codegraph/` salvo opt-out explícito, más las cuatro directivas de estado local del harness (`/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/`, `/.repo-harness/memory-bk/`).
- `.gitattributes` raíz con protección LF exacta para cada ruta generada comparada byte a byte.

Un harness es solo en inglés salvo que se declaren más locales de documentación. Consulte [Idioma de la documentación](#idioma-de-la-documentacion).

| Parte | Aporte |
|---|---|
| `REPO-HARNESS.md` | Libro de reglas conciso que siempre se lee: propósito, reglas obligatorias y rutas hacía detalles. |
| `docs/repo-harness/` | Propietarios durables de uso, estado actual y decisiones, para que cada hecho tenga un solo hogar. |
| `CHANGELOG.md` | Registro fechado de cambios completados, separado del estado y trabajo futuro. |
| Continuidad de sesión | Backups locales estructurados obligatorios bajo `.repo-harness/memory-bk/`, más continuidad Engram opcional cuando está disponible; el repositorio conserva la autoridad. |
| Proyecciones | Superficies delgadas para Claude Code, OpenCode o Pi generadas desde habilidades canónicas y pares de comandos por familia. |
| `.repo-harness/check.py` | Verificación de solo lectura de las copias generadas; consulte el comando anterior. |

## Constructor y habilidades generadas
<!-- doc-role:skills -->

El constructor instala o actualiza. Las tres habilidades del repositorio forman el flujo operativo diario después de la instalación.

| Habilidad | Cuando usarla | Resultado |
|---|---|---|
| `repo-harness-builder` | Crear, auditar o actualizar con seguridad un harness | Un harness planificado, aprobado y verificado |
| `repo-harness-session-start` | Iniciar o reanudar trabajo | Evidencia del repositorio primero, backup local válido más nuevo, contexto Engram acotado opcional y reconciliación visible |
| `repo-harness-documentation` | Cambiaron hechos durables o estado visible | Documentos canónicos actualizados sin duplicación ni propiedad obsoleta |
| `repo-harness-session-stop` | Pausar o terminar | Backup local exclusivo verificado, resultado Engram independiente, unidades de trabajo revisables y cualquier pregunta Git requerida |

## Ciclo de vida
<!-- doc-role:lifecycle -->

```text
Primera ejecucion del constructor
  -> auditoría de solo lectura
  -> elecciones de entorno, integración y licencia
  -> plan exacto y digest
  -> aprobación explicita para aplicar
  -> harness generado y verificado

Sesion normal
  -> session-start lee evidencia del repositorio, luego backup local y después Engram opcional
  -> el trabajo sigue REPO-HARNESS.md
  -> documentation actualiza propietarios durables cuando hace falta
  -> session-stop verifica y escribe la transferencia local primero
  -> el siguiente session-start continua desde los archivos actuales y la continuidad reconciliada
```

Ambos modos usan el backup local nativo. El modo integrado compone de forma independiente las capacidades disponibles de Gentle AI y Engram. Engram conecta sesiones cuando está disponible, pero nunca reemplaza los archivos actuales; standalone no usa componentes externos de continuidad.

## Flujo de ejemplo
<!-- doc-role:example -->

```text
# Claude Code: /repo-harness-builder; Pi: /skill:repo-harness-builder
# OpenCode: pida al agente que use la skill repo-harness-builder.
# Revise la auditoria, responda una eleccion a la vez, inspeccione el plan y apruebe.

Cargue repo-harness-session-start y continue la migración de API.
# Trabaje, pruebe y verifique.

Cargue repo-harness-documentation y actualice el estado y el historial.

Cargue repo-harness-session-stop y prepare la transferencia.
```

El constructor se invoca directamente como skill, sin un wrapper separado de comando o prompt; las habilidades diarias conservan sus entradas generadas y los nombres registrados fijos mostrados arriba. La sintaxis de comandos depende del entorno, así que las habilidades se invocan mediante el mecanismo de habilidades del entorno. La ayuda generada lee `docs/repo-harness/usage.md`, y lee `docs/es/repo-harness/usage.md` solo cuando el español es un locale de documentación declarado y el idioma de conversación es español. Nunca nombra una guía que ese mismo render no creó.

Los resultados de sesión son `ready`, `paused`, `blocked`, `failed`, `completed` y `degraded`. Tienen alcance acotado: un fallo Git opcional puede degradar una sesión local exitosa sin declarar falsamente fallida la continuidad. El consentimiento explícito `commit and push` cubre la secuencia segura sin cambios hasta un push; consentir solo commit nunca implica push, y los cambios materiales requieren renovar el consentimiento.

## Comportamiento del repositorio
<!-- doc-role:repository-behavior -->

### Repositorios nuevos

Un repositorio nuevo o prácticamente vacío recibe una base útil completa después de aprobar el plan: README raíz, libro de reglas, documentación durable, habilidades diarias, proyecciones seleccionadas, manifiesto y checker.

### Repositorios existentes

El `check` del constructor no exige que un README de producto preservado adopte la plantilla del harness. Los README raíz se validan contra ese contrato de sección solo cuando declaran el rol `repository-harness` o alguno de sus marcadores de inicio o fin; un selector de idioma bilingüe no cuenta. Las secciones declaradas del harness y los demás documentos siguen validándose. Si se eliminan todas las declaraciones, se pierde esa evidencia; no se infiere un historial de instalación.

El constructor analiza el contenido existente relevante y propone una integración informada. Preserva bytes desconocidos y nunca entiende la preservación como congelar el repositorio. Si el contenido está obsoleto o es estructuralmente incompatible, archivar y partir de una base limpia es una elección distinta que igual requiere aprobación explícita.

Un repositorio existente recibe un locale de documentación nuevo solo cuando el usuario pide explícitamente ese cambio. Un harness heredado cuyo manifiesto es anterior a los metadatos de idioma conserva los árboles de locale realmente instalados, de modo que un repositorio bilingüe antiguo no pierde en silencio su comprobación en español y un repositorio solo en inglés no gana en silencio archivos en español.

### Idioma de la documentacion

La documentación humana del repositorio puede quedar solo en inglés, solo en español o bilingüe. Omita `--documentation-language` para inglés, use `--documentation-language es` para español o repita primero `en` y luego `es` para salida bilingüe.

Los modos monolingües usan rutas canónicas como `README.md`, `CHANGELOG.md` y `docs/repo-harness/`. El modo bilingüe conserva el inglés en rutas canónicas y agrega pares completos en español. Las habilidades, contratos, wrappers, verificadores, instrucciones y esquema del manifest, código, comentarios, instrucciones de runtime y artefactos técnicos permanecen en inglés en todos los modos.

Cuando se declara un segundo locale, se genera completo, no parcial. Sus documentos son espejos humanos completos que presentan los mismos hechos y enlazan a los propietarios en inglés. Nunca se convierten en propietarios factuales independientes.

El idioma de conversación es independiente de los locales de documentación. Define el idioma en que las skills de sesión generadas le hablan a usted. Cuando no puede resolver el idioma mediante elecciones explícitas o registradas, el constructor pregunta una sola vez en inglés y español. Esa respuesta fija el idioma del informe y la conversación; toda interacción posterior usa el idioma elegido sin volver a preguntarlo. Al actualizar, reutiliza los idiomas registrados salvo que usted pida cambiarlos.

El texto de aprobación se transmite exactamente desde el aviso localizado de la CLI y los conteos del plan salen de la herramienta. Un plan ejecutable vacío no necesita aprobación, tabla de acciones vacía ni digest; las entradas bloqueadas siguen impidiendo afirmar que el harness está actualizado. Una guía de contribución que coincide con la plantilla breve del constructor no recibe una sugerencia de revisión por posible desactualización.

Puede conservar a mano el idioma de conversación y los modos de respuesta ADHD-friendly o plain-language en un [archivo local de preferencias](usage.md#preferencias-de-respuesta), o pedir explícitamente al agente que los guarde; nunca guarda preferencias por iniciativa propia.

Los selectores recíprocos de README son una sola integración atómica. Si el repositorio ya tiene `README.md` y el plan ejecutable exacto no puede actualizarlo, `README.es.md` se clasifica como bloqueado en vez de crearse. Nunca se produce un selector unilateral que afirme un repositorio bilingüe.

### Actualizaciones seguras

Actualizar la skill del constructor no actualiza el harness instalado. Descargue el nuevo archivo de su entorno, revise la carpeta existente del constructor y apruebe reemplazar solo esa skill. Recargue el entorno como se indica arriba, invoque `repo-harness-builder` y pida actualizar el harness de este repositorio. Revise explícitamente los entornos seleccionados; no los infiera de las herramientas instaladas. Apruebe el plan resultante antes de aplicar.

Los harness anteriores se auditan, no se reemplazan a ciegas. Las rutas administradas se clasifican como `current`, `stale`, `missing`, `human-evolved` o `legacy`; reemplazo, integración, archivo y eliminación siguen siendo propuestas hasta recibir aprobación explícita. Una actualización reemplaza las skills generadas pero conserva los archivos humanos, así que `human-evolved` puede significar una edición humana o texto que generó una versión anterior; tras actualizar, el agente señala esos archivos como posiblemente desactualizados. Puede necesitarse una comparación aprobada con un render nuevo; preservarlos no certifica que sus instrucciones estén al día.

El README es propiedad humana después de su creación. Su sección acotada del constructor identifica las entradas iniciales del harness, mientras quienes mantienen el repositorio pueden evolucionar la guía completa del producto. Los archivos existentes `CLAUDE.md` y `AGENTS.md` conservan sus bytes: el constructor solo agrega una línea exacta de enlace faltante y las ejecuciones repetidas son idempotentes.

El `.gitignore` raíz sigue siendo propiedad humana. El plan aprobado crea el archivo si falta o agrega directivas exactas faltantes, preservando BOM, bytes, comentarios, líneas vacías, finales de línea y reglas ajenas. Incluye `.codegraph/` por defecto y siempre incluye `/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/` y `/.repo-harness/memory-bk/`. `--no-codegraph-ignore` registra la exclusión en el manifiesto y omite solo la directiva `.codegraph/`; las cuatro directivas de estado local permanecen y nunca se eliminan líneas de ignorado existentes. Cuando las reglas existentes ignorarían rutas planificadas del harness (a menudo `.claude/`, `.opencode/`, `.pi/`, `AGENTS.md` o `CLAUDE.md`), el agente pregunta antes de aprobar si mantenerlas locales (por defecto) o versionarlas, y edita `.gitignore` solo tras aprobación.

El `.gitattributes` raíz también sigue siendo propiedad humana. El constructor crea o agrega una regla exacta `<path> text eol=lf` por cada ruta generada comparada byte a byte, preservando reglas ajenas y finales de línea existentes. Una regla coincidente que asigne comportamiento incompatible de texto o EOL bloquea el plan en vez de crear semántica ambigua de checkout.

Los hechos operativos tienen un solo propietario. `REPO-HARNESS.md`, `CHANGELOG.md` y los propietarios durables de estado en inglés son canónicos. Cuando se declara otro locale, sus pares son presentaciones completas que enlazan a esos propietarios. Consulte el [contrato de mantenimiento bilingüe](i18n-maintenance.md), que aplica solo cuando se declara más de un locale.

## Desarrollo
<!-- doc-role:development -->

Las fuentes editables son:

- `skill/repo-harness-builder/`

`.claude/`, `.opencode/` y `.pi/` son proyecciones de ejecución y prueba, no fuentes canónicas. Este repositorio usa su propio harness para mejorar el constructor que genera harness de repositorio. Consulte [CONTRIBUTING.es.md](../../../CONTRIBUTING.es.md) para el flujo de contribución.

Tras editar la fuente canónica, sincronice las salidas administradas afectadas en `.repo-harness/`, `.claude/`, `.opencode/` y `.pi/` mediante preview de mantenimiento y apply con el digest vigente, y verifique la paridad en disco. La validación por nombre registrado puede requerir un proceso nuevo o una recarga explícita soportada por el entorno; una caché anterior del proceso no inválida los archivos verificados y no debe suponerse un comando de recarga.

Ejecute la suite canónica desde la raíz del repositorio:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

Compruebe las proyecciones del repositorio desde su raíz:

```powershell
python .repo-harness/check.py
```

Para la retirada manual, consulte [Retirar el harness manualmente](usage.md#retirar-el-harness-manualmente). La ayuda existente lee esa guía sin eliminar nada. Las instalaciones nuevas reciben la sección; las guías de uso existentes de propiedad humana requieren una edición documental aprobada explícitamente, no un reemplazo automático.
