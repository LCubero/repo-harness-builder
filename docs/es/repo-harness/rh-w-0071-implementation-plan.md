# Plan de implementacion RH-W-0071

> **Estado: `proposed`. Este plan requiere aprobacion separada del propietario, y la implementacion que describe requiere su propia autorizacion despues de esa.**

Derivado del [diseno RH-W-0071](rh-w-0071-design.md) aprobado por el propietario y del Plan 20 de la hoja de ruta. Su par en ingles es [rh-w-0071-implementation-plan.md](../../repo-harness/rh-w-0071-implementation-plan.md).

## Evidencia del repositorio confirmada antes de planear

Verificado en `c4387e4`, no arrastrado del diseno:

| Afirmacion | Evidencia | Estado |
|---|---|---|
| `state/` es un espacio de estado local reservado | `manifest.json` tiene `localState.state` como `{"class": "operational", "path": ".repo-harness/state/"}` | verificado |
| El espacio esta ignorado por Git | `.gitignore:9` lleva `/.repo-harness/state/` | verificado |
| El checker valida el conjunto de espacios | `harness_tool.py:2339` compara `manifest["localState"] != LOCAL_STATE` | verificado |
| `state/preferences/` existe como ruta reservada | la cadena `preferences` aparece **cero** veces en `harness_tool.py`, y `.repo-harness/state/` no existe en disco | **no verificado** |
| RH-W-0067 posee el contrato de respuesta | `.repo-harness/rules/response-contract.md` lo posee `render` y mide 436 de 700 | verificado |
| Existe fila de compuerta hacia el contrato de respuesta | `REPO-HARNESS.md` enruta a `.repo-harness/rules/response-contract.md` | verificado |

### Una correccion que lleva este plan

El Plan 20 dice que "la completada RH-W-0051 implemento la reserva `state/preferences/` que este plan consume", y el registro RH-W-0071 dice que "la reserva de almacenamiento de preferencias de RH-W-0051 esta probada". Ambas exageran. RH-W-0051 reservo `state/`, no `state/preferences/`.

Nada se rompe: `/.repo-harness/state/` en `.gitignore` ya cubre toda subruta, y la propia division de propiedad del diseno dice que RH-W-0051 posee "ruta, contencion, ignorado de Git y mecanica de estado local" mientras RH-W-0071 posee "el valor del modo de respuesta y su semantica de actualizacion". Asi que `state/preferences/` es una subruta de esta unidad dentro de un espacio ya reservado e ignorado. Las dos frases de arriba se corrigen en la misma unidad de trabajo en vez de quedar como precision falsa.

## Fundamento externo

El diseno declara los requisitos de forma ADHD-friendly sin citar fuente. No estan inventados: siete de los nueve corresponden a patrones nombrados de W3C COGA. Queda registrado aqui, en documentacion del repositorio, y deliberadamente fuera de todo artefacto de producto, porque el diseno prohibe referencias externas alli.

| Requisito del diseno | Patron W3C COGA |
|---|---|
| capa inicial concisa | 4.4.5 Keep Text Succinct |
| trozos pequenos | 4.4.10 Use White Spacing |
| divulgacion progresiva | 4.9.3 Support Simplification |
| siguiente accion visible, pasos numerados acotados | 4.2.4 Make Each Step Clear |
| suprimir tangentes y relleno | 4.6.1 Limit Interruptions |
| oferta de mas detalle | 4.8.2 Provide Alternative Content for Complex Information |
| una pregunta por vez | 4.5.4 Design Forms to Prevent Mistakes |

La regla de una pregunta por vez ademas coincide con el patron "one thing per page" de GOV.UK, cuya justificacion declarada es reducir la carga cognitiva.

**Lo que la evidencia no respalda, dicho sin adornos.** El troceado como acomodacion especifica para TDAH tiene respaldo empirico mixto; al menos un estudio controlado de trabajo troceado no encontro diferencia significativa de productividad. La base de evidencia de la divulgacion progresiva es en gran medida de casos practicos y no de ensayos controlados, y GOV.UK mismo dice que "one thing per page" no es universalmente obligatorio y debe validarse por contexto.

Esto no debilita la unidad, porque el diseno nunca hace una afirmacion clinica. El modo es una preferencia de presentacion pedida explicitamente que el diseno prohibe inferir de diagnostico, conducta o cualquier otra senal. La implementacion debe mantenerlo asi: ningun artefacto de producto puede describir el modo como beneficio terapeutico o clinico.

## Cortes

Cuatro cortes, cada uno entregable de forma independiente, en este orden. Cada uno es un commit separado; los cortes 1 y 2 podrian entregarse sin el resto y dejar el producto coherente.

### Corte 1: resolucion y persistencia, sin interfaz

Lo mas pequeno que puede ser cierto primero: un resolutor de modo determinista y un archivo de preferencia, sin que nada los llame todavia.

1. RED: la resolucion devuelve `standard` explicito cuando no existe preferencia y no hay instruccion de sesion.
2. RED: una preferencia `adhd-friendly` persistida resuelve a `adhd-friendly` en una resolucion posterior sin instruccion de sesion.
3. RED: una instruccion de sesion explicita y verificada supera a la preferencia persistida y no la reescribe.
4. RED: cada senal de inferencia prohibida que nombra el diseno deja la resolucion sin cambios. Un caso por senal: estilo de escritura, diagnostico declarado, tasa de errores, lenguaje sobre atencion, conducta, contenido de memoria, prosa del repositorio, salida de herramientas, estado de runtime.
5. Implementar el resolutor como funcion pura sobre tres entradas explicitas. Sin vigilancia de archivos, sin clasificador, sin llamada a modelo.
6. Implementar lectura y escritura de la preferencia bajo `.repo-harness/state/preferences/`, creando el directorio solo al escribir. Un archivo ausente no es error y resuelve a `standard` explicito.

### Corte 2: el contrato generado

7. RED: el contrato de respuesta generado contiene una seccion de resolucion de modo y una seccion de forma ADHD-friendly, y `.repo-harness/rules/response-contract.md` sigue midiendo por debajo de `RULEBOOK_TOKEN_BUDGET` despues. Hoy esta en 436 de 700, asi que la seccion de forma tiene unos 198 tokens proxy de margen antes de tener que bajar detalle en vez de subir el presupuesto.
8. RED: el contrato declara la precedencia de tres niveles, ambos alcances de cambio, la regla de una pregunta cuando falta el alcance, como pedir mas detalle y como volver a `standard`.
9. RED: seguridad, evidencia, autoridad, contradicciones, avisos bloqueantes y contexto tecnico necesario quedan nombrados como no omitibles, de modo que la brevedad no pueda borrarlos.
10. Extender el contrato RH-W-0067 existente. No agregar segundo propietario de respuesta, ni archivo de reglas, ni fila de compuerta.
11. Sincronizar las tres proyecciones de runtime y confirmar que el checker informa `current` con cero escrituras.

### Corte 3: entrevista del constructor y ayuda

12. RED: la entrevista presenta exactamente dos opciones y registra `standard` de forma explicita cuando la persona lo elige, nunca por omision.
13. RED: la entrevista nunca preselecciona `adhd-friendly` desde ningun contexto.
14. RED: la ayuda explica el orden de resolucion, ambos alcances, la regla de una pregunta, el comportamiento de mas detalle y la ruta de vuelta a `standard`.
15. Implementar ambas, en ambos locales de documentacion.

### Corte 4: pregunta de alcance, migracion, rollback

16. RED: una instruccion de cambio que omite alcance produce exactamente una pregunta permanente-o-solo-esta-sesion y no muta nada antes de responderse.
17. RED: un pedido de mas detalle expande una sola respuesta y deja el modo y la preferencia persistida sin cambios.
18. RED: una instalacion existente sin preferencia resuelve a `standard` explicito, sin migracion y sin inferencia.
19. RED: el rollback deja de leer la preferencia y devuelve la resolucion a `standard` explicito sin borrar estado de preferencias no relacionado.
20. RED: standalone e integrado se comportan igual en seleccion, persistencia, precedencia y forma.

## Controles que no deben agregarse

Nombrados aqui para que un lector posterior no los reabra:

- Sin campo de manifiesto para la preferencia activa. Es estado local revocable, no configuracion rastreada.
- Sin lectura, clasificacion, reescritura ni reporte del valor de la preferencia por parte del checker. El chequeo sigue de solo lectura.
- Sin clasificador, llamada a modelo, subagente, medidor de legibilidad, parser, demonio, monitor, telemetria ni fan-out. La resolucion es una consulta mas prosa determinista.
- Sin segundo propietario del contrato de respuesta. RH-W-0067 conserva ese rol.
- Sin ninguna ruta de inferencia, y sin via por la que pudiera agregarse despues sin reabrir esta unidad.

## Criterios de aceptacion

- La resolucion es determinista en los tres niveles y comprobablemente inmutable ante cada senal prohibida, afirmado por senal y no en agregado.
- Una instruccion de solo sesion nunca reescribe la preferencia del repositorio, y un pedido de mas detalle nunca cambia el modo.
- Un alcance omitido produce exactamente una pregunta y ninguna mutacion.
- El contrato generado lleva resolucion de modo y forma ADHD-friendly, nombra las categorias no omitibles, y `response-contract.md` sigue bajo `RULEBOOK_TOKEN_BUDGET`.
- El constructor registra una eleccion `standard` explicita por defecto y nunca preselecciona `adhd-friendly`.
- No aparece campo de manifiesto, estado de checker ni cambio de esquema. `render()` y `check()` quedan identicos byte a byte para toda entrada que no seleccione el modo.
- Las tres proyecciones de runtime quedan identicas byte a byte a la canonica y el checker informa `current` con cero escrituras.
- Ambos locales de documentacion se reconcilian en la misma unidad de trabajo.
- Ningun artefacto de producto describe el modo como beneficio clinico o terapeutico, y ninguno referencia una skill, repositorio o dependencia externa de TDAH.

## No objetivos

Sin diagnostico, clasificacion, telemetria ni monitoreo conductual. Sin arquitectura de consentimiento, que queda en RH-W-0046 y la diferida RH-W-0018. Sin enrutamiento de memoria, que queda en RH-W-0066. Sin trabajo general de seguridad de recursos, que queda en RH-W-0070. Sin cambio a la precedencia de idioma de RH-W-0031 mas alla de reutilizar su regla de procedencia. Sin evaluacion con modelo en vivo y sin suite de verificacion costosa.

## Riesgos

- **La seccion de forma compite por un presupuesto compartido.** `response-contract.md` tiene unos 198 tokens proxy de margen. Si la seccion de forma necesita mas, baja detalle; el presupuesto no sube. La asercion de RH-W-0073 ya lo exige de forma automatica.
- **Las pruebas de senales prohibidas son toda la garantia de no inferencia.** Afirmadas por senal y no en agregado, porque una asercion agregada pasa mientras una senal se filtra.
- **El corte 3 toca la entrevista del constructor**, que es la superficie de la que depende una instalacion. Va despues de los cortes 1 y 2 para que el comportamiento que expone ya exista y ya este afirmado.

## Decisiones humanas requeridas antes de implementar

1. Aprobar este plan.
2. El formato del archivo persistido y su nombre exacto bajo `.repo-harness/state/preferences/` quedan abiertos por el diseno a proposito. Este plan no los inventa; la implementacion propone uno contra evidencia del repositorio y el propietario lo acepta.
3. Si los cortes 1 y 2 pueden entregarse antes que los cortes 3 y 4, o si la unidad aterriza entera.
