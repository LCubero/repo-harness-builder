# Diseño RH-W-0022 de relaciones documentales declaradas

> **Estado: DISEÑO APROBADO POR EL PROPIETARIO. RH-W-0022 permanece `proposed`; un plan bilingüe de implementación se redactó con autorización posterior solo de planificación y ahora está aprobado por el propietario. La implementación recibió autorización posterior; R22-1 está en curso, no entregado.**

## Decisión

Agregar una sola declaración pequeña y local al repositorio con aristas documentales exactas, y hacer que el checker de solo lectura existente valide únicamente el contrato nombrado por cada arista. La declaración es explícita: enlaces, nombres, directorios y símbolos de código nunca crean aristas.

El propietario único es `.repo-harness/document-relationships.json`. Es un contrato técnico de propiedad humana, sembrado solo cuando falta y luego preservado byte por byte. No se convierte en propietario de documentación, ciclo de vida, cronología, decisiones ni investigación.

## Formato mínimo

El documento JSON UTF-8 usa una versión y una lista ordenada `edges`. Cada arista contiene exactamente tres strings: `source`, `relation` y `target`. Las rutas son relativas al repositorio, distintas, normalizadas, contenidas y de archivos regulares. Campos desconocidos, duplicados, relaciones desconocidas, rutas absolutas, escapes, enlaces, extremos ausentes o JSON/UTF-8 inválido dejan la arista incumplida.

La versión 1 admite solo `localized-peer` y `lifecycle-projection`. No almacena digest, fecha, estado, locale, evidencia, nota, wildcard, directorio, transitividad ni arista inversa inferida. El orden solo controla la presentación.

## Aristas iniciales

El sembrado contiene solo relaciones ya exigidas por propietarios del repositorio:

- `REPO-HARNESS.md`, `README.md`, `CONTRIBUTING.md` y `CHANGELOG.md` hacia sus pares exactos en español;
- `docs/repo-harness/README.md`, `usage.md`, `current-state.md`, `roadmap.md`, `decisions.md` e `i18n-maintenance.md` hacia sus pares exactos bajo `docs/es/repo-harness/`;
- `docs/repo-harness/roadmap.md` hacia `docs/repo-harness/current-state.md` como `lifecycle-projection`, y la arista española equivalente.

Diseños, planes, investigación, archivos históricos, proyecciones de runtime, skills generadas y Markdown arbitrario quedan sin declarar. Solo se agrega una arista exacta cuando un propietario canónico existente exige la relación.

## Semántica de verificación

Para `localized-peer`, el checker verifica que ambos extremos sean archivos Markdown UTF-8 directos y contenidos, que el destino identifique la fuente exacta como propietario canónico y que pasen las comprobaciones existentes de locale y roles semánticos. No afirma equivalencia de traducción, no reescribe prosa, no compara archivos completos ni exige que ambos cambien en el mismo diff Git.

Para `lifecycle-projection`, reutiliza el parser de ciclo de vida del roadmap y verifica contra la fuente cada estado y hecho derivado de cartera que afirme el destino. No obliga a repetir todos los registros ni infiere relaciones para otros documentos.

La declaración y los extremos se leen sin modificar. Un fallo preserva todos los bytes humanos. Una fuente modificada puede seguir satisfecha si su contrato objetivo continúa verdadero: la arista es una obligación, no una regla mecánica de mismo commit.

## Resultado exacto del checker

Cada arista identificable incumplida agrega una entrada al arreglo `files`, en orden de declaración. UTF-8 o JSON inválido en el propietario completo produce una sola entrada del propietario con `path`, `state` y `reason: invalid-declaration`, sin campos de arista inventados. Las entradas de aristas identificables tienen con `path` igual a `.repo-harness/document-relationships.json`, `state` igual a `relationship-unsatisfied`, y los campos exactos `source`, `relation`, `target` y `reason`.

`reason` usa uno de estos tokens cerrados: `invalid-declaration`, `missing-endpoint`, `unsafe-endpoint`, `canonical-owner-mismatch`, `locale-contract-failed` o `status-mismatch`. El `status` global es `drift`, `writes` permanece `0` y la CLI sale con `1`, usando la serialización JSON existente. Las aristas satisfechas no agregan entrada y el reporte nunca incluye contenido documental.

## Localización y coordinación

El propietario JSON es significativo para máquina y permanece solo en inglés. Este diseño y su par canónico en inglés lo explican. Los documentos ingleses conservan propiedad factual; los pares españoles son presentaciones humanas completas. El checker solo informa: nunca repara, traduce, prepara ni propone bytes.

RH-W-0044 puede sugerir después un cambio humano aprobado individualmente; no posee ni infiere aristas. RH-W-0065 puede declarar en el futuro una arista exacta para un propietario de referencias aprobado por separado, pero conserva la procedencia y revalidación de fuentes, no este mapa ni su ciclo de checker. Ninguna unidad se fusiona con RH-W-0022.

## Pruebas RED requeridas para una implementación futura

- Un render bilingüe nuevo carece del propietario antes de implementar y lo siembra exactamente después; uno existente nunca se sobrescribe.
- Una arista inválida, duplicada, insegura o desconocida produce la entrada exacta, salida `1` y cero escrituras.
- Un par español que nombra otro propietario produce `canonical-owner-mismatch` sin cambiar bytes humanos.
- Un fallo existente de locale o roles produce `locale-contract-failed` sin afirmar equivalencia de traducción.
- Un estado de current-state que contradice al roadmap produce `status-mismatch` y pasa al corregir solo la proyección.
- Una edición de fuente que conserva el contrato sigue satisfecha, sin regla Git ni timestamp.
- Un enlace, nombre, símbolo o fuente RH-W-0065 no declarado no crea arista ni resultado.
- Los checkers canónico, OpenCode, Claude y Pi coinciden usando solo la biblioteca estándar de Python 3.10+.

## No objetivos

- El trabajo de diseño original no incluyó un plan de implementación ni autorizó implementación, avance de ciclo, staging, commit, push, PR, release ni publicación.
- Sin inferencia por enlaces, nombres, directorios, sufijos de locale o similitud.
- Sin grafo de símbolos, rutas de llamada, motor de dependencias, cierre transitivo, daemon, scheduler, watcher, base de datos, servicio ni dependencia externa.
- Sin puntuación de traducción, equivalencia semántica, reparación automática, regeneración completa ni cambio de propiedad humana.
- Sin duplicar el ciclo de referencias, procedencia, archivo o revalidación de RH-W-0065.

## Límite de aprobación

El propietario revisó directamente y aprobó técnicamente este diseño el 2026-09-06. Una autorización posterior solo de planificación produjo un [plan de implementación bilingüe](rh-w-0022-implementation-plan.md) propuesto, ahora aprobado por el propietario. RH-W-0022 permanece `proposed`; la implementación recibió autorización separada y sigue en curso.
