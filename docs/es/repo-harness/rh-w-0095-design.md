# Diseño RH-W-0095 de acuse contextual

> **Estado: IMPLEMENTADO, VERIFICADO DE FORMA INDEPENDIENTE, ENTREGADO Y `completed`.**

## Contrato de revisión

El [diseño en inglés](../../repo-harness/rh-w-0095-design.md) es canónico; este par debe conservar equivalencia semántica. Revisar el resultado implementado contra el [registro de hoja de ruta](roadmap.md#rh-w-0095), la propiedad de respuesta RH-W-0067, la carga al redactar RH-W-0083, la continuación orgánica RH-W-0049, la carta y los acuerdos de trabajo, y la regla vigente de compuerta pendiente.

## Decisión y propietario único

Después de un acuse breve sin acción ni objetivo explícitos, conservar el tema conversacional inmediatamente activo e indicar una dirección sensata para la siguiente conversación o acción. El acuse no es aprobación ni autoridad para planificar, delegar, mutar, entregar o continuar automáticamente.

El `.repo-harness/rules/response-contract.md` generado de RH-W-0067 sigue como único propietario del contrato de respuesta. RH-W-0095 solo agrega allí esta cláusula semántica acotada; no crea un segundo propietario.

## Precedencia y autoridad

| Situación | Resultado requerido |
| --- | --- |
| El turno actual nombra explícitamente acción y objetivo | Aplican las reglas de autorización vigentes; `ok` no agrega autoridad. |
| Hay una compuerta humana pendiente | La compuerta sigue pendiente bajo su contrato vigente; el acuse no la responde, aprueba, descarta, repite ni desplaza. |
| Solo hay acuse y se recupera el tema activo | Conservar el tema y sugerir una dirección sensata; no ejecutar nada. |
| Hay una comparación o conversación activa | Continuarla sin elegir una opción por la persona. |
| No se recupera ningún tema | No inventar; hacer como máximo una aclaración mínima solo si continuar la requiere. |
| Turnos posteriores | No inferir preferencia, instrucción recordada, consentimiento reutilizable, decisión de ciclo ni autorización. |

La autoridad vigente no cambia: `REPO-HARNESS.md` posee autoridad y no-trabajo-no-solicitado; `working-agreements.md` posee detención y proporcionalidad; RH-W-0049 posee conversación orgánica; RH-W-0083 posee carga inmediatamente antes de redactar; la regla de compuerta pendiente posee preguntas pendientes.

## Ejemplos de comportamiento

| Contexto y entrada | Límite esperado |
| --- | --- |
| Conversación activa sobre diseño; `ok` | Acusar brevemente, conservar el tema y sugerir una conversación útil siguiente; no planificar ni editar. |
| Hay una compuerta de decisión pendiente; `ok` | Dejarla pendiente; no tratar `ok` como respuesta ni iniciar otro trabajo. |
| Se comparan dos enfoques; `ok` | Conservar la comparación y sugerir una dimensión material para examinar; no elegir. |
| No hay contexto conversacional recuperable; `ok` | No inventar un tema; si continuar es necesario, pedir una aclaración mínima. |
| `ok, implement RH-W-0095` | Enrutar la acción y objetivo explícitos por las reglas vigentes; `ok` no aporta nada. |
| Nada está pendiente tras una explicación completada; `ok` | Usar estado simple más una dirección útil ligada al tema activo, no bloque vacío, menú fijo ni volcado de backlog. |

## Superficies implementadas

- **Semilla canónica y propietario generado:** una viñeta concisa del contrato de respuesta en `skill/repo-harness-builder/scripts/harness_tool.py` y `.repo-harness/rules/response-contract.md`.
- **Pruebas canónicas:** una prueba enfocada en `RuleFileTests` existente de `skill/repo-harness-builder/tests/test_harness_tool.py`.
- **Proyecciones de paquete declaradas:** proyecciones canónicas de script y pruebas en `.claude/skills/repo-harness-builder/`, `.opencode/skills/repo-harness-builder/` y `.pi/skills/repo-harness-builder/`.
- **Propiedad sin cambios:** no se agregó copia del contrato específica por runtime ni edición de skills diarias.

## Evidencia de verificación

- RED estricto: `RuleFileTests.test_contextual_acknowledgement_has_one_generated_response_contract_owner` falló antes de cambiar la fuente con `AssertionError: 0 != 1`; una repetición enfocada independiente pasó después las 7 pruebas exactas, 0 omitidas, en 0.003s.
- Mantenimiento: el preview `english` no aplicado mostró 13 reemplazos y digest `d8ed32e...`; el preview `espanol` alineado con el manifiesto contenía exactamente seis proyecciones de script/pruebas para Claude/OpenCode/Pi, y el digest `8d51b4782cf0eb2ba5723ed7239948426e34c9303c5bdb081f82c5314dcc6d0f` aplicó `VALIDATED`.
- El checker informó `current`, salida 0 y 0 escrituras. Pasó la paridad exacta de bytes canónico/Claude/OpenCode/Pi para script y pruebas.
- La primera corrida completa no aislada recopiló 559 pruebas y falló cinco comprobaciones: un conteo documentado obsoleto de 558 y cuatro fallos de fixtures Git directos/anidados causados por el repositorio ancestro ajeno `C:/Users/LD9/.git`. Después de actualizar ambos conteos de colección y aplicar `GIT_CEILING_DIRECTORIES=C:/Users/LD9/AppData/Local/Temp` solo al proceso, pasaron los tres selectores Git directos y la suite completa pasó 559 pruebas con 10 omitidas, salida 0, en 205.400s. No hubo cambio persistente del entorno.
- El proxy de tokens de la regla de respuesta renderizada es 698 contra presupuesto 700. Pasó `git diff --check` y el estado previo/posterior fue idéntico.

Estas pruebas de implementación verifican el contrato de instrucciones generado; no garantizan el cumplimiento de un modelo arbitrario. La [hoja de ruta](roadmap.md#rh-w-0095) posee los detalles de entrega y verificación posterior a la implementación.

## Rollback

Quitar la cláusula acotada y sus aserciones enfocadas de fuente/pruebas canónicas, regenerar solo proyecciones declaradas y propietario generado, y reconciliar documentación de ciclo. No aplica limpieza de estado ni migración.

## Exclusiones y decisiones

Sin clasificador, parser, detector, allowlist de frases, máquina de estados, formatter, middleware, rechazo en runtime, eje de preferencia, reapertura RH-W-0018, persistencia, telemetría, analytics, log de cumplimiento, observador en vivo, schema, manifest, config, migración, dependencia, componente externo, copia del contrato por runtime, duplicación en skills diarias, menú fijo, volcado de backlog, ejecución automática ni segundo propietario de respuesta.

Decisiones de diseño restantes: **ninguna**. La implementación, la verificación independiente y la entrega del PR están completas; no se afirma release ni garantía de modelo en vivo.
