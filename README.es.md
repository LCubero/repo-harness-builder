# repo-harness-builder
<!-- doc-role:repository-harness -->

<!-- README-I18N:START -->

[English](./README.md) | **Español**

<!-- README-I18N:END -->

`repo-harness-builder` proporciona a un repositorio un sistema operativo pequeno y durable para trabajo asistido por IA. Mantiene visibles las reglas, vigente el estado del proyecto, recuperable el contexto entre sesiones y verificables las transferencias, sin quitar propiedad a quienes mantienen el repositorio.

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
| Continuidad en Engram | Memoria temporal entre sesiones en modo integrado, reconciliada siempre con evidencia del repositorio. |
| Proyecciones | Superficies delgadas para Claude Code, OpenCode o Pi generadas desde habilidades canonicas y pares de comandos por familia. |
| `.repo-harness/check.py` | Comprobacion de solo lectura que detecta proyecciones de habilidades, comandos y prompts faltantes u obsoletas. |

## Constructor y habilidades generadas
<!-- doc-role:skills -->

El constructor instala o actualiza. Las tres habilidades del repositorio forman el flujo operativo diario.

| Habilidad | Cuando usarla | Resultado |
|---|---|---|
| `repo-harness-builder` | Crear, auditar o actualizar con seguridad un harness | Un harness planificado, aprobado y verificado |
| `repo-harness-session-start` | Iniciar o reanudar trabajo | Resultado acotado, evidencia actual etiquetada con su clase, estado Git local y una pregunta concreta de verificacion remota cuando es seguro |
| `repo-harness-documentation` | Cambiaron hechos durables o estado visible | Documentos canonicos actualizados sin duplicacion |
| `repo-harness-session-stop` | Pausar o terminar | Continuidad verificada y unidades de trabajo revisables |

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
  -> session-start recupera y reconcilia contexto
  -> el trabajo sigue REPO-HARNESS.md
  -> documentation actualiza propietarios durables
  -> session-stop verifica y registra la transferencia
```

En modo integrado, Gentle AI y Engram deben estar disponibles. Engram conecta sesiones, pero nunca reemplaza los archivos actuales. En modo independiente, la continuidad permanece solo en el repositorio.

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

El `.gitignore` raiz sigue siendo propiedad humana. El plan aprobado crea el archivo si falta o agrega exactamente `.codegraph/` si no existe esa linea, preservando BOM, bytes, comentarios, lineas vacias, finales de linea y reglas ajenas. `--no-codegraph-ignore` registra el opt-out en el manifiesto y no modifica el archivo.

El `.gitattributes` raiz tambien sigue siendo propiedad humana. El constructor crea o agrega una regla exacta `<path> text eol=lf` por cada ruta generada comparada byte a byte, preservando reglas ajenas y finales de linea existentes. Una regla coincidente que asigne comportamiento incompatible de texto o EOL bloquea el plan en vez de crear semantica ambigua de checkout.

En modo bilingue, los propietarios canonicos en ingles conservan la verdad operativa unica y los pares en espanol son presentaciones completas. Consulte el [contrato de mantenimiento bilingue](docs/es/repo-harness/i18n-maintenance.md), que aplica solo cuando se declaran ambos locales.

## Desarrollo
<!-- doc-role:development -->

Las fuentes editables son `skill/repo-harness-builder/` y `command/repo-harness-builder.md`. `.claude/` y `.opencode/` son proyecciones de ejecucion y prueba.

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
python .repo-harness/check.py
```

## Licencia
<!-- doc-role:license -->

Licenciado bajo la [licencia MIT](LICENSE).
