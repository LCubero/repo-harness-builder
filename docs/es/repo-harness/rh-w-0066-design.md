# Diseño RH-W-0066 de enrutamiento de recordar y anotar

Estado: diseño mínimo decidido por delegación del propietario (2026-10-01). El [roadmap](roadmap.md#rh-w-0066) posee el ciclo de vida.

## Decisión

Agregar una regla de Honestidad a `.repo-harness/rules/working-agreements.md` generado, junto a la regla de enlaces de RH-W-0065:

> When asked to remember or note something, route it to its existing owner: a decision to `docs/repo-harness/decisions.md`, pending work to `docs/repo-harness/roadmap.md`, a working rule to this file, a personal preference to Engram when available. If it is already recorded, do not duplicate it; if the owner is unclear, ask once. Never create a generic notes file.

Los harness standalone no tienen Engram, así que su copia omite la cláusula de preferencia personal.

Es comportamiento generado del builder, no una convención solo del proyecto: todo harness recibe estos pedidos y cada propietario nombrado es un archivo generado. El trabajo restante registrado queda cubierto sin maquinaria nueva: la clasificación es la lista de propietarios, la deduplicación es "no duplicar", el conflicto y el alcance dudoso son "preguntar una vez", y la privacidad y el límite de Engram siguen en la regla de memoria existente.

## Rechazado

Un archivo de notas, esquema, clasificador, contador de refuerzo o almacén separado de conflictos: ninguno resuelve un problema presente. `.repo-harness/rules/memory.md` sería el lugar natural, pero está en su presupuesto de 700 tokens; los acuerdos de trabajo ya poseen cómo se trabaja y tienen espacio.

## Implementación

1. RED: una prueba que exige la regla en los acuerdos de trabajo renderizados.
2. GREEN: agregar la viñeta a la plantilla en `skill/repo-harness-builder/scripts/harness_tool.py` y a la copia evolucionada por humanos de este repositorio.
3. Sincronizar las proyecciones del builder con preview de mantenimiento y apply con el digest actual; ejecutar la suite completa y el checker.
4. Entregar con un issue y un PR, y luego cerrar el ciclo de vida.
