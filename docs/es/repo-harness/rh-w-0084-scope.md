# Alcance propuesto de investigacion RH-W-0084

## Estado y proposito

**Actualizacion de cierre del ciclo de vida, 2026-09-04: el propietario acepto el hallazgo de refuerzo como control bajo `RH-W-0083`, satisfizo la condicion de avance de la hoja de ruta y completo `RH-W-0084`. La implementacion de `RH-W-0083`, autorizada por separado, esta ahora completada. La investigacion se ejecuto el 2026-08-31 despues de reconciliar y completar `RH-W-0012`. La redaccion historica de abajo se conserva tal cual.**

**Estado: propuesto; alcance bilingue aprobado por el propietario.** Este alcance solo encuadra investigacion auditable y LLM-first. Aprobarlo deja `RH-W-0084` en `proposed` y no autoriza lectura externa de red o fuentes, implementacion de producto, pruebas de producto, superficie generada, manifiesto, checker ni avance de ciclo de vida.

**Etiquetas de evidencia:** el objetivo LLM-first y este alcance son artefactos de direccion humana. Toda guia externa sigue siendo desconocida hasta leerse bajo el limite de investigacion aprobado; aqui no se presenta como hecho.

## Preguntas exactas de investigacion

1. **Formato:** Que formatos de instrucciones consume y aplica con mas fiabilidad el contexto LLM objetivo: prosa breve, encabezados, listas, tablas, bloques cercados, formas clave-valor u otra forma acotada?
2. **Tamano:** Que rangos de tamano de cuerpo, seccion y contexto cargado conservan recuperacion y aplicacion correctas sin perder instrucciones materiales?
3. **Ruta y ubicacion:** Que ruta canonica, ubicacion de enlace y punto de carga vuelven alcanzable una instruccion requerida cuando ocurre la accion relevante?
4. **Refuerzo:** Cuando debe repetirse una instruccion en el punto de accion y cuando basta un propietario canonico con una referencia?
5. **Formas y visibilidad:** Que formas visibles, etiquetas, delimitadores o estructuras de respuesta hacen reconocible y util una accion, advertencia o pregunta requerida?
6. **Pregunta de control:** Dado un hallazgo, cuando se vuelve una prueba o control de tiempo de desarrollo, un requisito de contrato generado o una guia documentada, y que propietario existente lo recibe?

## Evidencia admisible y limite de fuentes

Las clases de evidencia admisibles, en orden de prioridad, son:

1. mediciones y comportamiento verificados de este repositorio;
2. documentacion primaria del propietario relevante del modelo, runtime o estandar;
3. investigacion revisada por pares o de organismos de estandares con procedencia suficiente para evaluar aplicabilidad; y
4. material secundario solo como pista o comparacion, nunca por si solo como hecho del repositorio u objetivo de control.

Este alcance no autoriza **ninguna lectura** de clases externas. Tras la aprobacion del propietario, `RH-W-0012` debe reevaluarse contra estas preguntas antes de leer cualquier fuente externa. `RH-W-0065` conserva el limite de evidencia de investigacion y referencias durables, incluido el tratamiento de fuentes localizadas pero no leidas.

## Del hallazgo al control del repositorio

1. Registrar cada hallazgo candidato en el futuro propietario de investigacion tematica con su etiqueta de evidencia, clase de fuente, limite de aplicabilidad e incertidumbre sin resolver.
2. Compararlo con mediciones del repositorio; una afirmacion de fuente por si sola no es una conclusion del repositorio.
3. Enrutar una conclusion aceptada hacia un propietario existente: un requisito determinista y repetible se vuelve una prueba o control de tiempo de desarrollo; una redaccion operacional exacta para el agente se vuelve un requisito de contrato generado; una explicacion no determinista se vuelve guia documentada.
4. Obtener la aprobacion del propietario y la autoridad de implementacion requeridas por separado antes de cambiar cualquier destino. Este alcance no crea ningun control ni cambio de destino.

## Criterios de aceptacion

- Ambos pares de idioma nombran las seis preguntas, las clases de fuente admisibles, el limite de evidencia y la regla de enrutamiento anterior.
- El alcance exige expresamente una nueva evaluacion de `RH-W-0012` despues de aprobarse y antes de toda lectura externa.
- Nombra a `RH-W-0065` como limite de evidencia y referencias, y conserva los propietarios distintos de las superficies informadas.
- No presenta como hecho una guia externa no investigada ni autoriza ejecutar investigacion, implementar ni cambiar una superficie generada.

## Coordinacion y no objetivos

Coordinar evidencia y referencias con `RH-W-0065`; coordinar posibles superficies informadas con `RH-W-0067`, `RH-W-0073`, `RH-W-0076`, `RH-W-0078` y `RH-W-0083`. Estas unidades conservan sus propios limites de alcance, ciclo de vida e implementacion; esta investigacion no las fusiona, reabre ni avanza.

No objetivos: elegir ahora un objetivo; leer una fuente externa; cambiar fuente de producto, pruebas, proyecciones generadas, manifiesto, comportamiento del checker, politica de red o comportamiento de runtime; crear un nuevo almacen de referencias o ciclo de vida; y tratar consejo de vendors o comunidades como guia verificada del repositorio sin evidencia aplicable.

## Relacion con el ciclo de vida

`RH-W-0084` permanece `proposed`. El alcance bilingue aprobado por el propietario no vuelve el registro `ready` ni autoriza implementar. `RH-W-0012` ya se reevaluo para las preguntas nombradas; su unidad de seguridad debe reconciliarse antes de toda investigacion externa autorizada por separado. El documento tematico de investigacion y cada control resultante siguen siendo trabajo con autorizacion separada.

Ambas condiciones quedaron satisfechas el 2026-08-31: `RH-W-0012` se reconcilio, sus siete limites quedaron especificados y aceptados, y despues el propietario autorizo la ejecucion de investigacion. Todo control resultante sigue requiriendo la autorizacion separada de su propio duenio, y ninguno se ha aplicado.
