# Diseño RH-W-0044 de sugerencias para documentos de propiedad humana

> **Estado: DISEÑO APROBADO POR EL PROPIETARIO. RH-W-0044 permanece `proposed`; la planificación de implementación y la implementación no están autorizadas.**

## Decisión

El builder puede comparar un conjunto acotado de documentos contractuales de propiedad humana que genera con las versiones actuales del repositorio e informar sugerencias concretas e independientes. Comparar y aceptar nunca transfiere propiedad al builder: preview no escribe, cada sugerencia se acepta por separado y una sugerencia aceptada debe entrar al plan existente ligado a contenido con comprobaciones nuevas de bytes fuente y digest antes de cualquier escritura.

El propietario aprobó este diseño después de la decisión técnica de aprobación. Esa aprobación no autoriza planificación de implementación ni implementación. Los límites canónicos siguen en el [registro canónico](../../repo-harness/roadmap.md#rh-w-0044), las [decisiones permanentes](decisions.md), la [arquitectura actual](../../../skill/repo-harness-builder/references/architecture.md), las [reglas de preservación](../../../skill/repo-harness-builder/references/preservation-and-safety.md) y el [diseño canónico en inglés](../../repo-harness/rh-w-0044-design.md).

## Conjunto de documentos comparados

La comparación se limita a rutas Markdown de propiedad humana que emite el renderer y cuyo material define contratos reutilizables de operación o navegación del harness: `REPO-HARNESS.md` y su par; material acotado de harness en `README.md` y su par; `docs/repo-harness/README.md`, `usage.md` e `i18n-maintenance.md` con pares declarados; y archivos renderizados bajo `.repo-harness/rules/`.

Se excluyen los historiales, `current-state.md`, `roadmap.md` y `decisions.md` con sus pares. Sus hechos, ciclo, cronología y decisiones no tienen reemplazo genérico. También quedan fuera prosa arbitraria, producto, investigación, planes, diseños, archivos, sesiones, rutas no rastreadas o ignoradas, archivos generados poseídos y el manejo de directivas. Un documento apto debe ser regular, directo, legible y contenido; otros casos conservan su clasificación existente.

## Unidades de comparación

El archivo completo del renderer es evidencia, no reemplazo. Solo se comparan unidades estables identificadas por `doc-role`, rol de fuente, selector exacto o encabezado de regla. La prosa sin propietario estable no se compara.

| Resultado | Significado | Comportamiento |
|---|---|---|
| `satisfied` | Conserva la obligación con prosa específica permitida. | Sin sugerencia. |
| `missing-obligation` | Falta la contraparte de una obligación estable. | Inserción acotada. |
| `outdated-obligation` | Redacción anterior identificable contradice el contrato actual. | Reemplazo mínimo. |
| `ambiguous` | Emparejar exigiría interpretar intención o equivalencia. | Incertidumbre sin bytes. |
| `accepted-divergence` | Una declaración válida excluye esa unidad exacta. | Informar y suprimir. |

Esto compara presencia contractual; no reescribe prosa general, corrige estilo, resume ni puntúa equivalencia.

## Forma y presentación

Cada sugerencia contiene `suggestionId` estable, ruta, idioma, rol o ancla, clasificación, motivo, evidencia, SHA-256 fuente, bytes UTF-8 propuestos, extracto y preview acotados, y opciones `accept`, `keep` o `declare-intentional-divergence`.

Se ordenan y quedan fuera del plan ejecutable hasta aceptarse. Preview informa `writes: 0`. Se presenta una por vez por ruta y resolverla obliga a comparar de nuevo.

## Aceptación y autoridad

`accept` autoriza solo lo mostrado. El builder relee, recalcula e incorpora una integración al plan ligado a contenido, con ruta, digests, tipo y digest del plan. Todo cambio invalida la aceptación. No existen aceptación masiva, aprobación por documento, reutilización, archivo automático ni escritura directa. `keep` solo conserva bytes durante el preview actual y no crea estado.

## Divergencia intencional

La intención durable se declara junto a la unidad:

```text
<!-- repo-harness:accepted-divergence <contract-unit-id> -->
```

Solo suprime esa unidad, documento e idioma. No suprime estructura, idioma, directivas, otras sugerencias, seguridad o deriva generada. IDs desconocidos, duplicados, mal ubicados o no suprimibles producen `ambiguous`. Crear el marcador es otra sugerencia individual sujeta al mismo plan; registra intención, no equivalencia ni corrección permanente.

No son suprimibles el límite del repositorio, aprobaciones de mutación y publicación, propiedad generada, preservación de bytes y enrutamiento visible hacia sus propietarios. Pueden reescribirse, pero su ausencia no puede declararse satisfecha.

## Localización y límites

Solo participan idiomas declarados. Inglés y español usan sugerencias y aceptaciones independientes. RH-W-0022 conserva relaciones, RH-W-0066 enruta futuras peticiones de recordar y RH-W-0045 conserva referencias legacy. No se fusionan.

## Criterios de aceptación

- Preview no escribe y solo sugiere unidades aptas del conjunto exacto.
- No cambian clasificaciones actuales.
- Historial, estado, roadmap y decisiones no reciben sugerencias genéricas.
- Cada sugerencia lleva identidad, ruta, digest, motivo, evidencia, bytes y tres opciones.
- Se acepta una por vez mediante plan nuevo ligado a digest; cualquier cambio invalida antes de escribir.
- `keep` es efímero y la supresión durable requiere declaración válida y aceptada.
- La divergencia no suprime autoridad, contención, preservación o enrutamiento protegidos.
- Los idiomas siguen siendo decisiones independientes.
- No se afirma equivalencia semántica ni propiedad sobre prosa humana.

## Pruebas RED futuras

- Una obligación identificable ausente recibe una sugerencia y cero escrituras.
- Prosa específica válida no recibe reemplazo completo.
- Historial, estado, roadmap, decisiones, producto e ignorados no reciben sugerencias.
- No se aceptan dos juntas y la primera obliga a comparar de nuevo.
- Cambios posteriores rechazan antes de escribir.
- `keep` reaparece; un marcador válido suprime solo su unidad.
- Marcadores inválidos no suprimen y reportan ambigüedad.
- Aceptar inglés no acepta ni muta español.
- Los bytes humanos quedan intactos ante cualquier fallo.

## No objetivos

- Sin implementación, plan de implementación, avance, staging, commit, push, PR, release ni mutación de consumidores.
- Sin regeneración completa, aceptación masiva, merge automático, modelo semántico, juez LLM, puntuación, daemon, base de datos, ledger ni dependencia externa.
- Sin nuevo propietario durable ni reemplazo de límites RH-W-0022, RH-W-0045 o RH-W-0066.

## Ciclo de vida y decisión humana

RH-W-0044 permanece `proposed`. El propietario aprobó el conjunto, las unidades, el schema, la aceptación individual y la divergencia. La planificación de implementación y la implementación requieren autorización posterior y separada.
