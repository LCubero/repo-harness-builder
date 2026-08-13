# Uso del harness del repositorio
<!-- doc-role:overview -->

Use las tres habilidades generadas como un ciclo: recupere contexto, mantenga vigente la documentacion durable y deje una transferencia verificada.

## Flujo diario
<!-- doc-role:daily-workflow -->

1. Inicie o reanude con `repo-harness-builder-session-start` y declare el objetivo.
2. Use `repo-harness-builder-documentation` cuando cambien hechos durables o el estado visible.
3. Cierre con `repo-harness-builder-session-stop`, que verifica evidencia y guarda continuidad antes de evaluar Git.

## Disponibilidad integrada
<!-- doc-role:availability -->

Confirme que las habilidades, Gentle AI y Engram esten disponibles. Si falla una dependencia de continuidad, informe el fallo y no declare continuidad integrada exitosa.

## Congruencia y propiedad
<!-- doc-role:ownership -->

Ejecute `python .repo-harness/check.py` despues de cambiar fuentes canonicas o proyecciones. El control demuestra paridad en disco, no la vigencia de contenido registrado en el proceso activo.

## Limites de aprobacion
<!-- doc-role:approval-boundaries -->

Las habilidades diarias son de solo lectura por defecto y usan aprobacion conversacional. La aplicacion del constructor es distinta: en ingles exige `APPROVE` o `DENY`; en espanol exige `APROBAR` o `DENEGAR`, junto con el digest vigente del plan.
