# Investigacion del flujo de contribucion

La fuente factual es [`../../repo-harness/contribution-workflow-research.md`](../../repo-harness/contribution-workflow-research.md).

## Recomendacion

Adoptar una guia breve con issue primero, ramas enfocadas, PR enlazado, Conventional Commits, pruebas y documentacion por unidad de trabajo, y revision del maintainer antes del merge. La aprobacion debe ser conversacional hasta que exista automatizacion real.

## Fuentes primarias

- El CONTRIBUTING de un repositorio privado, `repositorio 1`: ruta breve, issue primero flexible, ramas cortas, commits convencionales, verificacion, revision de secretos y objetivo de 400 lineas.
- El CONTRIBUTING de un repositorio consumidor privado, `consumidor 2`: integracion operativa con REPO-HARNESS y Engram, ademas de limites claros de rama, PR y aprobacion.
- [Gentle AI CONTRIBUTING](https://github.com/Gentleman-Programming/gentle-ai/blob/main/CONTRIBUTING.md): entrega estricta basada en issues, convenciones, unidades de trabajo, presupuesto de revision y politica automatizada.
- [Gentle AI cognitive-doc-design](https://github.com/Gentleman-Programming/gentle-ai/tree/main/skills/cognitive-doc-design): respuesta primero, revelado progresivo, bloques pequenos, reconocimiento y empatia de revision.

## Hallazgos y adaptacion

Una ruta rapida y una lista final reducen memoria necesaria. El issue fija alcance; el PR conserva la trazabilidad. Ramas pequenas y unidades entregables reducen costo de revision y rollback. Conventional Commits aporta vocabulario sin nuevas dependencias.

La guia local nombra Python 3.10+ con biblioteca estandar, fuentes canonicas en `skill/` y `command/`, proyecciones `.claude/` y `.opencode/`, `python .repo-harness/check.py`, continuidad mediante Engram y Gentle AI, y `.codegraph/` sin seguimiento. Prohibe cambios directos a `main` salvo emergencia autorizada explicitamente.

## Tradeoffs

`repositorio 1` tiene el mejor tamano cognitivo, pero necesita reglas propias de fuentes y proyecciones. `consumidor 2` aporta continuidad util, pero es demasiado largo y especifico para copiar. Gentle AI ofrece el modelo conceptual mas fuerte, pero sus labels, regex, templates y checks no existen aqui. Copiarlos prometeria automatizacion falsa.

## Limite futuro del producto

El builder deberia poder proponer, crear y gestionar un harness de contribucion para repositorios consumidores. Primero debe descubrir la politica existente y las capacidades reales; luego podra proponer `CONTRIBUTING`, issue primero, ramas y PR, y solo templates, labels o checks existentes o aprobados explicitamente. Esta investigacion no implementa esa capacidad.

## Confianza

La confianza es alta para el flujo local porque se comprobaron archivos, labels, propiedad de fuentes y comandos. La automatizacion futura requiere diseno de producto y descubrimiento de capacidades.
