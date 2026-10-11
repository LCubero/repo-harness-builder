# Diseño RH-W-0046 / RH-W-0018 de consentimiento por repositorio

**Estado:** diseño conjunto aprobado por delegación del propietario con implementación RH-W-0046 completada. RH-W-0046 está `completed`; RH-W-0018 se rechazó (`rejected`) el 2026-10-11 sin implementación. Es un diseño para dos unidades con seguimiento independiente; no fusiona sus ciclos de vida, implementaciones, pruebas ni entrega. La porción RH-W-0046 completada conserva el límite histórico de este diseño; RH-W-0018 nunca se implementó.

Revise este diseño contra [RH-W-0046](roadmap.md#rh-w-0046), [RH-W-0018](roadmap.md#rh-w-0018), la [taxonomía de estado local](rh-w-0051-design.md), las [decisiones permanentes](decisions.md) y el [propietario canónico en inglés](../../repo-harness/rh-w-0046-rh-w-0018-design.md).

## Decisión

Conservar `git fetch` dentro del umbral de aprobación de red. Permitir que una persona recuerde el consentimiento para un repositorio, un remoto identificado, un destino exacto de fetch y únicamente la operación `git-fetch`. La concesión es explícita, visible cada vez que se usa y revocable mediante una instrucción humana simple. Sin una concesión válida, session-start usa la aprobación interactiva existente.

RH-W-0018 puede reutilizar el mismo **patrón de preferencia**, pero no esta concesión. El comportamiento general de menos preguntas sigue siendo una preferencia `interaction-behavior` separada y nunca autoriza Git. No se almacena en este archivo ni se infiere de él.

## Propietario y formato mínimos

El archivo ignorado y no autoritativo `.repo-harness/state/preferences/fetch-consent.md` posee únicamente el consentimiento recordado para fetch. RH-W-0051 posee el namespace padre y su exclusión de Git. Este archivo no duplica autoridad de ciclo o aprobación y es distinto del diseño compartido aplazado `.repo-harness/state/preferences/user-preferences.json`. El acuerdo Markdown de preferencias anterior nunca se implementó y ahora es histórico.

Es Markdown estructurado con una sola concesión vigente y estos campos obligatorios:

```markdown
# Repository fetch consent
schema: 1
operation: git-fetch
remote: origin
fetch-url: https://example.invalid/owner/repository.git
transport: https
upstream-ref: refs/heads/main
destination-ref: refs/remotes/origin/main
credential-path: helper=<none>;useHttpPath=<unset>
granted-at: 2026-09-06T00:00:00Z
```

Los valores son entradas exactas observadas, no alias ni identidades de proveedor inferidas. `credential-path` describe sin secretos la ruta efectiva de credenciales/helpers: identidades ordenadas de helpers y configuración de alcance por ruta para HTTPS, o identidad efectiva de comando/configuración SSH. Nunca registra credenciales, tokens, nombres de usuario embebidos en URLs, salida de helpers, secretos de entorno, OIDs ni datos descargados.

## Ciclo de vida

- **Lectura:** session-start puede leer el archivo fijo sin mutarlo después de resolver el repositorio seleccionado. Ausencia, contenido inválido, esquema no soportado, campos duplicados o valores inseguros/sensibles equivalen a no tener concesión.
- **Concesión:** solo una instrucción humana actual y explícita para recordar el consentimiento puede crear o reemplazar el archivo. La confirmación visible nombra operación, remoto, identidad saneada de URL, upstream, destino e instrucción de revocación.
- **Uso:** antes de cada fetch, session-start vuelve a resolver todos los enlaces desde la configuración Git actual e informa que usa consentimiento recordado. Leer o usar la concesión nunca la renueva, reescribe ni extiende.
- **Revocación:** instrucciones comunes como “olvida el consentimiento de fetch para este repositorio” eliminan el archivo después de informar la ruta local exacta. No cambian configuración Git ni acceden a red.
- **Invalidación:** cualquier diferencia de esquema, destino del repositorio, nombre del remoto, URL saneada, esquema/transporte, ref upstream, ref destino, orden efectivo de helpers, alcance de ruta de credenciales o comando/configuración SSH vuelve inutilizable la concesión. No se reescribe ni elimina automáticamente; se informa la categoría y se vuelve a aprobación interactiva.

Copiar o mover el archivo no transfiere consentimiento: la validez también depende del directorio Git/árbol de trabajo resuelto y los enlaces exactos. Una redirección observada solo durante acceso de red no amplía silenciosamente la identidad; un endpoint materialmente distinto exige consentimiento nuevo.

## Límite de fetch y transporte hacia session-start

Session-start deriva la identidad en solo lectura desde el repositorio y configuración Git actuales: raíz/directorio Git, rama, upstream, remoto del upstream, URL exacta de fetch, esquema, ref remota de destino y ruta efectiva de credenciales/helpers. Después consume directamente el Markdown fijo. No se introduce servicio de parser, daemon, base de datos, API de runtime ni estado global.

Una concesión recordada autoriza un único comando acotado equivalente a:

```text
git fetch --no-recurse-submodules --no-auto-maintenance <remote> <upstream-ref>:<destination-ref>
```

Siguen vigentes las protecciones no interactivas existentes para HTTPS o SSH. `--no-recurse-submodules` impide alcanzar otros repositorios. `--no-auto-maintenance` impide iniciar mantenimiento automático fuera de la operación nombrada; el mantenimiento explícito queda separado. Git LFS está excluido. Fetch aún puede escribir objetos locales, la ref remota acotada y `FETCH_HEAD`; invocar hooks de fetch configurados, la ruta seleccionada de credenciales/helper y helpers de transporte remoto; y revelar al remoto datos de conexión, repositorio, cliente y OIDs. Por eso la aprobación de red sigue siendo predeterminada. `GIT_TERMINAL_PROMPT=0` no neutraliza helpers gráficos de credenciales.

La integración fast-forward no es consentimiento de fetch. La intención actual combinada de sincronizar puede seguir cubriendo fetch acotado e integración solo fast-forward, pero una concesión recordada de fetch por sí sola no autoriza integración. Commit, push, publicación, herramientas de proveedor, acciones destructivas, resolución de conflictos, cambios de remoto y cambios de ruta de credenciales conservan autoridad separada.

## Reporte y privacidad

Cada uso recordado informa `operation`, `remote`, identidad saneada de host/esquema, refs upstream/destino y cómo revocar. Nunca imprime userinfo, query, fragmentos, credenciales, salida de helpers, rutas privadas de URL marcadas como sensibles ni OIDs descargados. Si no puede mostrar identidad segura, informa una descripción estable redactada y exige aprobación interactiva.

## Criterios de aceptación

- Sin concesión válida, se formula la pregunta de red existente y no se ejecuta fetch antes.
- La concesión explícita queda ligada a repositorio, operación, remoto, upstream/destino, es visible al usarse y fácil de revocar.
- Cada cambio material enumerado invalida su uso sin mutar la preferencia.
- El fetch autorizado es no interactivo, acotado a refs exactas, con `--no-recurse-submodules` y `--no-auto-maintenance`.
- No autoriza LFS, submódulos, integración, commit, push, proveedor, publicación, acciones destructivas ni el modo RH-W-0018.
- Lecturas y checks no crean directorios, archivos, consentimiento ni cambios Git.
- La documentación bilingüe conserva el ciclo RH-W-0046 completado mientras RH-W-0018 permanece `deferred`.

## Evidencia

Documentación primaria de Git revisada para esta decisión: [`git-fetch`](https://git-scm.com/docs/git-fetch), [protocolo v2](https://git-scm.com/docs/gitprotocol-v2), [credenciales](https://git-scm.com/docs/gitcredentials), [helpers remotos](https://git-scm.com/docs/gitremote-helpers), [hooks](https://git-scm.com/docs/githooks) y [submódulos](https://git-scm.com/docs/gitsubmodules). Los dos registros de roadmap y el contrato actual de session-start poseen la evidencia del repositorio. Los hechos externos son entradas del diseño, no afirmaciones de implementación.

## Fuera de alcance

Límite histórico del diseño: no hay excepción global de fetch, ledger, caché, base de datos, servicio, daemon, protocolo, expiración programada, limpieza automática ni framework general de preferencias. La implementación, pruebas, proyecciones de runtime, verificación y aprobación nativa RH-W-0046 están completas bajo autoridad separada; el comportamiento, la implementación y la entrega RH-W-0018 siguen sin seleccionar.
