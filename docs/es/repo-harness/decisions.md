# Decisiones y restricciones

La fuente canonica es [`../../repo-harness/decisions.md`](../../repo-harness/decisions.md).

## Decisiones permanentes
<!-- doc-role:standing-decisions -->

- La evidencia actual del repositorio prevalece sobre memoria previa.
- Gentle AI y Engram son obligatorios en modo integrado, pero no reemplazan evidencia actual.
- `skill/repo-harness-builder/` y `command/repo-harness-builder.md` son fuentes editables; `.claude/` y `.opencode/` son proyecciones.
- Este repositorio usa recursivamente su harness generado para mejorar el constructor.
- Inicializacion Git y publicacion son limites opcionales con aprobaciones separadas y eleccion explicita de visibilidad.
- Los repositorios nuevos reciben una base completa; los existentes reciben analisis e integracion propuesta, nunca sobrescritura automatica.
- Los marcadores generados delimitan material, pero no eximen documentos humanos del analisis.
- Solo la aplicacion del constructor usa tokens estrictos localizados y digest del plan.
- Los punteros raiz preservan bytes y agregan solo una directiva exacta faltante.
- Los harness anteriores se clasifican antes de proponer reemplazo, archivo o eliminacion.
- Las aprobaciones de cierre son puntos reanudables; la transferencia final sigue evidencia renovada y continuidad guardada.
- El inicio sincroniza solo despues de inspeccion y aprobacion; nunca usa pull ciego, rebase, reset o resolucion automatica.
- El cierre completa documentacion y continuidad antes de Git. El consentimiento combinado cubre alcance exacto, commit, comprobaciones, un fetch y un push mientras los hechos no cambien.
- Los resultados `ready`, `paused`, `blocked`, `failed`, `completed` y `degraded` tienen alcance separado para continuidad y Git opcional.
- Cada aprobacion pendiente aparece como una unica pregunta concreta; las propuestas de commit incluyen rutas, justificacion, mensaje, exclusiones y estado preparado.
- La verificacion autoalojada separa paridad en disco, vigencia registrada del proceso y capacidad de recarga.
- `REPO-HARNESS.md`, `CHANGELOG.md` y los documentos durables en ingles conservan la propiedad factual unica. Los pares en espanol son presentaciones completas.
- La validacion bilingue combina roles semanticos y bases proporcionales de contenido por idioma, sin afirmar que una maquina prueba equivalencia semantica.
- Los idiomas de documentacion declarados y el idioma de conversacion controlan la ruta de ayuda generada, por lo que solo puede nombrar una guia que la misma generacion crea. El idioma del informe es presentacional y no cambia ningun byte generado, asi que el digest del plan es identico entre idiomas sin normalizacion.
- Los selectores README son atomicos: sin integracion reciproca ejecutable, `README.es.md` queda bloqueado y toda aplicacion falla de forma segura.
- Las contribuciones usan issue primero, ramas cortas, PR enlazado, Conventional Commits y pruebas con documentacion en la misma unidad; requieren revision del maintainer antes del merge. Cambiar `main` directamente exige una emergencia explicita, y la aprobacion es conversacional porque no existen labels, templates ni checks personalizados para imponerla.
