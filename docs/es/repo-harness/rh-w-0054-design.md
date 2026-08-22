# Diseno RH-W-0054 de autoridad de correccion sin estado

> **Estado: DISENO APROBADO POR EL PROPIETARIO. La implementacion esta pendiente.**
>
> El propietario aprobo este transporte corregido el 2026-08-21. RH-W-0054 permanece `ready`; este documento no autoriza implementacion ni entrega.

## Contrato de revision

Este diseno se revisa contra el [registro canonico RH-W-0054](../../repo-harness/roadmap.md#rh-w-0054), el flujo actual de preview y apply ligado al digest y la [base RH-W-0050](../../repo-harness/rh-w-0050-design.md). La evidencia del repositorio es canonica. Este par debe permanecer semanticamente equivalente al [diseno en ingles](../../repo-harness/rh-w-0054-design.md).

## Decision

Preview y apply siguen siendo procesos CLI separados. La autoridad cruza ese limite como JSON canonico explicito, nunca como memoria mutable del proceso.

El host entrega el contexto de autoridad completo y actual a preview y apply. Preview expone identificadores de autoridad tipados y seguros, y deriva `authorityContextSha256` y `generationSha256`. Apply recibe ese contexto exacto y el digest aprobado, vuelve a resolver y leer cada vinculo de evidencia, reconstruye salidas y plan, recalcula cada vinculo y rechaza diferencias antes de escribir.

No se agrega almacen persistente, sesion en memoria entre comandos, recibo, ledger, cache, token aleatorio no verificable ni autoridad oculta. La terminacion del comando es el reset natural porque no existe sesion mutable.

## Problema y limite de confianza

Una correccion material puede conservar un plan byte a byte identico. La igualdad prueba estabilidad de salida, no continuidad de autoridad humana. Un contexto material nuevo debe producir una generacion nueva aunque el plan no cambie.

La CLI garantiza vinculacion determinista y rechazo para tipos de evidencia que soporta de forma explicita. No puede descubrir una correccion omitida ni verificar evidencia no soportada, indirecta, externa o incompleta. El host DEBE refrescar y reenviar el contexto despues de cada correccion material. Evidencia no verificable falla de forma cerrada y exige preview nuevo.

Despues de la primera escritura, invalidar autoridad no deshace la mutacion. Los errores conservan el limite transaccional y de remediacion cerrado existente. RH-W-0054 no asigna propietario posterior, no hace rollback silencioso, no corrige automaticamente ni afirma recuperacion.

## Contexto canonico de autoridad

El host DEBE entregar una secuencia exacta de bytes JSON canonicos. Usa claves ordenadas recursivamente, separadores compactos, `ensure_ascii=True`, codificacion UTF-8 y ningun salto final. Los arreglos conservan orden semantico. Los bytes o texto crudos deben igualar exactamente esa representacion. Se rechaza espacio inicial o final, cualquier LF, orden o espaciado alterno, claves duplicadas, raiz distinta de objeto y campos desconocidos o ausentes.

```json
{
  "schemaVersion": 1,
  "materialRevision": "revision opaca emitida por el host",
  "project": {"id": "project-id", "name": "Project name"},
  "builder": {
    "runtimes": ["opencode"],
    "license": "MIT",
    "integrationMode": "integrated",
    "reportLanguage": "en",
    "documentationLanguages": ["en", "es"],
    "conversationLanguage": "es",
    "ignoreCodegraph": true,
    "activation": "builder"
  },
  "premises": [{
    "id": "premise-id",
    "class": "factual_claim",
    "value": "current value",
    "decisionIds": ["decision-id"],
    "evidenceIds": ["repo-rulebook", "engram-input"]
  }],
  "decisions": [{"id": "decision-id", "kind": "human_decision", "statement": "approved bounded correction"}],
  "evidence": [{"id": "repo-rulebook", "kind": "repository_file", "locator": "REPO-HARNESS.md", "sha256": "64 lowercase hexadecimal characters"}, {"id": "engram-input", "kind": "host_json", "source": "engram_evidence_json", "sha256": "64 lowercase hexadecimal characters"}]
}
```

`materialRevision` es opaca, pero no es autoridad aleatoria de la CLI. El host la cambia cuando cambia una premisa, decision, interpretacion o linaje de evidencia material. Asi una correccion produce generacion nueva aunque `planSha256` sea igual. Preview conserva revision mediante identificadores tipados seguros y metadata de vinculo; un digest solo es insuficiente.

El contexto liga todas las entradas del host que afectan salida o autoridad, premisas clasificadas, decisiones humanas tipadas y evidencia tipada. La identidad del plan calculada por la CLI es una entrada separada de la generacion. No contiene secretos ni la ruta absoluta del objetivo.

### Vinculo de evidencia

`repository_file` es el tipo minimo soportado. Su `locator` es relativo al repositorio y se resuelve mediante las reglas protegidas existentes. Preview y apply exigen por separado que sea un archivo regular existente y no indirecto, releen sus bytes y comparan el SHA-256 exacto. Rutas absolutas, traversal, archivos ausentes o cambiados, directorios, symlinks, reparse points, tipos no soportados y objetivos no verificables producen `fresh_preview_required` antes de escribir.

`host_json` liga entradas estructuradas explicitas ya aceptadas por la CLI, incluido `--engram-evidence-json`. La CLI rechaza claves duplicadas, serializa con el mismo JSON canonico y compara el digest de esos bytes. Solo verifica identidad de contenido, no verdad ni procedencia. Entrada ausente, fuente no soportada, JSON invalido o digest distinto falla de forma cerrada.

`human_decision` es autoridad tipada separada, nunca evidencia del repositorio. Premisas referencian IDs unicos y tipados. Se prohiben referencias opacas.

La CLI no lee rutas externas arbitrarias, no persiste evidencia y no copia contenido de evidencia a la salida. Preview muestra solo IDs seguros, tipo, locator relativo cuando sea seguro y digest. Nunca imprime contenido crudo, JSON crudo del host, rutas absolutas ni valores sensibles.

La presentacion en otro idioma de informe por si sola NO DEBE cambiar la identidad ejecutable del plan. Solo entra a la autoridad cuando cambia el dominio de aprobacion u otra interpretacion ligada. La localizacion solo de presentacion queda fuera de `planSha256`.

## Vinculo con el objetivo

```text
targetBindingSha256 = SHA256(UTF8(identidad canonica resuelta del objetivo))
```

La identidad canonica usa la raiz absoluta resuelta, normalizada por plataforma y mayusculas o minusculas, solo dentro del digest. Nunca se imprime ni se inserta en el contexto. Mover o renombrar el objetivo cambia el digest y exige preview nuevo.

## Contrato de digests

```text
authorityContextSha256 = SHA256(bytesCanonicosDelContexto)

generationSha256 = SHA256(bytes JSON canonicos de {
  "domain": "repo-harness-builder/authority-generation/v1",
  "authorityContextSha256": authorityContextSha256,
  "materialRevision": context.materialRevision,
  "planIdentity": {"schemaVersion": 1, "planSha256": currentPlanSha256},
  "targetBindingSha256": targetBindingSha256
})
```

Preview calcula `currentPlanSha256` desde entradas administradas y configuracion actuales. Apply recalcula contexto, cada digest de evidencia soportada, vinculo, salidas, plan y digests. Rechaza antes de `write_outputs()` si difiere cualquier valor. La CLI solo afirma frescura para esos vinculos resueltos y entradas administradas del plan.

## Preview y apply

### Preview builder

1. Resolver objetivo y configuracion CLI actual.
2. Analizar bytes canonicos exactos y validar schema, configuracion, objetivo y evidencia tipada.
3. Releer evidencia soportada, renderizar salidas y calcular el plan.
4. Exigir igualdad de proyecto y configuracion entre contexto y CLI.
5. Ligar el digest recalculado como identidad del plan dentro del payload de generacion.
6. Emitir identificadores seguros y metadata de evidencia, `authorityContextSha256`, `targetBindingSha256`, `planSha256` y `generationSha256`.
7. Obtener fuera de la CLI aprobacion humana de esa generacion exacta.

### Apply builder

1. Exigir contexto exacto y `generationSha256` aprobado.
2. Repetir todos los calculos, resolucion y lectura de evidencia desde estado y configuracion actuales.
3. Exigir igualdad de contexto, configuracion, objetivo, plan y generacion.
4. Rechazar diferencias antes de escribir con `fresh_preview_required`.
5. Solo entonces ejecutar las guardas existentes de digest, bytes, acciones bloqueadas y transaccion.

La activacion builder exige contexto y generacion aprobada. `maintenance` conserva su flujo solo con digest y no exige ni acepta campos builder. Este diseno no crea contexto de correccion para mantenimiento.

## Enrutamiento de fallos

| Evento | Resultado requerido |
|---|---|
| Correccion antes del preview | El host refresca contexto y solicita preview nuevo. |
| Correccion despues del preview y antes del apply | La generacion anterior no sirve; contexto y preview nuevos. |
| Contexto corregido con plan identico | `materialRevision` nueva crea generacion distinta y aprobacion nueva. |
| Evidencia no soportada, ausente, indirecta, externa, cambiada o no verificable | Rechazo `fresh_preview_required`; no se escribe. |
| Cambia o falta JSON ligado del host | Rechazo `fresh_preview_required`; no se escribe. |
| Se mueve o renombra el objetivo | Difiere el vinculo; preview nuevo. |
| Difiere configuracion CLI | Difiere contexto; preview nuevo. |
| Apply falla antes de escribir | No se conserva autoridad ni se necesita reset. |
| Error despues de escribir | Se conserva el limite cerrado existente. |
| Preview identico se repite | Se repite la generacion determinista; no se acumula sesion. |

## Cobertura RED/GREEN requerida

- Invocaciones independientes de `main()` y subprocess genuino sin estado compartido.
- Exito builder con contexto y generacion exactos.
- Rechazo previo a escritura por raiz, objetivo movido, configuracion, contexto, identidad del plan, bytes de evidencia, JSON del host o generacion diferentes.
- Correccion con plan identico que produce generacion distinta y rechaza aprobacion anterior.
- Evidencia obsoleta antes de escribir que produce `fresh_preview_required`.
- Apply fallido, preview repetido, mantenimiento compatible y ausencia de API de reset.
- Identificadores y metadata seguros sin evidencia cruda, JSON crudo del host, ruta absoluta, secreto, token oculto ni autoridad mutable.
- Errores posteriores a escritura conservando el limite cerrado sin afirmar rollback ni recuperacion.

## Impacto de implementacion

| Ruta | Responsabilidad futura |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` | Analisis canonico, validacion, digests, transporte CLI builder, vinculo del objetivo y rechazo previo a escritura, conservando mantenimiento y guardas existentes. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Cobertura unitaria y CLI genuinamente separada. |
| Contratos y uso | Refresco del host, revision de identificadores y metadata seguros, aprobacion nueva y limite de confianza. |
| Proyecciones | Sincronizacion exacta despues de que la fuente canonica este verde. |

## Matriz de evidencia

| Afirmacion | Clase | Propietario |
|---|---|---|
| Preview y apply son procesos separados | Comportamiento actual verificado | Modelo de `main` |
| La memoria del proceso no cruza comandos normales | Bloqueo verificado | Revision fallida y semantica de procesos |
| Se exige transporte canonico sin estado | Decision aprobada | Este diseno |
| Identificadores tipados y metadata segura deben ser revisables | Decision aprobada | Este diseno |
| Una correccion con plan identico exige generacion nueva | Decision aprobada | Este diseno |
| La CLI no descubre una correccion omitida por el host | Restriccion de confianza | Este diseno |
| Mantenimiento conserva autoridad solo por digest | Restriccion aprobada | Este diseno |
| La remediacion posterior no tiene propietario | Decision humana requerida | Diseno futuro |

## Decisiones humanas restantes

Ninguna para implementar este diseno. El nombre y propietario de remediacion posterior quedan fuera de RH-W-0054.
