# Diseño RH-W-0091 de prioridad de inicio y reanudación primero

> **Estado: DISEÑO HISTÓRICO COMPLETADO. RH-W-0091 está `completed`. El plan bilingüe corregido de implementación fue aprobado, implementado y verificado de forma independiente bajo autoridad separada.**

Ciclo canónico: [RH-W-0091](roadmap.md#rh-w-0091). Alcance: [rh-w-0091-scope.md](rh-w-0091-scope.md). Par en inglés: [rh-w-0091-design.md](../../repo-harness/rh-w-0091-design.md).

## Límite y propiedad

[decisions.md](decisions.md) sigue siendo el único propietario de la carta y de LLM-first; este diseño solo lo señala. La carta es una entrada acotada, no política copiada. El roadmap conserva el ciclo de vida, `current-state.md` conserva la proyección concisa y `CHANGELOG.md` conserva la cronología completada. Este diseño no reordena esos documentos durables.

`RH-W-0069` conserva el único esquema nativo de backup, su seguridad, privacidad, validación y semántica de escritura. `RH-W-0088` conserva la recuperación de Engram como evidencia de disponibilidad. Solo `RH-W-0078` posee la división física de skills generadas y la mecánica de divulgación progresiva. `RH-W-0031`, `RH-W-0071`, `RH-W-0082` y `RH-W-0018` conservan la procedencia de idioma y la semántica de preferencias o restricciones continuas; el idioma de conversación verificado más las preferencias estables activas o restricciones continuas de esos propietarios existentes preceden la recuperación del backup. Gentle AI conserva enrutamiento, revisión, recibos y corrección en un límite habilitado y aplicable.

Engram es evidencia de continuidad y disponibilidad con alcance, nunca autoridad del repositorio. No requiere preflight separado. El inicio no prueba Gentle AI por separado; solo llega a Gentle AI en su límite habilitado y aplicable.

## Modelo mínimo de lectura con reanudación primero

Para un inicio de sesión pertinente, leer solo esta secuencia acotada antes de cualquier disparador nombrado:

1. el puntero acotado a la carta en `decisions.md`;
2. idioma de conversación verificado más preferencias estables activas o restricciones continuas desde sus propietarios existentes;
3. `Where We Left Off` del backup válido más nuevo de `RH-W-0069`; y
4. propietarios más profundos solo cuando lo exija la matriz de disparadores.

Los propietarios del repositorio siguen siendo autoritativos. La evidencia faltante, inválida, obsoleta o contradictoria se informa y resuelve contra su propietario vigente; nada se infiere del orden. La recuperación Engram con alcance aporta evidencia de disponibilidad/continuidad cuando aplique, sin prueba, preflight ni informe repetido de éxito sin cambios.

### Matriz de disparadores para lectura profunda

| Disparador | Lectura profunda requerida |
| --- | --- |
| Objetivo explícito o `RH-W-####` | El propietario nombrado y la evidencia directamente pertinente |
| Contradicción | Los propietarios vigentes en conflicto hasta que la evidencia del repositorio la resuelva, o una decisión humana concreta si no puede resolverla |
| Evidencia de prioridad inválida o ausente | El propietario que puede validar o reemplazar esa evidencia |
| Reconciliación obligatoria | Los propietarios canónicos aplicables de ciclo, proyección, localización y continuidad |

Ningún disparador autoriza lectura exhaustiva, continuación automática, inferencia de preferencias ni un almacén nuevo.

## Forma de reanudación en un solo backup y orden de cierre

Dentro del esquema existente de un solo backup de `RH-W-0069`, `Where We Left Off` es la primera sección después de `# Session Continuity`. Contiene exactamente estos encabezados, en este orden:

```markdown
## Where We Left Off

### Next Action

### Pending Work

### Current Blocker
```

Un valor vacío es `None.` Las etiquetas de evidencia en línea existentes siguen aplicando. Las secciones detalladas de continuidad existentes siguen este bloque de reanudación sin cambiar de propiedad. Es una modificación de orden de lectura/escritura dentro del mismo backup exclusivo con timestamp UTC, no un segundo almacén de resumen, ciclo, aprobación, recibos o conflictos.

En una pausa o cierre deliberado, session-stop debe: (1) refrescar la evidencia aplicable; (2) determinar los tres campos de reanudación en el orden indicado; (3) escribir y validar ese bloque antes de la continuidad detallada; y luego (4) conservar los filtros de contenido, escritura exclusiva, orden local-antes-de-Engram, validación y resultados independientes de RH-W-0069. Ningún comportamiento de backup cambia hasta una implementación autorizada por separado.

## Categorías RED futuras enfocadas

Antes de una implementación autorizada por separado, la cobertura RED enfocada debe demostrar:

1. referencia a la carta sin copiar la carta;
2. idioma de conversación verificado más preferencias estables activas o restricciones continuas antes de recuperar el backup, sin inferir desde evidencia ausente o inválida;
3. selección del backup válido más nuevo y `Where We Left Off` antes del detalle posterior;
4. `Next Action`, `Pending Work` y `Current Blocker` exactamente ordenados, con `None.` cuando estén vacíos;
5. cada ruta de la matriz de disparadores y ninguna lectura profunda sin disparador;
6. evidencia Engram con alcance sin preflight separado, y Gentle AI solo en un límite habilitado y aplicable;
7. esquema, privacidad, creación exclusiva, resultados local-antes-de-Engram y ausencia de segundo almacén de resumen preservados de RH-W-0069; y
8. propiedad preservada de RH-W-0078, idioma, preferencias, ciclo, cronología, enrutamiento, revisión, recibos y corrección.

Son categorías de contrato, no detalle de implementación ni prueba de modelo en vivo.

## Compatibilidad, rollback y exclusiones

Los backups válidos existentes sin `Where We Left Off` siguen siendo legibles mediante la continuidad detallada de RH-W-0069; no reciben campos sintetizados. Una implementación futura debe escribir el nuevo bloque solo en backups creados después y preservar todos los backups existentes. Ninguna migración, cambio de retención, limpieza, parser/API, base de datos, daemon, cache, escritura de preferencias, proyección de skill generada, implementación de pruebas, acción Git, acción de red o entrega pertenece a este diseño.

El rollback del diseño elimina solo este documento y sus referencias documentales. Un rollback futuro de implementación elimina el contrato de reanudación primero y sus pruebas enfocadas, preservando los backups y observaciones Engram existentes. Ninguno borra continuidad ni altera propietarios durables.

## Condiciones de finalización

- Los diseños en inglés y español son semánticamente equivalentes y están enlazados.
- `RH-W-0091` está `completed`; este diseño sigue como evidencia histórica aprobada por el propietario.
- El modelo de lectura, la matriz exacta de disparadores, los campos de reanudación, la compatibilidad, el rollback y las exclusiones preservan cada propietario nombrado.
- La planificación y ejecución se autorizaron por separado; la verificación final y el cierre de ciclo están completos, sin afirmar entrega.
