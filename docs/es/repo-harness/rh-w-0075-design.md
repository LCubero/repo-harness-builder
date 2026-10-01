# Diseno RH-W-0075 de necesidad y justificacion externa

> **Estado: diseño aprobado por el propietario, implementado en su forma mínima el 2026-10-01 (ver abajo) y entregado mediante el PR #257.**

## Decision

El acceso externo incluye rutas fuera del repositorio seleccionado, repositorios hermanos, acceso de red y tooling de proveedor. Antes de pedir cualquiera, el harness debe probar que el objetivo acotado no puede completarse dentro del repositorio seleccionado.

Si se prueba necesidad estricta, una unica solicitud declara el objetivo exacto, por que es necesario, el uso previsto y la consecuencia de declinar. Si el objetivo se puede resolver dentro del repositorio, el harness no pregunta.

## Limites

- Las compuertas de aprobacion existentes no cambian. Necesidad y justificacion no conceden acceso.
- RH-W-0074 gobierna lo que ocurre despues de la respuesta y no se fusiona con este ciclo.
- El modo por lotes RH-W-0080 no puede omitir este propietario.
- No se introduce store de permisos, intermediario, telemetria, autoridad recordada ni negativa automatica.

## Objetivos de verificacion posteriores

El [plan de implementacion](rh-w-0075-implementation-plan.md) propuesto cubre cada clase externa, evidencia de necesidad estricta, los cuatro campos obligatorios, ausencia de solicitud para soluciones locales, comportamiento ante rechazo y compuertas sin cambios. Espera aprobacion y no autoriza implementar.

## Implementación mínima (2026-10-01)

El 2026-10-01 un agente escribió por costumbre en el temp del sistema, que en Windows queda fuera del repositorio, y listó una ruta externa sin decir por qué, aunque `.repo-harness/tmp/` estaba disponible. El propietario delegó el alcance. Dos líneas generadas responden a la dirección original ("quedarse en el repositorio; al pedir, decir por qué y para qué"):

1. La regla dura del reglamento queda: "Ask for explicit approval before reading or modifying paths outside it. Say why and what for."
2. Los acuerdos de trabajo generados agregan: "Put temporary files in `.repo-harness/tmp/`; use the system temp only when that fails or a task needs a location outside Git."

El contrato de cuatro campos, las pruebas de necesidad y el acople con el modo por lotes del plan propuesto quedan reemplazados: agregan proceso sin una falla presente que detecten. RH-W-0074 sigue separado.
