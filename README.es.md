# repo-harness-builder
<!-- doc-role:repository-harness -->

<!-- README-I18N:START -->

[English](./README.md) | **Español**

<!-- README-I18N:END -->

`repo-harness` es un conjunto de prompts, reglas, documentacion, habilidades y comandos acotado al repositorio para agentes de programacion con IA como Claude Code, OpenCode y Pi. Mantiene las guias del proyecto y las transferencias de sesion en el repositorio, donde quienes lo mantienen pueden revisarlas. `repo-harness-builder` crea, audita y evoluciona ese harness con seguridad.

## Que genera
<!-- doc-role:outputs -->

- Un libro de reglas conciso en `REPO-HARNESS.md` y un README cuando falta.
- Documentacion del proyecto sobre uso, estado actual, decisiones y cambios completados.
- Habilidades de sesion y documentacion, con comandos para los entornos elegidos.
- Un verificador para mantener coherentes las copias de cada entorno y ajustes de archivos del repositorio.

El harness funciona por si solo. El modo integrado compone de forma independiente las capacidades disponibles de Gentle AI y Engram; CodeGraph es opcional. Consulte la [guia del constructor](docs/es/repo-harness/builder-guide.md) para conocer los limites de integracion y la lista completa de archivos generados.

## Instalacion
<!-- doc-role:quick-start -->

1. Descargue y extraiga el zip de la [ultima release](https://github.com/LCubero/repo-harness-builder/releases).
2. Copie la carpeta de la habilidad (`skill/repo-harness-builder/`) y el archivo de comando (`command/repo-harness-builder.md`) al repositorio donde los usara, en los destinos siguientes. Mantenga ambos dentro de ese repositorio, nunca en una carpeta de configuracion global o de usuario.

   | Entorno | Carpeta de la habilidad | Archivo de comando |
   |---|---|---|
   | Claude Code | `.claude/skills/repo-harness-builder/` | `.claude/commands/repo-harness-builder.md` |
   | OpenCode | `.opencode/skills/repo-harness-builder/` | `.opencode/commands/repo-harness-builder.md` |
   | Pi | `.pi/skills/repo-harness-builder/` | `.pi/prompts/repo-harness-builder.md` |

3. Abra ese repositorio en su entorno, ejecute `/repo-harness-builder` y siga sus pasos.

Requiere Python 3.10+ y su entorno ya instalado; si un destino ya existe, inspeccionelo antes de copiar.

## Uso diario
<!-- doc-role:skills -->

Use `/repo-harness-builder` para crear, auditar o actualizar el harness. Una vez instalado, use `repo-harness-session-start` para iniciar o reanudar trabajo, `repo-harness-documentation` cuando cambien hechos durables del proyecto y `repo-harness-session-stop` para pausar o terminar con una transferencia. Invoque estas habilidades mediante el mecanismo de habilidades de su entorno. La [guia de uso](docs/es/repo-harness/usage.md) explica el flujo diario y los limites de aprobacion.

## Mas informacion
<!-- doc-role:license -->

- [Guia del constructor](docs/es/repo-harness/builder-guide.md): archivos generados, idiomas, actualizaciones seguras y desarrollo.
- [Guia de uso](docs/es/repo-harness/usage.md): trabajo con el harness instalado.
- [CONTRIBUTING.es.md](CONTRIBUTING.es.md): contribuciones a este proyecto.
- [Historial de cambios](CHANGELOG.es.md): cambios completados.
- [Licencia MIT](LICENSE).
