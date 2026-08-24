# RH-W-0054 Stateless Correction Authority Implementation Plan

> **Completed lifecycle notice:** RH-W-0054 completed all eight tasks and delivery on 2026-08-23. The canonical [roadmap record](roadmap.md#rh-w-0054) owns delivery evidence. The historical task instructions and checkboxes below are retained as execution evidence, not pending work.

**Goal:** Bind builder preview approval across separate CLI processes to one reviewable canonical authority context, current repository evidence, complete output-affecting configuration, exact plan identity, and opaque target identity.

**Architecture:** Add pure canonicalization and digest functions, then integrate them into builder preview and apply through explicit CLI arguments. Apply reconstructs every value from current evidence and configuration before calling existing write guards. No process-global authority, persistence, session object, reset API, receipt, ledger, cache, random token, or hidden state is introduced.

**Tech Stack:** Python 3.10+ standard library, `TypedDict`, `unittest`, `hashlib`, deterministic JSON, independent `main()` calls, subprocess CLI tests, and exact canonical-to-runtime projection copies.

**Lifecycle owner:** [RH-W-0054](roadmap.md#rh-w-0054), status `completed`. The [approved design](rh-w-0054-design.md) owns behavior; this plan preserves the completed implementation sequence. The roadmap owns final verification and delivery evidence.

---

## File Map

| Path | Planned responsibility |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` | Define authority schema, exact canonical parser, typed evidence resolution, target binding, digest computation, builder CLI transport, and pre-write mismatch routing. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Unit, independent-main, and subprocess regressions. |
| `skill/repo-harness-builder/SKILL.md` | Require host context refresh, private host-side content review, safe CLI metadata review, and exact generation approval. |
| `skill/repo-harness-builder/references/audit-and-interview.md` | Define classified premise, decision, and evidence lineage supplied by the host. |
| `skill/repo-harness-builder/references/architecture.md` | Define stateless authority transport and trust boundary. |
| `skill/repo-harness-builder/references/preservation-and-safety.md` | Define target privacy, pre-write rejection, and post-write boundary. |
| `skill/repo-harness-builder/references/validation.md` | Own the authority validation matrix. |
| `docs/repo-harness/usage.md` | Explain review and apply workflow for humans. |
| `.opencode/skills/repo-harness-builder/**` | Exact projections of changed canonical package files. |
| `.claude/skills/repo-harness-builder/**` | Exact projections of changed canonical package files. |

Use one projection strategy only: after canonical tests pass, copy each changed package-relative canonical file byte for byte to both runtime roots with the Python command in Task 7. Never edit a projection independently.

## Exact Internal Contract

Introduce these types before any function that uses them:

```python
AuthorityClass = Literal[
    "factual_claim",
    "assistant_interpretation",
]

EvidenceKind = Literal["repository_file", "host_json"]

class AuthorityProject(TypedDict):
    id: str
    name: str

class AuthorityBuilderConfig(TypedDict):
    runtimes: list[str]
    license: str
    integrationMode: Literal["integrated", "standalone"]
    reportLanguage: str
    documentationLanguages: list[str]
    conversationLanguage: str
    ignoreCodegraph: bool
    activation: Literal["builder"]

AuthorityPremise = TypedDict(
    "AuthorityPremise",
    {
        "id": str,
        "class": AuthorityClass,
        "value": str,
        "decisionIds": list[str],
        "evidenceIds": list[str],
    },
)

class HumanDecision(TypedDict):
    id: str
    kind: Literal["human_decision"]
    statement: str

class RepositoryFileEvidence(TypedDict):
    id: str
    kind: Literal["repository_file"]
    locator: str
    sha256: str

class HostJsonEvidence(TypedDict):
    id: str
    kind: Literal["host_json"]
    source: Literal["engram_evidence_json"]
    sha256: str

class AuthorityContext(TypedDict):
    schemaVersion: Literal[1]
    materialRevision: str
    project: AuthorityProject
    builder: AuthorityBuilderConfig
    premises: list[AuthorityPremise]
    decisions: list[HumanDecision]
    evidence: list[RepositoryFileEvidence | HostJsonEvidence]

class EvidenceBinding(TypedDict, total=False):
    id: str
    kind: EvidenceKind
    locator: str
    source: str
    sha256: str

class AuthorityBinding(TypedDict):
    evidence: list[EvidenceBinding]
    authorityContextSha256: str
    targetBindingSha256: str
    generationSha256: str
```

`class` remains the JSON key. Python validation reads it by dictionary lookup; no constructor uses `AuthorityPremise(class_=...)`.

Add these exact function signatures in this order:

```python
def canonical_json(value: Any) -> str:
def parse_authority_context(raw: str) -> tuple[AuthorityContext, str]:
def validate_authority_context(
    root: Path,
    context: AuthorityContext,
    project_id: str,
    project_name: str,
    expected_config: AuthorityBuilderConfig,
    host_json_inputs: dict[str, str],
) -> list[EvidenceBinding]:
def expected_authority_builder_config(args: argparse.Namespace) -> AuthorityBuilderConfig:
def target_binding_sha256(root: Path) -> str:
def authority_binding(
    root: Path,
    context: AuthorityContext,
    canonical_context: str,
    evidence: list[EvidenceBinding],
    plan_sha256: str,
) -> AuthorityBinding:
def validate_write_preconditions(root: Path, outputs: dict[str, str], plan: dict[str, Any]) -> None:
```

`canonical_json()` returns `json.dumps(value, ensure_ascii=True, allow_nan=False, sort_keys=True, separators=(",", ":"))`. It translates `ValueError` for non-finite values into a content-free `ContractError`. `parse_authority_context()` rejects any `"\n"` or `"\r"`, parses with an `object_pairs_hook` that rejects duplicate keys and a `parse_constant` callback that rejects `NaN`, `Infinity`, and `-Infinity`, validates exact fields and scalar/list types, and requires `raw == canonical_json(parsed)`. The accepted transport and returned canonical text have NO trailing newline. Preview serializes the enclosing report normally but never changes or relaxes the accepted authority bytes.

`target_binding_sha256()` resolves the root strictly, applies `os.path.normcase(os.path.normpath(str(resolved)))`, encodes UTF-8, and hashes those bytes. No caller returns or logs the normalized path.

`authority_binding()` receives only validated context and validated safe evidence metadata. It validates `plan_sha256` as lowercase 64-character hexadecimal, combines context identity with plan identity, and hashes this exact object through `canonical_json()`:

```python
{
    "domain": "repo-harness-builder/authority-generation/v1",
    "authorityContextSha256": authority_context_sha256,
    "materialRevision": context["materialRevision"],
    "planIdentity": {"schemaVersion": 1, "planSha256": plan_sha256},
    "targetBindingSha256": target_sha256,
}
```

`validate_authority_context()` validates schema, exact project id/name, exact builder configuration, target-scoped evidence bindings, unique non-empty IDs, and typed references. For `repository_file`, resolve `locator` only with `guarded_target(root, locator)`, require an existing regular non-indirect file, re-read bytes, and compare exact SHA-256. For `host_json`, accept only `engram_evidence_json`, require the corresponding current CLI input, reject duplicate JSON keys, canonicalize its parsed value with `canonical_json()`, and compare that canonical-content digest. Human decisions remain separate `human_decision` entries. The validator does not receive or compare a plan digest. `authority_binding()` alone combines validated context and evidence identity with `plan_sha256`.

Builder CLI additions:

```text
--project-id PROJECT_ID
--authority-context-file PATH
--approved-generation-sha256 SHA256
```

`--authority-context-file -` reads stdin; any other transport path is read as UTF-8 only because the user explicitly supplied that context transport. It is not an evidence locator and must itself be a direct regular file when a path is used. Preview under `--activation builder` requires project id and context file, rejects approved generation, and emits only stable premise, decision, and evidence IDs; classes or kinds; safe repository-relative locator or host source; declared content digests; `authorityContextSha256`; `targetBindingSha256`; `planSha256`; `generationSha256`; and explicit redaction markers. It never emits complete `authorityContext`, premise values, decision statements, raw repository evidence, raw `--engram-evidence-json`, absolute paths, secrets, or exception text derived from those values. Builder apply additionally requires approved generation. Maintenance rejects all three authority arguments and preserves its current digest-only flow.

## Task 1: Canonical Context Parser And Host JSON Validation

**Files:**
- Modify: `skill/repo-harness-builder/tests/test_harness_tool.py`
- Modify: `skill/repo-harness-builder/scripts/harness_tool.py`

- [x] **Step 1: Add failing canonical parser tests**

Add `AuthorityContextTests` with a `context(revision="revision-1", evidence=None)` helper returning the exact typed schema above and defaulting to host JSON evidence only. Add tests that assert canonical round trip, duplicate-key rejection, unknown/missing field rejection, invalid authority class or evidence kind, duplicate IDs, dangling decision/evidence IDs, and non-canonical JSON rejection. Add exact table-driven rejection for `NaN`, `Infinity`, and `-Infinity` in authority context. Test malformed plan digest through `authority_binding()` in Task 2. Repository-file resolution is added only in Task 3.

The duplicate-key case must call:

```python
TOOL.parse_authority_context('{"schemaVersion":1,"schemaVersion":1}')
```

The canonical case must assert:

```python
parsed, displayed = TOOL.parse_authority_context(TOOL.canonical_json(context))
self.assertEqual(parsed, context)
self.assertEqual(displayed, TOOL.canonical_json(context))
```

Add these exact byte-variant assertions:

```python
canonical = TOOL.canonical_json(context)
variants = {
    "one LF": canonical + "\n",
    "multiple LF": canonical + "\n\n",
    "leading space": " " + canonical,
    "trailing space": canonical + " ",
    "alternate key order": '{"schemaVersion":1,"materialRevision":"revision-1"}',
    "duplicate key": '{"schemaVersion":1,"schemaVersion":1}',
}
for name, raw in variants.items():
    with self.subTest(name=name), self.assertRaises(TOOL.ContractError):
        TOOL.parse_authority_context(raw)
```

For each non-finite token, replace one premise value with the raw token in otherwise canonical object text and require `ContractError` without the token or sentinel content in its message. Do not use `json.dumps()` to construct those raw invalid JSON fixtures because its default permits non-finite values.

- [x] **Step 2: Verify RED**

Run: `python -B skill/repo-harness-builder/tests/test_harness_tool.py AuthorityContextTests -v`

Expected RED: missing `canonical_json` or `parse_authority_context`, not a fixture or syntax error.

- [x] **Step 3: Implement the exact parser and AuthorityContext types**

Add the types and first two functions from the Exact Internal Contract. Implement validation as one depth-first exact-field check with these field sets:

```python
CONTEXT_FIELDS = {"schemaVersion", "materialRevision", "project", "builder", "premises", "decisions", "evidence"}
PROJECT_FIELDS = {"id", "name"}
BUILDER_FIELDS = {"runtimes", "license", "integrationMode", "reportLanguage", "documentationLanguages", "conversationLanguage", "ignoreCodegraph", "activation"}
PREMISE_FIELDS = {"id", "class", "value", "decisionIds", "evidenceIds"}
DECISION_FIELDS = {"id", "kind", "statement"}
REPOSITORY_FILE_EVIDENCE_FIELDS = {"id", "kind", "locator", "sha256"}
HOST_JSON_EVIDENCE_FIELDS = {"id", "kind", "source", "sha256"}
```

Reject booleans where integers are expected. Reject empty IDs, statements, locators, and values after stripping. Require lowercase 64-character SHA-256. Keep semantic array order unchanged. Do not resolve evidence in the parser. Introduce every `AuthorityContext` supporting type and `EvidenceBinding`; do not add the validator function yet.

- [x] **Step 4: Verify parser GREEN**

Run the focused class. Expected: all parser tests pass with no warnings.

- [x] **Step 5: Add failing host JSON validator tests**

Using a host-only context and a directly supplied expected builder configuration, call the exact `validate_authority_context()` API. Assert exact project id/name checks, exact context structural checks, unique typed references, canonical host JSON digest binding, safe returned `EvidenceBinding` metadata, missing current host input rejection, changed host input rejection, duplicate host JSON key rejection, and exact table-driven rejection for `NaN`, `Infinity`, and `-Infinity` in bound host JSON. The RED test invokes the exact wished-for API before its implementation, never an API deferred to another task.

For each non-finite token, supply the raw token through the `host_json` validation path with `--engram-evidence-json` equivalents and require `ContractError` without the token or sentinel content in its message. Do not use `json.dumps()` for these invalid fixtures.

- [x] **Step 6: Verify validator RED**

Run the host JSON validation methods. Expected RED: missing `validate_authority_context()`, not a missing supporting type, fixture, or syntax error.

- [x] **Step 7: Implement minimal host JSON validation**

Add `validate_authority_context()` with its exact signature and implement it for the already-parsed exact schema, exact project id/name, direct expected-config dictionary comparison, unique typed references, and `host_json` evidence. Accept only `engram_evidence_json`, require the corresponding current input, parse with the same duplicate-key and non-finite rejection helpers, canonicalize with `canonical_json()`, compare its lowercase SHA-256, and return only safe typed binding metadata. If repository-file evidence is present, fail with the existing unsupported evidence refusal until Task 3 adds that branch. The validator does not receive or compare a plan digest.

- [x] **Step 8: Verify complete Task 1 GREEN**

Run: `python -B skill/repo-harness-builder/tests/test_harness_tool.py AuthorityContextTests -v`

Expected: canonical parser and host JSON validator methods all pass with no warnings. No later task may call `validate_authority_context()` before this GREEN boundary.

- [ ] **Step 9: Work-unit boundary**

Planned commit: `feat(authority): validate canonical authority context`

Rollback: authority types, parser constants and functions, host JSON validator foundation, and focused tests only. No target digest, generation binding, repository-file resolution, or CLI behavior exists yet.

## Task 2: Deterministic Configuration, Target, And Generation Binding

**Files:** same canonical script and test module.

- [x] **Step 1: Add failing configuration projection tests**

Construct an `argparse.Namespace` containing project, runtimes, license, standalone, report language, documentation languages, conversation language, ignore choice, and activation. Assert `expected_authority_builder_config()` returns all fields exactly. Table-drive one mutation per field through the already-green `validate_authority_context()` and require `ContractError` containing `authority context configuration mismatch`. Add project id and project name mismatch tests.

Add one report-language test with two layers:

```python
self.assertEqual(TOOL.plan_digest(plan_en), TOOL.plan_digest(plan_es))
self.assertNotEqual(config_en["reportLanguage"], config_es["reportLanguage"])
```

This proves presentation alone does not alter executable plan identity while an authority-relevant approval domain remains explicitly bound in context.

- [x] **Step 2: Verify configuration RED**

Run the focused configuration methods. Expected RED: missing `expected_authority_builder_config()`, not missing validator types or behavior.

- [x] **Step 3: Implement deterministic configuration projection**

Add `expected_authority_builder_config()` with its exact signature. Derive `integrationMode` from `args.standalone`, preserve runtime and documentation-language order exactly as normalized by current CLI parsing, and return every declared builder field. Keep the already-green validator API unchanged and compare dictionaries directly.

- [x] **Step 4: Verify configuration GREEN**

Run the focused configuration and mismatch methods. Expected: all pass with no warnings.

- [x] **Step 5: Add failing target and generation binding tests**

Add tests proving:

```python
evidence = TOOL.validate_authority_context(root, context, "example", "Example", config, host_json_inputs)
binding = TOOL.authority_binding(root, context, TOOL.canonical_json(context), evidence, "0" * 64)
self.assertRegex(binding["authorityContextSha256"], r"^[0-9a-f]{64}$")
self.assertRegex(binding["targetBindingSha256"], r"^[0-9a-f]{64}$")
self.assertRegex(binding["generationSha256"], r"^[0-9a-f]{64}$")
self.assertNotIn(str(root.resolve()), json.dumps(binding))
```

Use host-only contexts so this task builds exclusively on Task 1 GREEN behavior. Use two temporary roots with identical files and assert different target and generation digests. Copy the same root to a renamed directory and assert a new target digest. Change only `materialRevision` while keeping the plan digest identical and assert a new generation. Change only the plan digest and assert a new generation. Repeat unchanged input and assert identical bindings. Add malformed plan digest rejection through `authority_binding()`.

- [x] **Step 6: Verify binding RED**

Run the five binding tests. Expected RED: missing binding functions.

- [x] **Step 7: Implement the exact digest functions**

Add `target_binding_sha256()` and `authority_binding()` exactly as specified. Use existing `digest_text()` only for canonical JSON text; use `hashlib.sha256(normalized_path.encode("utf-8")).hexdigest()` for target identity so no path string enters a returned object.

- [x] **Step 8: Verify complete Task 2 GREEN and current plan compatibility**

Run all `AuthorityContextTests` plus:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py RootPointerTests.test_preview_withholds_the_strict_domain_unless_the_builder_is_active RootPointerTests.test_ordinary_maintenance_applies_on_the_digest_alone -v`

Expected: all parser, host validation, configuration, target, generation, malformed plan digest, and compatibility tests pass.

- [x] **Step 9: Work-unit boundary**

Planned commit: `feat(authority): bind generations to context and target`

Rollback: deterministic configuration projection, two pure digest helpers, authority binding, and focused tests. No repository-file resolution or write path is affected.

## Task 3: Resolve Typed Repository Evidence And Complete Validation

**Files:** same canonical script and test module.

- [x] **Step 1: Add failing repository evidence tests**

Extend the Task 1 context helper with one `repository_file` entry and references to it. Assert `validate_authority_context()` returns the exact safe repository binding after reading current bytes. Add missing, directory, symlink or reparse point, traversal, absolute locator, changed bytes, malformed digest, unsupported evidence kind, duplicate evidence ID, and dangling evidence reference cases. Require every failure to remain content-free and to expose no absolute root or evidence bytes. Plan changes remain covered as generation changes in Tasks 2 and 5.

Add a mixed-evidence test that preserves semantic evidence array order while returning complete typed metadata for both kinds:

```python
self.assertEqual(
    TOOL.validate_authority_context(root, context, "example", "Example", config, host_json_inputs),
    [
        {"id": "repo-evidence", "kind": "repository_file", "locator": "evidence.txt", "sha256": repository_sha256},
        {"id": "engram-input", "kind": "host_json", "source": "engram_evidence_json", "sha256": host_sha256},
    ],
)
```

- [x] **Step 2: Verify RED**

Run the focused repository evidence methods. Expected RED: the already-green validator rejects `repository_file` as unsupported, not a missing validator, type, parser, configuration, target, or binding function.

- [x] **Step 3: Extend validation with typed repository resolution**

Extend only the `repository_file` branch of the already-green `validate_authority_context()`. Resolve `locator` only with `guarded_target(root, locator)`, require an existing regular non-indirect file, re-read bytes, compare exact SHA-256, and return safe repository-relative metadata. Preserve the already-green host JSON, project, configuration, ID, and typed-reference behavior unchanged. `validate_authority_context()` validates schema, project, configuration, and evidence bindings only. Target identity remains in `target_binding_sha256()`, and the validator never receives or compares plan identity.

- [x] **Step 4: Verify complete validation GREEN and current plan compatibility**

Run focused tests plus:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py RootPointerTests.test_preview_withholds_the_strict_domain_unless_the_builder_is_active RootPointerTests.test_ordinary_maintenance_applies_on_the_digest_alone -v`

Expected: all pass.

- [x] **Step 5: Work-unit boundary**

Planned commit: `feat(authority): resolve typed repository evidence`

Rollback: repository-file validation branch and focused tests only. Canonical parsing, host JSON validation, deterministic configuration, target binding, and generation binding remain independently green.

## Task 4: Integrate Stateless Builder Preview

**Files:** same canonical script and test module.

- [x] **Step 1: Add independent-main preview tests**

Write a helper that calls `TOOL.main(argv)` with fresh patched stdin/stdout each time and never stores module authority. Build canonical host context, then invoke a builder preview `main()` call using a temporary context file. The CLI computes plan identity during that preview, so no preliminary maintenance preview is required.

Assert output contains exact safe entry metadata, explicit redaction markers, `authorityContextSha256`, `targetBindingSha256`, `planSha256`, and `generationSha256`. Assert it does not contain an `authorityContext` field, premise values, decision statements, host JSON, evidence content, sentinel secrets, the resolved root string, or any field named session, receipt, ledger, cache, token, or reset. Add repeated-preview equality and missing-context rejection.

- [x] **Step 2: Verify RED**

Run `MaterialCorrectionCliTests.test_builder_preview_transports_reviewable_bindings_without_process_state` and its repeated/missing variants. Expected RED: unrecognized authority arguments or absent fields.

- [x] **Step 3: Patch `main()` preview in this exact order**

1. Add parser arguments `--project-id`, `--authority-context-file`, and `--approved-generation-sha256` to `render`.
2. After current argument normalization and after outputs, plan, and `planSha256` are computed, branch on activation.
3. For maintenance, reject any authority argument and leave output unchanged.
4. For builder, require project id and context file; reject approved generation unless `--apply` is present.
5. Read context, parse it, construct expected config and current host JSON inputs, validate context and evidence, compute binding with the separately computed plan digest, and merge only stable entry IDs, classes or kinds, safe locators or sources, content digests, explicit redaction markers, and the four binding digests into preview output.
6. Do not store context or binding globally and do not change `plan_digest()`, `plan_changes()`, or `write_outputs()`.

**Correction applied on 2026-08-22, recorded rather than silently followed.** Step 3 point 4 said to require project id and context file for builder activation. Applied literally that removes the `RH-W-0030` boundary, where `--activation builder` on its own means only the strict approval domain, and it broke four existing tests including the compatibility test this same task's Step 4 requires to pass. The plan contradicted itself. The authority transport is therefore opt-in: supplying `--authority-context-file` activates it and then requires `--project-id`, while builder activation without a context behaves exactly as before. Maintenance still rejects all three arguments.

- [x] **Step 4: Verify GREEN**

Run `MaterialCorrectionCliTests` preview methods and the two compatibility tests from Task 3.

- [x] **Step 5: Work-unit boundary**

Planned commit: `feat(cli): emit stateless builder authority preview`

Rollback: three parser arguments, builder preview branch, and its tests. Maintenance remains unchanged.

## Task 5: Integrate Apply Recompute And Pre-Write Rejection

**Files:** same canonical script and test module.

- [ ] **Step 1: Add genuine separate-process tests**

Use `subprocess.run([sys.executable, "-B", str(ROOT / "scripts" / "harness_tool.py"), ...], text=True, capture_output=True)` for preview and a second subprocess for apply. Pass context by file and parse preview stdout. The success test passes preview `generationSha256`, plan digest, and exact localized approval token.

Add separate tests for cross-root, renamed-root, one mutation of every builder config field, changed canonical context bytes, changed plan identity, old generation after corrected identical-plan context, changed repository evidence bytes that leave generated plan bytes unchanged, missing and indirect evidence, unsupported evidence kind, and changed `--engram-evidence-json`. Add transport tests for missing, unreadable, indirect, invalid UTF-8, syntactically invalid, contract-invalid, and changed context input. Inject or provoke `OSError`, `UnicodeDecodeError`, JSON parse errors, and `ContractError` at their actual load or validation boundaries. Every apply rejection asserts exit 2, `fresh_preview_required` in safe output, unchanged repository snapshots, and absence of raw context and sentinel secrets from both stdout and stderr.

The corrected identical-plan test copies context, changes only `materialRevision`, previews again, asserts equal plan digests and unequal generation digests, then applies new context with old generation and expects rejection.

- [ ] **Step 2: Verify RED**

Run the subprocess test class. Expected RED: apply ignores approved generation or authority arguments.

- [ ] **Step 3: Patch builder apply in this exact order**

1. Require context file, project id, approved generation, approved plan digest, and exact localized approval.
2. Re-read context from the provided direct transport as strict UTF-8.
3. Re-render outputs and recompute plan from current root and normalized CLI arguments using the existing code path.
4. Validate schema, project, configuration, target, and all evidence bindings against current values.
5. Recompute target and generation binding using the separately recomputed plan identity.
6. During builder apply, translate `OSError`, `UnicodeDecodeError`, JSON errors, and `ContractError` from steps 2 through 5 into one content-free `ContractError("fresh_preview_required: authority input is unavailable, invalid, or changed")` before `write_outputs()`. Do not include a path, raw value, nested exception, or exception representation. Initial preview uses a distinct content-free validation refusal because no prior generation exists.
7. Require supplied approved generation equal recomputed `generationSha256`.
8. Extract current `write_outputs()` lines that recompute the plan and validate blocked actions, existing targets, append source bytes, create absence, and replacement source/destination/plan digests into `validate_write_preconditions()` with the exact signature declared above. Call it at the start of `write_outputs()` and leave `apply_additive()` plus migration execution unchanged.
9. In builder apply, call `validate_write_preconditions()` before `write_outputs()`. Translate any `ContractError` from this explicit preflight call to `fresh_preview_required`. Then call `write_outputs()`, whose repeated preflight is intentional defense in depth against a race between validation and mutation.
10. Do not catch errors from `write_outputs()`: once it begins its mutation path, its existing rollback and post-first-write remediation boundary remain authoritative.

- [ ] **Step 4: Verify GREEN**

Run subprocess tests plus existing stale-byte, transaction rollback, exact approval, and maintenance tests named in the old plan review evidence.

- [ ] **Step 5: Work-unit boundary**

Planned commit: `feat(cli): reject stale builder generations before writes`

Rollback: builder apply authority branch and subprocess tests. Existing write transaction remains independently usable by maintenance.

## Task 6: Failure, Reset, Leakage, And Compatibility Regressions

**Files:** same canonical script and test module.

- [ ] **Step 1: Add failing boundary tests**

Add tests that:

- Cause apply validation failure, then run an unrelated valid preview and prove no cleanup or reset call is needed.
- Call `main()` repeatedly and assert the module exposes none of `ACTIVE_PREVIEW_SESSION`, `PreviewAuthoritySession`, `reset_authority`, `save_authority`, or `load_authority`.
- Run ordinary maintenance preview/apply with only the current plan digest and assert success.
- Assert maintenance rejects authority arguments.
- Scan builder preview JSON recursively and assert the absolute root, parent path, context file path, raw repository evidence, raw host JSON, and sentinel secret are absent. Require only evidence ID, kind, safe locator or source, and digest.
- Run success, localized denial, initial preview validation failure, apply preflight failure, and `fresh_preview_required` subprocess cases with distinct sentinel secrets in premise values, decision statements, host JSON, repository evidence, and transport filenames. Assert every sentinel is absent independently from `stdout` and `stderr`.
- Pass host Engram evidence containing a sentinel through current `detect_engram()` and capability preflight. Require internal classification to remain correct while every returned or printed capability projection contains only component name, classification, and safe availability metadata, never the raw `evidence` object or sentinel.
- Force a failure after the first write through the existing transaction fixture and assert current rollback/remediation wording is preserved without a new recovery claim.

- [ ] **Step 2: Verify RED**

Run focused methods. Expected RED: leakage or compatibility assertion fails until CLI integration is complete.

- [ ] **Step 3: Apply only bounded corrections**

If a test exposes content or a path, remove that output field rather than masking the string. Add one focused capability projection helper only if needed to keep `detect_engram()` verification internal while `select_integration_mode()` and CLI output return classification metadata. Do not redesign detection or change authorization semantics. If maintenance accepts authority fields, reject them during argument validation. If failed apply affects later calls, remove the mutable value causing coupling rather than adding cleanup. Do not introduce a reset function.

- [ ] **Step 4: Verify GREEN**

Run `AuthorityContextTests`, `MaterialCorrectionCliTests`, existing maintenance tests, and existing transaction rollback tests.

- [ ] **Step 5: Work-unit boundary**

Planned commit: `test(authority): prove stateless failure and privacy boundaries`

Rollback: boundary tests and only the minimal leak/compatibility corrections they require.

### Complete test fixture and assertion contract

Define these helpers before `MaterialCorrectionCliTests`; later tests MUST use these names and return shapes without inventing undeclared APIs:

```python
def sha256_bytes(value: bytes) -> str:
    return hashlib.sha256(value).hexdigest()

def repository_snapshot(root: Path) -> dict[str, bytes]:
    return {
        str(path.relative_to(root)): path.read_bytes()
        for path in sorted(root.rglob("*"))
        if path.is_file() and not TOOL.is_indirect(path)
    }

def run_cli(*args: str, stdin: str | None = None) -> subprocess.CompletedProcess[str]:
    return subprocess.run(
        [sys.executable, "-B", str(SCRIPT), *args],
        input=stdin,
        text=True,
        capture_output=True,
        check=False,
    )

def assert_fresh_preview(test: unittest.TestCase, result: subprocess.CompletedProcess[str], before: dict[str, bytes], root: Path) -> None:
    test.assertEqual(result.returncode, 2, result.stdout + result.stderr)
    test.assertIn("fresh_preview_required", result.stdout + result.stderr)
    test.assertEqual(repository_snapshot(root), before)
```

The class fixture creates `root`, `other_root`, `context_path`, and `evidence_path = root / "evidence.txt"`; writes `b"evidence-v1"`; creates canonical `engram_json = canonical_json({"observations":[{"id":1,"fact":"current"}]})`; and builds context with one `repository_file` digest and one `host_json` digest. Reuse the existing subprocess capability fixture so integrated Gentle AI and Engram detection is deterministic; do not mock `main()` inside subprocess tests.

`builder_args(root, context_path, engram_json)` returns this complete list in this order:

```python
[
    "render", "--root", str(root),
    "--project-name", "Example",
    "--project-id", "example",
    "--runtime", "opencode",
    "--license", "MIT",
    "--report-language", "english",
    "--documentation-language", "en",
    "--documentation-language", "es",
    "--conversation-language", "espanol",
    "--activation", "builder",
    "--authority-context-file", str(context_path),
    "--engram-evidence-json", engram_json,
]
```

The fixture context builder block exactly matches these normalized values: `project={"id":"example","name":"Example"}`, runtime `opencode`, license `MIT`, integrated mode, report `english`, documentation `en,es`, conversation `espanol`, `ignoreCodegraph=True`, and builder activation. `preview` writes exact canonical context bytes with `Path.write_bytes(canonical.encode("utf-8"))`, invokes `run_cli(*builder_args(...))`, asserts exit 0, and returns parsed stdout JSON. `apply` rewrites the exact current context bytes, then invokes the same arguments plus `--apply`, `--approved-plan-sha256`, the preview plan digest, `--approved-generation-sha256`, the supplied generation, `--approval`, and `APPROVE`; it returns the raw completed process. Optional keyword mutations replace one named argument without changing the others.

Implement these exact test bodies or semantic equivalents with the same complete assertions:

```python
def test_subprocess_rejects_generation_from_another_root(self):
    first = self.preview(self.root, self.context, self.engram_json)
    before = repository_snapshot(self.other_root)
    result = self.apply(self.other_root, self.context, self.engram_json, first["generationSha256"])
    assert_fresh_preview(self, result, before, self.other_root)

def test_evidence_byte_change_rejects_even_when_plan_is_unchanged(self):
    first = self.preview(self.root, self.context, self.engram_json)
    plan_sha = first["planSha256"]
    self.evidence_path.write_bytes(b"evidence-v2")
    before = repository_snapshot(self.root)
    self.assertEqual(self.maintenance_preview(self.root)["planSha256"], plan_sha)
    result = self.apply(self.root, self.context, self.engram_json, first["generationSha256"])
    assert_fresh_preview(self, result, before, self.root)

def test_cross_configuration_rejects_before_write(self):
    first = self.preview(self.root, self.context, self.engram_json)
    before = repository_snapshot(self.root)
    result = self.apply(self.root, self.context, self.engram_json, first["generationSha256"], runtimes=["claude"])
    assert_fresh_preview(self, result, before, self.root)

def test_failed_apply_does_not_affect_later_preview(self):
    first = self.preview(self.root, self.context, self.engram_json)
    failed = self.apply(self.root, self.context, self.engram_json, "f" * 64)
    self.assertEqual(failed.returncode, 2)
    self.assertEqual(self.preview(self.root, self.context, self.engram_json), first)

def test_repeated_preview_is_deterministic(self):
    self.assertEqual(
        self.preview(self.root, self.context, self.engram_json),
        self.preview(self.root, self.context, self.engram_json),
    )

def test_ordinary_maintenance_remains_digest_only(self):
    preview = self.maintenance_preview(self.root)
    result = self.maintenance_apply(self.root, preview["planSha256"])
    self.assertEqual(result.returncode, 0, result.stdout + result.stderr)

def test_preview_leaks_no_authority_content(self):
    preview = self.preview(self.root, self.context, self.engram_json)
    rendered = json.dumps(preview, sort_keys=True)
    for forbidden in (str(self.root.resolve()), str(self.context_path), "evidence-v1", self.engram_json, "current", "premise-secret", "decision-secret"):
        self.assertNotIn(forbidden, rendered)
    self.assertNotIn("authorityContext", preview)
    self.assertEqual(
        preview["evidence"],
        [
            {"id":"engram-input","kind":"host_json","source":"engram_evidence_json","sha256":self.context["evidence"][1]["sha256"]},
            {"id":"repo-evidence","kind":"repository_file","locator":"evidence.txt","sha256":self.context["evidence"][0]["sha256"]},
        ],
    )

def test_corrected_revision_requires_new_approval_for_identical_plan(self):
    first = self.preview(self.root, self.context, self.engram_json)
    corrected = copy.deepcopy(self.context)
    corrected["materialRevision"] = "revision-2"
    second = self.preview(self.root, corrected, self.engram_json)
    self.assertEqual(first["planSha256"], second["planSha256"])
    self.assertNotEqual(first["generationSha256"], second["generationSha256"])
    before = repository_snapshot(self.root)
    result = self.apply(self.root, corrected, self.engram_json, first["generationSha256"])
    assert_fresh_preview(self, result, before, self.root)
```

For canonical variants, use the exact table in Task 1. For `--engram-evidence-json`, change only `fact` from `current` to `corrected`, keep generated plan bytes unchanged, and assert `assert_fresh_preview`. For missing, directory, symlink or reparse point, traversal, absolute locator, unsupported kind, malformed host JSON, and duplicate host JSON keys, snapshot first and assert the same fail-closed result.

Add one shared `assert_secret_absent(test, result, *sentinels)` helper that checks `result.stdout` and `result.stderr` separately. Use it in success, denial, validation failure, preflight failure, and every `assert_fresh_preview` path. The denial fixture must fail exact localized approval after valid authority loading; the initial validation fixture must fail before a generation exists; the preflight fixture must fail after valid apply arguments but before `write_outputs()`; and the fresh-preview fixture must use changed or unavailable apply transport. This separation proves all five routes rather than passing several labels through one failure.

For the post-write boundary, reuse the existing transaction fixture that fails the second mutation. Patch only `apply_additive` at its established call boundary, assert it was entered, assert the existing rollback result byte for the first target, and assert output does not contain `fresh_preview_required`, `recovered`, or `automatic correction`. This proves authority preflight ended before mutation and later failures remain owned by the existing transaction boundary.

## Task 7: Update Product Contracts And Synchronize Projections

**Files:** all canonical contract and projection paths in the File Map.

- [ ] **Step 1: Add failing contract tests**

In `ProductWorkflowTests`, read the five canonical package documents and require these phrases across their combined text: `canonical authority context`, `typed evidence binding`, `authorityContextSha256`, `generationSha256`, `targetBindingSha256`, `fresh preview`, `host must refresh`, `separate CLI processes`, and `maintenance`. Assert the safety document contains `do not print the absolute target path`, `do not print raw evidence`, `do not print premise values or decision statements`, `capability classification metadata only`, and excludes claims that the CLI discovers omitted conversational corrections or verifies unsupported evidence.

- [ ] **Step 2: Verify RED**

Run the two focused contract tests. Expected RED: required stateless transport language is absent.

- [ ] **Step 3: Update canonical documentation**

Use `SKILL.md` for the executable host-to-preview-to-approval-to-apply sequence; `audit-and-interview.md` for classified premises, separate human decisions, typed evidence bindings, and mandatory refresh after correction; `architecture.md` for exact no-newline canonical transport and digest formulas; `preservation-and-safety.md` for evidence and target privacy plus the first-write boundary; `validation.md` for all test scenarios; and `usage.md` for concise human review instructions. Remove opaque evidence-reference and in-memory generation/session language superseded by this design.

- [ ] **Step 4: Verify canonical contracts, then copy projections**

Run focused contract tests. After GREEN, run this portable Python command:

```powershell
python -c "from pathlib import Path; import shutil; paths=['scripts/harness_tool.py','tests/test_harness_tool.py','SKILL.md','references/audit-and-interview.md','references/architecture.md','references/preservation-and-safety.md','references/validation.md']; src=Path('skill/repo-harness-builder'); roots=[Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; [(dst.parent.mkdir(parents=True,exist_ok=True),shutil.copyfile(src/p,dst)) for p in paths for dst in [r/p for r in roots]]"
```

Then verify byte identity:

```powershell
python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py','SKILL.md','references/audit-and-interview.md','references/architecture.md','references/preservation-and-safety.md','references/validation.md']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad, bad"
```

- [ ] **Step 5: Work-unit boundary**

Planned commit: `docs(authority): document stateless correction transport`

Rollback: canonical contract prose, focused contract tests, usage paragraph, and exact corresponding projections.

## Task 8: Verification And Conditional Delivery Reconciliation

**Files:** verify all paths changed by Tasks 1 through 7. Lifecycle and changelog edits remain conditional on separately authorized completed delivery.

- [ ] **Step 1: Run focused authority coverage**

Run: `python -B skill/repo-harness-builder/tests/test_harness_tool.py AuthorityContextTests MaterialCorrectionCliTests -v`

Expected: all canonical authority scenarios pass.

- [ ] **Step 2: Run canonical and projection suites**

Run these exact commands from the repository root:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v
```

The verified baseline is 341 tests in each package: canonical has 8 skips, while OpenCode and Claude each have 20 because repository-only fixtures are intentionally skipped. This plan requires at least 24 and at most 49 new methods across the named classes, so each command MUST report the same total in the executable range 365 through 390, with skip counts exactly `[8, 20, 20]` in canonical, OpenCode, and Claude order. Verify equality, range, skips, and exit status portably with:

```powershell
python -c "import pathlib,re,subprocess,sys; roots=[pathlib.Path('skill/repo-harness-builder'),pathlib.Path('.opencode/skills/repo-harness-builder'),pathlib.Path('.claude/skills/repo-harness-builder')]; results=[subprocess.run([sys.executable,'-B','-m','unittest','discover','-s',str(r/'tests'),'-p','test_*.py','-v'],text=True,capture_output=True) for r in roots]; assert all(p.returncode==0 for p in results),[(p.returncode,p.stdout,p.stderr) for p in results]; texts=[p.stdout+p.stderr for p in results]; totals=[int(re.search(r'Ran (\d+) tests?',t).group(1)) for t in texts]; skips=[int(re.search(r'skipped=(\d+)',t).group(1)) for t in texts]; assert len(set(totals))==1 and 365<=totals[0]<=390,totals; assert skips==[8,20,20],skips"
```

- [ ] **Step 3: Run checker and whitespace validation**

Run `python .repo-harness/check.py` and `git diff --check`. Expected: checker `current`, 0 writes, and no whitespace errors.

- [ ] **Step 4: Run portable static scans**

```powershell
python -c "from pathlib import Path; p=Path('docs/repo-harness/rh-w-0054-implementation-plan.md'); text=p.read_text(encoding='utf-8'); terms=['T'+'ODO','T'+'BD','implement'+' later','fill in'+' details','similar to'+' Task']; found=[t for t in terms if t.lower() in text.lower()]; assert not found, found"
python -c "from pathlib import Path; roots=[Path('docs/repo-harness/rh-w-0054-implementation-plan.md'),Path('docs/repo-harness/rh-w-0054-design.md'),Path('docs/es/repo-harness/rh-w-0054-design.md'),Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; files=[p for r in roots for p in ([r] if r.is_file() else r.rglob('*')) if p.is_file()]; bad=[str(p) for p in files if any(c in p.read_text(encoding='utf-8',errors='ignore') for c in ('\u2013','\u2014'))]; assert not bad, bad"
```

Task 6 asserts at runtime that the superseded session APIs are not defined.

- [ ] **Step 5: Inspect scope**

Run `git status --short`, `git diff --stat`, and `git diff -- docs/repo-harness/roadmap.md docs/es/repo-harness/roadmap.md docs/repo-harness/current-state.md docs/es/repo-harness/current-state.md CHANGELOG.md`.

Historical pre-delivery expectation: RH-W-0054 remained `active` until all eight tasks and delivery evidence completed. This statement records the gate that applied during execution and is not current lifecycle status.

- [ ] **Step 6: Conditional completion reconciliation**

This conditional reconciliation was reached after separate implementation authorizations, observed RED evidence, GREEN checks, reviewed projection synchronization, and delivery evidence. The roadmap records the completed lifecycle; this historical plan does not independently own or advance status.

- [ ] **Step 7: Work-unit boundary**

Planned commit after authorized verification: `test(authority): verify stateless generation boundaries`

Rollback: verification-only refinements and later completion documentation. Post-first-write correction never authorizes rollback.

## Review Workload Forecast

| Review slice | Tasks | Estimated authored change | Review question |
|---|---|---:|---|
| 1. Canonical authority primitives | 1 through 3 | 260 to 340 lines | Does each green layer progress from canonical host validation, through deterministic configuration and generation binding, to guarded repository evidence without leaking paths? |
| 2. Separate-process CLI enforcement | 4 through 6 | 300 to 420 lines | Does apply recompute and reject every mismatch before writes while maintenance stays compatible? |
| 3. Contracts, projections, verification | 7 through 8 | 220 to 320 authored lines plus exact projection copies | Do product contracts match behavior and are all package copies byte-identical? |

Task count: 8. Estimated authored workload: 780 to 1,080 changed lines. Exact OpenCode and Claude projection copies add approximately 1,000 to 1,600 generated changed lines and are reviewed as generated mirrors, not independent design.

Use three chained review slices because authored work exceeds the 400-line target. Each work unit keeps tests with behavior, records exact RED/GREEN evidence, names a runtime boundary, and has an independent rollback boundary. No commit is authorized by this plan revision.

## Acceptance Checklist

- [ ] Preview and apply work in separate processes with explicit context transport.
- [ ] Safe typed identifiers and binding metadata are reviewable without raw evidence or host JSON.
- [ ] Context, target, configuration, plan identity, material revision, repository-file bytes, and canonical host JSON are bound.
- [ ] No absolute target path is printed.
- [ ] Corrected identical plans require a fresh generation and approval.
- [ ] Unsupported, missing, indirect, external, changed, or unverifiable evidence routes to `fresh_preview_required` before first write.
- [ ] Canonical transport rejects every newline, surrounding whitespace, alternate ordering or spacing, and duplicate keys.
- [ ] Canonical serialization uses `allow_nan=False` and rejects `NaN`, `Infinity`, and `-Infinity` in authority context and host JSON.
- [ ] Complete authority context, premise values, decision statements, host JSON, evidence content, secrets, and absolute paths never appear in stdout or stderr.
- [ ] Success, denial, validation failure, preflight failure, and `fresh_preview_required` each prove sentinel-secret absence independently.
- [ ] Missing, unreadable, indirect, invalid UTF-8, malformed, contract-invalid, or changed apply transport routes to `fresh_preview_required` before writes; malformed initial preview remains a distinct safe validation refusal.
- [ ] Capability preflight returns classification metadata only and never raw host evidence.
- [ ] Existing post-first-write fail-closed behavior remains intact.
- [ ] Maintenance retains digest-only compatibility.
- [ ] No persistent or in-memory authority state, token, receipt, ledger, cache, or reset API exists.
- [ ] Genuine subprocess tests and independent `main()` tests pass.
- [ ] Portable Python replaces every repository-content `rg` verification.
- [ ] Canonical package files and both runtime projections are byte-identical.
- [x] RH-W-0054 stayed `active` until separately authorized implementation and delivery evidence supported completion; Task 4 completion did not authorize Tasks 5 through 8.

## Remaining Human Decisions

None remain for completed RH-W-0054. Naming and ownership of a future post-first-write remediation workflow remain a separate design decision outside this unit.

## Handoff

**Terminal status:** All eight tasks and delivery completed on 2026-08-23. Continue from the canonical [roadmap](roadmap.md#rh-w-0054), not from this historical handoff.

## Second-Correction Review Dispositions

| Prior finding or warning | Disposition | Closing evidence in this plan |
|---|---|---|
| Opaque evidence references did not bind evidence content | integrate | Typed `repository_file` and `host_json` schemas, guarded resolution, canonical content digests, and preview/apply re-read contract. |
| Repository evidence could change while generated plan bytes stayed identical | integrate | Complete evidence-byte test snapshots the root, proves unchanged plan digest, changes evidence bytes, and requires `fresh_preview_required` with no write. |
| Explicit `--engram-evidence-json` was not bound | integrate | `host_json` accepts only the named current CLI source, rejects duplicate keys, canonicalizes content, and verifies SHA-256 on preview and apply. |
| Human decisions were conflated with evidence | integrate | Separate `HumanDecision` type and `decisionIds`; evidence kinds cannot encode human authority. |
| Evidence could be external, indirect, missing, unsupported, or leaked | integrate | Guarded repository-relative regular-file resolution, fail-closed matrix, safe metadata-only preview, and exact leakage assertions. |
| Canonical parsing accepted one or multiple trailing LF bytes | integrate | Exact `raw == canonical_json(parsed)`, `ensure_ascii=True`, no trailing newline, and explicit one-LF, multiple-LF, whitespace, ordering, spacing, and duplicate-key tests. |
| Verification named discovery suites without exact commands or totals | integrate | Three exact unittest discovery commands plus executable portable equality check against baseline-derived range 365 through 390 and 8 skips. |
| Validator wording implied plan comparison without a plan parameter | integrate | `validate_authority_context()` owns schema, configuration, target, and evidence; `authority_binding()` alone receives and binds plan identity. |
| Subprocess mismatch and boundary tests were descriptive rather than executable | integrate | Complete helpers and test bodies cover cross-root, configuration, evidence-byte, failed apply, repeated preview, maintenance, leakage, corrected identical plan, and pre/post-write boundaries. |
| APIs or types appeared after use | integrate | Exact Internal Contract defines every type and signature before task bodies; validation precedes binding in declaration and execution order. |
| CLI freshness claim exceeded what it could verify | integrate | Design and summaries limit freshness to explicitly resolved repository files, canonical host JSON, and managed plan inputs. |
| Stateless architecture could regress into persistence or shared process state | close | No store, session, reset, receipt, ledger, cache, or hidden token is added; genuine subprocess tests remain mandatory. |
| Complete authority context or sensitive host-owned values could leak through preview or errors | integrate | Metadata-only output contract plus exact sentinel-secret absence tests for success, denial, validation failure, preflight failure, and `fresh_preview_required`. |
| Canonical JSON could accept non-finite values | integrate | `allow_nan=False`, rejecting `parse_constant`, and exact `NaN`, `Infinity`, and `-Infinity` tests for authority context and host JSON. |
| Context transport failures did not consistently route stale apply to a fresh preview | integrate | Distinct initial-preview validation refusal and apply translation of `OSError`, `UnicodeDecodeError`, JSON errors, and `ContractError` to content-free `fresh_preview_required` before writes. |
| Capability preflight could return raw Engram host evidence | integrate | Current code inspection confirms `detect_engram()` and `select_integration_mode()` expose raw evidence; Task 6 adds the smallest classification-only projection and sentinel regression without redesigning detection. |
