# Diseno RH-W-0080 de interaccion efimera por lotes

> **Estado: DISENO APROBADO POR EL PROPIETARIO. Existe un plan propuesto; la implementacion sigue sin autorizacion separada.**

## Decision

Una solicitud explicita puede activar un modo efimero de interaccion por lotes para un unico objetivo acotado dentro del repositorio seleccionado. Despues de analizar primero la evidencia, el harness puede agrupar solo aclaraciones no autorizantes de requisitos que ese analisis vuelva detectables. Luego ejecuta solo el trabajo ya autorizado para ese objetivo sin interrupciones archivo por archivo. El modo vence con el objetivo y nunca persiste, amplia, agrupa, normaliza, suministra, infiere ni reemplaza autoridad.

## Activacion y consulta

La propiedad queda fijada por la arquitectura actual. `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` posee activacion explicita, intake despues de inspeccionar, aclaraciones agrupadas no autorizantes y continuacion acotada al objetivo. `::session_stop_skill()` posee el vencimiento al pausar, cerrar, completar, abandonar o reemplazar el objetivo. `::rule_outputs()` posee exclusiones compartidas de autoridad en working agreements para que los skills no las dupliquen ni debiliten.

1. La persona usuaria nombra explicitamente el modo por lotes y un objetivo acotado.
2. Se inspecciona la evidencia actual del repositorio antes de preguntar.
3. Se agrupan solo aclaraciones no autorizantes de requisitos detectables despues de esa inspeccion.
4. Tras las respuestas, se ejecuta el objetivo autorizado sin confirmaciones por archivo.
5. Las brechas de evidencia que puedan resolverse dentro del repositorio se resuelven sin preguntar.

Las compuertas de aprobacion y autoridad NUNCA se agrupan. Acceso externo, entrega Git, publicacion, acciones destructivas, secretos, preview o apply del builder, aprobacion exacta por digest o token y decisiones materiales de producto recien descubiertas siguen como compuertas separadas, reanudables y de una sola pregunta.

La activacion del modo y las respuestas agrupadas no autorizan bytes del preview ni apply. No pueden suministrar ni reemplazar `APPROVE`, `DENY`, `APROBAR`, `DENEGAR`, digest de plan, digest de generacion, evidencia refrescada ni el dominio activo exacto de aprobacion. La autoridad del builder ligada a digest no cambia.

## Limites

- RH-W-0075 conserva en exclusiva la necesidad estricta y justificacion del acceso externo.
- RH-W-0074 conserva en exclusiva el cumplimiento con una sola advertencia despues de una respuesta.
- RH-W-0069 conserva en exclusiva la continuidad local y Engram.
- RH-W-0068 conserva la disposicion del changelog durante el cierre documental.
- RH-W-0081 posee en exclusiva la derivacion de fecha local del host para la cronologia humana del changelog. Esa cronologia no se infiere de la identidad UTC de RH-W-0069 ni la controla.

Este diseno y el plan propuesto no autorizan implementacion, pruebas, proyecciones, correccion, Git, red, proveedor ni publicacion.

## Objetivos de verificacion del plan propuesto

El [plan de implementacion](rh-w-0080-implementation-plan.md) propuesto cubre activacion explicita, no persistencia, aclaracion estrecha despues de inspeccionar, ejecucion acotada, compuertas separadas y autoridad por digest intacta. Espera aprobacion.

## Condiciones de finalizacion

- El diseno bilingue y las proyecciones del ciclo coinciden.
- RH-W-0080 permanece `proposed` con diseno aprobado.
- No cambia comportamiento de producto.
