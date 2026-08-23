# Diseno RH-W-0057: comandos canonicos y cobertura de wrappers

Estado: borrador, pendiente de aprobacion del propietario. Este documento no autoriza ninguna implementacion.

## El defecto, medido

`.repo-harness/check.py` compara proyecciones de habilidades y nada mas. `SKILL_SURFACES` tiene exactamente dos entradas, ambas `skills/{name}/SKILL.md`, y `check_projections()` recorre unicamente `manifest["canonicalSkills"]`. Ninguna ruta de comando o prompt se abre jamas. Un wrapper de comando generado que se borre, se trunque o se edite sigue informando `current` para siempre.

El builder si posee esos archivos. `harness_tool.py` escribe `{base}/commands/{command}.md` para cada entrada de `COMMAND_TO_SKILL` mas un wrapper `repo-harness-help`, y `.pi/prompts/{command}.md` para el entorno Pi. Es decir, el harness genera archivos que ningun control del repositorio verifica.

Esto importa especialmente en un repositorio consumidor. Ahi el builder no esta instalado, como el propio `check_script()` declara, asi que `.repo-harness/check.py` es el unico control local que existe. Un wrapper ausente permanece en silencio hasta que alguien nota que un comando dejo de funcionar.

## Causa raiz

`.repo-harness/` contiene `check.py`, `manifest.json`, `rules/` y `skills/`. No hay `commands/`. Una proyeccion de habilidad se puede comparar porque existe un par canonico contra el cual compararla; un wrapper de comando no tiene par, asi que no hay nada contra que compararlo. La brecha es estructural, no un descuido del bucle del verificador.

## Decision 1: distribucion canonica

Las fuentes canonicas de comandos viven en `.repo-harness/commands/{command}.md`, un archivo por comando generado, junto a `.repo-harness/skills/`.

Se elige esto en vez de registrar un digest de wrapper en el manifest porque un par canonico conserva la forma que el verificador ya tiene: leer canonico, leer proyeccion, comparar. Un campo de digest agregaria un segundo mecanismo de comparacion y haria crecer el manifest cada vez que se agregue un comando. Las exclusiones de la unidad ya rechazan el enfoque de digest por esta razon.

## Decision 2: declaracion en el manifest

El manifest gana `canonicalCommands`, espejo de `canonicalSkills`:

```json
"canonicalCommands": {"session-start": ".repo-harness/commands/session-start.md"}
```

El verificador lo valida igual que valida `canonicalSkills`: un objeto no vacio, cada clave coincidiendo con `[a-z0-9]+(?:-[a-z0-9]+)*`, cada valor pasando `safe_relative()`.

## Decision 3: que superficies se comparan

Verificado: `wrapper(runtime, skill_name)` difiere entre entornos en exactamente una palabra. Emite `skill mechanism` para `claude-code` y `opencode`, y `prompt mechanism` para `pi`. Verificado en disco: los cinco wrappers bajo `.claude/commands/` son identicos byte a byte a sus pares de `.opencode/commands/`. Verificado: `help_wrapper()` acepta un argumento `runtime` que nunca lee, de modo que su salida no varia por entorno en absoluto.

Entonces un unico archivo canonico por comando es identico byte a byte a las proyecciones de Claude Code y OpenCode, y difiere de la proyeccion de Pi por una palabra dentro de una frase.

Las habilidades resuelven su problema equivalente con `comparable()`, que descarta la linea completa `canonical-source:`. Eso funciona porque la diferencia es una linea entera. No se traslada aca: la diferencia de Pi es una palabra dentro de una frase, y normalizarla significaria ensenarle al verificador que palabras tiene permitido cambiar un entorno. Esa regla se pudre la primera vez que se reescriba un wrapper.

Dos opciones honestas, y esta es la unica decision que este diseno deja al propietario:

- **Opcion A, recomendada.** Comparar `.claude/commands/{name}.md` y `.opencode/commands/{name}.md` contra el par canonico. Informar la superficie de Pi como `unchecked` con su razon, de modo que el informe declare su propio limite en vez de insinuar una cobertura que no tiene. Es el cambio mas chico, no mueve ningun byte generado, y quien use Pi ve explicitamente que este control no lo cubre.
- **Opcion B.** Hacer `wrapper()` neutral respecto del entorno para que un unico archivo canonico cubra las tres superficies. Esto elimina la limitacion en vez de documentarla, pero cambia bytes generados para toda instalacion de Pi y pertenece a una unidad aparte con su propio razonamiento de migracion.

## Decision 4: instalaciones anteriores al campo

No hace falta ningun parche de compatibilidad dentro del verificador generado, y agregarlo seria incorrecto.

El verificador autentica el manifest por SHA-256 exacto antes de leer cualquier campo, y despues exige igualdad exacta del conjunto de campos. Un verificador viejo junto a un manifest nuevo ya falla ruidosamente con `manifest digest mismatch`, y el contrato existente reemplaza verificador y manifest juntos en una sola transaccion. Un harness instalado anterior a `canonicalCommands` se actualiza entonces por la actualizacion ordinaria de una sola pasada, que es el mecanismo que `RH-W-0043` ya entrego.

El `check()` propio del builder es distinto y debe tolerar la ausencia. Lee manifests instalados que no necesariamente escribio. Cuando `canonicalCommands` falta, informa la cobertura de comandos como `undeclared` y continua, porque un builder que se cae con una instalacion vieja no puede ser la herramienta que la actualice.

## Estados informados

Se reutiliza el vocabulario existente en vez de inventar uno:

| Estado | Significado |
|---|---|
| `current` | La proyeccion coincide con su par canonico |
| `stale` | La proyeccion existe y difiere |
| `missing` | La proyeccion esta ausente |
| `canonical-missing` | Falta el par canonico, asi que no hay nada que comparar |
| `unchecked` | Superficie deliberadamente no comparada, con su razon (solo en la Opcion A) |

`drift` sigue siendo cualquier estado distinto de `current`, y `unchecked` no cuenta como drift, porque un limite declarado no es un defecto.

## Exclusiones

Sin escrituras desde el verificador, sin modo de reparacion ni regeneracion, sin cambios en la comparacion de proyecciones de habilidades, sin campo de digest de wrapper en el manifest, y sin cambios en `wrapper()` salvo que el propietario elija la Opcion B, que entonces seria su propia unidad.

## Observacion incidental, fuera de alcance

`help_wrapper()` toma un parametro `runtime` que nunca lee. Es un parametro muerto, no un defecto, y queda registrado aca para que quien lea despues no suponga que el wrapper de ayuda varia por entorno. Removerlo no forma parte de esta unidad.

## Que debe fallar antes de implementar

Pruebas enfocadas que fallen primero:

- El verificador generado informa `missing` para un wrapper de comando borrado.
- Informa `stale` para un wrapper editado.
- Informa `canonical-missing` para un par canonico ausente.
- Un manifest que declara `canonicalCommands` con una ruta insegura o absoluta se rechaza antes de cualquier lectura.
- El `check()` propio del builder informa `undeclared` para un manifest instalado sin el campo en vez de lanzar una excepcion.
- Bajo la Opcion A, la superficie de Pi se informa `unchecked` y no vuelve `drift` el estado.
