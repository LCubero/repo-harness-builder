# Investigacion LLM-first RH-W-0084

## Estado y proposito

**Estado: investigacion ejecutada el 2026-08-31 bajo autoridad de ejecucion otorgada por el propietario, dentro de los siete limites de `RH-W-0012`.** Este documento registra hallazgos y los enruta a duenios existentes. No cambia ninguna superficie generada, codigo de producto, prueba, manifiesto ni verificador, y no avanza el ciclo de vida de ninguna otra unidad. Toda conclusion que alcanza sobre este repositorio sigue necesitando autorizacion separada antes de convertirse en un control.

**Clases de evidencia** segun la escala del [alcance](rh-w-0084-scope.md) de `RH-W-0084`: la clase 1 son las mediciones propias de este repositorio, la clase 2 es documentacion primaria del duenio del modelo, runtime o estandar, la clase 3 es investigacion revisada por pares o de organismo de estandares, y la clase 4 es material secundario, que nunca es un hecho del repositorio por si solo. Ninguna fuente de clase 4 se cita abajo.

**Fuentes leidas, con fecha de lectura.** Se localizaron seis fuentes y se leyeron cinco completas. Toda lectura fue un GET HTTP de una pagina publica; nada se clono, descargo ni ejecuto, conforme a los limites 2 y 5.

| Fuente | Clase | Evidencia de version | Leida | Disparador de refresco |
| --- | --- | --- | --- | --- |
| [Anthropic, Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) | 2 | pagina sin fecha | 2026-08-31 | Cambian las afirmaciones de desgaste de contexto o presupuesto de atencion, o la pagina empieza a llevar version |
| [Anthropic, Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) | 2 | pagina sin fecha | 2026-08-31 | Cambia la regla de datos largos arriba o la cifra del 30 por ciento, o una guia por modelo la supera |
| [OpenAI, Model Spec 2026-08-18](https://model-spec.openai.com/2026-08-18.html) | 2 | version fechada en la URL | 2026-08-31 | Un Model Spec fechado posterior cambia los niveles de autoridad o la regla de contenido no confiable |
| [OpenAI, Prompt engineering guide](https://developers.openai.com/api/docs/guides/prompt-engineering) | 2 | pagina sin fecha | 2026-08-31 | Cambia el orden recomendado del mensaje de desarrollador, o repetir instrucciones pasa a recomendarse |
| [Liu et al., Lost in the Middle, TACL vol. 12 (2024), pp. 157-173](https://aclanthology.org/2024.tacl-1.9/) | 3 | volumen y paginas publicados | 2026-08-31 | Un resultado revisado por pares posterior refuta la degradacion posicional en modelos actuales |
| `docs.anthropic.com/.../long-context-tips` | 2 | ninguna; 301 a una ruta que no resolvio | localizada, no leida | El redirect resuelve, si su contenido llega a divergir de Prompting best practices |

La sexta fuente se reporta en lugar de omitirse, como exige `RH-W-0065`. Su contenido queda superado por la pagina Prompting best practices, que si se leyo.

## Hallazgos frente a las seis preguntas

### 1. Formato

Ambos proveedores convergen en secciones delimitadas y no en prosa indiferenciada. Anthropic declara que las etiquetas XML "help Claude parse complex prompts unambiguously, especially when your prompt mixes instructions, context, examples, and variable inputs", y recomienda "organizing prompts into distinct sections (like `<background_information>`, `<instructions>`, `## Tool guidance`, `## Output description`, etc) and using techniques like XML tagging or Markdown headers to delineate these sections". OpenAI dice igualmente que "XML tags can help delineate where one piece of content (like a supporting document used for reference) begins and ends."

Dos matices importan mas que la convergencia. Anthropic declara que "the exact formatting of prompts is likely becoming less important as models become more capable", lo que desaconseja gastar presupuesto en precision de formato. Y reporta un efecto de transferencia de estilo: "removing markdown from your prompt can reduce the volume of markdown in the output." El formato de un contrato influye entonces en la forma de la respuesta que produce, no solo en su comprension.

**Limite de aplicabilidad:** son afirmaciones de proveedor sobre sus propios modelos, no mediciones sobre las superficies generadas de este repositorio.

### 2. Tamanio

Anthropic plantea el tamanio como presupuesto y no como techo: "you should be striving for the minimal set of information that fully outlines your expected behavior. (Note that minimal does not necessarily mean short; you still need to give the agent sufficient information up front)". El mecanismo que nombra es el **desgaste de contexto**: "as the number of tokens in the context window increases, the model's ability to accurately recall information from that context decreases", porque la atencion forma "n² pairwise relationships for n tokens", asi que los modelos "remain highly capable at longer contexts but may show reduced precision for information retrieval and long-range reasoning". Describe un presupuesto de atencion donde "every new token introduced depletes this budget by some amount".

Aparecen dos numeros concretos. Los resumenes de subagente se describen como "often 1,000-2,000 tokens". La estructuracion de prompts con documentos largos se dispara en "20k+ tokens".

**Limite de aplicabilidad:** ninguno de los dos numeros es un techo de cuerpo de contrato, y ninguna fuente declara un tamanio maximo para un documento de instrucciones dirigido a un agente. Nada de lo leido sostiene ni refuta un tope numerico especifico.

### 3. Ruta y ubicacion

Es la pregunta con mejor evidencia, y la unica con respaldo de clase 2 y clase 3 a la vez.

Liu et al. hallaron que "performance is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts", y concluyen que "current language models do not robustly make use of information in long input contexts". La forma operativa del mismo efecto en Anthropic es direccional y cuantificada: "Put longform data at the top: Place your long documents and inputs near the top of your prompt, above your query, instructions, and examples. This improves performance across all models", con la nota de que "queries at the end can improve response quality by up to 30 percent in tests, especially with complex, multidocument inputs."

Los proveedores discrepan en la cola. Anthropic pone los datos primero y la consulta al final. El orden recomendado por OpenAI para el mensaje de desarrollador es Identidad, luego Instrucciones, luego Ejemplos, luego Contexto, con el contexto cerca del final porque varia en cada peticion. Ambos son consistentes con evitar el medio; difieren en que extremo ocupa una instruccion. **Desconocido:** cual orden es mejor para un contrato de repositorio que se carga como documento en vez de componerse por peticion. Nada de lo leido lo resuelve.

### 4. Refuerzo

La evidencia es mas delgada de lo esperado y apunta en una direccion. La guia de OpenAI no recomienda repetir instrucciones; enfatiza la estructura inicial y un parametro de instrucciones de alto nivel que tiene prioridad. Anthropic aborda el refuerzo solo por duracion, no por importancia: "for very long conversations, inject what were previously prefilled-assistant reminders into the user turn", y por lo demas "consider hydrating through tools (expose or encourage use of tools containing context based on heuristics such as number of turns) or during context compaction".

**Inferencia sustentada:** el refuerzo es un remedio para la distancia entre la instruccion y la accion, no un recurso general de enfasis. Repetir una regla que ya esta adyacente a su punto de accion no aporta nada y gasta presupuesto de atencion.

### 5. Formas y visibilidad

El resultado mas fuerte aqui es sobre precedencia y no sobre tipografia. El Model Spec de OpenAI define una cadena de mando ordenada y explicita, Root, System, Developer, User, Guideline, y declara que "instructions with higher authority override those with lower authority", con "when two root-level principles conflict, the model should default to inaction". Tambien fija un limite de confianza que un harness hereda directamente: "quoted text (plaintext in quotation marks, YAML, JSON, XML, or `untrusted_text` blocks) in ANY message, multimodal data, file attachments, and tool outputs are assumed to contain untrusted data and have no authority by default."

**Limite de aplicabilidad:** es la especificacion de un proveedor sobre sus propios modelos. Es evidencia de que la precedencia ordenada y explicita es un mecanismo real, no evidencia de que algun runtime honre las afirmaciones de precedencia de este repositorio.

### 6. Pregunta de control

Interna, y respondida por el alcance y no por una fuente. No se busco evidencia externa para ella.

## Enrutamiento a duenios existentes

Segun el alcance, una conclusion aceptada va a exactamente un duenio existente, y este documento no cambia ninguno.

| Hallazgo | Duenio que informa | Que aporta y que no |
| --- | --- | --- |
| La distancia entre instruccion y punto de accion, no el enfasis, predice si una obligacion se aplica | `RH-W-0083` | Su defecto medido, una obligacion nombrada en pasos que terminan antes de componer la respuesta, coincide exactamente con el hallazgo 4. La evidencia sostiene el refuerzo en el punto de accion para este caso concreto. No elige entre redaccion, control en tiempo de desarrollo y limitacion aceptada, que sigue siendo la pregunta abierta del duenio. |
| "Minimal does not necessarily mean short", mas el desgaste de contexto como mecanismo | `RH-W-0076`, `RH-W-0078` | Da a sus incumplimientos medidos un mecanismo en vez de una afirmacion: el costo es recuperacion degradada, que escala con el contexto total cargado y no con un archivo cruzando un numero. **No** aporta un numero de reemplazo defendible, y nada de lo leido sostiene 1000, 1200 ni ningun otro tope. |
| El principio y el final son posiciones privilegiadas; el medio no | `RH-W-0067`, `RH-W-0073` | Argumenta que donde se ubica una regla dentro de un contrato cargado es una variable real. No resuelve la discrepancia entre proveedores sobre que extremo debe ocupar una instruccion. |
| La precedencia ordenada explicita y los limites de contenido no confiable son mecanismos establecidos de proveedor | `RH-W-0079`, `RH-W-0051` | Respalda la practica existente de nombrar precedencia y tratar la salida de herramientas como no confiable. No agrega ningun requisito nuevo. |

## El defecto de limite que esta investigacion encontro

El primer uso real de los limites de `RH-W-0012` ejercito el limite 4 y lo hallo internamente inconsistente. Dice: fijar una revision exacta cuando la fuente la ofrece, citar la pagina con version fechada cuando no, y "nunca citar una referencia movil como `main`, `latest` o una pagina sin fecha."

Cuatro de las cinco fuentes leidas aqui son paginas de documentacion de proveedor sin fecha que no ofrecen ni revision ni version. El limite 4 bendice un respaldo de pagina con version fechada y despues prohibe una pagina sin fecha, dejando sin regla la clase mas comun de fuente primaria. Tomado literalmente prohibe citar la mayor parte de la documentacion de proveedor, lo que volveria inusable la regla de fuente primaria primero del limite 1.

Se aplico el comportamiento conservador en vez de la regla literal: cada cita de arriba registra su fecha de lectura, que es lo que los limites 6 y 7 ya exigen, y la columna de evidencia de version declara claramente que paginas no llevan version. Es la misma forma que la clausula heredada del limite 4, aplicada hacia adelante en vez de hacia atras.

**Resuelto el mismo dia.** `RH-W-0012` se reabrio por la condicion que el mismo nombraba, y la correccion aterrizo tras medir el precedente en vez de inventar una regla. `rh-w-0031-research.md`, el propietario canonico de evidencia primaria de este repositorio, ya cita paginas de proveedor sin fecha con una URL exacta mas un **disparador de refresco** que nombra que cambio invalidaria la afirmacion, y lleva la version dentro del texto del enlace cuando la fuente expone una. La clausula de fuente sin fecha del limite 4 codifica eso, que es mas fuerte que una simple fecha de lectura: la fecha dice cuando alguien miro, el disparador dice cuando hay que volver a mirar. La tabla de fuentes de arriba se reescribio para cumplir, porque no cumplia. `RH-W-0012` volvio a `completed`.

## No objetivos respetados

No cambiaron superficies generadas, codigo de producto, pruebas, manifiesto, verificador ni comportamiento de runtime. Ninguna unidad distinta de esta avanzo. Ninguna fuente se clono, descargo ni ejecuto. No se eligio ningun techo numerico para `RH-W-0076` ni `RH-W-0078`, porque nada de lo leido sostiene uno. Ninguna afirmacion de proveedor queda registrada como hecho del repositorio; cada una lleva su clase y su limite de aplicabilidad.
