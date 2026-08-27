# Plan de implementacion RH-W-0073

**Estado: RH-W-0073 esta `completed`. Este plan se ejecuto el 2026-08-27 y su cuerpo se conserva como evidencia historica de ejecucion, no como instrucciones a seguir.**

Ejecutado contra el [diseno RH-W-0073](rh-w-0073-design.md) aprobado por el propietario y el Plan 22 de la hoja de ruta. Su par en ingles es [rh-w-0073-implementation-plan.md](../../repo-harness/rh-w-0073-implementation-plan.md).

## Linea base confirmada antes de editar

Re-medida en `d7e8cea` en vez de confiar en el diseno, que midio en `e290c53`:

| Superficie | Antes | Presupuesto | Despues |
|---|---|---|---|
| `REPO-HARNESS.md` | 763 | 700 | 650 |
| `.repo-harness/rules/working-agreements.md` | 344 | 700 | 483 |
| `.repo-harness/rules/response-contract.md` | 436 | 700 | 436 |
| `.repo-harness/rules/memory.md` | 317 | 700 | 317 |
| `REPO-HARNESS.es.md` | 581 | 700 | 581 |

El incumplimiento no habia cambiado respecto de la medicion del diseno. 574 palabras sostenian 763 tokens proxy contra un techo de 525 palabras.

## Tareas tal como se ejecutaron

- [x] 1. Escribir la asercion RED de presupuesto que mide `REPO-HARNESS.md` y cada archivo de `.repo-harness/rules/` en disco, nombrando la ruta infractora. Fallo con `763 not less than 700 : REPO-HARNESS.md holds 763 proxy tokens against 700`.
- [x] 2. Escribir la asercion companera de que `RULEBOOK_TOKEN_BUDGET` sigue en 700, que `render_with_skill_mapping` no contiene referencia al presupuesto, y que `over-budget` no aparece en la herramienta. Paso antes y despues del cambio, probando que ningun rechazo volvio.
- [x] 3. Mover la regla de anunciar contra ejecutar a la seccion `Stopping` de `.repo-harness/rules/working-agreements.md`, dejando `Execute authorized work you have announced; do not stop at the announcement.` en el reglamento.
- [x] 4. Mover la taxonomia de clasificacion de evidencia a la seccion `Honesty` del mismo archivo, dejando `Label every claim as verified fact, supported inference, unknown, or human decision required, and never substitute confidence for evidence.` en el reglamento.
- [x] 5. Agregar el guard contra regeneracion. `.repo-harness/rules/` lo posee `render`, asi que una regeneracion dejaria caer el detalle movido y las lineas cortas del reglamento apuntarian a nada. La asercion comprueba que cada linea corta esta en el reglamento, que la fila de compuerta hacia el archivo de reglas esta presente, y que cada bloque movido sigue existiendo en ese archivo.
- [x] 6. Corregir el criterio de aceptacion muerto de `over-budget` en `references/validation.md` y agregar el criterio que cubre la medicion en tiempo de desarrollo. Sincronizar ambas proyecciones de runtime de esa referencia.
- [x] 7. Reconciliar `REPO-HARNESS.es.md`. Ver la desviacion abajo.
- [x] 8. Actualizar el conteo documentado de la suite en ambos pares de estado actual, de 451 a 454.
- [x] 9. Verificar: suite completa, checker, paridad de proyecciones.

## Una desviacion del Plan 22, con su razon

El paso 5 del Plan 22 decia reconciliar `REPO-HARNESS.es.md` como espejo equivalente. El reglamento en espanol se comprimio igual que el ingles y luego se **revirtio**, porque la compresion habria dejado la obligacion inalcanzable.

El reglamento en ingles lleva una fila de compuerta que enruta a `.repo-harness/rules/working-agreements.md`. El reglamento en espanol no la lleva, y no la llevaba antes de este cambio: en `d7e8cea` la cadena `working-agreements` aparece cero veces en `REPO-HARNESS.es.md`. El reglamento ingles tiene 13 lineas de tabla de compuertas, el espanol 9.

Sacar detalle del reglamento espanol hacia un archivo al que ese reglamento nunca enruta habria satisfecho un conteo de palabras perdiendo una obligacion, que es justo lo que el criterio de aceptacion del propio diseno prohibe. El reglamento espanol conserva la taxonomia en linea, mide 581 contra 700, y queda fuera del conjunto medido exactamente como el diseno especifica.

Los dos pares difieren entonces en estructura mientras declaran la misma obligacion: el ingles la delega detras de una fila de compuerta, el espanol la lleva en linea. Es la unica disposicion en que ambos la preservan.

## Hallazgo levantado, no resuelto

Al reglamento espanol le falta al menos la fila de compuerta hacia `working-agreements.md` que el ingles si lleva, y los conteos de lineas de la tabla de compuertas difieren 13 contra 9. Esa brecha es anterior a esta unidad y no es su alcance. Queda para el propietario en vez de arreglarse aqui, porque decidir que filas de compuerta debe llevar el reglamento espanol es una decision de paridad documental, no de presupuesto.

## Verificacion

```
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
  Ran 454 tests in 95.936s
  OK (skipped=9)

python .repo-harness/check.py
  status: current, writes: 0
```

Se agregaron tres aserciones, moviendo la suite de 451 a 454. La asercion existente que compara el conteo documentado con el real fallo primero y se satisfizo en el mismo cambio.

## Criterios de aceptacion cumplidos

- `REPO-HARNESS.md` mide 650 contra 700, y `working-agreements.md` 483 contra 700 despues de recibir ambos bloques.
- No se pierde ninguna obligacion de las reglas obligatorias. Cada bloque movido es alcanzable desde el reglamento ingles por exactamente una fila de compuerta, comprobado y no revisado a ojo. El par en espanol conserva su obligacion en linea.
- La asercion en tiempo de desarrollo falla cuando cualquier superficie medida alcanza el presupuesto y nombra la ruta infractora.
- `render()` y `check()` quedan sin cambios. `RULEBOOK_TOKEN_BUDGET` sigue en 700. No volvio ningun rechazo de render ni estado de checker, comprobado directamente.
- `references/validation.md` ya no afirma que un mecanismo retirado emita `over-budget`.
- El checker informa `current` con cero escrituras y ambas proyecciones de runtime son identicas byte a byte a la canonica.

## No hecho

La plantilla del generador sigue renderizando ambos bloques en linea en cada reglamento generado. Si la compresion cambia tambien esa plantilla sigue siendo la pregunta abierta del propietario, registrada en la hoja de ruta y no tocada aqui. Un reglamento generado mide 650 contra 700, asi que no hay incumplimiento de consumidor por delante.
