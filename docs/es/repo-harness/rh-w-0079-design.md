# Diseno RH-W-0079 de capacidades opcionales y limite de release

Estado: `ready`, diseno completo y aprobado por el propietario el 2026-08-28. El plan de implementacion bilingue separado tambien esta aprobado por el propietario. Ninguna aprobacion concede autoridad de implementacion, release ni entrega.

Progreso de implementacion: el Corte 1 autorizado por el propietario esta implementado localmente con evidencia RED y GREEN. Los Cortes 2 a 5, el release y la entrega quedan fuera de este resultado, por lo que el ciclo sigue `ready`.

## Proposito

Definir un unico contrato generico de repo-harness que siga siendo util cuando Git o una integracion del ecosistema no esten disponibles, mientras conserva comportamiento mas fuerte respaldado por capacidades disponibles. El autoalojamiento reciproco es el campo de prueba, no permiso para exportar la politica de este repositorio a consumidores.

## Evidencia y confianza

### Evidencia verificada del repositorio

- `skill/repo-harness-builder/scripts/harness_tool.py` usa actualmente configuracion `integrated` por defecto cuando falta `--standalone`, y `select_integration_mode()` permite esa ruta solo cuando Gentle AI y Engram se clasifican como `verified`.
- RH-W-0025 completada posee los resultados existentes `not_found`, `found_on_path` y `verified` para detectar Gentle AI y Engram. Este diseno cambia enrutamiento, no esas semanticas.
- RH-W-0051 completada posee los namespaces locales fijos y no autoritativos. Este diseno no crea store de capacidades ni otro namespace.
- El roadmap entrega a RH-W-0048 recuperacion de proyecciones de runtime y a RH-W-0052 diseno de archivo, desinstalacion, reinstalacion y rollback. RH-W-0079 coordina sin absorberlos.
- `REPO-HARNESS.md` identifica este repositorio como autoalojado y distingue el builder del harness consumidor.

### Evidencia externa primaria

- Git define un repositorio mediante metadata que incluye `HEAD`, referencias, objetos, configuracion y normalmente un indice. `HEAD` puede nombrar una rama aun inexistente o apuntar directamente a un commit en estado separado. Esto respalda tratar por separado los estados inicializado, sin commits y separado. [Layout de repositorio Git](https://git-scm.com/docs/gitrepository-layout)
- `git init` crea metadata y una rama inicial sin commits. Por eso es una mutacion de la carpeta seleccionada, no deteccion inocua. [Git init](https://git-scm.com/docs/git-init)
- `git status` distingue cambios del indice, del arbol de trabajo y no rastreados, y su formato porcelain se destina a scripts. Esto respalda conservar el estado con cambios como evidencia en vez de reducirlo a fallo generico. [Git status](https://git-scm.com/docs/git-status)
- GitHub documenta issues como funcion propia para rastrear trabajo y pull requests como propuestas de fusion con contexto de revision de GitHub. Son capacidades de proveedor, no propiedades de Git ni requisitos universales. [GitHub Issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues), [pull requests de GitHub](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)

La confianza es alta para el comportamiento actual y el modelo de estados Git porque provienen de fuente canonica, pruebas, evidencia Git local y documentacion oficial. Es alta para la propiedad de issues y pull requests por GitHub segun sus contratos. Las superficies exactas de implementacion permanecen desconocidas hasta que un plan aprobado las mida.

## Modelo de propiedad de capacidades

El repositorio conserva autoridad sobre hechos durables, politica local, fuente, pruebas y aprobaciones. Cada capacidad opcional posee solo lo que realmente proporciona:

| Capacidad | Comportamiento propio | Comportamiento que no posee |
|---|---|---|
| Filesystem y documentos del repositorio | Inspeccion base, documentacion, ediciones locales y handoff visible | Historial de versiones, estado remoto, workflow de proveedor o continuidad externa |
| Git | Historial, comparacion rastreada, ramas, commits, evidencia de rollback Git, remotos y comparacion upstream | Issues, pull requests, checks de proveedor o memoria semantica |
| Tooling de proveedor o GitHub | Autenticacion y acciones especificas de issues, pull requests, checks y metadata | Correccion Git local o politica del repositorio |
| `CONTRIBUTING.md` o equivalente | Politica especifica del repositorio cuando existe y aplica | Politica generica cuando falta |
| Gentle AI | Enrutamiento, revision, recibos y coordinacion de entrega verificados | Verdad del repositorio o continuidad Engram |
| Engram | Continuidad semantica y operaciones de memoria verificadas | Verdad del repositorio, historial Git o autoridad de entrega |
| CodeGraph | Exploracion estructural indexada y evidencia de impacto verificadas | Compilador, pruebas, Git o autoridad del repositorio |
| Runtime de agente compatible | Leer y aplicar contratos emitidos | Capacidades que el runtime no expone |

Las capacidades se componen de forma independiente. Git mas Engram sin Gentle AI conserva ambos comportamientos. Ningun par nombrado se vuelve prerrequisito oculto.

## Limites de deteccion, uso y mutacion

1. **Detectar** con la superficie propietaria mas estrecha y de solo lectura. Conservar resultado tipado y limitacion exacta. Detectar no autoriza usar ni mutar.
2. **Usar** una capacidad solo cuando su estado respalda la operacion pedida. El uso de solo lectura queda acotado por intencion y politica actuales.
3. **Mutar** solo cuando el resultado pedido autoriza esa mutacion y se cumplen todas las compuertas aplicables. Tener una capacidad instalada nunca concede autoridad.
4. Volver a detectar antes de apply cuando evidencia cambiada pueda invalidar bytes, alcance o autoridad aprobados.
5. La ausencia estable entre preview y apply no falla. Una diferencia material rechaza solo la accion afectada.
6. Nunca inferir proveedor, remoto, politica, historial, rama, continuidad o garantias de grafo desde otra capacidad.

## Matriz de estados de capacidad

| Estado | Significado | Comportamiento generico | Mutacion y aprobacion |
|---|---|---|---|
| `available` | La deteccion verifica lo necesario para la accion | Usar solo comportamiento propio y declarar garantias respaldadas | Siguen aplicando las aprobaciones de la accion |
| `unavailable` | Falta ejecutable, servicio, archivo propietario o evidencia requerida | Continuar lo no afectado y omitir solo garantias propias | No instalar, inicializar, autenticar ni configurar automaticamente |
| `partial` | Una parte se verifica y otra parte necesaria falta o falla | Usar el subconjunto verificado, nombrar lo ausente y evitar fallo global | Preguntar solo si la accion necesita la parte faltante |
| `disabled` | Repositorio o persona excluyo explicitamente una capacidad disponible | Respetar exclusion y degradar igual que por ausencia | Reactivar es otra decision explicita |
| `policy-required` | Autoridad actual exige una capacidad para un workflow concreto | Bloquear solo ese workflow cuando no puede satisfacerse | Nunca promover politica local al producto generico |

`policy-required` es contextual, no un sexto resultado tecnico ni un default de proveedor. Una integracion ausente degrada solo su comportamiento propio.

## Estados Git y recomendacion

| Estado Git | Comportamiento compatible | Garantias no disponibles |
|---|---|---|
| Ejecutable ausente | Continuar sin Git y dar como maximo una recomendacion util por interaccion que nombre la seguridad concreta de historial, comparacion, recuperacion, rollback o entrega | Toda afirmacion respaldada por Git |
| Ejecutable presente, carpeta sin inicializar | Continuar sin Git. Mencionar inicializacion solo cuando aporte | Historial, ramas, commits, comparacion rastreada y rollback Git |
| Inicializado, rama sin commits | Usar metadata y evidencia del arbol | Historial previo y rollback a commit anterior |
| Inicializado, HEAD separado | Usar evidencia del commit y del arbol | Propiedad de rama actual y entrega ordinaria por rama |
| Inicializado, con cambios | Preservar cambios, identificar solapamiento y limitar escrituras | Afirmar que los cambios pertenecen solo a esta tarea |
| Inicializado, limpio | Usar evidencia Git local aplicable | Remoto y proveedor salvo verificacion separada |
| Sin upstream o remoto | Conservar seguridad Git local | Adelante o atras, sincronizacion, publicacion y recuperacion remota |
| Divergente u operacion sin resolver | Preservar estado y detener solo integracion o entrega afectada | Integracion o entrega automatica segura |

La recomendacion no crea estado persistente, ledger, contador ni modo de insistencia. Se emite una vez por interaccion aplicable y se omite cuando no aporta accion. `git init` siempre exige aprobacion humana explicita porque muta la carpeta. Nunca se ejecuta como setup, recuperacion o efecto lateral de apply.

## Politica del repositorio y comportamiento de proveedor

- GitHub, issues, pull requests, checks y tooling de proveedor son opcionales incluso con remoto Git.
- Un `CONTRIBUTING.md` presente es autoridad especifica solo en su alcance. Su ausencia no crea requisitos issue-first, de rama, proveedor o revision.
- La ausencia de proveedor no debilita evidencia local ni seguridad Git local.
- Las funciones de proveedor se ofrecen solo tras reconocer y verificar la capacidad. El contrato no supone GitHub por una URL, `.git` ni la palabra `origin`.
- La politica autoalojada issue-first y bilingue sigue especifica del repositorio salvo evidencia separada que sostenga una regla generica.

## Contrato de seleccion y apply

La seleccion integrada predeterminada actual acopla la ruta no standalone a Gentle AI y Engram. La migracion queda acotada:

1. Conservar detectores RH-W-0025 y evidencia tipada.
2. Reemplazar el par binario obligatorio con un conjunto derivado de resultados verificados, deshabilitados, parciales y ausentes.
3. Renderizar por defecto comportamiento util de repositorio standalone cuando no haya integracion verificada.
4. Agregar el contrato propio de cada capacidad opcional verificada sin volver obligatoria otra.
5. Ligar la configuracion efectiva exacta con la autoridad existente de preview y apply usando mecanismos actuales cuando alcancen.
6. Antes de apply, rechazar solo deriva material que invalide plan, objetivo, alcance o autoridad aprobados.
7. No agregar campo de manifest, archivo de migracion, store de compatibilidad ni segunda ruta de autoridad salvo evidencia que pruebe insuficiente el binding actual.

El standalone explicito sigue compatible. Instalaciones integradas migran mediante reemplazo ordinario de salida generada tras autorizacion separada. RH-W-0025 y RH-W-0051 permanecen cerradas.

## Coordinacion con RH-W-0048 y RH-W-0052

- RH-W-0048 conserva recuperacion de proyecciones de runtime, incluida politica rastreada contra local y prueba aditiva sin sobrescritura.
- RH-W-0052 conserva archivo, desinstalacion, reinstalacion, rollback y consecuencias para carpetas sin Git despues de que RH-W-0048 pruebe propiedad y recuperacion.
- RH-W-0079 aporta solo la regla compartida: conservar utilidad disponible, omitir visiblemente garantias ausentes y nunca fabricar restauracion.
- Cada registro conserva ciclo de vida, RED, implementacion, rollback y entrega separados. Este diseno no reabre ni avanza ninguno.

## Limites de aprobacion

- No se requiere aprobacion para informar deteccion de solo lectura ya autorizada por la tarea.
- Escrituras usan la autoridad normal aplicable al cambio pedido.
- Red, mutacion de proveedor, publicacion, accion destructiva, rutas externas, autenticacion, commits, pushes e inicializacion Git conservan compuertas explicitas propias.
- Una aprobacion cubre una secuencia acotada sin cambios solo donde el contrato propietario ya lo permite. Un cambio material la invalida.
- La ausencia de evidencia nunca autoriza instalar, configurar, inicializar, autenticar ni sustituir proveedor.

## Pruebas RED enfocadas

La implementacion debe probar primero el fallo para:

1. preview predeterminado sin Gentle AI ni Engram;
2. cada integracion por separado y ambas juntas;
3. resultados `available`, `unavailable`, `partial`, `disabled` y `policy-required` sin fallo global;
4. Git ausente con salida util y una recomendacion acotada;
5. carpeta sin inicializar y sin `git init` antes de aprobar;
6. Git limpio, con cambios, separado, sin commits, sin upstream y divergente con solo afirmaciones compatibles;
7. tooling de proveedor ausente con Git presente;
8. GitHub ausente y proveedor no GitHub disponible;
9. `CONTRIBUTING.md` ausente sin inventar issue-first;
10. politica que exige una capacidad ausente y bloquea solo su workflow;
11. ausencia estable entre preview y apply;
12. deriva material que invalida solo autoridad afectada;
13. migracion standalone e integrada sin store duplicado;
14. limites independientes de RH-W-0048 y RH-W-0052;
15. inventario de release rechazando comportamiento sin clasificar.

Las pruebas usan fakes o repositorios temporales, sin proveedor vivo, red, daemon ni mutacion destructiva. Deben preservar cambios preexistentes.

## Aceptacion multiplataforma y de agentes

- Pruebas enfocadas pasan en Windows, Linux y macOS donde exista evidencia CI. Evidencia ausente se informa, no se infiere.
- Comandos y rutas usan limites Python y Git ya existentes, sin suponer shell POSIX.
- Agentes compatibles con OpenCode, Claude y Pi y agentes genericos reciben semantica equivalente en superficies compatibles.
- Un agente sin integracion sigue comportamiento solo de repositorio en vez de fallar.
- El comportamiento actual sigue como base hasta que implementacion y release prueben el reemplazo.

## Aceptacion de release y rollback

Antes del release:

1. clasificar cada comportamiento cambiado exactamente una vez como scaffolding del builder, politica del repositorio o contrato generico;
2. probar RED, GREEN y suite canonica aplicable;
3. verificar checker, proyecciones, pares localizados, migracion, standalone y combinaciones;
4. repetir carpeta sin Git, repositorio Git local sin proveedor y este repositorio autoalojado;
5. verificar ausencia de inicializacion automatica, supuesto de proveedor, estado duplicado o garantia sin respaldo;
6. registrar evidencia exacta de rollback.

Rollback restaura seleccion, apply, pruebas y proyecciones como unidad versionada consistente. No reescribe consumidores, inicializa o elimina Git, borra estado ni finge capacidades. Un rollback que restaure preflight emparejado obligatorio no satisface esta direccion y no se publica sin otra decision explicita.

## Sobreingenieria rechazada y fuera de alcance

- Sin framework de adapters, plugins, capa abstracta de proveedor, servicio de capacidades, daemon, scheduler ni telemetria.
- Sin store duplicado de capacidades, continuidad, aprobaciones, ledger de warnings, contador de recomendaciones ni registro de migracion.
- Sin `git init`, instalacion de proveedor, autenticacion, issue, pull request, commit, push o publicacion automaticos.
- Sin asumir que Git implica GitHub, issues, pull requests, `CONTRIBUTING.md` o tooling.
- Sin reescritura amplia de contratos, manifest, checker, taxonomia local, recuperacion o reinstalacion.
- Sin reabrir RH-W-0025 o RH-W-0051, ni fusionar o avanzar RH-W-0048 o RH-W-0052.
- Sin implementacion, pruebas, proyecciones, release ni entrega bajo esta autoridad.

## Compuerta del propietario

Decision del propietario registrada: el propietario respondio `daale` a la pregunta exacta de aprobacion de este diseno bilingue completo el 2026-08-28. El plan de implementacion bilingue separado ahora identifica superficies exactas, bytes de migracion, RED, comandos de rollback y evidencia de entrega, y despues fue aprobado por el propietario tras una revision final PASS nueva. La implementacion sigue sin autorizacion separada.
