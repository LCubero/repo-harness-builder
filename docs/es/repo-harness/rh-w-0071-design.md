# Diseno RH-W-0071 de estructura de respuesta ADHD-friendly

> **Estado: `completed`. Implementado el 2026-10-10 con RH-W-0071/RH-W-0082; la [hoja de ruta](roadmap.md) posee el ciclo y la evidencia.**

**Limite vigente:** lector JSON existente con tres campos opcionales, una lectura en session-start y modulos de respuesta por preferencias implementados. Solo el humano edita preferencias; RH-W-0018 sigue aplazada.

## Modos de respuesta por preferencias (2026-10-10)

1. **Reutilizar:** el `.repo-harness/state/preferences/user-preferences.json` existente e ignorado y el lector fijo `read-preference` en `skill/repo-harness-builder/scripts/harness_tool.py` (`read_conversation_preference`); leer una vez al inicio mediante el lector existente del runtime activo. Sin almacen nuevo.
2. **Schema:** claves opcionales `conversationLanguage: english | espanol` (sin cambio), `responseStructure: standard | adhd-friendly` (RH-W-0071) y `explanationStyle: standard | plain-language` (RH-W-0082); al menos una clave. Claves desconocidas o duplicadas, tipos incorrectos o valores invalidos invalidan todo el archivo, conservando validacion fail-closed. Devolver valor tipado y fuente de cada eje; clave ausente no aporta valor persistido.
3. **Escritura:** no existe escritor de preferencias implementado. La persona edita manualmente el archivo solo ante instruccion explicita de recordar una preferencia del repositorio; instrucciones solo de sesion nunca escriben. Evidencia: el lector solo abre `rb` in `read_conversation_preference`, la CLI solo expone `read-preference` in `main`, y session-start prohibe escribir (`.repo-harness/skills/repo-harness-session-start/SKILL.md:23`). La actualizacion atomica Markdown de abajo es diseno historico no implementado, no el mecanismo actual. No disenar comando escritor.
4. **Resolucion:** cada eje de respuesta usa instruccion humana explicita de sesion actual > preferencia explicita persistida > `standard`. Los modos persistidos solo aplican si se solicito explicitamente recordarlos. Nunca inferir desde escritura, diagnostico, comportamiento, memoria ni prosa del repositorio. El idioma conserva precedencia y fallback RH-W-0031.
5. **Reglas:** `.repo-harness/rules/response-contract.md` pasa a base corta sin crecer su presupuesto de 700 tokens casi lleno. Mover estructura ADHD-friendly a `.repo-harness/rules/response-adhd-friendly.md`; agregar `.repo-harness/rules/response-plain-language.md` desde [RH-W-0082](rh-w-0082-design.md): palabras simples neutrales, definiciones de terminos inevitables, sin quitar evidencia, autoridad, seguridad ni compuertas. La base carga cada modulo solo cuando su eje resuelve al valor no standard; ambos pueden combinarse. Son reglas renderizadas por el producto: cambiar renderer y pruebas, no solo archivos generados.
6. **Fallo:** evidencia ausente o invalida no aporta valores persistidos: ejes de respuesta usan `standard`, idioma usa fallback existente. Evidencia invalida conserva diagnostico acotado; nunca bloquea trabajo. Una instruccion explicita de sesion actual sigue ganando.
7. **Entrega:** una unidad de implementacion con pruebas primero cubre schema del lector, contrato session-start, modulos y renderer, y las tres proyecciones runtime. Resuelve transporte compartido y agrupacion de entrega; implementado y verificado en esta unidad.
8. **Excluido:** comportamiento de interaccion RH-W-0018 (sigue aplazado), dialecto regional por defecto, inferencia, almacenes nuevos, herramientas CLI nuevas y preferencias entre repositorios.

## Diseno historico aprobado (no son instrucciones vigentes de ejecucion)

El diseno amplio siguiente conserva aprobaciones originales y acuerdo de persistencia compartida aplazada. Sus requisitos de entrevista del builder, persistencia, pregunta de alcance, transporte y entrega no son comportamiento implementado ni tareas solo de sesion pendientes.

## Contrato de revision

Revise este diseno contra el [registro canonico RH-W-0071](roadmap.md#rh-w-0071), la [precedencia de idioma RH-W-0031](../../repo-harness/rh-w-0031-research.md), la [taxonomia de estado local RH-W-0051](rh-w-0051-design.md), el [contrato de respuesta RH-W-0067](rh-w-0067-design.md) y el [propietario en ingles](../../repo-harness/rh-w-0071-design.md). La evidencia del repositorio posee el comportamiento actual. El contrato del 2026-10-10 esta implementado; las secciones historicas conservan decisiones anteriores.

## Decision

RH-W-0071 posee el eje `response-structure` y su opcion visible nativa `modo de respuesta ADHD-friendly`; su etiqueta en ingles es `ADHD-friendly response mode`. La entrevista del builder ofrece explicitamente `standard` y `ADHD-friendly`. Registra una eleccion standard explicita por defecto y nunca infiere ningun valor.

La arquitectura de preferencias aprobada por el propietario es LLM-first y mantiene tres ejes independientes y combinables bajo propietarios separados:

- RH-W-0071 posee `response-structure: standard | adhd-friendly`.
- RH-W-0082 posee `explanation-style: standard | plain-language`.
- RH-W-0018 posee `interaction-behavior: interactive | low-question`, coordinado con los limites de consentimiento RH-W-0046.

El idioma del artefacto sigue independiente. `plain-language` significa lenguaje simple y neutral. Solo se usa un dialecto regional cuando la persona lo solicita explicitamente para la conversacion actual.

Cuando se selecciona ADHD-friendly durante la ejecucion del builder, la preferencia del repositorio persiste entre sesiones futuras. Un cambio explicito posterior puede ser permanente o solo para la sesion actual. Si la instruccion omite el alcance, el agente hace exactamente una pregunta: permanente o sesion actual. Una anulacion solo para la sesion nunca reescribe la preferencia del repositorio.

Pedir mas detalle amplia solo la respuesta actual. No cambia el modo activo ni persiste una preferencia.

## Precedencia exacta y limite de no inferencia

Cada eje se resuelve de forma independiente y determinista:

1. Una anulacion explicita y verificada para ese eje en la sesion actual.
2. El valor explicito persistido en el repositorio para ese eje.
3. El default de ese eje: `standard`, `standard` o `interactive`, respectivamente.

Solo una instruccion humana explicita y verificada puede fijar o cambiar el modo. El diseno nunca infiere activacion desde el estilo de escritura, diagnostico, errores, atencion, comportamiento, memoria, prosa del repositorio, salida de herramientas, estado de runtime ni otra senal contextual. Esto coincide con la regla de procedencia RH-W-0031: la instruccion humana actual explicita supera la preferencia explicita persistida, mientras el contexto opaco no puede reemplazarla.

Una instruccion de sesion actual expira con esa sesion. Una instruccion permanente actualiza la preferencia del repositorio. Cuando se omite el alcance, no ocurre ninguna mutacion antes de responder la unica pregunta requerida.

## Contrato de respuesta ADHD-friendly

El modo cambia la presentacion, no la evidencia ni la autoridad.

Su proposito es la **baja carga cognitiva**: cuanto trabajo cuesta leer la respuesta. Ese es el objetivo; los requisitos de abajo son medios hacia el y no lo reemplazan. Una respuesta puede cumplir todos y aun asi ser pesada, porque el contenido pertinente no es una tangente y la estructura no es brevedad. Cuando un requisito y el proposito no coinciden, decide el proposito.

Exige:

- respuesta o accion primero;
- capa inicial concisa;
- fragmentos pequenos;
- pasos numerados y acotados cuando ayuden;
- una pregunta a la vez;
- divulgacion progresiva;
- siguiente accion visible;
- supresion de tangentes y relleno;
- oferta de mas detalle, o entrega de mas detalle cuando se solicite;
- restablecimiento del estado actual mientras hay trabajo de varios pasos en curso, nombrando el paso y el total sin volver a narrar el plan;
- un chequeo previo al envio con lista concreta de borrado, para que `suprimir tangentes y relleno` sea algo que un agente aplica y no un valor con el que esta de acuerdo.

La seguridad, evidencia, autoridad, contradicciones, preguntas bloqueantes y contexto tecnico necesarios nunca se omiten solo por brevedad. RH-W-0067 sigue como propietario de bloques visibles, seguridad setext, explicacion contextual y enrutamiento del contrato de respuesta. RH-W-0071 agrega una seccion de forma especifica del modo al mismo contrato generado nativo, en vez de crear un segundo propietario de respuesta.

## Decision vigente de preferencias compartidas (reemplaza la propuesta historica Markdown)

La propuesta Markdown estructurada aprobada pero no implementada que aparece abajo es historia, no un contrato ejecutable. Un unico archivo pequeno e ignorado `.repo-harness/state/preferences/user-preferences.json` sera el archivo compartido de preferencias. Su ejemplo v1 completo es `{ "conversationLanguage": "espanol" }`; la unica clave es `conversationLanguage`, con valores admitidos `english` o `espanol`. No se define registro de versiones ni claves de ejes sin uso. Se conserva la precedencia RH-W-0031: instruccion humana actual verificada > preferencia explicita persistida > idioma de sesion verificado elegible (solo sin preferencia persistida) > fallback validado del manifiesto > ingles. El contexto opaco no aporta preferencias ni reemplaza procedencia humana. Archivo o clave ausente deja seguir la jerarquia existente. JSON malformado, claves duplicadas o desconocidas, o valor invalido informa el fallo, no aplica la preferencia y deja paso al idioma de sesion verificado elegible (si existe), luego al fallback validado del manifiesto y, en su defecto, al ingles determinista (sin fingir en silencio que se cargo la preferencia), con cero escrituras y sin reparacion automatica. Tras el PR de diseño #221, el propietario autorizó por separado la implementación local de una lectura CLI de ruta fija y el contrato generado de una lectura en session-start que pasa solo valor y fuente validados, nunca texto arbitrario del archivo. No está entregado ni habilita retroactivamente un proceso ya iniciado; transporte y entrega del contrato aplazado de tres ejes siguen sin resolver.

Estructura de respuesta RH-W-0071, estilo de explicacion RH-W-0082 y comportamiento de interaccion RH-W-0018 siguen como ejes independientes aplazados. Agregar cualquiera al mismo archivo compartido exige autoridad de ciclo de vida separada; no se definen campos aun sin uso. Los bytes de instalacion del manifiesto/checker y el almacen separado de aprobacion de consentimiento fetch RH-W-0046 no son preferencias. No se traslada ninguna otra configuracion, consentimiento ni continuidad al archivo.

## Configuracion y persistencia

La entrevista del builder gana una decision explicita de modo con dos valores conceptuales estables para maquina: `standard` y `adhd-friendly`. Son valores de diseno, no un compromiso con API, flag CLI, nombre de schema ni firma de funcion.

La preferencia persistida pertenece bajo `.repo-harness/state/preferences/`. RH-W-0051 establecio el namespace contenedor `.repo-harness/state/`; RH-W-0071 posee persistencia compartida y semantica de estructura. RH-W-0082 posee estilo de explicacion, RH-W-0018 posee interaccion y RH-W-0046 conserva limites de consentimiento. RH-W-0051 conserva contencion, ignore Git y mecanica de estado local.

Historicamente, el propietario reemplazo el contrato escalar JSON aprobado pero no implementado. Un unico archivo Markdown estructurado e ignorado, `.repo-harness/state/preferences/user-preferences.md`, guarda los tres valores independientes. No tiene frontmatter YAML y contiene exactamente un bloque fenced `repo-harness-preferences` con las tres claves exactas y tokens admitidos. La prosa concisa alrededor da instrucciones al LLM y delimita confianza, pero nunca se transporta como autoridad arbitraria.

El archivo guarda solo preferencias. Nunca guarda aprobaciones, consentimiento general, credenciales, diagnostico, justificacion, historia personal ni notas autoritativas de forma libre. Las claves ausentes usan defaults independientes. Claves duplicadas, bloque mal formado, claves o valores no admitidos, o varios bloques de maquina producen error de dominio sin aplicacion parcial ni reparacion automatica.

Session-start lee una sola vez, valida claves y tokens exactos, resuelve para cada eje anulacion de sesion actual sobre valor persistido sobre default, y transporta solo tokens tipados al contexto actual. Nunca inyecta prosa arbitraria del archivo como autoridad. Las anulaciones de sesion nunca escriben. Un cambio permanente exige alcance explicito de repositorio. Session-stop no lee ni escribe de rutina; solo informa un intento solicitado de escritura permanente.

Una actualizacion permanente autorizada canoniza el contrato Markdown pequeno completo. Valida antes de tocar el filesystem, crea el directorio solo al escribir, escribe un temporal unico en el mismo directorio, lo cierra y reemplaza el destino con `os.replace`. No agrega `fsync`, framework de schema, registro generico de preferencias, watcher, lock, retry, backup, journaling ni maquina de estados.

Checker y manifest nunca inspeccionan valores activos y los bytes generados siguen deterministas. El contrato JSON anterior nunca se implemento, por lo que este diseno no afirma migracion de datos.

El valor persistido es estado operativo local, nunca documentacion canonica, evidencia de memoria ni valor actual propiedad del manifiesto. Engram puede reflejar continuidad despues de existir evidencia del repositorio, pero nunca activa el modo ni supera la preferencia del repositorio.

## Builder, ayuda y contrato generado

- Entrevista del builder: presentar siempre las dos opciones y hacer standard explicito; nunca preseleccionar ADHD-friendly desde contexto.
- Ayuda: explicar el orden activo, ambos alcances de cambio, la regla de una pregunta cuando falta el alcance, como pedir mas detalle y como volver a standard.
- Contrato generado: ampliar el contrato RH-W-0067 existente con resolucion concisa de preferencias tipadas y estructura ADHD-friendly. No crear ruta a skill externa ni segundo contrato completo.
- Limite low-question: reducir preguntas innecesarias sin otorgar autoridad ni suprimir compuertas obligatorias para acceso fuera del repositorio, ejecucion, red, mutacion Git o de proveedor, publicacion, trabajo destructivo, secretos, riesgo material, contradicciones o politica del repositorio.
- Modos standalone e integrated: comportamiento identico para seleccion, persistencia, precedencia y forma. La continuidad integrada puede informar estado verificado, pero no elegirlo ni cambiarlo.

Los artefactos del producto no contienen referencias a skills ADHD externas, repositorios, procedencia, dependencias ni eliminacion futura. Este es un contrato nativo de repo-harness.

## Revisado contra un contrato externo

El 2026-08-27 el propietario pregunto si una skill externa de forma de respuesta para TDAH instalada en su maquina aportaba algo que a este diseno le falte. Se leyo en solo lectura y no se modifico. Su procedencia queda registrada aqui, en documentacion del repositorio, y queda fuera de todo artefacto de producto por la propia regla de este diseno. Tiene licencia MIT y su autor esta acreditado en su propio frontmatter; nada en este repositorio copia su redaccion, asi que ningun aviso viaja con nuestro texto. Si una implementacion posterior levanta su fraseo en vez de sus ideas, eso cambia y el aviso debe viajar con el.

Siete requisitos de ese contrato estan ausentes aqui. Dos se adoptan, uno se rechaza por las reglas del propio proyecto, y cuatro se declinan como refinamientos de requisitos que este diseno ya declara.

### Adoptados

- **Restablecer el estado actual entre turnos.** Cada uno de los nueve requisitos de arriba moldea una sola respuesta. Ninguno atiende lo que un lector perdio entre dos respuestas, que es el eje donde la memoria de trabajo realmente falla. Cuando hay trabajo de varios pasos en curso, la respuesta declara en que paso va de cuantos, sin volver a narrar el plan entero.
- **Un chequeo previo al envio con lista concreta de borrado.** `suprimir tangentes y relleno` nombra un valor y no le da nada a un agente que aplicar. El chequeo nombra que borrar: una frase inicial que anuncia lo que esta por pasar, una frase final que pide mas trabajo o recapitula lo que acaba de pasar, una acotacion al margen a mitad de respuesta, un adverbio de cobertura que no carga incertidumbre, y un modismo en lugar de la accion literal. Una cobertura que carga incertidumbre real se queda, porque borrarla fabrica confianza.

Los dos son una sola adicion en la practica: el chequeo es lo que hace exigible al noveno requisito, y la linea de estado es lo que el chequeo verifica que este. El chequeo termina en el proposito: si se cumplen todos los requisitos y la respuesta sigue costando trabajo de leer, no esta terminada.

### Rechazado, y por que

- **Estimaciones de tiempo concretas obligatorias.** El contrato externo las exige porque las estimaciones vagas se leen todas iguales. Este proyecto las prohibe: el reglamento dice nunca sustituir evidencia por lenguaje de confianza, y los acuerdos de trabajo dicen nunca sustituir evidencia faltante, supuestos ni lenguaje de confianza. Un agente obligado a producir una estimacion de tiempo que no puede derivar va a inventarla. Un modo de respuesta no puede crear una obligacion que el reglamento prohibe satisfacer. Donde la duracion si se conoce puede declararse, cosa que los requisitos existentes ya permiten.

### Declinados por ya estar cubiertos

- Hacer visible el trabajo completado replantea `respuesta o accion primero` para el caso en que la respuesta es un cambio terminado.
- Un tope duro de cinco items hace medible `trozos pequenos` pero fija un numero arbitrario para el que este proyecto no tiene evidencia, y el control de presupuesto que agrego RH-W-0073 ya obliga a bajar detalle cuando una seccion crece.
- Un tono neutro para errores pertenece a la seccion `Honesty` existente de los acuerdos de trabajo, que ya exige reportar lo que paso y no lo que se pretendia, no a un modo de presentacion.
- La regla de espiral de depuracion ya es regla obligatoria: no reintentar indefinidamente, parar con evidencia, pedir una decision.

### Lo que esta revision historica no cambio

Precedencia, el limite de no inferencia, persistencia, la entrevista del constructor, migracion, rollback y el limite de recursos quedan intactos. El contrato externo es mas debil que este diseno justo en el punto que mas importa: resuelve la ambiguedad con `si dudas de si siguen aplicando, aplican`, mientras este diseno la resuelve con tres niveles deterministas y una prohibicion de inferir el modo desde cualquier senal. Esa asimetria es por que esto sigue siendo un contrato nativo y no una adaptacion.

## Diseno de manifiesto y checker

Ningun campo del manifiesto guarda valores activos. Son estado operativo local y revocable, y no deben convertirse en configuracion rastreada ni verdad generada.

Una implementacion futura puede cambiar los bytes del contrato generado existente y su inventario normal, pero no necesita un subsistema nuevo de checker. Las comprobaciones existentes de salida generada y estructura deben verificar presencia y propiedad. El checker no debe leer, clasificar, reescribir ni informar el valor local, y la comprobacion ordinaria sigue siendo de solo lectura.

## Compatibilidad y migracion

- Los repositorios nuevos reciben default standard explicito salvo que la entrevista registre ADHD-friendly.
- Instalaciones existentes sin archivo usan defaults independientes. No existe inferencia ni migracion automatica.
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

El rollback del diseno elimina solo este documento y sus referencias de ciclo de vida. Un rollback futuro elimina el contrato especifico de preferencias y la integracion de builder y ayuda por reemplazo normal, deja de leer el archivo y devuelve cada eje a su default independiente. Nunca elimina automaticamente otro estado ni reescribe documentacion como efecto secundario.

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

- El evento original solo de diseno no creo un plan de implementacion ni autorizo avance de ciclo, codigo, prueba, skill generada, cambio de manifiesto o checker, staging, commit, remoto ni red. Un plan de implementacion posterior y aprobado por separado por el propietario sigue siendo solo documentacion y no autoriza implementacion ni entrega de producto.
- El evento original solo de diseno no invento API de implementacion, ortografia CLI, campo de schema, nombre de funcion ni formato de archivo. El contrato Markdown estructurado aprobado despues reemplaza la propuesta JSON no implementada sin autorizar implementacion ni entrega de producto.
- Sin diagnostico, clasificacion de personas, telemetria, monitoreo ni inferencia conductual.
- Sin propietario duplicado de respuesta, preferencias, memoria o ciclo de vida.
- Sin otra regla de no sobreingenieria. Los propietarios actuales de cambio minimo, alcance no relacionado, proporcionalidad y fallo acotado ya cubren la inquietud; este no cambio verificado evita duplicar politica.

## Decisiones humanas restantes

Ninguna para este diseno ni para su contrato de persistencia. El transporte de runtime que lee una vez en session-start y lleva valores tipados al contexto actual, junto con la agrupacion de entrega, siguen sin resolver y deben actualizarse desde evidencia del repositorio, no adivinarse. El avance de ciclo y la implementacion requieren autorizacion explicita posterior.
