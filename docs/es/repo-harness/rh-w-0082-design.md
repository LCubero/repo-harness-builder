# Diseno RH-W-0082 de estilo de explicacion plain-language

> **Estado: active. El diseno delegado del 2026-10-10 agrupa entrega con RH-W-0071; implementacion en unidad posterior separada.**

## Decision

RH-W-0082 posee el eje independiente `explanation-style: standard | plain-language`. `plain-language` usa lenguaje simple y neutral sin cambiar evidencia, autoridad, verdad tecnica, idioma del artefacto ni los otros ejes de preferencias. Solo se usa un dialecto regional cuando la persona lo solicita explicitamente para la conversacion.

Este eje se combina con `response-structure: standard | adhd-friendly` de RH-W-0071 e `interaction-behavior: interactive | low-question` de RH-W-0018. RH-W-0067 conserva la propiedad completa del contrato de respuesta, RH-W-0031 conserva procedencia de idioma, RH-W-0046 conserva limites de consentimiento y RH-W-0051 conserva contencion de estado local.

## Contrato compartido de preferencias

El [contrato de modos por preferencias del 2026-10-10](rh-w-0071-design.md#modos-de-respuesta-por-preferencias-2026-10-10) posee schema JSON compartido, transporte existente de una lectura, limite de escritura manual, precedencia, modulos, fallo y entrega conjunta. Esta unidad solo posee semantica del estilo. Los bytes de instalacion del manifiesto/checker y consentimiento fetch siguen separados; RH-W-0018 sigue excluida y aplazada.

`plain-language` nunca elimina terminos tecnicos necesarios, clases de evidencia, contradicciones, contexto de seguridad, limites de autoridad ni compuertas obligatorias. Los explica con palabras directas, frases cortas y definiciones cuando hagan falta. No simplifica por omision.

## Ciclo de vida y falta de autoridad

RH-W-0082 esta `active` por decision delegada del propietario el 2026-10-10. Transporte y agrupacion de entrega quedan resueltos en el contrato RH-W-0071 enlazado. Sigue implementacion con pruebas primero autorizada por separado junto con RH-W-0071, y luego entrega; esta unidad documental no cambia comportamiento del producto.

## Condiciones de finalizacion

- El eje de estilo de explicacion sigue independiente y combinable.
- `plain-language` sigue neutral, simple, exacto y no regional por defecto.
- El idioma del artefacto sigue independiente.
- Los propietarios existentes conservan sus limites.
- Implementar el contrato compartido enlazado con pruebas primero en una unidad autorizada por separado junto con RH-W-0071, y luego entregar.
