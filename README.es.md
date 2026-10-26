# repo-harness-builder
<!-- doc-role:repository-harness -->

<!-- README-I18N:START -->

[English](./README.md) | **Español**

<!-- README-I18N:END -->

`repo-harness` es un conjunto de instrucciones, reglas, documentación, habilidades y comandos para agentes de programación con IA como Claude Code, OpenCode y Pi. Guarda las guías y notas de sesión en el repositorio para poder revisarlas.

**El constructor instala y actualiza; el harness acompaña el trabajo diario.** Ninguno exige Git ni un remoto: también funcionan en proyectos privados y solo locales.

## Qué genera
<!-- doc-role:outputs -->

- Un libro de reglas conciso en `REPO-HARNESS.md` y un README cuando falta.
- Documentación del proyecto sobre uso, estado actual, decisiones y cambios completados.
- Habilidades de sesión y documentación, con comandos para los entornos elegidos.
- Un verificador para mantener coherentes las copias de cada entorno y ajustes de archivos del repositorio.

El harness funciona por sí solo. El modo integrado compone de forma independiente las capacidades disponibles de Gentle AI y Engram; CodeGraph es opcional. Consulte la [guía del constructor](docs/es/repo-harness/builder-guide.md) para conocer los límites de integración y la lista completa de archivos generados.

## Instalación
<!-- doc-role:quick-start -->

1. Descargue el zip marcado **pi**, **claude-code** u **opencode** para su entorno de la [versión estable v3.1](https://github.com/LCubero/repo-harness-builder/releases/tag/v3.1).
2. Extráigalo en la raíz del repositorio donde lo utilizará. Crea la carpeta de skills de su entorno indicada abajo (`skills` en plural). Manténgala dentro del repositorio; nunca la instale en una carpeta global o de usuario.

   - **Claude Code:** `.claude/skills/repo-harness-builder/`
     Invoque `/repo-harness-builder`.
   - **OpenCode:** `.opencode/skills/repo-harness-builder/`
     Pida al agente que use la skill `repo-harness-builder`.
   - **Pi:** `.pi/skills/repo-harness-builder/`
     Invoque `/skill:repo-harness-builder`.

3. Abra ese repositorio en su entorno e invoque la skill una vez para preparar el harness. Responda sus preguntas y apruebe el plan antes de que haga cambios.

Requiere Python 3.10+ y su entorno ya instalado; si un destino ya existe, revíselo antes de extraer o sobrescribir.

## Uso diario
<!-- doc-role:skills -->

1. Inicie o reanude con `repo-harness-session-start`.
2. Trabaje normalmente: pida al agente que haga su tarea.
3. Cuando cambie información importante del proyecto, use `repo-harness-documentation` para mantener la documentación al día.
4. Al pausar o terminar, use `repo-harness-session-stop` para dejar contexto para la próxima sesión.

Pida al agente que use cada habilidad cuando la necesite. Las notas quedan locales en `.repo-harness/memory-bk/`; la memoria opcional de Engram nunca reemplaza los archivos del repositorio. Consulte la [guía de uso](docs/es/repo-harness/usage.md) para más detalles.

## Más información
<!-- doc-role:license -->

Puede ejecutar el constructor de nuevo para una revisión o actualización opcional; consulte su guía.

- [Entrada de instalación para agentes](llms.txt): instrucciones técnicas en inglés para ayudar a instalar el harness.
- [Guía del constructor](docs/es/repo-harness/builder-guide.md): archivos generados, idiomas, actualizaciones seguras y desarrollo.
- [Guía de uso](docs/es/repo-harness/usage.md): trabajo con el harness instalado.
- [CONTRIBUTING.es.md](CONTRIBUTING.es.md): contribuciones a este proyecto.
- [Historial de cambios](CHANGELOG.es.md): cambios completados.
- [Licencia MIT](LICENSE).

<a href="https://github.com/Gentleman-Programming/gentle-ai">
  <img width="220" src="https://raw.githubusercontent.com/Gentleman-Programming/gentle-ai/main/docs/assets/brand/built-with-gentle-ai.png" alt="Built with Gentle-AI" />
</a>
