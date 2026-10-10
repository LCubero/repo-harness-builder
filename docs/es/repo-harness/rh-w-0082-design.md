# Diseno RH-W-0082 de estilo de explicacion plain-language

> **Estado: `completed`. Implementado el 2026-10-10 con RH-W-0071/RH-W-0082; la [hoja de ruta](roadmap.md) posee el ciclo y la evidencia.**

## Decision

RH-W-0082 posee el eje independiente `explanation-style: standard | plain-language`. `plain-language` usa lenguaje simple neutral sin cambiar evidencia, autoridad, verdad tecnica, idioma del artefacto ni otros ejes. Solo usa un dialecto regional si el humano lo pide explicitamente en la conversacion actual.

Este eje se combina con `response-structure: standard | adhd-friendly` de RH-W-0071 e `interaction-behavior: interactive | low-question` de RH-W-0018. RH-W-0067 conserva la propiedad completa del contrato de respuesta, RH-W-0031 conserva procedencia de idioma, RH-W-0046 conserva limites de consentimiento y RH-W-0051 conserva contencion de estado local.

## Contrato compartido de preferencias

El [contrato de modos por preferencias del 2026-10-10](rh-w-0071-design.md#modos-de-respuesta-por-preferencias-2026-10-10) posee schema JSON compartido, transporte existente de una lectura, limite de escritura manual, precedencia, modulos, fallo y entrega conjunta. Esta unidad solo posee semantica del estilo. Los bytes de instalacion del manifiesto/checker y consentimiento fetch siguen separados; RH-W-0018 sigue excluida y aplazada.

`plain-language` nunca elimina terminos tecnicos necesarios, clases de evidencia, contradicciones, contexto de seguridad, limites de autoridad ni compuertas obligatorias. Los explica con palabras directas, frases cortas y definiciones cuando hagan falta. No simplifica por omision.

## Ciclo de vida y falta de autoridad

RH-W-0082 esta `completed` el 2026-10-10. Lector compartido, transporte de una lectura, rutas base y modulo plain-language implementados junto con RH-W-0071. Las pruebas verifican instrucciones generadas, no cumplimiento del modelo en vivo. RH-W-0018 sigue aplazada.

## Condiciones de finalizacion

- El eje de estilo de explicacion sigue independiente y combinable.
- `plain-language` sigue neutral, simple, exacto y no regional por defecto.
- El idioma del artefacto sigue independiente.
- Los propietarios existentes conservan sus limites.
- El contrato compartido enlazado esta implementado con pruebas primero junto con RH-W-0071.
