# Diseno RH-W-0069 de backup local de memoria y continuidad entre fuentes

> **Estado: DISENO APROBADO POR EL PROPIETARIO. La implementacion no esta autorizada por separado.**

## Contrato de revision

Revise este diseno contra el [registro RH-W-0069](roadmap.md#rh-w-0069), la [taxonomia RH-W-0051](rh-w-0051-design.md), la reconciliacion completada [RH-W-0050](rh-w-0050-design.md), las [reglas de memoria](../../../.repo-harness/rules/memory.md) y el [propietario en ingles](../../repo-harness/rh-w-0069-design.md). La evidencia del repositorio sigue siendo autoritativa.

## Decision

Cada pausa o cierre deliberado escribe un backup Markdown estructurado en `.repo-harness/memory-bk/`. Cuando Engram esta disponible, session-stop tambien escribe Engram. Las dos escrituras son independientes y visibles. El exito parcial es `degraded`; ninguna operacion fabrica el exito de la otra.

Session-start lee primero la evidencia actual del repositorio, despues Engram cuando esta disponible y luego el backup local mas nuevo. Comprueba Engram y el backup entre si y contra los propietarios actuales. Los conflictos se informan y nunca se fusionan en silencio. Backups anteriores se leen solo cuando el objetivo, un conflicto o continuidad faltante los vuelve relevantes.

La disponibilidad de Gentle AI es independiente de memoria y queda fuera de la mecanica. La memoria incorporada de Claude, OpenCode o Pi no es autoridad ni dependencia de continuidad.

## Schema minimo

Nombre:

```text
YYYYMMDDTHHMMSSZ-<source>-<short-session-id>.md
```

El timestamp es UTC. `source` es `pause` o `close`. El ID corto usa solo letras ASCII minusculas, digitos y guiones. La creacion es exclusiva; una colision falla visiblemente y nunca sobrescribe.

Forma obligatoria:

```markdown
---
project: <id estable del proyecto>
timestamp_utc: <timestamp UTC RFC 3339>
source: pause | close
session_id: <id acotado no secreto>
---

# Session Continuity

## Goal

## Instructions

## Discoveries

## Accomplished

## Next Steps

## Relevant Files
```

Cada afirmacion que no sea claramente una instruccion humana o intencion futura lleva una clasificacion en linea: `verified fact`, `supported inference`, `unknown or unverifiable` o `human decision required`. Una inferencia nombra evidencia y que la elevaria a verificada. Las rutas son relativas al repositorio. Las secciones vacias usan `None.` y no placeholders.

El backup contiene continuidad concisa, no fuente copiada, logs, diffs, salida completa de pruebas, recibos, tablas de ciclo, aprobaciones, estado de conflictos ni documentacion duplicada.

## Orden de autoridad

Cada afirmacion recordada se verifica contra su propietario actual:

1. Fuente y pruebas poseen comportamiento.
2. `roadmap.md` posee ciclo y orden.
3. `current-state.md` posee la proyeccion concisa.
4. `decisions.md` posee razones permanentes.
5. `CHANGELOG.md` posee cronologia completada.
6. Git y checker actuales poseen entrega y congruencia.
7. Engram y backups son evidencia de continuidad no autoritativa.
8. La memoria incorporada se ignora como autoridad y nunca es obligatoria.

Cuando difieren propietarios, gana el mas especifico y aplicable. Si el repositorio no resuelve una decision material, session-start hace una sola pregunta concreta.

## Orden de lectura al iniciar

1. Resolver y validar la raiz seleccionada.
2. Leer `REPO-HARNESS.md`, documentacion aplicable, estado actual, registros exactos, decisiones relevantes y ultimas entradas pertinentes del changelog.
3. Inspeccionar fuente, pruebas, Git y checker requeridos.
4. Resolver proyecto Engram explicito y leer contexto relevante cuando exista, con resultados por operacion.
5. Ubicar y leer el Markdown regular valido mas nuevo dentro de `.repo-harness/memory-bk/`; el orden por nombre solo vale si la metadata coincide.
6. Leer uno anterior solo si el mas nuevo es invalido, el objetivo lo nombra, un conflicto requiere cronologia o falta continuidad relevante.
7. Comprobar cada afirmacion Engram y local contra el repositorio y despues entre fuentes.
8. Informar continuidad aceptada, obsoleta, conflictiva, no disponible y no verificable. Nunca mutar ni continuar automaticamente una operacion inconclusa.

Directorio o archivos ausentes producen `not_found`, no fallo. Un backup ilegible, malformado, inseguro o contradictorio se informa y omite; no se repara al iniciar.

## Orden de escritura al cerrar

1. Refrescar evidencia y construir el resumen.
2. Aplicar el filtro acotado antes de ambos destinos.
3. Escribir primero el backup local con creacion exclusiva y verificarlo reabriendo, analizando metadata y encabezados.
4. Si Engram existe, escribir el resumen equivalente con alcance de proyecto y preservar cada resultado, incluidos candidatos.
5. Si ambos tuvieron exito, comprobar conflicto semantico material.
6. Informar `local_backup`, `engram_write` y `cross_check` de forma independiente.

Fallo local deja la continuidad local obligatoria en `failed`. Exito local y fallo o ausencia Engram produce `degraded`. Fallo local y exito Engram conserva ese exito, pero el cierre requerido queda `failed`. Si ambos escriben y chocan, queda `degraded`. Un conflicto material no resuelto es `human decision required`.

## Privacidad y filtro

Excluir antes de escribir:

- credenciales, tokens, contrasenas, llaves, cookies, cadenas de conexion y material de autenticacion;
- nombres de entorno unidos a valores, dumps, salida de comandos sensible y payloads de proveedor;
- rutas absolutas, usando forma relativa solo cuando sea util y segura;
- fuente, diffs, logs, trazas, contenido generado o salida de pruebas innecesarios;
- datos personales sensibles y preferencias personales no relacionadas;
- nombres o ubicaciones privadas externas salvo alias estable ya aprobado.

Es seleccion acotada de contenido, no prueba de deteccion, cifrado, infraestructura de redaccion ni defensa ante filesystem hostil. Si no se puede resumir de forma segura, se omite y se informa continuidad reducida por privacidad.

## Seguridad de ruta e ignore

Solo archivos `.md` regulares directos dentro de `.repo-harness/memory-bk/` son elegibles. Se rechazan entradas absolutas, `..`, separadores en campos de nombre, rutas indirectas, escapes por symlink o junction, streams alternos y resolucion entre raices. Nunca se siguen enlaces del contenido.

RH-W-0051 posee el ignore exacto y la exclusion de arbol limpio. RH-W-0069 crea el directorio de forma diferida al primer write autorizado. Nunca lo prepara ni rastrea.

## Conflictos

| Evidencia | Resultado |
|---|---|
| Memoria coincide con propietario actual | Aceptar tras verificar propietario |
| Memoria es anterior | Marcar obsoleta y usar propietario actual |
| Engram y backup difieren, repositorio resuelve | Informar y usar repositorio |
| Difieren y el repositorio no resuelve decision material | Preguntar; no fusionar ni elegir en silencio |
| Falta una fuente | Continuar con la disponible e informar faltante |
| Fuente malformada o insegura | Rechazarla, conservar otros resultados e informar razon |

No se crea store de conflictos. Se reutilizan RH-W-0050 y relaciones consultivas Engram sin reabrir RH-W-0050.

## Compatibilidad y migracion

- Sin directorio significa sin historia local y es valido.
- Sin importacion automatica desde memoria incorporada, handoffs viejos o ubicaciones fuera del directorio exacto de backups del repositorio.
- Repo-harness posee este contrato nativo de continuidad. Solo el propietario controla la politica de continuidad y autoriza cada operacion que cambia la continuidad conservada.
- Las observaciones Engram existentes quedan intactas y solo se leen cuando son relevantes.
- Markdown plano no necesita base de datos, cifrado, daemon, servicio ni API generica.

## Impacto en manifest y checker

RH-W-0051 posee el unico `localState` y el ignore. RH-W-0069 no agrega campos. El checker sigue de solo lectura, no inspecciona backups ni exige el directorio. Las pruebas enfocadas validan contratos y helpers; el checker solo valida taxonomia e ignore.

## Pruebas RED enfocadas

Antes de implementar deben fallar pruebas para:

1. Intento local obligatorio en cada pausa y cierre deliberado.
2. Metadata, siete secciones, rutas relativas y clasificaciones exactas.
3. Creacion exclusiva, diferida, con readback y sin overwrite.
4. Escritura local antes de Engram y resultados independientes en cada combinacion.
5. Comprobacion cruzada con casos obsoletos, conflictos resueltos o no, fuente malformada y faltante.
6. Orden repositorio, Engram, backup mas nuevo y lectura acotada de anteriores.
7. Verificacion de cada afirmacion contra su propietario.
8. Rechazo de credenciales, entorno crudo, rutas absolutas, fuente innecesaria y datos personales.
9. Contencion, archivos regulares, cero rutas indirectas y cero staging.
10. Independencia de Gentle AI, exclusion de memoria incorporada y uso exclusivo del contrato nativo de continuidad de sesion.
11. Ausencia de semantica de checkpoint, aprobacion, ciclo, conflicto, recibo o entrega.
12. Renders integrated y standalone, paridad bilingue, enlaces, placeholders y Unicode prohibido.

## Exclusiones

- Sin implementacion ni avance mas alla de `proposed`.
- Sin codigo, skills generadas, pruebas, manifest, checker, directorio, migracion, staging, commit, push, publicacion o red.
- Sin borrado automatico ni limpieza inicial.
- Sin API generica, base de datos, cifrado, daemon, scheduler, servicio, sync engine ni configuracion compleja.
- Sin continuacion automatica, reutilizar aprobacion, checkpoint ejecutable, duplicar documentacion, ciclo, recibo, entrega o conflictos.
- Sin acoplar Gentle AI ni depender de memoria incorporada.

## Rollback

El rollback de diseno elimina documento y referencias. Un rollback futuro quita contratos y pruebas, dejando intactos backups y Engram. Borrar backups siempre exige accion explicita separada.

## Condiciones de finalizacion

- Los disenos y registros bilingues son equivalentes y estan enlazados.
- RH-W-0051 reserva `.repo-harness/memory-bk/` sin poseer sesiones.
- RH-W-0069 posee schema, autoridad, ordenes, resultados, privacidad, conflictos, compatibilidad, pruebas y exclusiones.
- El orden propuesto coloca RH-W-0069 inmediatamente despues de implementar RH-W-0051 y antes de RH-W-0053, porque consume la taxonomia y establece continuidad ordinaria sin absorber checkpoints tipados.
- La implementacion sigue no autorizada por separado.

## Decisiones humanas restantes

Ninguna para este diseno. Implementacion y cualquier migracion o borrado futuro exigen decisiones explicitas separadas.
