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

## Qué hace el harness
<!-- doc-role:features -->

- **Retoma donde quedó:** cada sesión parte de la evidencia del repositorio y del último traspaso local.
- **Mantiene la documentación al día:** un solo lugar para cada dato, con estado actual, decisiones e historial de cambios sincronizados.
- **Pregunta antes de actuar:** escrituras, commits, push y acceso a red requieren su aprobación; Git sigue siendo opcional.
- **Dice qué tan seguro está:** cada afirmación se marca como verificada, inferida, desconocida o pendiente de su decisión.
- **Habla su idioma:** respuestas en inglés o español, con documentación en inglés, español o bilingüe.
- **Actualiza con seguridad:** el constructor revisa un harness existente y conserva sus propias ediciones.

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

## Preferencias de respuesta
<!-- doc-role:response-preferences -->

Usted elige cómo responde el agente:

- **Idioma:** inglés o español.
- **Estructura amigable con TDAH:** la respuesta primero, bloques cortos, una pregunta a la vez.
- **Lenguaje sencillo:** palabras simples, frases cortas y términos técnicos explicados.

Pida un modo durante la sesión, o pida al agente que lo guarde para próximas sesiones. Nunca adivina ni guarda preferencias por su cuenta. Consulte las [preferencias de respuesta](docs/es/repo-harness/usage.md#preferencias-de-respuesta).

## Qué puede pedir
<!-- doc-role:requests -->

Pídalo con sus propias palabras; no hace falta una frase exacta.

- **Zona horaria:** indique cuál usar (por ejemplo, `America/Costa_Rica`). Las fechas del historial de cambios, los respaldos y los reportes la usan; si no, la hora local del equipo, nunca UTC. Para conservarla en próximas sesiones, pida registrarla como decisión.
- **Idioma y estilo de respuesta:** cámbielos para esta sesión o pida guardarlos.
- **Más detalle:** pídalo cuando quiera; su estilo de respuesta se mantiene.
- **Modo por lotes:** nombre una tarea acotada y pida el modo por lotes. El agente agrupa sus preguntas abiertas una vez y luego trabaja sin preguntar por cada archivo. Las aprobaciones se siguen pidiendo de una en una.
- **Sincronización segura:** pida sincronizar de forma segura. El agente descarga y actualiza solo si no hace falta fusionar ni resolver conflictos.
- **Commit o push:** pida un commit, un push o ambos al cerrar. El agente muestra antes los archivos exactos y el mensaje.
- **Actualizar la documentación:** pídalo después de cambios importantes.
- **Pausar o terminar:** pida pausar o cerrar la sesión; la siguiente sesión retoma desde ese traspaso.
- **Ayuda:** ejecute `repo-harness-help` para una guía breve.
- **Revisar o actualizar el harness:** ejecute el constructor de nuevo.
- **Retirar el harness:** pida los [pasos de retiro manual](docs/es/repo-harness/usage.md#retirar-el-harness-manualmente); no hay desinstalador automático.

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
