# Alcance completado RH-W-0089

**Estado: completado y entregado.** Este alcance bilingüe conserva la regla acotada aprobada de contrato generado y su límite histórico de autorización. `RH-W-0089` está completado con trabajo restante `none`; el [registro de la hoja de ruta](roadmap.md#rh-w-0089) sigue siendo el propietario del ciclo de vida. El issue #182 aprobado y el PR #183 se fusionaron por squash a `main` como `8ff35bb66739b8acd45b9ca42c199fe4b83685ab` a las 2026-09-12T05:19:32Z; el issue #182 se cerró un segundo después. Inmediatamente después del fast-forward, `main` local y `origin/main` eran iguales en ese commit, 0 por delante/0 por detrás, con worktree e índice limpios.

## Contrato exacto implementado

La única regla implementada pertenece al propietario existente `.repo-harness/rules/response-contract.md` y se alcanza solo mediante la `RESPONSE_CONTRACT_REFERENCE` diaria existente; no crea ni duplica un propietario de respuesta ni una regla de contrato diario.

Mientras una compuerta humana bloqueante esté pendiente, una notificación diferida de subagente sigue siendo informativa:

1. Nunca es intención ni autoridad humana y no puede responder la pregunta pendiente ni activar una acción.
2. No reemplaza, repite ni desplaza la pregunta pendiente.
3. La forma bloqueante vigente permanece sin cambios, incluida su única pregunta final y sus delimitadores de exactamente 32 bytes ASCII `=`.
4. El resultado terminal verificado más reciente sustituye resultados intermedios obsoletos. Un resultado terminal se limita a evidencia autoritativa de finalización del proveedor o de la operación del subagente; el orden de roles, la llegada de la notificación, el progreso intermedio y texto no verificado no son evidencia terminal.

Esta regla es solo conducta generada defensiva. No afirma que Pi preserve compuertas pendientes ni define la semántica de sus notificaciones.

## Pruebas enfocadas implementadas de contrato

La implementación autorizada por separado incluye pruebas enfocadas de contrato que demuestran:

1. que la regla aparece exactamente una vez en el propietario del contrato de respuesta, sin propietario duplicado ni redacción en contratos diarios;
2. que la `RESPONSE_CONTRACT_REFERENCE` diaria existente sigue siendo la única ruta desde los contratos diarios generados hacia ese propietario;
3. que una compuerta humana pendiente sigue siendo la misma pregunta bloqueante visible después de una notificación diferida de subagente, conservando la forma existente de 32 `=` y el límite de una pregunta;
4. que una notificación diferida es solo informativa y no puede volverse intención humana, autoridad, respuesta ni disparador de acción; y
5. que solo la finalización terminal verificada más reciente del proveedor o de la operación del subagente sustituye resultados intermedios obsoletos, mientras informes no verificados o intermedios no lo hacen.

Estas pruebas demuestran únicamente propiedad de instrucciones renderizadas y redacción de contrato. No demuestran comportamiento vivo de Pi ni semántica de entrega de notificaciones.

## Límites

- Sin estado de runtime, colas, coordinadores, daemons, schedulers, buses de eventos, intercepción de notificaciones, parches de Pi, autoridad automática ni rediseño amplio de subagentes.
- Bajo esta unidad de redacción, no se autorizó implementación, pruebas, ediciones de contrato generado, fuente, proyección de runtime, manifest, checker, configuración, red, índice Git, commit, push, issue, pull request, release, publicación ni otra acción de entrega.
- Sin reemplazar, repetir, desplazar, reformatear ni debilitar la forma bloqueante vigente.
- Sin propietario nuevo de respuesta, referencia diaria, protocolo de notificación, almacén de estado terminal ni adaptador de proveedor.
- Sin afirmar semántica de notificaciones de Pi, causa raíz, propiedad final ni capacidad del runtime para conservar una compuerta pendiente. Son elementos externos y desconocidos.
- Sin inferir que una notificación diferida aporta intención humana, autoridad, aprobación ni una operación completada.

## Evidencia y ciclo de vida

Hecho verificado del repositorio: la compuerta existente de working-agreements exige una pregunta bloqueante y espera; el contrato de respuesta posee su forma; los contratos diarios generados se enrutan hacia ese propietario mediante `RESPONSE_CONTRACT_REFERENCE`; y la regla acotada y las pruebas enfocadas están implementadas y verificadas. RED registró `0 != 1`; GREEN registró `RuleFileTests` 15 pasadas. La suite completa pasó 546 con 10 omitidas; el checker estuvo `current` con 0 escrituras; pasaron la paridad exacta de proyecciones canónica/autoalojada/Claude/OpenCode/Pi y `git diff --check`.

Evidencia de sesión: se observó una terminación diferida de subagente Pi como turno posterior de rol usuario mientras una compuerta de autoridad humana seguía sin resolver. No establece semántica ni causa raíz del runtime de Pi. Desconocido: semántica de notificaciones de Pi, causa raíz, propiedad final y si el runtime puede preservar por sí mismo una compuerta pendiente. La revisión nativa se detuvo antes de START porque los assets administrados de Pi estaban desactualizados, por lo que no hay linaje ni veredicto.

Decisión humana: el padre registró el cierre técnico delegado del propietario después de una revisión completa aplicable del repositorio y una verificación independiente PASS. La regla acotada y las pruebas enfocadas están implementadas y verificadas; `RH-W-0089` está `completed` con trabajo restante `none`. La aprobación histórica acotada por etapa no autorizó entrega Git, acceso de red, commit, push, issue, pull request ni publicación; el issue #182 aprobado y el PR #183 entregaron después el trabajo mediante squash merge a `main` como `8ff35bb66739b8acd45b9ca42c199fe4b83685ab` a las 2026-09-12T05:19:32Z, y el issue #182 se cerró un segundo después.
