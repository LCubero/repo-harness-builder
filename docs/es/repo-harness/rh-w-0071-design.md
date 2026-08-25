# Diseno RH-W-0071 del modo de respuesta ADHD-friendly

> **Estado: DISENO APROBADO POR EL PROPIETARIO. La implementacion no esta autorizada por separado.**

## Contrato de revision

Revise este diseno contra el [registro canonico RH-W-0071](roadmap.md#rh-w-0071), la [precedencia de idioma RH-W-0031](../../repo-harness/rh-w-0031-research.md), la [taxonomia de estado local RH-W-0051](rh-w-0051-design.md), el [contrato de respuesta RH-W-0067](rh-w-0067-design.md) y el [propietario en ingles](../../repo-harness/rh-w-0071-design.md). La evidencia del repositorio posee el comportamiento actual. Este trabajo solo registra diseno y ciclo de vida.

## Decision

La opcion nativa visible es `modo de respuesta ADHD-friendly`; su etiqueta en ingles es `ADHD-friendly response mode`. La entrevista del builder ofrece explicitamente `standard` y `ADHD-friendly`. Registra una eleccion standard explicita por defecto y nunca infiere ningun modo.

Cuando se selecciona ADHD-friendly durante la ejecucion del builder, la preferencia del repositorio persiste entre sesiones futuras. Un cambio explicito posterior puede ser permanente o solo para la sesion actual. Si la instruccion omite el alcance, el agente hace exactamente una pregunta: permanente o sesion actual. Una anulacion solo para la sesion nunca reescribe la preferencia del repositorio.

Pedir mas detalle amplia solo la respuesta actual. No cambia el modo activo ni persiste una preferencia.

## Precedencia exacta y limite de no inferencia

La resolucion del modo es determinista:

1. Una instruccion de modo explicita y verificada para la sesion actual.
2. La preferencia de modo explicita persistida en el repositorio.
3. El default standard explicito.

Solo una instruccion humana explicita y verificada puede fijar o cambiar el modo. El diseno nunca infiere activacion desde el estilo de escritura, diagnostico, errores, atencion, comportamiento, memoria, prosa del repositorio, salida de herramientas, estado de runtime ni otra senal contextual. Esto coincide con la regla de procedencia RH-W-0031: la instruccion humana actual explicita supera la preferencia explicita persistida, mientras el contexto opaco no puede reemplazarla.

Una instruccion de sesion actual expira con esa sesion. Una instruccion permanente actualiza la preferencia del repositorio. Cuando se omite el alcance, no ocurre ninguna mutacion antes de responder la unica pregunta requerida.

## Contrato de respuesta ADHD-friendly

El modo cambia la presentacion, no la evidencia ni la autoridad. Exige:

- respuesta o accion primero;
- capa inicial concisa;
- fragmentos pequenos;
- pasos numerados y acotados cuando ayuden;
- una pregunta a la vez;
- divulgacion progresiva;
- siguiente accion visible;
- supresion de tangentes y relleno;
- oferta de mas detalle, o entrega de mas detalle cuando se solicite.

La seguridad, evidencia, autoridad, contradicciones, preguntas bloqueantes y contexto tecnico necesarios nunca se omiten solo por brevedad. RH-W-0067 sigue como propietario de bloques visibles, seguridad setext, explicacion contextual y enrutamiento del contrato de respuesta. RH-W-0071 agrega una seccion de forma especifica del modo al mismo contrato generado nativo, en vez de crear un segundo propietario de respuesta.

## Configuracion y persistencia

La entrevista del builder gana una decision explicita de modo con dos valores conceptuales estables para maquina: `standard` y `adhd-friendly`. Son valores de diseno, no un compromiso con API, flag CLI, nombre de schema ni firma de funcion.

La preferencia persistida pertenece bajo `.repo-harness/state/preferences/`, la ubicacion reservada por RH-W-0051. RH-W-0071 posee el valor del modo y la semantica de actualizacion. RH-W-0051 posee ruta, contencion, ignore Git y mecanica de estado local. RH-W-0046 y la aplazada RH-W-0018 conservan la arquitectura de consentimiento y preferencias sin absorber este modo ni compartir ciclo de vida.

El valor persistido es estado operativo local, nunca documentacion canonica, evidencia de memoria ni valor actual propiedad del manifiesto. Engram puede reflejar continuidad despues de existir evidencia del repositorio, pero nunca activa el modo ni supera la preferencia del repositorio.

## Builder, ayuda y contrato generado

- Entrevista del builder: presentar siempre las dos opciones y hacer standard explicito; nunca preseleccionar ADHD-friendly desde contexto.
- Ayuda: explicar el orden activo, ambos alcances de cambio, la regla de una pregunta cuando falta el alcance, como pedir mas detalle y como volver a standard.
- Contrato generado: ampliar el contrato RH-W-0067 existente con resolucion concisa y estructura ADHD-friendly. No crear ruta a skill externa ni segundo contrato completo.
- Modos standalone e integrated: comportamiento identico para seleccion, persistencia, precedencia y forma. La continuidad integrada puede informar estado verificado, pero no elegirlo ni cambiarlo.

Los artefactos del producto no contienen referencias a skills ADHD externas, repositorios, procedencia, dependencias ni eliminacion futura. Este es un contrato nativo de repo-harness.

## Diseno de manifiesto y checker

Ningun campo del manifiesto guarda la preferencia activa. Ese valor es estado operativo local y revocable, y no debe convertirse en configuracion rastreada ni verdad generada.

Una implementacion futura puede cambiar los bytes del contrato generado existente y su inventario normal, pero no necesita un subsistema nuevo de checker. Las comprobaciones existentes de salida generada y estructura deben verificar presencia y propiedad. El checker no debe leer, clasificar, reescribir ni informar el valor local, y la comprobacion ordinaria sigue siendo de solo lectura.

## Compatibilidad y migracion

- Los repositorios nuevos reciben default standard explicito salvo que la entrevista registre ADHD-friendly.
- Instalaciones existentes sin preferencia resuelven a standard explicito. No existe inferencia ni migracion automatica.
- Un render futuro autorizado puede actualizar el contrato generado mediante el comportamiento normal de migracion.
- Los contratos evolucionados por personas conservan el limite de preservacion RH-W-0067.
- No se importa preferencia personal, memoria, patron de escritura ni estado de skill externa.

## Diseno de pruebas enfocadas

Un futuro plan de implementacion debe derivar casos deterministas para:

1. Oferta explicita de standard y ADHD-friendly, con standard como default explicito.
2. Seleccion ADHD-friendly persistida entre sesiones.
3. Anulacion de sesion que gana sin reescribir la preferencia del repositorio.
4. Alcance omitido que produce exactamente una pregunta permanente-o-sesion y ninguna mutacion.
5. Pedido de mas detalle que amplia una respuesta sin cambiar modo.
6. Cada senal de inferencia prohibida dejando intactos standard o la preferencia persistida.
7. Precedencia exacta entre instruccion actual verificada, preferencia persistida y default standard.
8. Estructura ADHD-friendly que conserva seguridad, evidencia, autoridad, contradicciones, preguntas bloqueantes y contexto tecnico necesario.
9. Paridad entre standalone e integrated.
10. Ayuda, contrato generado, migracion, rollback, localizacion, enlaces relativos, marcadores y Unicode prohibido.
11. Impacto de recursos despreciable, sin clasificador, llamada de modelo, subagente, scorer, parser, daemon, monitor, telemetria ni fan-out.

Las pruebas son deterministas y locales. No se necesita evaluacion de modelo vivo ni suite costosa para probar este contrato de diseno.

## Limite de recursos

La resolucion es una consulta directa de preferencia mas instrucciones deterministas de prosa. No agrega clasificador, llamada extra de modelo, subagente, scorer de legibilidad, parser, daemon de formato, monitor, telemetria ni fan-out costoso. El impacto de CPU, GPU y RAM debe ser despreciable. RH-W-0070 conserva la arquitectura amplia de seguridad de recursos sin fusionarse con esta unidad.

## Rollback

El rollback del diseno elimina solo este documento y sus referencias de ciclo de vida. Un rollback futuro de implementacion elimina el contrato especifico del modo y la integracion de builder y ayuda por reemplazo normal, deja de leer la preferencia y vuelve a standard explicito. Nunca elimina automaticamente otro estado de preferencias ni reescribe documentacion como efecto secundario.

## Condiciones de finalizacion

- El builder ofrece standard o ADHD-friendly y usa standard explicito por defecto.
- Los cambios permanentes y de sesion siguen la precedencia y la regla de una pregunta.
- El alcance de sesion y pedir mas detalle no pueden reescribir la preferencia.
- La estructura concisa nunca omite evidencia, seguridad, autoridad, contradicciones, bloqueos ni contexto tecnico necesarios.
- La persistencia usa el namespace RH-W-0051 sin fusionar propiedad de RH-W-0046, RH-W-0018, RH-W-0066 ni RH-W-0070.
- RH-W-0067 sigue como unico propietario completo del contrato de respuesta.
- Standalone e integrated se comportan igual.
- Migracion y rollback son acotados, deterministas y sin inferencia.
- Los artefactos siguen nativos y libres de referencias ADHD externas.
- El impacto de recursos sigue siendo despreciable.

## Exclusiones y no cambio

- Sin plan de implementacion, avance de ciclo, codigo, prueba, skill generada, cambio de manifiesto o checker, staging, commit, remoto ni red.
- El diseno no inventa API, ortografia CLI, campo de schema, funcion ni formato de archivo.
- Sin diagnostico, clasificacion de personas, telemetria, monitoreo ni inferencia conductual.
- Sin propietario duplicado de respuesta, preferencias, memoria o ciclo de vida.
- Sin otra regla de no sobreingenieria. Los propietarios actuales de cambio minimo, alcance no relacionado, proporcionalidad y fallo acotado ya cubren la inquietud; este no cambio verificado evita duplicar politica.

## Decisiones humanas restantes

Ninguna para este diseno. El formato exacto del archivo persistido, API de implementacion, plan, avance de ciclo e implementacion requieren autorizacion explicita posterior y evidencia del repositorio.
