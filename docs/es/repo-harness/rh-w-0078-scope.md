# Alcance propuesto RH-W-0078

**Estado: propuesto; el propietario aprobó el alcance bilingüe corregido.** Este artefacto registra solo el alcance. La aprobación del alcance por sí sola no autorizaba diseño ni implementación, y este documento no se aprueba a sí mismo. El [registro de la hoja de ruta](roadmap.md#rh-w-0078) posee el ciclo de vida.

## Base verificada

Los `repo-harness-session-start` y `repo-harness-session-stop` generados exceden el techo establecido del cuerpo; `repo-harness-documentation` generado cumple. La [guía empaquetada de estilo de Gentle AI](../../../skill/repo-harness-builder/references/gentle-ai-style-guide.md) ya define estructura de skills, divulgación progresiva, referencias locales y guía de tamaño.

## Dirección propuesta

**Inferencia respaldada:** la [investigación local de Gentle AI](rh-w-0078-gentle-ai-binary-research.md) respalda skills generadas de varios archivos con contratos de entrada concisos y referencias locales alcanzables. Solo la mecánica determinista de renderizado, validación, rutas y bytes pasaría selectivamente al renderer/checker existente en Python. La autoridad, el consentimiento, el juicio y las obligaciones en el punto de acción de las personas permanecen como contratos visibles para el modelo.

Un diseño posterior debe exigir líneas de base reproducibles por runtime y criterios explícitos de aprobación/rechazo para el contexto efectivo cargado y el alcance de referencias en Claude Code, OpenCode y Pi. También debe exigir comprobaciones de preservación que prueben que las obligaciones de seguridad, consentimiento, evidencia, autoridad y punto de acción siguen siendo alcanzables. Dividir archivos por sí solo no prueba ningún resultado.

## Límites

- Sin binario compilado, runtime, servicio ni dependencia nuevos.
- Sin borrar prosa a ciegas ni reducir obligaciones de seguridad, consentimiento, evidencia, autoridad o punto de acción.
- Sin maquinaria que traslade autoridad, consentimiento o juicio humanos a decisiones automáticas.
- Sin cambios de producto, contrato generado, proyección de runtime, manifest, checker, pruebas, Git, red ni entrega bajo esta unidad de redacción.

## Próxima decisión

El 2026-10-04 el propietario autorizó por separado preparar el diseño bilingüe acotado y el procedimiento reproducible de medición, y después aprobó ambos. El propietario autorizó por separado preparar el [plan de implementación](rh-w-0078-implementation-plan.md) bilingüe y lo aprobó el 2026-10-05 solo para registro documental. Bajo la autoridad vigente, el propietario autorizó explícitamente implementar RH-W-0078 y se confirmó el issue #266; el registro está `active`, `P2`. Se elimina la antigua compuerta de base del runtime antes de empezar. Los resultados y ahorros siguen desconocidos; la medición real sigue siendo necesaria antes de afirmar ahorros o aceptar efectividad. Esta tarea no autoriza cambios de herramientas externas, experimentos de runtime ni entrega.
