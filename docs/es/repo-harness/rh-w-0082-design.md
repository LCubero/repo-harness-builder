# Diseno RH-W-0082 de estilo de explicacion plain-language

> **Estado: DISENO APROBADO POR EL PROPIETARIO. La implementacion no esta autorizada por separado.**

## Decision

RH-W-0082 posee el eje independiente `explanation-style: standard | plain-language`. `plain-language` usa lenguaje simple y neutral sin cambiar evidencia, autoridad, verdad tecnica, idioma del artefacto ni los otros ejes de preferencias. Solo se usa un dialecto regional cuando la persona lo solicita explicitamente para la conversacion.

Este eje se combina con `response-structure: standard | adhd-friendly` de RH-W-0071 e `interaction-behavior: interactive | low-question` de RH-W-0018. RH-W-0067 conserva la propiedad completa del contrato de respuesta, RH-W-0031 conserva procedencia de idioma, RH-W-0046 conserva limites de consentimiento y RH-W-0051 conserva contencion de estado local.

## Contrato compartido de preferencias

El contrato LLM-first compartido aprobado es `.repo-harness/state/preferences/user-preferences.md`. RH-W-0071 posee su diseno completo de persistencia y resolucion en runtime. Esta unidad posee solo la semantica del estilo de explicacion y no puede crear otro archivo, registro, framework de parser, propietario de respuesta, store de consentimiento ni store de ciclo de vida.

`plain-language` nunca elimina terminos tecnicos necesarios, clases de evidencia, contradicciones, contexto de seguridad, limites de autoridad ni compuertas obligatorias. Los explica con palabras directas, frases cortas y definiciones cuando hagan falta. No simplifica por omision.

## Ciclo de vida y falta de autoridad

RH-W-0082 esta `proposed`. Este diseno solo registra la decision del propietario y el siguiente ID inmutable verificado. No autoriza fuente de producto, pruebas, proyecciones generadas, lector de runtime, creacion del archivo de estado, migracion, accion Git, accion de red, avance de ciclo ni implementacion.

## Condiciones de finalizacion

- El eje de estilo de explicacion sigue independiente y combinable.
- `plain-language` sigue neutral, simple, exacto y no regional por defecto.
- El idioma del artefacto sigue independiente.
- Los propietarios existentes conservan sus limites.
- La implementacion exige transporte de runtime reconciliado y autorizacion explicita separada.
