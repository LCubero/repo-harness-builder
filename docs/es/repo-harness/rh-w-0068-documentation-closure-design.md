# Diseno RH-W-0068 de compuerta documental obligatoria para cierre

> **Estado: IMPLEMENTADO Y VERIFICADO. RH-W-0068 sigue proposed por sus defectos separados sin resolver.**

## Decision

Despues de verificar con exito una implementacion o entrega, una unidad solo puede quedar `completed` cuando se hayan reconciliado desde hechos probados todas las superficies documentales canonicas aplicables a esa unidad. El orden exacto es:

1. Implementacion.
2. Evidencia de verificacion.
3. Reconciliacion documental desde esa evidencia.
4. Traspaso de continuidad.
5. Elegibilidad para completar.

Esto cierra una brecha nativa verificada dentro del propietario [RH-W-0068](roadmap.md#rh-w-0068). Es trabajo separado de RH-W-0051, ya completada, y de los defectos RH-W-0068 aun sin resolver sobre disposicion del changelog, politica Git y activacion de skills ajenas.

## Aplicabilidad Y Superficies

La aplicabilidad se decide por unidad despues de verificar. Solo son obligatorias las superficies que poseen un hecho cambiado por esa unidad:

| Superficie | Es obligatoria cuando |
|---|---|
| Roadmap canonico o registro de ciclo | La unidad tiene ciclo rastreado o cambia la verdad de ciclo |
| Proyeccion de estado actual | Cambio su ciclo canonico o estado visible actual |
| Pares localizados | El propietario aplicable declara paridad localizada |
| Changelog significativo | El comportamiento, entrega u otro cambio significativo completado pertenece a la cronologia |
| Mapa documental | Se creo, movio, elimino un documento o cambio su propiedad en el mapa |
| Otra documentacion durable | El trabajo cambio un hecho durable que ella posee |

Una superficie aplicable termina `completed`. Una superficie realmente irrelevante termina `not_applicable` con razon concisa. Trabajo trivial, no-op, solo de redaccion o no aplicable no crea churn obligatorio.

## Resultados De Cierre

- Reconciliacion obligatoria declinada, pendiente, fallida o incompleta deja el cierre `paused` cuando aprobacion o informacion humana puede resolverla; en otro caso queda `blocked`. Nunca queda `completed`.
- El usuario puede declinar documentacion. Eso declina completar, no conservar implementacion, entrega, commits, evidencia u otros resultados exitosos.
- Documentacion opcional o `not_applicable` nunca bloquea completar.
- La documentacion se escribe solo despues de que la verificacion de implementacion o entrega aporta hechos probados. Nunca se fabrican ni documentan prematuramente evidencia, estado, entrega, cronologia o paridad.
- La continuidad registra resultados reales y nunca sustituye documentacion canonica.

El roadmap conserva autoridad de ciclo. Su proyeccion de estado actual y cada par localizado declarado deben cambiar en la misma unidad. Un fallo localizado conserva la evidencia exitosa de implementacion, pero impide completar autoritativamente el ciclo.

## Forma Minima De Implementacion

La implementacion cambia solo el contrato canonico `session_stop_skill()`, su prueba canonica enfocada y las proyecciones generadas. Agrega reconciliacion documental obligatoria a la elegibilidad para completar, conserva `completed` o `not_applicable` por superficie y reemplaza la regla anterior que permitia continuar hacia cierre completado tras declinar documentacion. No agrega store, schema, campo de manifest, comportamiento de checker, migracion ni motor general de workflow.

## Evidencia Enfocada De Aceptacion

RED necesita una prueba enfocada del contrato canonico que demuestre la brecha actual: una implementacion o entrega verificada no puede quedar `completed` si algun resultado documental canonico aplicable esta declinado, pendiente, fallido o incompleto. La misma prueba debe exigir el orden de cinco pasos, manejo `not_applicable`, preservacion del trabajo completado tras declinacion y ausencia de documentacion prematura o fabricada.

RED fallo por ausencia de la compuerta ordenada. GREEN paso la regresion enfocada de cierre y el conjunto relacionado del contrato session-stop despues del cambio minimo. El plan de mantenimiento `1db45a1a6d561fa44f34a5f4032874ea707b3acfda0851f14231f0d2b0630a1a` reemplazo solo las tres proyecciones generadas de session-stop y devolvio `VALIDATED`.

## Limites

- Este diseno no cambia fuente, salidas generadas, pruebas, evidencia de ciclo ni entrega de RH-W-0051.
- Este corte de cierre documental esta implementado; RH-W-0068 sigue `proposed` hasta resolver sus otros defectos.
- No hay commit, push, red, flujo RDD, store, schema ni cambio de checker en este corte.
- Trabajo verificado existente nunca se revierte porque la documentacion sea declinada o bloqueada.

## Disposicion Del Ciclo

El defecto de cierre documental esta resuelto. RH-W-0068 no esta completado porque los defectos de disposicion del changelog, politica Git y activacion de skills ajenas aun requieren diseno y autoridad de implementacion separados.
