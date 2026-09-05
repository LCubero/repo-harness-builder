# Alcance propuesto RH-W-0083

**Estado: `completed`; alcance bilingue implementado bajo autorizacion separada del propietario el 2026-09-04.** Este documento conserva el limite aprobado de la correccion y su historia previa a la implementacion. La implementacion se entrego despues bajo una excepcion de emergencia autorizada por separado; el [registro de la hoja de ruta](roadmap.md#rh-w-0083) posee esa evidencia. No existe issue ni PR.

## Obligacion inmediata

Exigir que cada skill diaria generada, dentro de su Output Contract, cargue y aplique `.repo-harness/rules/response-contract.md` inmediatamente antes de redactar toda respuesta o salida informada que lleve aviso, accion pendiente, pregunta, aprobacion, siguiente accion visible o explicacion contextual.

La redaccion debe reforzar al propietario existente en el punto de accion. No debe copiar ninguna de las formas de 32 bytes, la advertencia setext ni otra sintaxis de respuesta dentro de una skill diaria. `RH-W-0067` conserva la propiedad completa y unica del contrato de respuesta.

## Pruebas de aceptacion

La implementacion quedo limitada a aserciones enfocadas de tiempo de desarrollo que:

1. fallen antes de la correccion porque cada Output Contract diario generado carece de la obligacion inmediatamente anterior a la redaccion;
2. pasen solo cuando cada Output Contract diario generado lleve esa obligacion exactamente una vez y siga nombrando la ruta canonica de la regla;
3. mantengan verde `test_daily_contracts_do_not_duplicate_response_syntax`, conservando su comprobacion enfocada de que las frases prohibidas de sintaxis de respuesta no aparezcan en las tres skills diarias generadas; y
4. mantengan verde `test_response_syntax_has_one_generated_owner`, conservando su comprobacion enfocada de que `Decision required` aparezca solo en el propietario generado del contrato de respuesta.

No se exige evaluacion de modelo vivo ni observador de respuestas en runtime. Las pruebas demuestran el contrato de instrucciones generado, no que un agente arbitrario siempre obedezca la prosa.

## Limites

- Sin cambio en `REPO-HARNESS.md` ni `.repo-harness/rules/response-contract.md`.
- Sin clasificador, evaluador, formateador, rechazo en runtime, telemetria ni estado persistente de cumplimiento.
- Sin duplicacion en `RH-W-0076`, cuyo alcance de tres defectos sigue congelado.
- Sin cambio de fuente de producto, prueba de producto, skill generada, manifest, checker, proyeccion de runtime ni entrega solo por aprobar este alcance.

## Evidencia y ciclo de vida

Hecho verificado del repositorio antes de implementar: la hoja de ruta canonica registraba la referencia al contrato de respuesta despues de Execution Steps, dentro de cada Output Contract diario, y registraba que las aserciones existentes inspeccionaban texto generado y propiedad en vez de respuestas. Tambien conserva la ocurrencia posterior de mezcla del contrato de aprobacion como evidencia dentro del mismo grupo raiz sin crear una unidad nueva. Decision verificada del propietario: el hallazgo de `RH-W-0084` de que la distancia entre la instruccion y el punto de accion predice su aplicacion fue aceptado aqui como control. Bajo autorizacion separada, la referencia compartida se reforzo y se proyecto mediante el renderer ordinario de mantenimiento sin cambiar el propietario ni la sintaxis del contrato de respuesta.

`RH-W-0083` esta `completed`. La implementacion y la verificacion proporcional terminaron dentro de este alcance; la entrega Git posterior conservo una autorizacion y una accion separadas.
