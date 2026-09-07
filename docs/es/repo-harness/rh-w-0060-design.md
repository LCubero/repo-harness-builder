# Diseño RH-W-0060 de reporte del estado del repositorio

Estado: `proposed`; diseño limitado a C11 aprobado por el propietario. La planificación de implementación y la implementación no están autorizadas por separado.

## Decisión

Agregar una cláusula condicional a session-stop generado para C11. Su clasificación de unidades Git ya posee el reporte de rutas en staging, sin staging y no rastreadas. Cuando la normalización de EOL o filtros explique plausiblemente los bytes observados de una ruta rastreada, debe comparar el blob del índice o la vista del árbol de trabajo limpiada por Git para esa ruta, según corresponda, con el blob commiteado `HEAD:<path>`. La igualdad significa diferencia solo de EOL o filtro, no modificación de contenido. Una diferencia sustantiva permanece modificada en contenido. La evidencia no disponible o ambigua produce desconocido.

C12 queda cerrado sin residuo generado. La enumeración de ramas y el análisis de ascendencia pertenecen únicamente a una revisión de ramas solicitada de forma explícita, no al comportamiento rutinario de session-start generado. El reporte existente de la rama activa y su upstream permanece sin cambios.

## Por qué corresponde al comportamiento generado

Se prefirió política local salvo que existiera una necesidad del consumidor. C11 está dentro del informe ya generado de session-stop que clasifica rutas Git antes del staging. El builder también emite reglas de texto y EOL por ruta para archivos generados comparados byte a byte. Dejar la corrección solo en los acuerdos locales no cambiaría el informe consumidor que posee la afirmación.

No se introduce un propietario nuevo. Los acuerdos de trabajo conservan proporcionalidad y detención, sin duplicar mecánica de reportes Git.

## Alternativas consideradas

1. **Solo acuerdo de trabajo local.** Es la edición más pequeña, pero se rechaza porque corrige únicamente este repositorio y deja intactas las afirmaciones consumidoras.
2. **Una cláusula condicional en session-stop generado, seleccionada para C11.** Corrige la afirmación donde se emite y reutiliza Git.
3. **Inventario de ramas en session-start generado, rechazado para C12.** La enumeración de ramas pertenece a una revisión de ramas solicitada explícitamente; el reporte rutinario de rama y upstream queda sin cambios.

## Límites de comportamiento

### C11 clasificación de contenido

La comparación adicional se ejecuta solo para una ruta de texto rastreada cuyos atributos o bytes observados hagan plausible la normalización. Compara el blob del índice o la vista del árbol de trabajo limpiada por Git para esa ruta con el blob commiteado. No oculta cambios sustantivos porque también difieran los finales de línea.

El informe nombra las representaciones comparadas y el resultado. Si falta el blob, los atributos son ambiguos, un filtro personalizado impide una comparación confiable o Git no puede producir un lado, la clasificación queda desconocida y se indica la evidencia faltante.

## Obligaciones de aceptación para una implementación posterior autorizada

Las pruebas enfocadas deben fallar primero para estas conductas exactas:

1. Una diferencia solo de EOL no es modificación de contenido.
2. Una diferencia sustantiva permanece modificada en contenido.
3. Un blob de índice diferente permanece modificado en contenido.
4. La evidencia insuficiente produce desconocido.

La fuente canónica generada y las proyecciones necesarias solo cambiarían después de aprobación separada. Este diseño no agrega plan de implementación ni avanza el ciclo de vida.

## Exclusiones explícitas

Sin cambios en session-start; inventario de ramas ni comportamiento de ascendencia; limpieza ni borrado automático de ramas; cambios de fetch, red, credenciales o transporte; detección o resolución de conflictos; rediseño de `.gitattributes`; motor amplio de inventario Git; estado persistente; comando, API, dependencia, herramienta, resultado o almacén de autoridad nuevos.
