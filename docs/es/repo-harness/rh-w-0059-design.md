# Diseno RH-W-0059: contrato de transporte no interactivo

<!-- doc-role:design -->

La fuente factual canonica es [`../../repo-harness/rh-w-0059-design.md`](../../repo-harness/rh-w-0059-design.md). Este es su espejo humano completo en espanol. Borrador pendiente de aprobacion del propietario; este documento no autoriza ninguna implementacion. El propietario del ciclo de vida es [RH-W-0059](roadmap.md#rh-w-0059).

## El defecto

Los contratos de sesion generados prometen un banner de remediacion al que no siempre pueden llegar.

**Hecho verificado.** `.repo-harness/skills/repo-harness-session-start/SKILL.md` exige una aprobacion antes del acceso a red, nombra `git fetch` e integracion solo fast-forward, y cierra su gate de sincronizacion con la obligacion de nombrar una ruta de credencial alternativa disponible despues de un fallo de transporte. El paso 12 de `repo-harness-session-stop/SKILL.md` lleva la misma obligacion para la ruta de push. Ninguno especifica como se invoca el comando de red. Las cadenas `BatchMode`, `GIT_TERMINAL_PROMPT`, `TTY` y `passphrase` no aparecen en ninguna habilidad generada ni en la hoja de ruta.

**Inferencia respaldada.** Frente a una llave SSH protegida por passphrase o una credencial HTTPS sin cache, `git fetch` abre un prompt. Un agente no tiene TTY, asi que nadie lo contesta. La sesion se cuelga sin error, sin codigo de salida, sin banner y sin timeout, de modo que la fila de fallo de transporte del gate nunca se ejecuta y la obligacion de ruta alternativa nunca se dispara. La evidencia es la ausencia de cualquier exigencia de no interactividad en el contrato, mas el harness externo revisado declarando la falla identica y declarando que exactamente esa configuracion existe en una de sus dos maquinas. Reproducirlo contra una llave con passphrase lo elevaria a hecho verificado.

**Por que es peor en el cierre.** `repo-harness-session-stop` llega a la red despues de guardar memoria y despues del commit. Un cuelgue ahi deja a la persona con una sesion congelada, un commit que puede ya existir, y ninguna salida que lo diga.

## Que cambia

Ocho obligaciones en las dos habilidades de sesion generadas, seis en el gate de transporte y dos adyacentes. Cada una es una clausula; ninguna cambia un resultado, un umbral ni una aprobacion.

### 1. La invocacion es no interactiva, y el contrato nombra el mecanismo

Todo comando de red autorizado corre con el prompt interactivo deshabilitado en las dos rutas que pueden abrir uno:

- SSH: `-o BatchMode=yes`, pasado por `git -c core.sshCommand=` para que se parsee igual en toda shell.
- El prompt de credencial propio de Git: `GIT_TERMINAL_PROMPT=0`, puesto en el entorno, porque no tiene equivalente `-c`.
- Un host inalcanzable: `-o ConnectTimeout=20`, para que una red muerta falle en segundos y no en el valor por defecto del sistema operativo.

Fallar en segundos enruta a la fila existente de `authentication or transport failure`, que ya preserva el recibo del comando y ya exige nombrar una ruta de credencial alternativa. Esta obligacion no agrega un resultado nuevo; vuelve alcanzable el que ya existe.

### 2. La variable de entorno nunca se escribe como prefijo de comando

`GIT_TERMINAL_PROMPT=0 git fetch` es solo POSIX. PowerShell parsea la asignacion como nombre de comando y falla antes de que Git corra, en la plataforma exacta que este repositorio atiende primero. El contrato declara la forma por shell: `export` en Bash y Zsh, `$env:` en PowerShell.

### 3. La forma corta del upstream va entre comillas

Todo comando que use la forma corta del upstream la escribe como `"@{u}"`. Sin comillas, PowerShell puede parsearla como expresion de hashtable. Entre comillas es correcta en PowerShell, Bash y Zsh. Los contratos actuales usan integracion solo fast-forward sin declarar este riesgo.

### 4. La integracion es un merge local del upstream ya traido

El contrato ya prohibe `git pull`. No dice por que, y la razon es operativa y no estilistica: un pull vuelve a buscar por la configuracion de remoto por defecto, que puede resolver a un transporte distinto del que acaba de funcionar. La clausula gana su razon para que una edicion futura no la relaje como arbitraria.

### 5. Los dos limites conocidos se documentan donde se da el mecanismo

`-c core.sshCommand` se ignora cuando `GIT_SSH_COMMAND` ya esta en el entorno, y sobrescribe un `core.sshCommand` configurado, de modo que una instalacion que depende de un binario o identidad SSH concreta queda degradada en silencio en vez de respetada. `GIT_TERMINAL_PROMPT=0` silencia el prompt propio de Git pero no el prompt grafico de un helper de credenciales. Declarar ambos junto al mecanismo evita afirmar en falso que el cuelgue ya es imposible.

### 6. Las fechas se toman con un `date` local pelado

**Hecho verificado de la revision externa.** Un prefijo `TZ=` lo ignora sin error Git Bash en Windows y devuelve UTC. Despues de las 18:00 local, UTC ya es manana, asi que toda fecha escrita esa noche queda un dia adelante y ningun lector puede detectarlo. Este repositorio escribe fechas en `CHANGELOG.md`, en los historiales de decision de la hoja de ruta y en el estado actual. La clausula exige un `date` local pelado y una conversion explicita cuando haya que convertir una fuente UTC.

### 7. Una revision de estado del repositorio incluye las ramas

**Hecho verificado de la revision externa.** Una auditoria que cubrio documentacion, codigo, controles y entrega nunca listo las ramas, y una rama fusionada muerta hacia quince dias seguia apareciendo y sembrando duda sobre si guardaba trabajo. Lo que zanja el borrado es un chequeo de ascendencia contra las dos puntas, local y remota, nunca el nombre ni la fecha. El paso 3 generado de inicio hoy inspecciona rama, arbol, indice, upstream, remotos y URLs. Gana el listado de ramas.

### 8. La configuracion de fin de linea puede fingir un cambio de contenido

**Hecho verificado de la revision externa.** Con `core.autocrlf=true` peleando contra una regla `eol=lf` de `.gitattributes`, una herramienta que normaliza finales de linea informa una modificacion espuria por archivo afectado. Este repositorio ya fija LF por `.gitattributes` para toda ruta generada comparada byte a byte (`RH-W-0029`), y `RH-W-0042` ya arreglo la mitad de deteccion de conflictos. La mitad restante es el reporte: antes de registrar un cambio de contenido, comparar contra el blob commiteado. Esta clausula pertenece a la clasificacion de preparacion del cierre, no al gate de transporte.

## Que no cambia

- **El umbral de aprobacion.** `RH-W-0046` posee si un fetch de solo lectura se separa de la aprobacion de acceso a red. Esta unidad gobierna solo como se invoca un comando ya autorizado. Si `RH-W-0046` mas adelante quita una aprobacion, estas clausulas aplican sin cambios a lo que quede.
- **Las herramientas de proveedor siguen opcionales.** El contrato conserva Git nativo como transporte central y las herramientas de proveedor como ruta de credencial alternativa que debe estar instalada y aprobada aparte. Este diseno no agrega un respaldo ordenado con proveedor primero, porque el respaldo ordenado del hallazgo `C1` presupone un CLI de proveedor que este producto no debe exigir. De `C1` toma solo el fallo terminal ruidoso, que el contrato ya tiene.
- **El modelo de resultados.** `ready`, `paused`, `blocked`, `failed`, `degraded` y `completed` quedan intactos. Un fallo de transporte sigue siendo `failed` en la rama Git opcional y `degraded` para el arranque.
- **El hallazgo `C2`** (pasar una URL HTTPS explicita en vez de sobrescribir la URL del remoto por configuracion) queda fuera de alcance por la misma razon que el respaldo ordenado: es una mecanica de ruta de proveedor, y este producto no posee una ruta de proveedor. Se registra aca como excluido a proposito y no como omitido.

## La decision abierta, y la recomendacion

**Pregunta**: el contrato nombra flags exactos, o declara la obligacion y deja los flags a la implementacion.

**Recomendacion: nombrar los flags.** Tres razones, en orden de peso.

1. **El lector es un agente, no un compilador.** Un contrato generado que dice "invocalo de forma no interactiva" deja al agente derivar el conjuro. El hallazgo existe precisamente porque ese conjuro no es obvio: tres mecanismos separados, uno sin forma `-c`, y una suposicion equivocada reproduce el cuelgue que la clausula existe para evitar.
2. **El contrato ya esta acoplado a la superficie de CLI de Git.** Nombra `git fetch`, prohibe `git pull`, `git rebase` y `git reset`, y exige integracion solo fast-forward. Nombrar tres flags no agrega acoplamiento que no exista ya.
3. **Una obligacion sin nombrar no se puede probar.** Una prueba enfocada puede afirmar que el texto generado contiene el mecanismo. No puede afirmar que una parafrasis produciria un comando no interactivo.

**El costo, declarado con honestidad.** Si Git cambia esos flags, el contrato generado queda obsoleto y el checker no puede detectarlo, porque ningun control de aca ejecuta Git contra una llave protegida por passphrase. Eso es real, y la mitigacion es la clausula 5: los limites quedan documentados junto al mecanismo, de modo que quien se tope con un caso que los flags no cubren tiene la razon y no un conjuro pelado.

## Bosquejo de implementacion

Sitios de renderizado en `skill/repo-harness-builder/scripts/harness_tool.py`:

| Clausula | Sitio |
|---|---|
| 1, 2, 3, 5 | El parrafo de cierre del gate de sincronizacion, y el paso 12 del cierre de sesion |
| 4 | El mismo parrafo, como razon adjunta a la prohibicion existente de `git pull` |
| 6 | Una regla dura del rulebook generado, porque todas las habilidades escriben fechas |
| 7 | El paso 3 de ejecucion del inicio de sesion |
| 8 | La clasificacion de preparacion del cierre, paso 8 |

Las dos habilidades de sesion generadas llevan las clausulas de transporte, porque las dos llegan a la red y la ruta de cierre es la mas danina.

## Plan de pruebas

TDD estricto. Cada prueba falla antes de implementar.

1. `NonInteractiveTransportTests`: las habilidades renderizadas de inicio y cierre contienen cada una los tres mecanismos, la forma de entorno por shell, la forma corta del upstream entre comillas y los dos limites documentados.
2. Una guarda que afirme que la forma de prefijo solo POSIX no aparece en ninguna de las dos habilidades renderizadas, para que una edicion futura no pueda reintroducirla.
3. `RepositoryStateBranchTests`: el paso 3 renderizado del inicio exige un listado de ramas.
4. `LocalDateTests`: el rulebook renderizado prohibe el prefijo de zona horaria y exige un `date` local pelado.
5. Reporte de fin de linea: la clasificacion de preparacion renderizada del cierre exige comparar contra el blob commiteado.
6. La suite existente pasa sin cambios, y el renderizador de mantenimiento sincroniza las habilidades canonicas autoalojadas y ambas proyecciones de runtime.

Esperado antes de la entrega: `RH-W-0059` sigue `proposed` hasta que el propietario apruebe este diseno, luego `ready`, y luego `active` al autorizar la implementacion.

## Decisiones humanas pendientes

1. Aprobar o rechazar este diseno.
2. Confirmar la recomendacion de nombrar los flags, o elegir la forma de solo obligacion.
3. Confirmar que el respaldo ordenado de `C1` y la URL HTTPS explicita de `C2` quedan excluidos, porque ambos presuponen un CLI de proveedor que este producto no debe exigir.
