# Investigacion sobre continuidad y aprendizaje de agentes

## Resultado

Hermes Agent, OpenClaw y Pi Coding Agent ofrecen patrones utiles para continuidad acotada, recuperacion, confianza y extension. Ninguno debe copiarse como una autoridad nueva ni como una capa autonoma de automodificacion. La direccion reutilizable mas solida es mantener la verdad durable en los propietarios del repositorio, usar Engram para continuidad semantica, usar Gentle AI para enrutamiento y revision cuando este habilitado, y agregar solo protecciones respaldadas por evidencia que reduzcan perdida o acciones no autorizadas.

Este documento es orientacion de investigacion. No adopta una funcion, no crea trabajo de ciclo de vida, no cambia el estado del producto ni autoriza implementacion.

## Preguntas de investigacion

1. Como preservan los tres agentes sesiones y conocimiento ante perdida de contexto?
2. Como separan memoria, skills, estado de ejecucion y archivos autoritativos del proyecto?
3. Que controles evitan que el aprendizaje o las extensiones se conviertan en autoridad insegura?
4. Que patrones encajan en este harness, requieren adaptacion o deben rechazarse?

## Revisiones fijadas

| Proyecto | Revision revisada |
|---|---|
| Hermes Agent | [`1bbb6e5bce56e721ab685af4cd87df21bbff4d35`](https://github.com/NousResearch/hermes-agent/tree/1bbb6e5bce56e721ab685af4cd87df21bbff4d35) |
| OpenClaw | [`9cfb4ff956c9b50e713f8774b576d9edd8db50a7`](https://github.com/openclaw/openclaw/tree/9cfb4ff956c9b50e713f8774b576d9edd8db50a7) |
| Pi Coding Agent | [`a79b3733421ead0dcea3cbe32247ea2464400dcb`](https://github.com/earendil-works/pi/tree/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent) |

Las fuentes se revisaron el 2026-08-25. Los enlaces inmutables siguientes son los anclajes de evidencia.

## Hallazgos comparativos

| Tema | Hermes Agent | OpenClaw | Pi Coding Agent | Implicacion para el harness |
|---|---|---|---|---|
| Propiedad de sesion | Presenta sesiones buscables en SQLite y continuidad entre plataformas. | El gateway posee el estado de sesion y enruta mensajes a sesiones aisladas o compartidas. | Archivos JSONL preservan un arbol con `id` y `parentId`. | Mantener un propietario explicito por capa de continuidad. Una interfaz, skill o resumen no debe convertirse en autoridad paralela. |
| Recuperacion de contexto | Compresion, busqueda de sesiones, avisos de memoria y memoria persistente atienden la perdida de contexto. | Ejecuta un guardado de memoria antes de compactar; archivos durables y transcripciones quedan separados. | La compactacion pierde detalle en el contexto activo, pero conserva el JSONL completo y sus ramas. | Los resumenes deben ser vistas reemplazables de evidencia retenida, no la unica evidencia. |
| Conocimiento durable | Memoria curada, modelo del usuario y skills procedimentales forman un ciclo de aprendizaje. | `USER.md`, `MEMORY.md`, notas diarias y dreaming opcional separan perfil, conocimiento durable y observaciones de trabajo. | Archivos de contexto y skills guian conducta; las sesiones retienen la conversacion. | Preservar la division actual: repositorio para verdad del equipo, Engram para observaciones semanticas y resumenes para continuidad temporal. |
| Skills y extensiones | Las skills pueden crearse y mejorar desde experiencia; plugins y MCP agregan capacidades. | Skills y plugins extienden conducta; la memoria pertenece a un plugin. | Skills son instrucciones; extensiones TypeScript interceptan eventos y herramientas. | Toda capacidad nueva debe ser explicita, inspeccionable y subordinada a la politica del repositorio. |
| Recuperacion operativa | Checkpoints, rollback, undo y busqueda de sesiones son superficies de recuperacion. | Filas durables, transcripciones archivadas, migracion y mantenimiento acotado apoyan recuperacion. | Navegacion del arbol, forks, clones, resumenes de ramas e historial previo apoyan recuperacion. | Preferir evidencia acumulativa o reversible antes de agregar correccion autonoma. |
| Aprobacion y confianza | Documenta aprobacion de comandos y backends aislados. | La politica efectiva es la combinacion mas estricta entre herramientas y aprobaciones; la aprobacion vincula el contexto de ejecucion. | Configuracion y extensiones locales cargan solo despues de confiar en el proyecto; extensiones tienen permisos completos. | El estado de aprobacion debe quedar con quien lo aplica. La memoria puede registrar contexto, no conceder autoridad. |
| Configuracion | Proveedores, herramientas, gateway y memoria son configurables. | Contenido del workspace, bases de estado, rutas de sesion, motores de memoria y aprobaciones tienen superficies distintas. | Configuracion global y de proyecto tiene precedencia explicita y confianza para el proyecto. | Mantener visible el alcance y evitar fallbacks ocultos que amplien autoridad. |
| Verificacion | Trayectorias y evolucion de skills producen material de aprendizaje, pero el resumen revisado no prueba finalizacion para este harness. | Comandos de inspeccion, auditoria, doctor y dry-run exponen evidencia operativa. | Archivos deterministas, eventos y hooks son inspeccionables. | Senales de aprendizaje no son recibos de finalizacion. Las pruebas y la evidencia canonica siguen siendo obligatorias. |

## Clasificacion de evidencia

### Hechos verificados

- OpenClaw declara que la memoria se escribe en Markdown, que el modelo solo recuerda lo guardado y que la memoria no aplica politica. El guardado previo a compactacion esta habilitado por defecto. [Memoria](https://github.com/openclaw/openclaw/blob/9cfb4ff956c9b50e713f8774b576d9edd8db50a7/docs/concepts/memory.md)
- OpenClaw asigna el estado de sesion al gateway, separa filas SQLite activas de transcripciones archivadas y permite politicas explicitas de rutas y retencion. [Sesiones](https://github.com/openclaw/openclaw/blob/9cfb4ff956c9b50e713f8774b576d9edd8db50a7/docs/concepts/session.md)
- La ejecucion en host de OpenClaw requiere acuerdo entre politica, lista permitida y aprobacion opcional. La politica efectiva es la mas estricta y una ejecucion aprobada vincula comando, directorio, entorno y sesion. [Aprobaciones](https://github.com/openclaw/openclaw/blob/9cfb4ff956c9b50e713f8774b576d9edd8db50a7/docs/tools/exec-approvals.md)
- Pi almacena sesiones como JSONL con arbol, conserva el historial completo despues de compactacion con perdida y admite resumenes al navegar ramas. [README](https://github.com/earendil-works/pi/blob/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent/README.md) y [compactacion](https://github.com/earendil-works/pi/blob/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent/docs/compaction.md)
- La confianza de proyecto en Pi controla configuracion, recursos y extensiones locales. Pi advierte que paquetes y extensiones pueden ejecutar con acceso completo. [README](https://github.com/earendil-works/pi/blob/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent/README.md) y [extensiones](https://github.com/earendil-works/pi/blob/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent/docs/extensions.md)
- Hermes presenta un ciclo con memoria curada, avisos periodicos, creacion y mejora de skills, busqueda de sesiones y modelo del usuario. Tambien anuncia aprobacion de comandos y backends aislados. [README de Hermes](https://github.com/NousResearch/hermes-agent/blob/1bbb6e5bce56e721ab685af4cd87df21bbff4d35/README.md)

### Inferencias respaldadas

| Inferencia | Evidencia y razonamiento | Que elevaria la confianza |
|---|---|---|
| Persistir antes de perder contexto es mas seguro que depender solo de un resumen posterior. | OpenClaw guarda memoria antes de compactar y Pi conserva historial completo tras un resumen con perdida. | Un diseno acotado que pruebe trigger, propietario, fallos y pruebas. |
| Continuidad en arbol o acumulativa puede mejorar correccion sin otro ciclo de vida. | Pi conserva ramas e historial; OpenClaw separa transcripciones archivadas de filas actuales. | Un diseno aprobado que use propietarios existentes sin otro almacen. |
| Skills aprendidas requieren mas control que observaciones normales. | Hermes permite evolucion; Pi advierte que skills y extensiones pueden causar acciones arbitrarias. | Revision completa del curador de Hermes y contrato aprobado de procedencia, revision, rollback y pruebas. |
| La aprobacion debe vincular el contexto exacto de accion, no solo un si conversacional. | OpenClaw vincula aprobacion con argumentos, directorio y sesion canonicos. | Diseno acotado que identifique acciones actuales que requieran vinculacion ejecutable. |

## Clasificacion de integracion

| Clasificacion | Patron | Limite |
|---|---|---|
| Integrar como orientacion | Persistir continuidad critica antes de reducir contexto. | Usar Engram y propietarios existentes; no crear otra memoria. |
| Integrar como orientacion | Vincular resumenes con evidencia recuperable. | Un resumen no prueba entrega, aprobacion ni verdad actual. |
| Integrar como orientacion | Separar contenido del proyecto, estado de ejecucion y continuidad del usuario. | La evidencia del repositorio sigue siendo autoritativa. |
| Adaptar antes de implementar | Checkpoints reversibles o traspaso consciente de ramas. | Debe respetar Git y roadmap sin snapshots ocultos ni estado paralelo. |
| Adaptar antes de implementar | Vincular aprobacion con contexto exacto. | Gentle AI u otro propietario debe emitir y validar; Engram solo conserva contexto. |
| Adaptar antes de implementar | Propuestas de mejora derivadas de experiencia. | Un fallo puede proponer trabajo, pero no reescribir skills ni avanzar ciclo automaticamente. |
| Rechazar | Promocion autonoma de memoria o trayectorias a politica. | Evitaria propiedad del repositorio y autoridad humana. |
| Rechazar | Desplegar skills automodificadas sin revision, pruebas y rollback. | Calidad de aprendizaje no equivale a autorizacion ni verificacion. |
| Rechazar | Copiar base de sesiones, gateway, gestor de plugins o scheduler de otro agente. | El harness coordina propietarios existentes; no debe ser otro runtime general. |
| Rechazar | Tratar aprobaciones, resumenes o confianza de memoria como recibos de finalizacion. | La finalizacion depende de evidencia del repositorio y estado del propietario aplicable. |

## Requisitos de seguridad y verificacion

Toda propuesta futura basada en esta investigacion debe probar:

1. Existe un propietario canonico para cada hecho y transicion.
2. Evidencia cruda o recuperable sobrevive a la perdida del resumen cuando sea proporcional.
3. La memoria no concede aprobacion, cierra trabajo ni reemplaza verdad del repositorio.
4. Conducta aprendida entra mediante propuesta explicita, revision, pruebas deterministas y rollback.
5. Capacidad ejecutable local al proyecto esta controlada por confianza y falla de forma segura sin canal de decision.
6. Los fallos distinguen continuidad no disponible, autoridad denegada, verificacion fallida y entrega incompleta.
7. El diseno no agrega ciclo, autoridad de sesion, almacen de conflictos, cache de recibos ni mutacion oculta.

## Fuentes no leidas o incompletas

- `docs/architecture.md` en la revision fijada de Hermes devolvio 404. Las afirmaciones usan el README fijado, no esa ruta ausente.
- `packages/coding-agent/docs/session.md` en la revision fijada de Pi devolvio 404. Las afirmaciones de sesion usan README y `docs/compaction.md`.
- El documento grande de extensiones de Pi fue recuperado, pero la salida quedo truncada despues de las secciones relevantes de confianza, ciclo, persistencia y permisos. La parte no leida no sustenta afirmaciones.
- Listados anteriores de arbol de GitHub quedaron truncados. Fragmentos localizados pero no leidos se excluyen.
- Implementaciones detalladas de curador, proveedores de memoria, checkpoints, transacciones de plugins y trayectorias de Hermes no se releyeron por completo despues de compactar contexto. Son candidatos para investigacion enfocada, no guia verificada.

## Preguntas sin resolver

- Que elemento de ciclo existente, si alguno, debe poseer un futuro diseno de continuidad previo a compactacion? Requiere decision humana solo si se propone implementar.
- Puede Gentle AI exponer metadatos estables de aprobacion vinculada sin crear un recibo del harness? Desconocido hasta investigar ese contrato especifico.
- Que controles del curador de Hermes separan experiencia util de instruccion insegura? Desconocido porque no se retuvo evidencia completa de implementacion.
- Mejoraria la recuperacion por ramas este repositorio mas alla de Git, Engram y traspasos actuales? Desconocido hasta que datos de fallos demuestren una brecha.

## Confianza

La confianza es alta para patrones verificados de propiedad, continuidad, confianza y aprobacion de OpenClaw y Pi. Es media para implicaciones del ciclo de Hermes porque su resumen fijado es autoritativo sobre conducta del producto, pero esta revision acotada no retuvo evidencia completa de implementacion. Las recomendaciones son conservadoras y no constituyen adopcion.
