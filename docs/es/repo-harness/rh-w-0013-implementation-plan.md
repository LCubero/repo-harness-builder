# Plan minimo de implementacion RH-W-0013 de presupuesto de contexto

> **Para agentes ejecutores:** SUB-SKILL REQUERIDA: usar `test-driven-development` para cambios de comportamiento y `verification-before-completion` antes de entregar. Este plan no autoriza implementacion ni cambio de ciclo de vida.

**Objetivo:** Retirar dos bloqueos de presupuesto en ejecucion y conservar los presupuestos de plantilla durante desarrollo.

**Arquitectura:** Eliminar el rechazo de presupuesto en `render_with_skill_mapping()` y la rama de deriva `over-budget` en `check()`. No agregar feature de presupuesto en ejecucion, schema, parser, propagacion, contenido generado ni propietario nuevo.

**Stack:** Biblioteca estandar de Python 3.10+, `unittest` y proyecciones exactas del paquete canonico.

**Propietario de ciclo de vida:** [RH-W-0013](../../repo-harness/roadmap.md#rh-w-0013), estado `ready`. El [diseno](../../repo-harness/rh-w-0013-design.md) posee el alcance.

Solo `REPO-HARNESS.md` se carga siempre; reglas y skills diarias son condicionales. La guia solo documental permanece en 180-450 objetivo, 451-700 aceptable, 701-1000 warning, 1001-1200 atencion prioritaria y consejo de revision inmediata por encima de 1200. Estos tramos no agregan comportamiento en ejecucion.

## Mapa de archivos

| Ruta | Responsabilidad planificada |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` | Retirar los dos efectos bloqueantes; retirar opcionalmente `budgeted()` si queda sin uso. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Regresiones RED/GREEN exactas y preservacion de controles de plantilla. |
| `.opencode/skills/repo-harness-builder/scripts/harness_tool.py` | Proyeccion exacta del script despues de GREEN canonico. |
| `.opencode/skills/repo-harness-builder/tests/test_harness_tool.py` | Proyeccion exacta de pruebas despues de GREEN canonico. |
| `.claude/skills/repo-harness-builder/scripts/harness_tool.py` | Proyeccion exacta del script despues de GREEN canonico. |
| `.claude/skills/repo-harness-builder/tests/test_harness_tool.py` | Proyeccion exacta de pruebas despues de GREEN canonico. |
| Roadmap, current-state, changelog y pares localizados | Reconciliacion condicional solo despues de implementacion autorizada y evidencia de entrega. |

No cambiar manifest, checker generado, comando, prompt, wrapper, regla, skill diaria, referencia ni guia de estilo vendida.

## Tarea 1: Probar y retirar ambos efectos bloqueantes

**Archivos:**
- Modificar: `skill/repo-harness-builder/tests/test_harness_tool.py`
- Modificar: `skill/repo-harness-builder/scripts/harness_tool.py:2364-2447,3418-3516`

- [ ] **Paso 1: Agregar las regresiones de render en 700 y 701 exactos**

Agregar esta prueba enfocada usando el import `mock` que ya existe en el modulo:

```python
class ContextBudgetBlockingTests(unittest.TestCase):
    def test_exact_budget_rulebook_renders(self):
        original = TOOL.rulebook("example", False)
        exact = next(
            original + "\n" + ("filler " * count)
            for count in range(TOOL.RULEBOOK_TOKEN_BUDGET)
            if TOOL.token_proxy(original + "\n" + ("filler " * count)) == TOOL.RULEBOOK_TOKEN_BUDGET
        )
        self.assertEqual(TOOL.token_proxy(exact), TOOL.RULEBOOK_TOKEN_BUDGET)
        with mock.patch.object(TOOL, "rulebook", return_value=exact):
            outputs = TOOL.render("Example", [])
        self.assertEqual(outputs[TOOL.RULEBOOK_PATH], exact)

    def test_exact_701_rulebook_renders(self):
        original = TOOL.rulebook("example", False)
        exact = next(
            original + "\n" + ("filler " * count)
            for count in range(TOOL.RULEBOOK_TOKEN_BUDGET)
            if TOOL.token_proxy(original + "\n" + ("filler " * count)) == TOOL.RULEBOOK_TOKEN_BUDGET
        )
        exact_701 = exact + "\nfiller"
        self.assertEqual(TOOL.token_proxy(exact_701), 701)
        with mock.patch.object(TOOL, "rulebook", return_value=exact_701):
            outputs = TOOL.render("Example", [])
        self.assertEqual(outputs[TOOL.RULEBOOK_PATH], exact_701)
```

Los dos fixtures de limite exacto deben equivaler a 700 y 701. Ambos RED actuales deben ser el `ContractError` de presupuesto de `render_with_skill_mapping()` que contiene `over its 700 budget`, no fallos al preparar fixtures.

- [ ] **Paso 2: Observar RED de render**

Ejecutar:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py ContextBudgetBlockingTests.test_exact_budget_rulebook_renders -v`

Esperado: un fallo o error en el `require()` de presupuesto, no en la preparacion del fixture.

- [ ] **Paso 3: Observar por separado RED de render en 701 exactos**

Ejecutar:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py ContextBudgetBlockingTests.test_exact_701_rulebook_renders -v`

Esperado: un error en el mismo `require()` de presupuesto, con `ContractError` que contiene `over its 700 budget`; antes pasa la asercion de 701 exactos.

- [ ] **Paso 4: Reemplazar la regresion existente del checker por el contrato previsto**

Renombrar `RuleFileTests.test_check_reports_a_budgeted_file_that_outgrew_its_budget` a `test_check_keeps_large_human_evolved_rule_current`. Conservar la instalacion temporal y el append grande, y afirmar:

```python
self.assertEqual(entry["state"], "human-evolved")
self.assertNotIn("tokens", entry)
self.assertEqual(report["status"], "current")
self.assertEqual(report["writes"], 0)
```

- [ ] **Paso 5: Observar RED del checker**

Ejecutar:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_check_keeps_large_human_evolved_rule_current -v`

Esperado: falla porque el codigo actual devuelve `over-budget` y `drift`.

- [ ] **Paso 6: Hacer el cambio minimo de produccion**

Eliminar solamente:

```python
for path, content in outputs.items():
    if budgeted(path, rulebook_path):
        size = token_proxy(content)
        require(size < RULEBOOK_TOKEN_BUDGET, f"{path} is {size} tokens, over its {RULEBOOK_TOKEN_BUDGET} budget")
```

y esta rama de `check()`:

```python
elif budgeted(relative, rulebook_path) and size >= RULEBOOK_TOKEN_BUDGET:
    entry["state"] = "over-budget"
    entry["tokens"] = size
```

Retirar tambien la asignacion sin uso `size = token_proxy(current_text)`. Buscar callers despues de estas eliminaciones; retirar `budgeted()` solo si no queda ninguno. No retirar `token_proxy()` ni `RULEBOOK_TOKEN_BUDGET`.

- [ ] **Paso 7: Verificar GREEN enfocado y controles preservados**

Ejecutar:

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py ContextBudgetBlockingTests RuleFileTests.test_check_keeps_large_human_evolved_rule_current ProductWorkflowTests.test_rulebook_stays_within_its_token_budget RuleFileTests.test_rule_files_stay_within_the_same_budget_as_the_rulebook -v
```

Esperado: todo pasa. Las ultimas dos pruebas siguen aplicando los presupuestos de desarrollo del reglamento y los archivos de reglas.

- [ ] **Paso 8: Verificar por separado GREEN en 701 exactos**

Ejecutar:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py ContextBudgetBlockingTests.test_exact_701_rulebook_renders -v`

Esperado: pasa una prueba; la salida de 701 exactos se renderiza sin cambios.

- [ ] **Paso 9: Limite de unidad**

Commit planificado despues de autorizacion separada: `fix(budget): make runtime checks non-blocking`

Rollback solo de las pruebas enfocadas y las dos eliminaciones de produccion. No alterar RH-W-0067 ni superficies generadas en ejecucion.

## Tarea 2: Sincronizar proyecciones y verificar alcance

**Archivos:** las seis rutas canonicas y proyectadas de script/pruebas del mapa; archivos de ciclo de vida solo bajo el Paso 6.

- [ ] **Paso 1: Ejecutar GREEN canonico antes de copiar proyecciones**

Ejecutar:

`python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`

Esperado: salida 0. Detenerse si falla la suite canonica.

- [ ] **Paso 2: Copiar solo script y pruebas**

```powershell
python -c "from pathlib import Path; import shutil; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; src=Path('skill/repo-harness-builder'); roots=[Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; [(d.parent.mkdir(parents=True,exist_ok=True),shutil.copyfile(src/p,d)) for p in paths for d in [r/p for r in roots]]"
```

- [ ] **Paso 3: Probar identidad byte a byte**

```powershell
python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"
```

- [ ] **Paso 4: Ejecutar las tres suites y el checker generado**

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v
python .repo-harness/check.py
```

Esperado: todas las suites salen 0 con igual total; se permiten diferencias existentes de skips por runtime. El checker informa `current` con `writes: 0`.

- [ ] **Paso 5: Inspeccionar alcance exacto**

Ejecutar `git status --short`, `git diff --stat`, `git diff --name-only`, `git diff --check` e inspeccionar el diff completo. Rechazar cualquier schema en ejecucion, parser, propagacion por preview/apply, referencia generada, refactor de skill diaria, manifest, checker, comando, prompt, wrapper o cambio a la guia vendida.

- [ ] **Paso 6: Reconciliar ciclo de vida solo despues de evidencia de entrega**

Solo despues de implementacion autorizada, RED observado, GREEN completo, proyecciones sincronizadas y evidencia de entrega revisada: actualizar ambos roadmaps, ambas proyecciones de current-state, `CHANGELOG.md` y el topic Engram `work/RH-W-0013` en una unidad documental. Conservar `ready` hasta que esa evidencia y autorizacion respalden la siguiente transicion de ciclo de vida.

## Lista de aceptacion

- [ ] Render de 700 exactos ya no falla.
- [ ] Render de 701 exactos ya no falla.
- [ ] Archivos humanos presupuestados grandes conservan `human-evolved` y estado general `current`.
- [ ] El drift real conserva su comportamiento.
- [ ] Permanecen `token_proxy()`, `RULEBOOK_TOKEN_BUDGET` y las pruebas de presupuesto de plantilla.
- [ ] `budgeted()` se retira solo si la busqueda de callers demuestra que no tiene uso.
- [ ] Antes de reconciliar ciclo de vida solo cambian script/pruebas canonicos y sus proyecciones OpenCode/Claude.
- [ ] Cada artefacto excluido permanece ausente.
- [ ] La guia de estilo vendida permanece sin cambios.

## Decisiones humanas restantes

No queda ninguna decision de producto. La ejecucion y entrega todavia requieren autorizacion separada.
