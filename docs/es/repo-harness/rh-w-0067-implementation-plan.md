# Plan de implementacion RH-W-0067 del contrato de respuesta

> **Para trabajadores agenticos:** SUB-SKILLS OBLIGATORIAS: usar `test-driven-development` para cada cambio de comportamiento y `verification-before-completion` antes de entregar. Ejecutar las casillas en orden. Este plan no autoriza implementacion, transicion de ciclo de vida, commit, push, publicacion ni acceso externo.

**Objetivo:** Generar un unico contrato condicional de respuesta, enrutarle los seis disparadores y reducir los tres contratos diarios a una referencia compartida exacta sin cambiar limites de autoridad existentes.

**Arquitectura:** `rule_outputs()` pasa a ser el unico renderizador de `.repo-harness/rules/response-contract.md`; `rulebook()` recibe una fila exacta y seis compresiones exactas; los tres renderizadores diarios interpolan una referencia concisa compartida. El inventario renderizado, `human_owned()`, `byte_compared_paths()`, check, preview y apply existentes manejan el archivo nuevo sin schema ni cambio de manifiesto.

**Stack tecnico:** Python 3.10+ y biblioteca estandar, `unittest`, Markdown determinista y proyecciones canonica, OpenCode y Claude byte a byte identicas.

**Propietario del ciclo de vida:** [RH-W-0067](roadmap.md#rh-w-0067), estado `completed`. El [diseno aprobado](rh-w-0067-design.md) posee el comportamiento exacto. RH-W-0013 completada satisfizo la dependencia. Este plan se ejecuto sin concurrencia RH-W-0060 y se entrego mediante el issue #104, el commit de implementacion `7ef4993`, el PR #105 y el commit de merge `5ebaf941d81e9b512584d13b2a08dd800a179824`.

## Resultado de implementacion

Completado el 2026-08-24. Los fallos RED requeridos se observaron antes de editar produccion, GREEN enfocado paso 34 pruebas, canon/OpenCode/Claude pasaron 441 pruebas cada uno, maintenance apply ligado por digest devolvio `VALIDATED` y el checker generado informo `current` con cero escrituras. El issue #104, el commit de implementacion `7ef4993`, el PR #105 y el commit de merge `5ebaf941d81e9b512584d13b2a08dd800a179824` aportan evidencia de entrega. Los comandos y casillas siguientes conservan la secuencia historica aprobada.

## Mapa de archivos

| Ruta o simbolo | Responsabilidad planeada |
|---|---|
| `skill/repo-harness-builder/tests/test_harness_tool.py:4191 RuleFileTests` | Propiedad RED/GREEN, ruta, presupuesto, delimitadores, setext, contexto, migracion y atributos. |
| `skill/repo-harness-builder/tests/test_harness_tool.py:6249 QuestionVisibilityTests` | Sustituir aserciones de sintaxis diaria por referencia exacta y ausencia de duplicacion. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1117 VISIBILITY_CONTRACT` | Renombrar a una fuente compartida con la referencia diaria exacta. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1148 session_start_skill` | Interpolar la referencia una vez en Output Contract. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1280 session_stop_skill` | Interpolar la referencia una vez en Output Contract. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1391 documentation_skill` | Interpolar la referencia una vez en Output Contract. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1465 rulebook` | Aplicar seis compresiones y la fila exacta de seis disparadores. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1540 rule_outputs` | Renderizar el unico propietario completo con procedencia y presupuesto existentes. |
| `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` | Proyecciones identicas despues de GREEN canonico. |
| `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` | Proyecciones identicas despues de GREEN canonico. |
| `REPO-HARNESS.md` | Reglamento autoalojado con compresiones equivalentes y gate. |
| `.repo-harness/rules/response-contract.md` | Semilla autoalojada nueva y unico propietario de sintaxis. |
| `.repo-harness/skills/repo-harness-{session-start,session-stop,documentation}/SKILL.md` | Salidas autoalojadas con solo la referencia exacta. |
| Roadmap, estado actual, changelog, pares localizados y tema Engram `work/RH-W-0067` | Reconciliacion solo tras evidencia y autoridad separada. |

No cambiar campos del manifiesto, formato del checker, entrada `.gitattributes` para la regla humana, version, schema, parser, comandos, prompts, wrappers, copia de regla por runtime ni dependencias.

## Tarea 1: RED del propietario de regla y gate exacto

**Archivo:** `skill/repo-harness-builder/tests/test_harness_tool.py:4191-4245`.

- [ ] **Paso 1: Extender semillas y agregar pruebas enfocadas**

Agregar `.repo-harness/rules/response-contract.md` a `RuleFileTests.SEEDS`. Agregar a `RuleFileTests` exactamente el Bloque A de este plan; no sustituir cadenas ni traducir artefactos.

- [ ] **Paso 2: Observar RED por propietario ausente**

Ejecutar:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_rules_directory_is_seeded RuleFileTests.test_response_contract_has_one_exact_six_trigger_gate -v`

Esperado: dos fallos. El primero informa que `.repo-harness/rules/response-contract.md` no esta en outputs. El segundo informa conteo exacto `0 != 1`. Un error de sintaxis o importacion no vale como RED.

- [ ] **Paso 3: Observar RED independientes**

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_response_contract_owns_literal_32_byte_forms_and_setext_safety -v
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_response_contract_keeps_context_proportional_without_five_headings -v
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_rulebook_compression_preserves_obligations_and_budget -v
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_rulebook_contains_no_response_syntax_beyond_the_gate -v
```

Esperado: los dos primeros terminan con `KeyError: '.repo-harness/rules/response-contract.md'`; compresion falla en el primer reemplazo ausente; separacion pasa como control negativo.

## Tarea 2: RED de referencias diarias y comportamiento de ciclo existente

**Archivo:** `skill/repo-harness-builder/tests/test_harness_tool.py:4191-4299,6249-6332`.

- [ ] **Paso 1: Sustituir las pruebas diarias RH-W-0058**

Conservar `DAILY_SKILLS` y `rendered_daily()`. Sustituir los metodos indicados por el Bloque B de este plan, que prueba los bytes ingleses exactos del artefacto.

- [ ] **Paso 2: Agregar pruebas de instalacion, propiedad y proyeccion**

Agregar a `RuleFileTests` los dos metodos completos del Bloque C de este plan. No agregar campo `responseContract` ni otro campo al manifiesto.

- [ ] **Paso 3: Observar RED diario y de ciclo**

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py QuestionVisibilityTests -v
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_response_rule_is_missing_then_human_evolved_without_schema_changes RuleFileTests.test_response_rule_is_not_byte_pinned_or_runtime_duplicated -v
```

Esperado: cuatro metodos diarios fallan por referencia ausente, gate ausente, legado prohibido `a line of \`=\`` y lista de propietarios `[]`. La prueba de ciclo produce `KeyError` por regla ausente. La prueba negativa de runtime y atributos pasa.

## Tarea 3: GREEN minimo en el renderizador canonico

**Archivo:** `skill/repo-harness-builder/scripts/harness_tool.py:1117-1590`.

- [ ] **Paso 1: Sustituir la constante diaria compartida**

Reemplazar `VISIBILITY_CONTRACT` por el Bloque D de este plan. Cambiar solo las tres interpolaciones finales a `{RESPONSE_CONTRACT_REFERENCE}`.

- [ ] **Paso 2: Aplicar seis sustituciones exactas y el gate**

Aplicar literalmente estas seis salidas, sin traducirlas:

```text
Apply the smallest sufficient change.
Do not introduce unrelated scope.
Do not retry indefinitely. After repeated failure, stop with evidence and request a decision.
1. Update when a rule or gate changes.
2. Keep never-miss rules here; move detail to `.repo-harness/rules/` with at most one gate row.
3. Stay under {RULEBOOK_TOKEN_BUDGET} tokens, measured as `wc -w` times 1.33. Growth moves down; the budget never rises.
```

Insertar exactamente:

```markdown
| An answer requires a notice, pending action, question, approval, visible next action, or contextual explanation | `.repo-harness/rules/response-contract.md` |
```

- [ ] **Paso 3: Agregar el unico output de regla**

Copiar literalmente el Bloque E de este plan. Es la especificacion completa del artefacto: procedencia, seis disparadores, visibilidad, ejemplos, advertencia setext, contexto, limites y presupuesto.

- [ ] **Paso 4: Ejecutar GREEN enfocado**

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests QuestionVisibilityTests GeneratedCoreTests.test_rulebook_stays_within_its_token_budget GeneratedCoreTests.test_rulebook_owns_proportional_verification_principle_for_all_core_skills GeneratedCoreTests.test_rulebook_follow_through_preserves_pause_and_scope_boundaries RepositoryLineEndingContractTests -v
```

Resultado registrado: el comando reproducible corregido selecciona 34 pruebas y todas pasan. Si la regla excede el presupuesto, acortar solo prosa redundante conservando aserciones y ejemplos. Nunca subir `RULEBOOK_TOKEN_BUDGET`.

- [ ] **Paso 5: Limite de unidad**

Commit planeado solo tras autorizacion: `feat(harness): generate response contract`. Stage conjunto de script y pruebas canonicos. No incluir aun proyecciones, salidas autoalojadas, ciclo de vida ni este plan.

Rollback: restaurar constante e interpolaciones, seis cadenas previas, quitar gate y output, y restaurar pruebas. No existe limpieza de schema, manifiesto o estado.

## Tarea 4: Sincronizar proyecciones y autoalojamiento

**Archivos:** cuatro proyecciones y cinco salidas del mapa.

- [ ] **Paso 1: Probar suite canonica antes de copiar**

`python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`

Esperado: exit 0. Detenerse ante cualquier fallo.

- [ ] **Paso 2: Copiar solo script y pruebas**

```powershell
python -c "from pathlib import Path; import shutil; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; src=Path('skill/repo-harness-builder'); roots=[Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; [(d.parent.mkdir(parents=True,exist_ok=True),shutil.copyfile(src/p,d)) for p in paths for d in [r/p for r in roots]]"
python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"
```

Esperado: exit 0 sin salida.

- [ ] **Paso 3: Aplicar compresion y gate equivalentes al reglamento humano autoalojado**

Editar `REPO-HARNESS.md` directamente con las mismas seis sustituciones de la Tarea 3, Paso 2, conservando la redaccion especifica de este repositorio. Agregar la fila exacta despues del gate de memoria. Esta edicion directa es necesaria porque el reglamento es propiedad humana despues de sembrarse y maintenance apply preserva sus bytes. No copiar el Purpose generico ni retirar gates del repositorio.

- [ ] **Paso 4: Preview autoalojado de mantenimiento**

```powershell
$engramEvidence = '{"surface":"mcp","status":"verified","evidence":"Engram MCP capability verified for this implementation session"}'
$preview = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root . --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language espanol --engram-evidence-json $engramEvidence | ConvertFrom-Json
$preview | ConvertTo-Json -Depth 20
```

Esperado: exit 0 y solo acciones previstas, incluida creacion de la regla y reemplazo de tres skills canonicas y sus proyecciones. `REPO-HARNESS.md` permanece humano y no se reemplaza. No proponer campo de manifiesto, copia runtime de regla ni atributo exacto. Detenerse si preflight no verifica capacidades o `blocked` no esta vacio.

- [ ] **Paso 5: Aplicar una vez el preview exacto**

```powershell
$applied = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root . --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language espanol --engram-evidence-json $engramEvidence --apply --approved-plan-sha256 $preview.planSha256 | ConvertFrom-Json
$applied | ConvertTo-Json -Depth 20
```

Esperado: exit 0 y `status` igual a `VALIDATED`. No usar `--approval`, activacion builder ni authority context.

- [ ] **Paso 6: Verificar propiedad y bytes**

Ejecutar el Bloque F de este plan. Esperado: exit 0 sin salida, dos lineas `=` de 32 bytes, dos `+`, procedencia, sin atributo exacto y una referencia exacta por skill.

- [ ] **Paso 7: Limite de unidad**

Commit planeado solo tras autorizacion: `chore(harness): synchronize response contract projections`. Stage solo proyecciones y salidas cambiadas por apply revisado. Si se prefiere un solo work unit, mantener pruebas con comportamiento.

Pronostico: 180 a 260 lineas canonicas; las copias elevan el diff mecanico a unas 540 a 780. Revisar primero canon bajo 400 lineas conceptuales y luego sincronizacion mecanica. No crear PRs encadenados salvo que el diff medido no sea revisable como un PR enfocado.

## Tarea 5: Verificacion completa, ciclo de vida y entrega

- [ ] **Paso 1: Ejecutar las tres suites**

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v
```

Esperado: exit 0 y totales iguales. Diferencias existentes de skips por runtime pueden permanecer.

- [ ] **Paso 2: Checker, self-test, paridad y alcance**

Ejecutar el Bloque G de este plan. Esperado: checker `current` con `writes: 0`, self-test `pass`, paridad y `git diff --check` exitosos, y solo rutas previstas sin trabajo RH-W-0060.

- [ ] **Paso 3: Revision documental y contenido prohibido**

Ejecutar el Bloque H de este plan. Esperado: sin los dos caracteres Unicode prohibidos, terminos de marcador ni enlaces rotos. Releer cada archivo cambiado e inspeccionar el diff completo.

- [ ] **Paso 4: Reconciliar ciclo solo con evidencia y autoridad separada**

Tras RED observado, GREEN completo, apply revisado, verificacion y autorizacion explicita:

- Actualizar `docs/repo-harness/roadmap.md` y su par espanol a completed con evidencia exacta.
- Actualizar ambos current-state sin iniciar la siguiente unidad.
- Agregar una entrada factual a `CHANGELOG.md`.
- Reconciliar el tema Engram `work/RH-W-0067` despues del propietario del repositorio.

No cambiar estado solo por GREEN local. No iniciar RH-W-0060.

- [ ] **Paso 5: Limite de issue, commit, PR y publicacion**

Resultado historico de entrega: se uso el issue aprobado #104, el commit de implementacion `7ef4993` se entrego mediante el PR #105 y el commit de merge `5ebaf941d81e9b512584d13b2a08dd800a179824` completo la entrega. Los detalles anteriores de comandos y staging permanecen como guia historica de ejecucion, no como trabajo pendiente.

Rollback antes de entrega: revertir solo pruebas y renderer RH-W-0067, copiar canon revertido a ambas proyecciones, usar preview/apply de mantenimiento para restaurar salidas y repetir verificaciones. Preservar toda regla instalada evolucionada por humanos; nunca borrarla automaticamente. Despues de entrega, usar un commit revert normal revisado, no reescribir historia.

## Bloques exactos de ejecucion

### Bloque A: pruebas del propietario y gate

```python
    RESPONSE_RULE = ".repo-harness/rules/response-contract.md"
    GATE = (
        "| An answer requires a notice, pending action, question, approval, visible next action, "
        "or contextual explanation | `.repo-harness/rules/response-contract.md` |"
    )

    def test_response_contract_has_one_exact_six_trigger_gate(self):
        outputs = TOOL.render("Example", [])
        self.assertEqual(outputs[RULEBOOK].count(self.GATE), 1)
        self.assertIn("Load when: an answer requires a notice, pending action, question, approval, visible next action, or contextual explanation.", outputs[self.RESPONSE_RULE])

    def test_response_contract_owns_literal_32_byte_forms_and_setext_safety(self):
        text = TOOL.render("Example", [])[self.RESPONSE_RULE]
        equals, pluses = "=" * 32, "+" * 32
        self.assertEqual(text.splitlines().count(equals), 2)
        self.assertEqual(text.splitlines().count(pluses), 2)
        self.assertIn(f"```text\n{equals}\nDecision required", text)
        self.assertIn(f"```text\n{pluses}\nNext action", text)
        self.assertIn("setext heading", text)
        self.assertIn("blank line", text)

    def test_response_contract_keeps_context_proportional_without_five_headings(self):
        text = TOOL.render("Example", [])[self.RESPONSE_RULE]
        for dimension in ("why", "purpose", "location", "timing", "method"):
            self.assertIn(dimension, text)
        for phrase in ("only the relevant dimensions", "never requires five headings", "evidence classification", "authority", "no-unrequested-work", "authorized-follow-through"):
            self.assertIn(phrase, text)

    def test_rulebook_compression_preserves_obligations_and_budget(self):
        text = TOOL.render("A Fairly Long Project Name Here", ["opencode", "claude-code", "pi"])[RULEBOOK]
        for replacement in ("Apply the smallest sufficient change.", "Do not introduce unrelated scope.", "After repeated failure, stop with evidence and request a decision.", "1. Update when a rule or gate changes.", "2. Keep never-miss rules here; move detail to", "3. Stay under 700 tokens"):
            self.assertIn(replacement, text)
        for superseded in (
            "Apply the smallest change sufficient for the current objective.",
            "Do not introduce unrelated tools, processes, abstractions, or scope.",
            "After a repeated failure, stop, present evidence, and request a decision.",
            "1. **When.** Update this file when a rule, gate destination, or statement changes.",
            "2. **Where.** Keep never-miss rules here. Move detail to",
            "3. **Budget.** Stay under",
        ):
            self.assertNotIn(superseded, text)
        self.assertLess(TOOL.token_proxy(text), TOOL.RULEBOOK_TOKEN_BUDGET)

    def test_rulebook_contains_no_response_syntax_beyond_the_gate(self):
        text = TOOL.render("Example", [])[RULEBOOK]
        self.assertEqual(text.count(self.GATE), 1)
        for syntax in ("Decision required", "Next action", "setext heading", "=" * 32, "+" * 32):
            self.assertNotIn(syntax, text)
```

### Bloque B: pruebas diarias

```python
    REFERENCE = (
        "Response shaping: for any notice, pending action, question, approval, visible next action, or "
        "contextual explanation, apply `.repo-harness/rules/response-contract.md` to the agent's own "
        "reply and reported output."
    )

    def test_every_daily_contract_contains_the_exact_reference_once(self):
        for path, text in self.rendered_daily().items():
            with self.subTest(path=path):
                self.assertEqual(text.split("## Output Contract", 1)[1].count(self.REFERENCE), 1)

    def test_notice_and_pending_action_are_independent_gate_triggers(self):
        gate = TOOL.render("Example", ["opencode"])["REPO-HARNESS.md"]
        self.assertIn("notice, pending action, question", gate)
        self.assertIn("pending action", self.REFERENCE)
        self.assertIn("visible next action", self.REFERENCE)

    def test_daily_contracts_do_not_duplicate_response_syntax(self):
        forbidden = ("=" * 32, "+" * 32, "setext heading", "why, purpose, location, timing, and method", "a line of `=`", "a line of `+`")
        for path, text in self.rendered_daily().items():
            with self.subTest(path=path):
                for phrase in forbidden:
                    self.assertNotIn(phrase, text)

    def test_response_syntax_has_one_generated_owner(self):
        outputs = TOOL.render("Example", ["opencode"])
        self.assertEqual([path for path, text in outputs.items() if "Decision required" in text], [".repo-harness/rules/response-contract.md"])
```

### Bloque C: ciclo, manifiesto y runtime

```python
    def test_response_rule_is_missing_then_human_evolved_without_schema_changes(self):
        outputs = TOOL.render("Example", ["opencode"])
        self.assertNotIn("responseContract", json.loads(outputs[TOOL.MANIFEST_PATH]))
        expected_response_rule = outputs[self.RESPONSE_RULE]
        with tempfile.TemporaryDirectory() as directory:
            root = Path(directory)
            install(root, runtimes=("opencode",))
            target = root / self.RESPONSE_RULE
            target.unlink()
            missing = TOOL.check(root, "Example", ["opencode"])
            self.assertEqual(next(item for item in missing["files"] if item["path"] == self.RESPONSE_RULE)["state"], "missing")
            target.write_text(expected_response_rule + "\nHuman note.\n", encoding="utf-8")
            evolved = TOOL.check(root, "Example", ["opencode"])
            self.assertEqual(next(item for item in evolved["files"] if item["path"] == self.RESPONSE_RULE)["state"], "human-evolved")
            self.assertEqual(evolved["status"], "current")
            self.assertEqual(evolved["writes"], 0)

    def test_response_rule_is_not_byte_pinned_or_runtime_duplicated(self):
        outputs = TOOL.render("Example", ["claude-code", "opencode", "pi"])
        self.assertNotIn(f"{self.RESPONSE_RULE} text eol=lf", outputs[".gitattributes"])
        for prefix in (".claude/", ".opencode/", ".pi/"):
            self.assertNotIn(prefix + self.RESPONSE_RULE, outputs)
```

### Bloque D: referencia compartida

```python
RESPONSE_CONTRACT_REFERENCE = (
    "Response shaping: for any notice, pending action, question, approval, visible next action, or "
    "contextual explanation, apply `.repo-harness/rules/response-contract.md` to the agent's own "
    "reply and reported output."
)
```

### Bloque E: regla generada

````python
    response_contract = f"""# Response Contract

{marker}

Load when: an answer requires a notice, pending action, question, approval, visible next action, or contextual explanation.

## Visibility

- A notice, pending action, or question that must survive skimming gets a separate block with a short label in the reply language.
- A blocked reply that cannot continue until the human answers uses exactly 32 ASCII `=` bytes above and below the label, necessary explanation, and single final question.
- A reply that asks nothing and names a visible next action uses exactly 32 ASCII `+` bytes above and below the label and action.
- Never put both forms in one reply. Never use `=` for informational content or `+` to ask a question. Never use side borders or box corners.
- When nothing is pending and no visible next action is useful, close with a plain status line and one concrete recommendation, never an empty block.
- This binds the agent's own reply and reported output and strengthens, rather than replaces, the single-final-question rule.

```text
================================
Decision required

<Only the context needed to answer.>

<One concrete final question?>
================================
```

```text
++++++++++++++++++++++++++++++++
Next action

<One concrete next action.>
++++++++++++++++++++++++++++++++
```

A line of `=` directly below prose can become a Markdown setext heading and disappear as a delimiter. Keep a blank line before the opening `=` delimiter and keep both delimiter lines standalone.

## Contextual Explanation

When context helps the human understand or act, explain only the relevant dimensions among why, purpose, location, timing, and method. This semantic rule never requires five headings, a fixed template, empty dimensions, repeated information, or explanation when the answer is already self-explanatory.

## Preserved Boundaries

Use the existing owners for proportionality, evidence classification, authority, no-unrequested-work, and authorized-follow-through. This file does not expand those boundaries.

## Budget

Stay under {RULEBOOK_TOKEN_BUDGET} tokens, measured as `wc -w` times 1.33. A file that outgrows it splits or sheds detail; it never raises the number.
"""
    return {
        ".repo-harness/rules/memory.md": memory,
        ".repo-harness/rules/response-contract.md": response_contract,
        ".repo-harness/rules/working-agreements.md": agreements,
    }
````

### Bloque F: bytes autoalojados

```powershell
python -c "from pathlib import Path; p=Path('.repo-harness/rules/response-contract.md'); t=p.read_text(encoding='utf-8'); assert t.count('='*32)==2; assert t.count('+'*32)==2; assert 'source: core/rules:3.0' in t; assert '.repo-harness/rules/response-contract.md text eol=lf' not in Path('.gitattributes').read_text(encoding='utf-8')"
python -c "from pathlib import Path; ref=\"Response shaping: for any notice, pending action, question, approval, visible next action, or contextual explanation, apply `.repo-harness/rules/response-contract.md` to the agent's own reply and reported output.\"; paths=['.repo-harness/skills/repo-harness-session-start/SKILL.md','.repo-harness/skills/repo-harness-session-stop/SKILL.md','.repo-harness/skills/repo-harness-documentation/SKILL.md']; assert all(Path(p).read_text(encoding='utf-8').count(ref)==1 for p in paths)"
```

### Bloque G: verificacion de alcance

```powershell
python .repo-harness/check.py
python -B skill/repo-harness-builder/scripts/harness_tool.py self-test
python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"
git diff --check
git status --short
git diff --stat
git diff --name-only
```

### Bloque H: integridad documental

```powershell
python -c "from pathlib import Path; paths=[Path(p) for p in ['docs/repo-harness/rh-w-0067-design.md','docs/es/repo-harness/rh-w-0067-design.md','docs/repo-harness/rh-w-0067-implementation-plan.md','docs/es/repo-harness/rh-w-0067-implementation-plan.md']]; bad=[str(p) for p in paths if chr(0x2013) in p.read_text(encoding='utf-8') or chr(0x2014) in p.read_text(encoding='utf-8')]; assert not bad,bad"
python -c "from pathlib import Path; paths=[Path('docs/repo-harness/rh-w-0067-implementation-plan.md'),Path('docs/es/repo-harness/rh-w-0067-implementation-plan.md')]; banned=('T'+'BD','T'+'ODO','implement '+'later','fill in '+'details','Similar to '+'Task'); bad=[(str(p),term) for p in paths for term in banned if term in p.read_text(encoding='utf-8')]; assert not bad,bad"
python -c "from pathlib import Path; import re; paths=[Path('docs/repo-harness/rh-w-0067-implementation-plan.md'),Path('docs/es/repo-harness/rh-w-0067-implementation-plan.md')]; missing=[]; [(missing.append((str(p),link)) if not (p.parent/link).resolve().exists() else None) for p in paths for link in re.findall(r'\[[^]]+\]\(([^)#]+\.md)(?:#[^)]+)?\)',p.read_text(encoding='utf-8')) if '://' not in link]; assert not missing,missing"
```

## Lista de aceptacion

- [ ] Una regla generada es el unico propietario de sintaxis completa.
- [ ] Gate y referencias nombran los seis disparadores exactos.
- [ ] Ejemplos cercados tienen dos lineas de 32 bytes por forma y linea en blanco segura para setext.
- [ ] Contexto usa solo dimensiones relevantes y rechaza cinco encabezados obligatorios.
- [ ] Seis compresiones preservan proporcionalidad, evidencia, autoridad, trabajo no pedido, seguimiento autorizado y presupuesto.
- [ ] Skills diarias contienen una referencia exacta y ninguna sintaxis duplicada.
- [ ] Instalacion nueva, faltante, regla humana, mantenimiento autorizado y check sin escrituras usan mecanismos existentes.
- [ ] Manifiesto y checker no cambian de formato ni reciben campos nuevos.
- [ ] La regla no tiene copia runtime ni atributo exacto.
- [ ] Script y pruebas son byte a byte identicos en las tres copias.
- [ ] RH-W-0013 sigue completed y RH-W-0060 no corrio en paralelo.
- [ ] Sin RDD, schema, parser, analisis IA, dependencia runtime, taxonomia, reescritura automatica, refactor ajeno ni aumento de presupuesto.
- [ ] Ciclo y entrega respetan autoridad separada.

## Decisiones humanas restantes

No queda decision de producto. La implementacion, el cierre de ciclo y la entrega mediante el issue #104 y el PR #105 estan completos.
