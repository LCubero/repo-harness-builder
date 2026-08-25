# Diseno RH-W-0070 de seguridad preventiva de recursos

## Decision

RH-W-0070 define seguridad preventiva y proporcional de recursos para operaciones de agentes y del harness. Una revision delegada observada llevo la CPU cerca de saturacion, pero la evidencia disponible no verifica una causa raiz. La politica controla la forma y el alcance del lanzamiento sin atribuir el incidente a un comando, herramienta o subsistema especifico.

Revise este diseno contra el [registro canonico RH-W-0070](roadmap.md#rh-w-0070), los [acuerdos de trabajo](../../../.repo-harness/rules/working-agreements.md) y el [propietario en ingles](../../repo-harness/rh-w-0070-design.md). Esta es una unidad transversal y separada. Guia trabajo posterior sin fusionarse con RH-W-0051, RH-W-0071 ni otra implementacion.

## Autoridad y aplicabilidad

Las reglas del repositorio y las instrucciones explicitas actuales de la persona usuaria siguen siendo autoritativas. Este diseno agrega una politica preventiva para acciones iniciadas o delegadas por un agente o harness.

- Ediciones livianas de documentacion, lecturas acotadas, relectura y trabajo barato similar proceden sin pronostico ni ceremonia adicional.
- Trabajo largo, muy largo, costoso o de costo desconocido exige pronostico antes del lanzamiento.
- Una accion cuyo riesgo no pueda acotarse cierra en falso hacia un pronostico, una accion menor acotada o una decision humana.
- Se consideran CPU, GPU y RAM cuando sean observables y aplicables. La conciencia de GPU nunca crea un requisito de GPU.

El objetivo operativo es permanecer por debajo de 90 por ciento de utilizacion. Es un techo de planificacion, no una promesa de medicion exacta, control continuo o comportamiento identico entre hosts y proveedores.

## Entradas y precedencia

Las entradas son el resultado pedido, el alcance autorizado, la duracion esperada, la concurrencia y el tamano de lote esperados, la telemetria disponible de CPU, GPU y RAM, las senales de costo del proveedor cuando existan y la ruta de verificacion suficiente mas barata.

La precedencia es:

1. Limites y autorizacion explicitos de la persona usuaria dentro de las reglas de seguridad del repositorio.
2. Limites conocidos de la accion respaldados por evidencia actual.
3. Telemetria disponible de recursos o proveedor.
4. Limites estaticos conservadores cuando no exista telemetria.

La telemetria ausente se informa como no disponible. Nunca se infiere ni fabrica una medicion.

## Comportamiento preventivo

Antes de lanzar, se elige la accion menor que pueda establecer el resultado pedido:

- Usar alcance enfocado en vez de scans amplios o recursivos.
- Acotar concurrencia, workers y tamano de lote.
- Lanzar una vez por accion solicitada.
- Fijar timeout explicito para trabajo potencialmente largo.
- Usar la verificacion suficiente mas barata y evitar scans, builds, pruebas y revisiones redundantes.
- Pronosticar duracion, concurrencia, presion probable de CPU, GPU y RAM, y costo del proveedor antes de trabajo largo o muy largo.
- Si el pronostico puede exceder la politica, pedir aprobacion explicita antes de lanzar y permitir rechazar o reducir el alcance.

Cuando exista telemetria, se usa solo como entrada para decidir el lanzamiento. Cuando no exista, se aplican limites estaticos conservadores y se declara la limitacion. Nunca se amplia alcance, se agregan reintentos ni se aumenta concurrencia porque falten mediciones.

## Salidas y fallos

Las acciones livianas producen su resultado normal. Una accion pronosticada informa alcance propuesto, limites, timeout, clase de duracion esperada, riesgo de recursos o costo, disponibilidad de telemetria y la decision humana requerida cuando aplique.

No obtener telemetria no es por si solo un fallo. No poder acotar una accion costosa o desconocida bloquea ese lanzamiento y devuelve una accion segura menor o un pronostico para aprobacion. Timeout, evidencia parcial o incertidumbre del proveedor se informan con honestidad y nunca se convierten en exito.

Ninguna accion puede terminar, limitar, cambiar prioridad ni interferir de otra forma con procesos ajenos.

## Criterios de aceptacion

- Documentacion liviana y relectura acotada siguen directas y sin ceremonia.
- Acciones largas, muy largas, costosas o desconocidas reciben pronostico antes del lanzamiento.
- Concurrencia, lote, lanzamientos, alcance y timeout son explicitos y acotados cuando apliquen.
- La verificacion elegida es la suficiente mas barata y evita trabajo costoso redundante.
- Se considera evidencia disponible de CPU, GPU y RAM sin prometer control exacto.
- Telemetria ausente se informa y se reemplaza con limites conservadores, nunca valores inventados.
- Un pronostico que pueda exceder la politica exige aprobacion y permite rechazar o reducir alcance.
- Los procesos ajenos permanecen intactos.

## Pruebas y evidencia

Cualquier implementacion futura debe comenzar con evidencia enfocada, no con una suite completa o recursiva. Los casos esperados cubren bypass liviano, campos de pronostico, un lanzamiento, timeout, fallback conservador sin telemetria, rechazo o reduccion de alcance y prueba de que nunca se apuntan procesos ajenos. Revision documental y relectura acotada bastan para este trabajo solo de diseno.

## No objetivos

Este diseno no agrega daemon, subsistema de monitoreo, planificador, dependencia, terminador de procesos, loop de reintentos, requisito GPU, API especifica de comandos, implementacion especifica de comandos, throttle universal, garantia exacta de utilizacion ni telemetria continua. No diagnostica el incidente observado ni autoriza implementacion.

## Migracion y compatibilidad

El diseno no cambia bytes del producto, schema persistido, salida generada, manifest, checker, dependencia de runtime ni aprobacion existente. Los flujos livianos actuales siguen compatibles. Las unidades futuras adoptan esta politica dentro de sus propios limites de ciclo y verificacion.

## Relacion de ciclo de vida

RH-W-0070 sigue `proposed` como unidad transversal separada. Puede guiar RH-W-0051, RH-W-0071, revision delegada y trabajo posterior, pero no fusiona su propiedad ni implementacion. No se requiere plan de implementacion ahora. Un plan posterior exige evidencia de que documentacion y politica operativa no bastan, mas autorizacion separada del propietario.
