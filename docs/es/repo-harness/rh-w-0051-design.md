# Diseno RH-W-0051 de taxonomia de estado local

> **Estado: IMPLEMENTADO Y VERIFICADO. Se conservan abajo las declaraciones historicas previas a implementar.**

## Contrato de revision

Revise este diseno contra el [registro canonico RH-W-0051](roadmap.md#rh-w-0051), las [decisiones permanentes](decisions.md) y el [propietario en ingles](../../repo-harness/rh-w-0051-design.md). La evidencia del repositorio posee el comportamiento actual. Este diseno solo define la taxonomia de almacenamiento y no implementa comportamiento de sesion.

## Decision

El estado local propiedad del harness usa cuatro namespaces fijos relativos al repositorio:

| Ruta | Clase | Retencion | Autoridad |
|---|---|---|---|
| `.repo-harness/tmp/` | Estado de trabajo descartable | Puede eliminarlo una operacion autorizada del harness | No autoritativo |
| `.repo-harness/archive/` | Estado archivado recuperable | Nunca se elimina automaticamente | No autoritativo |
| `.repo-harness/state/` | Estado operativo actual pequeno reservado para consumidores declarados | Definida por cada consumidor | No autoritativo |
| `.repo-harness/memory-bk/` | Backups locales de continuidad de sesion reservados para RH-W-0069 | Al inicio nunca se eliminan automaticamente | No autoritativo |

El nombre es `memory-bk`, no `memory`, para mantener explicito el limite: es un backup de continuidad alterno, no una API generica de memoria ni un segundo almacen de documentacion.

## Limites de propiedad

- RH-W-0051 posee rutas fijas, contencion, comportamiento de ignore en Git, semantica de arbol limpio, clasificacion del checker y representacion en el manifest.
- RH-W-0069 posee en exclusiva el schema de backup, orden de lectura y escritura, filtro de privacidad, comprobacion cruzada con Engram e integracion de sesiones.
- RH-W-0053 conserva los checkpoints tipados. Su ubicacion reservada es `.repo-harness/state/checkpoints/`; un backup nunca se convierte en checkpoint ejecutable.
- RH-W-0046 y la aplazada RH-W-0018 conservan consentimiento o preferencias. Su ubicacion reservada es `.repo-harness/state/preferences/`.
- RH-W-0040 conserva el registro de violaciones. Su ubicacion reservada es `.repo-harness/state/violations/`.
- RH-W-0052 conserva archivo, desinstalacion, reinstalacion y aprobacion de borrado.
- El almacenamiento personal, `.ld9/`, `.atl/`, `.codegraph/` y `.engram/` quedan fuera de la propiedad del harness.

Ningun namespace posee documentacion, aprobaciones, ciclo de vida, conflictos, recibos, entrega ni verdad canonica del proyecto.

## Seguridad de rutas y Git

Cada ruta de estado local se resuelve desde la raiz seleccionada, se normaliza y se rechaza si escapa de la raiz, atraviesa una ruta indirecta o apunta fuera del namespace exacto declarado. Los espacios multiraiz resuelven cada repositorio seleccionado de forma independiente. Una carpeta sin Git usa las mismas reglas de contencion y propiedad sin fingir evidencia Git.

Los cuatro namespaces se ignoran en Git de forma predeterminada y quedan fuera de las preocupaciones de arbol limpio. La implementacion agrega directivas raiz exactas y preserva los bytes humanos de `.gitignore` mediante el modelo existente de append y aprobacion. Una anulacion por repositorio solo puede cambiar ignore o retencion mediante un futuro diseno explicito y acotado; este diseno no define su sintaxis.

`python .repo-harness/check.py` simple sigue siendo de solo lectura. Informa soporte o deriva sin crear directorios, backups ni entradas de ignore. La ausencia de un directorio local no es deriva por si misma, porque se crea solo cuando un consumidor escribe estado aprobado. La ausencia de un ignore obligatorio o una taxonomia declarada inconsistente si es deriva.

## Impacto en manifest y checker

La implementacion agrego un unico campo `localState`, no uno por consumidor. Su valor exacto es el mapa ordenado fijo de nombres hacia rutas relativas y clases. El manifest autenticado y el checker generado cambiaron atomicamente porque el checker exige campos exactos.

El checker valida rutas, clases, contencion e ignore. No enumera ni analiza archivos locales, juzga contenido de backups, aplica retencion ni trata estado ignorado como salida generada. RH-W-0069 puede agregar pruebas de comportamiento sin otro campo de manifest.

## Compatibilidad y migracion

- Las instalaciones sin `localState` usan la ruta existente de reemplazo autenticado en una pasada del par manifest/checker.
- La migracion no crea directorios ni mueve archivos.
- El contenido personal `.ld9/` nunca se descubre, copia, migra, renombra ni elimina automaticamente.
- El contenido existente bajo `.repo-harness/` queda intacto salvo el par manifest/checker autorizado por separado y la integracion acotada de `.gitignore`.
- Una futura migracion explicita desde almacenamiento personal o heredado requiere otro diseno aprobado.

## Pruebas RED enfocadas historicas

Los requisitos aprobados antes de implementar fueron:

1. Declarar exactamente `tmp`, `archive`, `state` y `memory-bk` bajo `.repo-harness/`.
2. Contencion raiz y rechazo de traversal, rutas absolutas, indirectas y entre raices.
3. Planificacion exacta de ignore que preserve bytes.
4. Check de solo lectura con cero escrituras cuando faltan directorios.
5. Deriva cuando difiere el ignore obligatorio o la taxonomia.
6. Reemplazo atomico del par manifest/checker autenticado.
7. Ausencia de creacion o migracion automatica durante preview, check o upgrade.
8. Cero lectura, escritura, movimiento o borrado bajo `.ld9/` y rutas de otras herramientas.
9. Comportamiento acotado por repositorio sin Git y multiraiz.
10. Paridad bilingue, enlaces validos y rechazo de Unicode prohibido.

## Exclusiones

- Sin comportamiento de inicio o cierre de sesion.
- Sin schema, nombre de archivo, filtro, operacion Engram, conflicto ni limpieza de retencion.
- Sin implementar checkpoints, preferencias, consentimiento, violaciones, borrado de archivo ni migracion.
- Sin base de datos, cifrado, daemon, scheduler, servicio de fondo, API generica ni configuracion compleja.
- El trabajo exclusivo de diseno no cambio skills, fuente, pruebas, manifest, checker, directorios, staging, commits, push ni red. La implementacion posterior cambio solo sus superficies autorizadas por separado.

## Rollback

El rollback historico de diseno eliminaba este documento y sus referencias. El rollback de implementacion restaura el par manifest/checker y el ignore por la ruta atomica normal. Nunca elimina automaticamente contenido local.

## Condiciones de finalizacion

- Ambos pares y mapas coinciden en los cuatro namespaces y sus limites.
- RH-W-0051 esta `completed`; su implementacion autorizada por separado cumplio este diseno sin agregar consumidores.
- RH-W-0069 depende de esta taxonomia sin ampliar RH-W-0051 hacia comportamiento de sesion.
- El orden coloca RH-W-0069 despues de RH-W-0051 y antes de consumidores que necesitan su continuidad.

## Decisiones humanas restantes

Ninguna para este diseno completado. La sintaxis de anulacion, limpieza, migracion o implementacion consumidora exige autorizacion separada bajo su unidad propietaria.
