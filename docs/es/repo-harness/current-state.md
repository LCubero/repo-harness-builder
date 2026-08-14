# Estado actual

La fuente canonica de estos hechos es [`../../repo-harness/current-state.md`](../../repo-harness/current-state.md).

## Estado verificado
<!-- doc-role:verified-state -->

- El harness esta presente y el verificador generado informa `current`, salida 0 y 0 escrituras.
- La suite canonica esta en verde: pasaron 197 pruebas con 6 omisiones. `RH-W-0023` reemplazo ambas afirmaciones fragiles de la hoja de ruta sin cambiar comportamiento de producto ni debilitar la validacion del ciclo de vida.
- El conjunto de entornos es `claude-code` y `opencode`; el modo es `integrated`, el generador es 3.0 y el paquete de habilidad es 3.1.
- La suite cubre estados Git generados, consentimiento por intencion, comparacion exacta del alcance preparado, separacion del indice previo, invalidacion por cambios materiales, resultados delimitados, deteccion sensible, recibos de transporte acotados y contratos de cache del entorno.
- La licencia MIT figura en `LICENSE` y en ambas copias de `SKILL.md`.
- El repositorio privado esta publicado y `main` sigue `origin/main`.

## Trabajo reciente completado
<!-- doc-role:recent-work -->

- Se separo el proyecto de `skills-pack-ld9`, se creo su harness y se alineo la licencia MIT.
- Se agregaron la guia del producto, el modelo recursivo autoalojado y la diferencia entre repositorios vacios y existentes.
- Se agregaron punteros raiz con preservacion exacta de bytes, clasificacion de actualizaciones previas y el ciclo operativo completo.
- Se hicieron reanudables las pausas de cierre y se exigieron evidencia posterior a mutaciones y continuidad final.
- Se agregaron consentimiento Git por intencion, compuertas operativas, revision por ruta, diagnosticos de transporte y clasificacion correcta del cache activo.
- Se agrego un control previo al commit que separa trabajo preparado con anterioridad.
- Se localizo la aprobacion estricta del constructor sin trasladarla a las habilidades conversacionales.
- Se sustituyeron las plantillas parciales en espanol, se agrego validacion proporcional por idioma y se bloqueo cualquier selector README unilateral.
- Se enruto la ayuda generada por los idiomas de documentacion declarados y el idioma de conversacion, nunca por el idioma del informe, para que jamas nombre una guia que esa misma corrida no crea.
- Se hizo que la remediacion ante fallos de transporte nombre una via de credenciales alternativa disponible, prefiriendo un asistente de credenciales del proveedor de un solo comando que no altera la configuracion global, y devolviendo el comando exacto cuando hace falta un secreto que el agente no puede aportar.
- Se ligo la respuesta de las sesiones generadas al idioma preferido declarado por la persona usuaria o, en su defecto, al idioma en que escribe, y se reemplazaron los literales en espanol fijos del informe del verificador que contradecian esa regla.
- El idioma de la documentacion pasa a ser una eleccion declarada: solo ingles por defecto, idiomas adicionales solo cuando se declaran, con el idioma de conversacion y los idiomas declarados registrados en el manifiesto, validados por el verificador generado y descubiertos por la comprobacion de congruencia.
- Se corrigio la comparacion por secciones para que el documento prometido elija el vocabulario de ambos lados; leer cada lado por separado comparaba roles contra cabeceras e informaba que un documento que agregaba marcadores habia perdido todas sus secciones.
- Se limito la comprobacion de idioma en documentos de propiedad humana al ingles sin traducir, que es un defecto a cualquier edad, en lugar de fijar frases sembradas que prohibian reescribir legitimamente.
- El repositorio quedo en congruencia total: marcadores `doc-role` incorporados a sus propios documentos durables, envoltorios `repo-harness-help` actualizados, y tanto el check del constructor como el verificador generado en `current`.
- Se cerraron las brechas del contrato de idiomas: los contratos de comando y skill ahora obtienen por separado los idiomas del informe, de conversacion y de documentacion; los ayudantes de ruteo validan las declaraciones igual que `render`; y la vista previa de CLI omite puntos de entrada documentales bloqueados.
- Se agrego un flujo bilingue de contribucion con issue primero, ramas enfocadas, PR enlazado, verificacion por unidad de trabajo, Conventional Commits y revision del maintainer antes del merge.
- Se completo el informe de ampliacion del conjunto de runtimes: `runtime_set_report()` ahora devuelve runtimes `unrecorded` y `staleManifest`, con cobertura de `RecordedRuntimeSetTests`.
- Se agrego la [hoja de ruta del harness adaptativo](roadmap.md) como propietaria ordenada del trabajo futuro acotado.
- Se completo la unidad 1 de la hoja de ruta al exigir que la guia generada del rulebook, inicio de sesion y documentacion reconcilie evidencia propietaria aplicable, informe discrepancias, prefiera la evidencia actual del repositorio sin lecturas ni ejecuciones de pruebas exhaustivas y continue de forma organica sin pedir a la persona usuaria que resuelva hechos verificables.
- Se completo la unidad 2, seguida por el issue #8, con ignore raiz exacto de `.codegraph/` por defecto, opt-out persistente mediante `--no-codegraph-ignore`, comprobacion desde el manifiesto, integracion de `.gitignore` que preserva bytes y rechazo de planes cambiados.
- Se completo `RH-W-0016` bajo issue #7: la hoja de ruta posee los registros inmutables y este archivo queda solo como proyeccion activa concisa.
- Se completo `RH-W-0003` bajo issue #11: los manifests heredados sin metadata de idioma conservan expectativas conocidas instaladas en ingles y espanol, mientras las declaraciones modernas explicitas siguen siendo autoritativas.
- Se completo `RH-W-0020`: el `README.md` y el `README.es.md` de producto dejaron de prometer salida bilingue incondicional. Ahora expresan el contrato de locales declarados, registran que un repositorio existente nunca gana un locale que no tenia, y el par espanol volvio a ser fiel al propietario en ingles.
- Se completo `RH-W-0023`: las pruebas autoalojadas de la hoja de ruta ahora afirman paridad estructural entre parser y encabezados, y el orden de la excepcion de prioridad P0, en lugar de un conteo magico y una frase que nombraba la siguiente unidad.

## Trabajo pendiente
<!-- doc-role:pending-work -->

- Proyeccion activa: `RH-W-0021`, `RH-W-0017`, `RH-W-0004`, `RH-W-0006` y `RH-W-0019` estan `ready`; `RH-W-0005`, `RH-W-0008`, `RH-W-0009` y `RH-W-0010` estan bloqueados por prerrequisitos.
- Proyeccion propuesta: el mapa de relaciones de documentacion `RH-W-0022` espera un diseno de alcance antes de poder pasar a `ready`.
- Proyeccion aplazada: `RH-W-0011` a `RH-W-0015` y la idea de mantenimiento interno desatendido `RH-W-0018` conservan condiciones explicitas en [roadmap.md](roadmap.md).
- Evidencia de bloqueo: la ronda 2 de Judgment Day sigue incompleta porque el juez B termino por limite. La hoja conserva los cinco hallazgos y marca `RH-W-0007` completado segun pruebas actuales.
- IDs completados recientemente: `RH-W-0023`, `RH-W-0020`, `RH-W-0003`, `RH-W-0016`, `RH-W-0007`, `RH-W-0002`.

## Siguiente paso
<!-- doc-role:next-step -->

Avanzar `RH-W-0021`: renombrar las habilidades generadas al prefijo fijo `repo-harness-`.

Las cuatro unidades listas despues de ella, en orden recomendado, son `RH-W-0017`, `RH-W-0004`, `RH-W-0006` y `RH-W-0019`. Cada una conserva su condicion de arranque en [roadmap.md](roadmap.md). Se toma una unidad por rama y no se agrupan.

### Como arrancar `RH-W-0021`

El registro completo, la evidencia y las exclusiones viven en [roadmap.md](roadmap.md). El camino ordenado es:

1. Escribir pruebas fallidas que afirmen los nombres generados `repo-harness-session-start`, `repo-harness-session-stop` y `repo-harness-documentation`.
2. Renombrar solo en la fuente canonica: `skill/repo-harness-builder/scripts/harness_tool.py`. Los nombres se construyen en las lineas 365, 466, 555 y 1455-1457, se consumen en 657-658, 842-844, 1049-1051 y 1161-1162, y se recortan para los comandos en 1475.
3. Mantener sin cambios los nombres de comandos. `removeprefix` debe seguir produciendo `session-start`, `session-stop` y `documentation`.
4. Regenerar el harness de este repositorio: renombrar los directorios bajo `.repo-harness/skills/`, `.claude/skills/` y `.opencode/skills/`, y actualizar `.repo-harness/manifest.json` en el mismo paso. El verificador falla hasta que manifiesto y directorios coincidan.
5. Actualizar cada documento que nombre una habilidad, en ingles y espanol, incluidos `REPO-HARNESS.md`, `README.md`, `CONTRIBUTING.md`, `AGENTS.md`, `CLAUDE.md` y los arboles de `docs/`.
6. Ejecutar la suite canonica y `python .repo-harness/check.py`, y luego reconciliar el registro `RH-W-0021`, esta proyeccion, su par espanol y el tema de Engram en la misma unidad de trabajo.
