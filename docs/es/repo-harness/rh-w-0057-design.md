# Diseno RH-W-0057: comandos canonicos y cobertura de wrappers

Estado: borrador, pendiente de aprobacion del propietario. Este documento no autoriza ninguna implementacion. Revisado el 2026-08-23 despues de que el propietario delimitara la cobertura de Pi: Pi se compara siempre que Pi este registrado, lo que elimino la opcion abierta que este diseno traia al principio.

## El defecto, medido

`.repo-harness/check.py` compara proyecciones de habilidades y nada mas. `SKILL_SURFACES` tiene exactamente dos entradas, ambas `skills/{name}/SKILL.md`, y `check_projections()` recorre unicamente `manifest["canonicalSkills"]`. Ninguna ruta de comando o prompt se abre jamas. Un wrapper de comando generado que se borre, se trunque o se edite sigue informando `current` para siempre.

El builder si posee esos archivos. `harness_tool.py` escribe `{base}/commands/{command}.md` para cada entrada de `COMMAND_TO_SKILL` mas un wrapper `repo-harness-help`, y `.pi/prompts/{command}.md` para el entorno Pi. Es decir, el harness genera archivos que ningun control del repositorio verifica.

Esto importa especialmente en un repositorio consumidor. Ahi el builder no esta instalado, como el propio `check_script()` declara, asi que `.repo-harness/check.py` es el unico control local que existe. Un wrapper ausente permanece en silencio hasta que alguien nota que un comando dejo de funcionar.

## Causa raiz

`.repo-harness/` contiene `check.py`, `manifest.json`, `rules/` y `skills/`. No hay `commands/`. Una proyeccion de habilidad se puede comparar porque existe un par canonico contra el cual compararla; un wrapper de comando no tiene par, asi que no hay nada contra que compararlo. La brecha es estructural, no un descuido del bucle del verificador.

## Decision 1: distribucion canonica

Las fuentes canonicas de comandos viven junto a `.repo-harness/skills/`, un archivo por comando generado por cada familia de superficie: `.repo-harness/commands/{command}.md` para los entornos que cargan un comando, y `.repo-harness/prompts/{command}.md` para los entornos que cargan un prompt. La Decision 3 da la medicion que obliga a dos familias en vez de una.

Se elige esto en vez de registrar un digest de wrapper en el manifest porque un par canonico conserva la forma que el verificador ya tiene: leer canonico, leer proyeccion, comparar. Un campo de digest agregaria un segundo mecanismo de comparacion y haria crecer el manifest cada vez que se agregue un comando. Las exclusiones de la unidad ya rechazan el enfoque de digest por esta razon.

## Decision 2: declaracion en el manifest

El manifest gana un unico campo, `canonicalCommands`, indexado por nombre de comando y despues por familia de superficie:

```json
"canonicalCommands": {
  "session-start": {
    "commands": ".repo-harness/commands/session-start.md",
    "prompts": ".repo-harness/prompts/session-start.md"
  }
}
```

Solo aparecen las familias que el render efectivamente produjo, de modo que un repositorio sin Pi no lleva entrada `prompts` y uno que solo use Pi no lleva entrada `commands`.

Un campo en vez de dos es deliberado. El verificador generado exige igualdad exacta del conjunto de campos del manifest, asi que un segundo campo opcional tendria que existir siempre, a veces vacio, o romper esa verificacion. Anidar mantiene el campo siempre presente y siempre no vacio mientras las familias varian.

El verificador lo valida como valida `canonicalSkills`, un nivel mas adentro: un objeto no vacio, cada clave de comando coincidiendo con `[a-z0-9]+(?:-[a-z0-9]+)*`, cada valor un objeto no vacio cuyas claves sean `commands` o `prompts` y cuyos valores pasen `safe_relative()`.

## Decision 3: que superficies se comparan

Se compara todo entorno registrado, Pi incluido. El verificador ya recorre `manifest["runtimes"]`, asi que una superficie se compara exactamente cuando el repositorio declara ese entorno y se omite cuando no. Nada necesita un caso especial de presencia.

Pi es el entorno que mas lo necesita. Verificado despues de agregar Pi a este repositorio el 2026-08-23: el builder no genera ningun `.pi/skills/`, asi que la unica superficie generada de Pi son sus cuatro prompts de comando `.pi/prompts/*.md`. El verificador actual compara seis archivos, todos proyecciones de habilidad de Claude Code y OpenCode. Pi no esta parcialmente verificado hoy: esta completamente sin verificar, y el control que lo cubriria es precisamente el que agrega esta unidad.

El obstaculo es una palabra. Verificado: `wrapper(runtime, skill_name)` emite `skill mechanism` para `claude-code` y `opencode`, y `prompt mechanism` para `pi`. Verificado en disco: los cinco wrappers bajo `.claude/commands/` son identicos byte a byte a sus pares de `.opencode/commands/`, y `.pi/prompts/session-start.md` difiere de `.claude/commands/session-start.md` exactamente en esa palabra. Verificado: `help_wrapper()` acepta un argumento `runtime` que nunca lee, de modo que la superficie de ayuda no varia por entorno en absoluto.

Las habilidades resuelven su problema equivalente con `comparable()`, que descarta la linea completa `canonical-source:`. Eso no se traslada, porque la diferencia de Pi es una palabra dentro de una frase y normalizarla significaria ensenarle al verificador que palabras tiene permitido cambiar un entorno. Esa regla se pudre la primera vez que se reescriba un wrapper.

Entonces la fuente canonica se guarda por familia de superficie, un archivo por comando por familia:

| Canonico | Se compara contra | Se crea cuando |
|---|---|---|
| `.repo-harness/commands/{command}.md` | `.claude/commands/{command}.md`, `.opencode/commands/{command}.md` | `claude-code` u `opencode` esta registrado |
| `.repo-harness/prompts/{command}.md` | `.pi/prompts/{command}.md` | `pi` esta registrado |

Cada archivo canonico es identico byte a byte a las superficies que cubre, asi que la comparacion sigue siendo una comparacion de bytes sin normalizacion ni reglas que conozcan el entorno. Una familia cuyos entornos no estan registrados no produce directorio canonico, igual que `.pi/prompts/` no existe hasta que se selecciona Pi. El costo es un archivo canonico extra por comando solo para repositorios que efectivamente usan Pi, y le compra a Pi la misma cobertura que tienen los otros dos en vez de un punto ciego documentado.

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

`drift` sigue siendo cualquier estado distinto de `current`. No hace falta un estado `unchecked`, porque todo entorno registrado se compara y uno no registrado no produce superficie que comparar.

## Exclusiones

Sin escrituras desde el verificador, sin modo de reparacion ni regeneracion, sin cambios en la comparacion de proyecciones de habilidades, sin campo de digest de wrapper en el manifest, y sin cambios en `wrapper()`. Volver el texto del wrapper neutral respecto del entorno colapsaria las dos familias canonicas en una, pero cambia bytes generados en toda instalacion de Pi y pertenece a su propia unidad con su propio razonamiento de migracion.

## Observacion incidental, fuera de alcance

`help_wrapper()` toma un parametro `runtime` que nunca lee. Es un parametro muerto, no un defecto, y queda registrado aca para que quien lea despues no suponga que el wrapper de ayuda varia por entorno. Removerlo no forma parte de esta unidad.

## Que debe fallar antes de implementar

Pruebas enfocadas que fallen primero:

- El verificador generado informa `missing` para un wrapper de comando borrado.
- Informa `stale` para un wrapper editado.
- Informa `canonical-missing` para un par canonico ausente.
- Un manifest que declara `canonicalCommands` con una ruta insegura o absoluta, una clave de familia desconocida, o un objeto de familia vacio, se rechaza antes de cualquier lectura.
- El `check()` propio del builder informa `undeclared` para un manifest instalado sin el campo en vez de lanzar una excepcion.
- Un `.pi/prompts/{command}.md` borrado se informa `missing` cuando `pi` esta registrado, y no se informa ninguna superficie de Pi cuando no lo esta.
