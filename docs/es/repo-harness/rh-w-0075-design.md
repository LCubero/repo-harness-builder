# Diseno RH-W-0075 de necesidad y justificacion externa

> **Estado: DISENO APROBADO POR EL PROPIETARIO. La implementacion no esta autorizada por separado.**

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
