# Diseno RH-W-0059: contrato de transporte no interactivo

<!-- doc-role:design -->

La fuente factual canonica es [`../../repo-harness/rh-w-0059-design.md`](../../repo-harness/rh-w-0059-design.md). Este es su espejo humano completo en espanol. Borrador pendiente de aprobacion del propietario; este documento no autoriza ninguna implementacion. El propietario del ciclo de vida es [RH-W-0059](roadmap.md#rh-w-0059).

## El defecto

Los contratos de sesion generados prometen un banner de remediacion al que no siempre pueden llegar.

**Hecho verificado.** `.repo-harness/skills/repo-harness-session-start/SKILL.md` exige una aprobacion antes del acceso a red, nombra `git fetch` e integracion solo fast-forward, y cierra su gate de sincronizacion con la obligacion de nombrar una ruta de credencial alternativa disponible despues de un fallo de transporte. El paso 12 de `repo-harness-session-stop/SKILL.md` lleva la misma obligacion para la ruta de push. Ninguno especifica como se invoca el comando de red. Las cadenas `BatchMode`, `GIT_TERMINAL_PROMPT`, `TTY` y `passphrase` no aparecen en ninguna habilidad generada ni en la hoja de ruta.

**Inferencia respaldada.** Frente a una llave SSH protegida por passphrase o una credencial HTTPS sin cache, `git fetch` abre un prompt. Un agente no tiene TTY, asi que nadie lo contesta. La sesion se cuelga sin error, sin codigo de salida, sin banner y sin timeout, de modo que la fila de fallo de transporte del gate nunca se ejecuta y la obligacion de ruta alternativa nunca se dispara. La evidencia es la ausencia de cualquier exigencia de no interactividad en el contrato, mas el harness externo revisado declarando la falla identica y declarando que exactamente esa configuracion existe en una de sus dos maquinas. Reproducirlo contra una llave con passphrase lo elevaria a hecho verificado.

**Por que es peor en el cierre.** `repo-harness-session-stop` llega a la red despues de guardar memoria y despues del commit. Un cuelgue ahi deja a la persona con una sesion congelada, un commit que puede ya existir, y ninguna salida que lo diga.

## Alcance: que dejan realmente por hacer los nueve hallazgos

La unidad se registro poseyendo nueve hallazgos. Al revisar cada uno contra el contrato actual en vez de asumirlo, cinco no necesitan nada aca:

| Hallazgo | Estado en este repositorio | Disposicion |
|---|---|---|
| `C3` invocacion no interactiva | Ausente. Ninguna habilidad generada nombra mecanismo alguno de supresion de prompt. | **En alcance.** Es el defecto completo. |
| `C7` limites documentados del mecanismo | Todavia no aplica: no hay mecanismo que acotar. | **En alcance**, condicionado a que aterrice `C3`. |
| `C5` forma corta del upstream entre comillas | El contrato nunca escribe `@{u}`, asi que no hay nada que corregir en el texto existente. Pero instruye al agente a comparar adelanto y atraso e integrar solo fast-forward, y un agente recurre a la forma corta para eso. | **En alcance** como riesgo adjunto a la instruccion de integracion, no como correccion de texto existente. |
| `C4` nunca una asignacion de entorno como prefijo de comando | Ya resuelto en otro lado por `RH-W-0035`, que separo por shell la asignacion inline solo POSIX en `references/validation.md`. Falta en el parrafo de transporte solo porque ese parrafo todavia no nombra ninguna variable de entorno. | **En alcance**, condicionado a que `C3` introduzca una. |
| `C1` respaldo ordenado con proveedor primero | El contrato conserva Git nativo como transporte central y las herramientas de proveedor como ruta alternativa opcional. Un respaldo ordenado con proveedor primero presupone un CLI de proveedor que este producto no debe exigir. Su fallo terminal ruidoso ya esta presente. | **Excluido**, a proposito. |
| `C2` URL HTTPS explicita para la ruta del proveedor | Misma razon: es mecanica de ruta de proveedor en un producto que no posee una. | **Excluido**, a proposito. |
| `C8` merge local en vez de un pull que vuelve a traer | Ya satisfecho: `git pull` ya esta prohibido y la integracion ya es solo fast-forward. Solo falta la razon, y la razon del harness externo no se transfiere, porque descansa en una ruta con proveedor primero que este producto no tiene. | **Fuera de alcance.** Se registra para que la brecha no se redescubra como nueva. |
| `C11` la configuracion de fin de linea puede fingir un cambio de contenido | Real y alcanzable localmente: `core.autocrlf` esta en `true` en este clon y `.gitattributes` fija `eol=lf`. Pero es un asunto de reporte de preparacion, no de transporte. | **Movido** a `RH-W-0060`. |
| `C12` una revision de estado incluye las ramas | Real, pero no es transporte. | **Movido** a `RH-W-0060`. |

El hallazgo `C10`, el `date` local pelado, estaba en un borrador anterior de este diseno y queda retirado. **No defecto verificado**: ninguna habilidad generada, ninguna regla del rulebook y ninguna linea del renderizador toma una fecha por un comando de shell, de modo que el riesgo que midio el harness externo no tiene camino alcanzable aca. Adoptarlo habria agregado una obligacion nueva disfrazada de correccion, y habria empujado a `REPO-HARNESS.md` mas alla de su propio techo, que hoy esta en 696 tokens de 700.

**Quedan cuatro clausulas, y tres de ellas existen solo porque existe la primera.**

## Las cuatro clausulas

### 1. La invocacion es no interactiva, y el contrato nombra el mecanismo

Todo comando de red autorizado corre con el prompt interactivo deshabilitado en las dos rutas que pueden abrir uno:

- SSH: `-o BatchMode=yes`, pasado por `git -c core.sshCommand=` para que se parsee igual en toda shell.
- El prompt de credencial propio de Git: `GIT_TERMINAL_PROMPT=0`, puesto en el entorno, porque no tiene equivalente `-c`.
- Un host inalcanzable: `-o ConnectTimeout=20`, para que una red muerta falle en segundos y no en el valor por defecto del sistema operativo.

Los dos mecanismos no son redundantes: el modo batch cubre SSH, el interruptor del prompt de terminal cubre HTTPS, y un repositorio puede usar cualquiera de los dos.

Fallar en segundos enruta a la fila existente de `authentication or transport failure`, que ya preserva el recibo del comando y ya exige nombrar una ruta de credencial alternativa. **Esta clausula no agrega ningun resultado. Vuelve alcanzable el que ya existe.**

### 2. La variable de entorno nunca se escribe como prefijo de comando

`GIT_TERMINAL_PROMPT=0 git fetch` es solo POSIX. PowerShell parsea la asignacion como nombre de comando y falla antes de que Git corra, en la plataforma exacta que este repositorio atiende primero. El contrato declara la forma por shell: `export` en Bash y Zsh, `$env:` en PowerShell.

Esta clausula existe solo porque la clausula 1 introduce la variable. `RH-W-0035` ya establecio la regla para el unico otro lugar donde este repositorio fija una variable de entorno inline.

### 3. La forma corta del upstream va entre comillas

Todo comando que use la forma corta del upstream la escribe como `"@{u}"`. Sin comillas, PowerShell puede parsearla como expresion de hashtable. Entre comillas es correcta en PowerShell, Bash y Zsh.

El contrato hoy no escribe la forma corta, asi que esto no es una correccion de texto existente. Es un riesgo adjunto a la instruccion que hace que un agente recurra a ella: comparar adelanto y atraso, y despues integrar solo fast-forward.

### 4. Los dos limites conocidos se documentan donde se da el mecanismo

`-c core.sshCommand` se ignora cuando `GIT_SSH_COMMAND` ya esta en el entorno, y sobrescribe un `core.sshCommand` configurado, de modo que una instalacion que depende de un binario o identidad SSH concreta queda degradada en silencio en vez de respetada. `GIT_TERMINAL_PROMPT=0` silencia el prompt propio de Git pero no el prompt grafico de un helper de credenciales, como Git Credential Manager en Windows.

Declarar ambos junto al mecanismo evita afirmar en falso que el cuelgue ya es imposible. La clausula 1 hace que el caso comun falle rapido; no hace que todo caso falle rapido.

## Que no cambia

- **El umbral de aprobacion.** `RH-W-0046` posee si un fetch de solo lectura se separa de la aprobacion de acceso a red. Esta unidad gobierna solo como se invoca un comando ya autorizado. Si `RH-W-0046` mas adelante quita una aprobacion, estas clausulas aplican sin cambios a lo que quede.
- **Las herramientas de proveedor siguen opcionales.** Git nativo sigue siendo el transporte central.
- **El modelo de resultados.** `ready`, `paused`, `blocked`, `failed`, `degraded` y `completed` quedan intactos. Un fallo de transporte sigue siendo `failed` en la rama Git opcional y `degraded` para el arranque.
- **La prohibicion de `git pull`.** Queda exactamente como esta escrita. Este diseno a proposito no le adjunta la razon del harness externo, porque esa razon descansa en una ruta con proveedor primero que este producto no tiene.

## La decision abierta, y la recomendacion

**Pregunta**: el contrato nombra flags exactos, o declara la obligacion y deja los flags a la implementacion.

**Recomendacion: nombrar los flags.** Tres razones, en orden de peso.

1. **El lector es un agente, no un compilador.** Un contrato generado que dice "invocalo de forma no interactiva" deja al agente derivar el conjuro. El hallazgo existe precisamente porque ese conjuro no es obvio: dos mecanismos de supresion separados que cubren dos transportes distintos, uno sin forma `-c`, mas un timeout, y una suposicion equivocada reproduce el cuelgue que la clausula existe para evitar.
2. **El contrato ya esta acoplado a la superficie de CLI de Git.** Nombra `git fetch`, prohibe `git pull`, `git rebase` y `git reset`, y exige integracion solo fast-forward. Nombrar tres flags no agrega acoplamiento que no exista ya.
3. **Una obligacion sin nombrar no se puede probar.** Una prueba enfocada puede afirmar que el texto generado contiene el mecanismo. No puede afirmar que una parafrasis produciria un comando no interactivo.

**El costo, declarado con honestidad.** Si Git cambia esos flags, el contrato generado queda obsoleto y el checker no puede detectarlo, porque ningun control de aca ejecuta Git contra una llave protegida por passphrase. Eso es real, y la mitigacion es la clausula 4: los limites quedan documentados junto al mecanismo, de modo que quien se tope con un caso que los flags no cubren tiene la razon y no un conjuro pelado.

## Bosquejo de implementacion

Un sitio de renderizado por habilidad en `skill/repo-harness-builder/scripts/harness_tool.py`:

| Clausula | Sitio |
|---|---|
| 1, 2, 3, 4 | El parrafo de cierre del gate de sincronizacion en el renderizador de inicio, y el paso 12 de ejecucion del cierre |

Las dos habilidades de sesion generadas llevan las cuatro clausulas, porque las dos llegan a la red y la ruta de cierre es la mas danina. Sin cambios en el rulebook, sin gate de decision nuevo, sin campo nuevo de manifiesto, y sin tocar `REPO-HARNESS.md`, que tiene 4 tokens de margen.

## Plan de pruebas

TDD estricto. Cada prueba falla antes de implementar.

1. `NonInteractiveTransportTests`: las habilidades renderizadas de inicio y cierre contienen cada una los dos mecanismos de supresion, el timeout de conexion acotado, la forma de entorno por shell, la forma corta del upstream entre comillas y los dos limites documentados.
2. Una guarda que afirme que la forma de prefijo solo POSIX no aparece en ninguna de las dos habilidades renderizadas, para que una edicion futura no pueda reintroducirla.
3. Una guarda que afirme que el rulebook renderizado no cambia y sigue dentro de su techo de tokens.
4. La suite existente pasa sin cambios, y el renderizador de mantenimiento sincroniza las habilidades canonicas autoalojadas y ambas proyecciones de runtime.

Esperado antes de la entrega: `RH-W-0059` sigue `proposed` hasta que el propietario apruebe este diseno, luego `ready`, y luego `active` al autorizar la implementacion.

## Revision de este diseno

Este diseno se reviso contra evidencia del repositorio despues de su primer borrador, y la revision lo recorto de ocho clausulas a cuatro.

- Se quitaron tres clausulas por scope creep: fechas, listado de ramas y reporte de fin de linea no son transporte, y viajaban sobre la urgencia de una unidad `P1`.
- Se retiro una clausula por no ser defecto: el riesgo de fechas no tiene camino alcanzable en este repositorio, y adoptarlo habria roto el techo de tokens del propio rulebook, que el rulebook prohibe subir.
- Se retracto una razon por importada: la justificacion del harness externo para prohibir `git pull` descansa en una ruta con proveedor primero que este producto no tiene, y adjuntarla aca habria acreditado a este contrato un motivo de una arquitectura que no usa.
- Se reclasificaron dos hallazgos como ya satisfechos en vez de pendientes, para que la brecha no se redescubra despues como trabajo nuevo.

## Decisiones humanas pendientes

1. Aprobar o rechazar este diseno.
2. Confirmar la recomendacion de nombrar los flags, o elegir la forma de solo obligacion.
3. Confirmar que `C1` y `C2` quedan excluidos, porque ambos presuponen un CLI de proveedor que este producto no debe exigir.
