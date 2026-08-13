# repo-harness-builder

<!-- README-I18N:START -->

[English](./README.md) | **Español**

<!-- README-I18N:END -->

`repo-harness-builder` proporciona a un repositorio un sistema operativo pequeno y durable para trabajo asistido por IA. Mantiene visibles las reglas, vigente el estado del proyecto, recuperable el contexto entre sesiones y verificables las transferencias, sin quitar propiedad a quienes mantienen el repositorio.

## Inicio rapido
<!-- doc-role:quick-start -->

1. Proyecte la habilidad y el comando del constructor en un entorno compatible.
2. Abra el repositorio objetivo e invoque `/repo-harness-builder`.
3. Seleccione proyecciones y licencia.
4. Revise el plan exacto. En espanol, la aprobacion estricta usa `APROBAR` o `DENEGAR`.
5. Inicie el trabajo normal con la habilidad generada `<proyecto>-session-start`.

Sin operacion explicita, el constructor realiza una auditoria de solo lectura. No inicializa Git ni modifica remotos, crea commits, publica o envia cambios.

## Que genera
<!-- doc-role:outputs -->

- `REPO-HARNESS.md` como entrada operativa canonica y `REPO-HARNESS.es.md` como presentacion completa en espanol.
- `README.md` y `README.es.md` con selectores reciprocos.
- Documentacion durable en ingles bajo `docs/repo-harness/` y pares en espanol bajo `docs/es/repo-harness/`.
- Habilidades neutrales bajo `.repo-harness/skills/` y proyecciones para Claude Code, OpenCode o Pi.
- `.repo-harness/check.py` para comprobaciones de congruencia sin escritura.

| Parte | Aporte |
|---|---|
| `REPO-HARNESS.md` | Libro de reglas conciso que siempre se lee: proposito, reglas obligatorias y rutas hacia detalles. |
| `docs/repo-harness/` | Propietarios durables de uso, estado actual y decisiones. |
| `CHANGELOG.md` | Registro fechado de cambios completados, separado del estado y trabajo futuro. |
| Continuidad en Engram | Memoria temporal entre sesiones en modo integrado, reconciliada siempre con evidencia del repositorio. |
| Proyecciones | Superficies delgadas para Claude Code, OpenCode o Pi generadas desde habilidades canonicas neutrales. |
| `.repo-harness/check.py` | Comprobacion de solo lectura que detecta proyecciones faltantes u obsoletas. |

## Constructor y habilidades generadas
<!-- doc-role:skills -->

El constructor instala o actualiza. Las tres habilidades del repositorio forman el flujo operativo diario.

| Habilidad | Cuando usarla | Resultado |
|---|---|---|
| `repo-harness-builder` | Crear, auditar o actualizar con seguridad un harness | Un harness planificado, aprobado y verificado |
| `<proyecto>-session-start` | Iniciar o reanudar trabajo | Resultado acotado, evidencia actual y estado Git local |
| `<proyecto>-documentation` | Cambiaron hechos durables o estado visible | Documentos canonicos actualizados sin duplicacion |
| `<proyecto>-session-stop` | Pausar o terminar | Continuidad verificada y unidades de trabajo revisables |

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

Cargue my-project-session-start y continue la migracion de API.
# Trabaje, pruebe y verifique.

Cargue my-project-documentation y actualice el estado y el historial.

Cargue my-project-session-stop y prepare la transferencia.
```

Los nombres de comandos dependen del entorno. La ayuda generada lee `docs/es/repo-harness/usage.md` cuando el idioma resuelto es espanol. Los resultados son `ready`, `paused`, `blocked`, `failed`, `completed` y `degraded`.

## Comportamiento del repositorio
<!-- doc-role:repository-behavior -->

### Repositorios nuevos

Los repositorios nuevos reciben una base bilingue completa despues de aprobar el plan. Los repositorios existentes reciben analisis y una propuesta de integracion; nunca se sobrescriben documentos evolucionados por personas solo para normalizar su estructura.

### Repositorios existentes

Si ya existe `README.md` y falta su par, el plan debe incluir integracion reciproca ejecutable o bloquear la creacion de `README.es.md`. No se permite afirmar un estado bilingue completo con un selector unilateral.

### Actualizaciones seguras

Los harness anteriores se auditan, no se reemplazan a ciegas. Las rutas se clasifican como `current`, `stale`, `missing`, `human-evolved` o `legacy`; reemplazo, integracion, archivo y eliminacion requieren propuesta y aprobacion explicitas.

`REPO-HARNESS.md`, `CHANGELOG.md` y los propietarios en ingles del estado durable conservan la verdad operativa unica. Los pares en espanol son presentaciones completas y enlazan a esas fuentes. Consulte el [contrato de mantenimiento bilingue](docs/es/repo-harness/i18n-maintenance.md).

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
