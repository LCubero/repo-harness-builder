# Diseno RH-W-0081 de cronologia local del host para el changelog

> **Estado: DISENO APROBADO POR EL PROPIETARIO. Existe un plan propuesto; la implementacion sigue sin autorizacion separada.**

## Decision

La cronologia humana del changelog deriva las fechas del timestamp local del host del cambio completado. Se usa el timestamp local exacto cuando haga falta, o la fecha calendario local con su offset. Nunca se sustituye UTC en silencio ni se inventa un minuto redondeado.

La correccion pendiente es fusionar ambas secciones `2026-08-29` de los changelogs en `2026-08-28`. Git local registra `1e5759e0` en `2026-08-28T22:33:40-06:00` y `c892cfb` en `2026-08-28T22:40:57-06:00`. Este diseno registra la correccion sin ejecutarla.

## Limites de propiedad

- RH-W-0068 conserva en exclusiva la disposicion del changelog: si el cierre exige una entrada.
- RH-W-0081 posee en exclusiva la derivacion de fecha local del host para la cronologia humana del changelog.
- RH-W-0069 posee la identidad y el ordenamiento de backups de continuidad orientados a maquina. Su nombre UTC y metadata `timestamp_utc` no derivan ni controlan fechas del changelog, y RH-W-0081 no altera esa identidad ni ese ordenamiento.
- RH-W-0080 posee solo la interaccion efimera por lotes. Su activacion y respuestas agrupadas no conceden autoridad cronologica ni de aprobacion.

## Contrato de derivacion

1. Usar el reloj local del host para un cambio completado sin Git. Para trabajo Git completado, preferir evidencia Git local verificada y ligada al cambio exacto.
2. Usar un encabezado por fecha calendario local del host.
3. Para trabajo sin Git cuando no se observo el reloj local, o cuando la evidencia Git local falta, se contradice o es ambigua, formular una pregunta separada y reanudable. No inferir timestamps desde metadata del filesystem ni agrupar la pregunta como autoridad.
4. Conservar segundos exactos y offset al presentar un timestamp. Si no hace falta precision de minuto, declarar solo fecha local y offset.

## Exclusiones

Este diseno y el plan propuesto no autorizan comportamiento, prueba, proyeccion, correccion, Git, red, proveedor, publicacion ni cambio de identidad RH-W-0069.

## Objetivos de verificacion posteriores

El [plan de implementacion](rh-w-0081-implementation-plan.md) propuesto cubre derivacion local, timestamps exactos, ambiguedad, un encabezado por fecha, independencia UTC, paridad y correccion registrada. Espera aprobacion.
