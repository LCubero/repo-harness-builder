# Diseño RH-W-0074 de omisión aceptada y finalización

> **Estado: diseño aprobado por el propietario, implementado en su forma mínima el 2026-10-01 (ver abajo) y entregado mediante el PR #259.**

Propietario canónico: [documento en inglés](../../repo-harness/rh-w-0074-design.md). Este es su par completo en español. Ciclo: [RH-W-0074](roadmap.md#rh-w-0074).

## Dirección aceptada

Terminar una conversación sin fingir que se cumplió cada obligación. Advertir una vez sobre la consecuencia concreta, respetar la decisión explícita del usuario dentro del alcance autorizado y conservar el resultado real de cada operación. No volver a pedir una decisión ya proporcionada y sin cambios. No hace falta un estado global de éxito con excepciones.

`completed` sigue exigiendo verificación exitosa y que hayan tenido éxito todas las tareas obligatorias aplicables, la reconciliación documental y la continuidad requerida. Declinar una tarea obligatoria no la vuelve opcional ni `not_applicable`, no completa la unidad ni avanza su ciclo en el roadmap.

## Reconciliación propuesta con RH-W-0068

**Hecho verificado:** el [propietario de cierre implementado](rh-w-0068-documentation-closure-design.md#resultados-de-cierre) prohíbe `completed` si la documentación obligatoria está declinada, pendiente, fallida o incompleta, y exige `paused` o `blocked`. El contrato instalado de session-stop también exige un resultado de cierre y define `paused` y `blocked` según información o requisitos realmente faltantes. Esas exigencias no pueden quedar intactas para una omisión aceptada sin una decisión pendiente.

**Modificación aprobada por el propietario:** es un cambio acotado de clasificación/salida, no una excepción a la elegibilidad para completar:

- Conservar el orden de verificación/documentación/continuidad de RH-W-0068, las obligaciones de las superficies aplicables y la prohibición de fabricar finalización.
- Reemplazar solo la clasificación forzada `paused`/`blocked` ante una omisión aceptada. Informar la documentación obligatoria como `declined`, conservar los resultados verificados y expresar en prosa que termina la conversación sin completar la unidad. No emitir éxito global ni inventar un estado global nuevo. Ajustar explícitamente el requisito de salida de session-stop para permitir este informe por alcance en lugar de exigir una etiqueta global de cierre en ese caso.
- Usar `paused` solo ante información humana realmente pendiente; usar `blocked` solo ante un requisito realmente faltante. La verificación u otra operación intentada que falla conserva `failed`; terminar la conversación no borra ese resultado.

Hasta que la implementación se autorice por separado y se realice, RH-W-0068 y el contrato generado siguen vigentes. La aprobación del diseño registra la modificación, pero no la activa ni la sustituye silenciosamente.

## Casos mínimos de aceptación

| Evidencia y decisión | Informe propuesto |
|---|---|
| Todas las obligaciones requeridas aplicables tienen éxito | `completed`, sin excepción |
| Trabajo verificado; el usuario declina explícitamente documentación obligatoria tras aceptar la consecuencia indicada; no falta otra respuesta | Conservar el trabajo verificado; documentación `declined`; termina la conversación sin completar la unidad; sin repetir consentimiento ni etiqueta de éxito global |
| La verificación falla, aunque el usuario pida terminar | Verificación `failed`; sin finalización ni prueba exitosa inventada |
| Falta realmente una aprobación o un requisito | Aplicar `paused` o `blocked` solo a la información o requisito faltante; no inferirlo únicamente de una negativa |

Son criterios de diseño, no pruebas ejecutadas ni evidencia de cumplimiento de un modelo en vivo.

## Cambio futuro mínimo y límites

Si se autoriza por separado, modificar solo la guía existente de detención en working-agreements y las cláusulas de cierre/salida de session-stop, sus aserciones enfocadas de contrato y las proyecciones generadas directamente afectadas. Reconciliar el diseño RH-W-0068 y el resumen de decisiones durables con la redacción aprobada en esa misma unidad de implementación. No hacen falta una compuerta nueva ni bytes nuevos en el reglamento.

No se propone esquema, registro de operaciones, runtime, parser, almacenamiento de estado, anulación automática ni resultado global nuevos. Acceso externo, acciones destructivas, cambios de proveedor, publicación y autoridad del builder vinculada al digest conservan sus límites de aprobación separados. RH-W-0075 conserva la necesidad previa a pedir acceso; RH-W-0068 conserva el orden de cierre. Este diseño no cambia la cadena aprobada RH-W-0069 → RH-W-0053 → RH-W-0048 → RH-W-0052 ni autoriza un plan, implementación, ciclo de revisión o entrega Git.

## Implementación mínima (2026-10-01)

El propietario autorizó la implementación y delegó el alcance bajo el issue #258. Cambian dos superficies generadas:

1. Acuerdos de trabajo, sección Stopping: "Once the human decides, warn once about the consequence, then comply within authorized scope; never re-ask."
2. Compuerta de cierre de session-stop: una reconciliación obligatoria declinada, pendiente, fallida o incompleta sigue sin dar `completed`, pero `paused` se usa solo para información humana realmente pendiente, `blocked` solo para un requisito realmente faltante y `failed` para una operación fallida. Una declinación aceptada se informa como `declined` tras una sola advertencia, indicando que la unidad de trabajo no se completó y sin etiqueta global de cierre; el contrato de salida permite ese informe acotado.

Para que entre la frase nueva, los acuerdos de trabajo propios de este repositorio sueltan la cláusula de Project Agreements que ya posee `usage.md`. No se agregó ningún estado, esquema ni almacén nuevo.
