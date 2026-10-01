# Diseño RH-W-0065 de evidencia de investigación y enlaces

Estado: diseño mínimo aprobado por el propietario (2026-09-30). El [roadmap](roadmap.md#rh-w-0065) posee el ciclo de vida.

## Decisión

Agregar dos reglas de Honestidad a `.repo-harness/rules/working-agreements.md` generado:

1. Cuando una investigación cita fuentes, listar toda fuente localizada pero no leída, con un motivo corto como autenticación, fallo de transporte, contenido incompleto o evidencia solo de fragmento. Nunca presentar una fuente no leída como evidencia.
2. Guardar un enlace útil en el documento que posee su tema. No crear una lista de referencias separada.

No se crea archivo de referencias, esquema ni propietario nuevo. El archivo de reglas existente ya posee cómo se trabaja; los documentos temáticos de investigación ya poseen sus fuentes.

## Rechazado

El esquema registrado de nueve campos con deduplicación, revalidación, archivado y localización: no resuelve un problema presente en un repositorio de un solo propietario y agrega un archivo que nadie mantendría. La evaluación de los repositorios externos candidatos del propietario sale de esta unidad; requiere autoridad separada de acceso externo si alguna vez se desea.

## Implementación

1. RED: una prueba que exige ambas reglas en los acuerdos de trabajo renderizados.
2. GREEN: agregar las dos viñetas a la plantilla en `skill/repo-harness-builder/scripts/harness_tool.py` y las mismas viñetas a la copia evolucionada por humanos de este repositorio, que el mantenimiento no reemplaza.
3. Sincronizar las proyecciones del builder con preview de mantenimiento y apply con el digest actual; ejecutar la suite completa y el checker.
4. Entregar con un issue y un PR, y luego cerrar el ciclo de vida.

El archivo se mantiene dentro de su presupuesto de 700 tokens.
