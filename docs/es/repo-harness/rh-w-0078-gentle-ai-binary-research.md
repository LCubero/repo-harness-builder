# Investigacion RH-W-0078 sobre el binario de Gentle AI

## Conclusion

**Inferencia respaldada:** Gentle AI respalda **cuerpos cargados pequenos, divulgacion progresiva y propiedad determinista de maquina para comportamiento que no exige juicio del modelo**. No justifica agregar un binario compilado a este builder en Python. Para `RH-W-0078`, la recomendacion proporcional es que el renderer existente emita skills generadas de varios archivos y que solo la mecanica determinista apropiada pase al renderer o checker existente en Python. **Hecho verificado:** el diseno exacto y la implementacion siguen sin autorizar.

**Hecho verificado:** la investigacion se leyo el 2026-09-04 sobre la revision de Gentle AI `c24a724a1fc109a0cd488e0562feff916a41a7a9` y el release `v2.6.0`. No se clono ningun repositorio, no se instalo ningun paquete y no se ejecuto fuente ni comando de runtime de Gentle AI.

## Preguntas

1. Que mecanismos de Gentle AI reducen contexto visible para el modelo y no solo reorganizan codigo fuente?
2. Que responsabilidades pueden pasar de prosa a mecanica determinista sin ocultar al modelo una decision humana?
3. Debe este repositorio agregar un binario compilado, ampliar su renderer/checker en Python o solo dividir las skills generadas?
4. Que evidencia falta antes de aprobar un alcance para `RH-W-0078`?

## Fuentes

**Hecho verificado:** todas son evidencia primaria clase 2 del propietario del proyecto.

| Fuente | Clase | Lectura | Disparador de actualizacion |
| --- | --- | --- | --- |
| [README de Gentle AI en la revision revisada](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/README.md) | 2 | 2026-09-04 | Cambian la arquitectura documentada, los runtimes soportados o la declaracion de version estable |
| [Arquitectura](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/architecture.md) | 2 | 2026-09-04 | Cambian la propiedad de componentes o los limites entre binario y runtime |
| [Guia de estilo de skills](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/skill-style-guide.md) | 2 | 2026-09-04 | Cambian las reglas de carga, tamano o referencias |
| [Registro de skills](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/skill-registry.md) | 2 | 2026-09-04 | Cambia el comportamiento de descubrimiento o carga |
| [Contrato compartido del review ledger](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/assets/skills/_shared/review-ledger-contract.md) | 2 | 2026-09-04 | Cambia la composicion de contratos compartidos entre skills |
| [Plugin de transporte de revision de OpenCode](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/assets/opencode/plugins/opencode-review-transport.ts) | 2 | 2026-09-04 | Cambian la propiedad del transporte o la integracion con el runtime |
| Adaptadores de revision para [Claude](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/reviewerprovider/claude_adapter.go), [Codex](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/reviewerprovider/codex_adapter.go) y [Pi](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/reviewerprovider/pi_adapter.go) | 2 | 2026-09-04 | Cambian los limites de proveedor o las responsabilidades de los adaptadores |
| [Release Gentle AI v2.6.0](https://github.com/Gentleman-Programming/gentle-ai/releases/tag/v2.6.0) | 2 | 2026-09-04 | El release se modifica o queda reemplazado para los hechos citados |

## Hallazgos

### La medida relevante es el contexto visible para el modelo

**Hecho verificado:** Gentle AI separa skills concisas y contratos compartidos de la orquestacion determinista, plugins y adaptadores. El [registro](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/skill-registry.md), la [guia de estilo](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/skill-style-guide.md) y el [contrato compartido](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/assets/skills/_shared/review-ledger-contract.md) muestran que el detalle reutilizable puede vivir fuera de un unico cuerpo de entrada y alcanzarse cuando aplica. **Inferencia respaldada:** esto se transfiere a `RH-W-0078`: importa reducir el cuerpo cargado siempre; mover la misma prosa a otro archivo cargado siempre no ayuda.

### La mecanica determinista puede salir de la prosa; las decisiones no

**Hecho verificado:** la [arquitectura](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/architecture.md), el [plugin de runtime](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/assets/opencode/plugins/opencode-review-transport.ts) y los adaptadores ubican transporte, validacion e invocacion especifica del proveedor en limites de maquina. **Inferencia respaldada:** el patron se transfiere donde este repositorio ya tiene propiedad determinista en Python: render, validacion de schema, comprobacion de rutas exactas y comparacion de bytes. Consentimiento, autoridad, elecciones sin resolver e instrucciones en el punto de accion deben seguir visibles para el modelo.

### Un binario compilado nuevo es desproporcionado

**Hecho verificado:** el programa compilado de Gentle AI forma parte de su arquitectura existente. Este repositorio ya distribuye un renderer/checker en Python y soporta archivos generados en tres runtimes. **Inferencia respaldada:** otro ejecutable agregaria obligaciones de empaquetado, plataforma, release, procedencia y recuperacion sin evidencia de que compilar reduzca los dos cuerpos medidos. Ampliar la propiedad Python existente consigue el beneficio transferible con menos cambio arquitectonico.

### La divulgacion progresiva debe probarse

**Desconocido o no verificable:** si una superficie generada `references/` reduce el contexto efectivo antes de demostrar que cada agente soportado carga la skill de entrada sin cargar automaticamente toda referencia y alcanza con fiabilidad la referencia requerida en el punto de accion. **Hecho verificado:** `RH-W-0084` establecio que la distancia de instruccion importa. **Inferencia respaldada:** el alcance futuro necesita mediciones de cuerpo y pruebas de carga y alcance para Claude Code, OpenCode y Pi; dividir archivos por si solo no prueba menor huella efectiva de contexto.

## Transferencia y limites

| Clasificacion | Resultado para RH-W-0078 |
| --- | --- |
| Transferir | **Inferencia respaldada:** skills de varios archivos, contratos de entrada concisos, divulgacion progresiva, referencias compartidas y transporte o validacion deterministas fuera de prosa |
| Transferir con prueba | **Desconocido o no verificable:** carga por runtime y alcance de referencias hasta medir el contexto efectivo en los tres runtimes soportados |
| No transferir | **Inferencia respaldada:** un binario compilado nuevo, la estructura Go de Gentle AI, su arquitectura de revisores por proveedor o su ciclo de review ledger |
| Nunca recortar a ciegas | **Inferencia respaldada:** consentimiento, autoridad, seguridad de transporte, clasificacion de evidencia y obligaciones de punto de accion deben conservar un propietario alcanzable verificado |

## Clasificacion de evidencia y brechas

- **Inferencia respaldada:** un binario compilado nuevo no es proporcional al defecto medido; el renderer/checker Python ya es el limite de entrega propiedad de maquina.
- **Desconocido o no verificable:** si las referencias generadas reducen contexto efectivo, porque el resultado depende de como cada runtime carga recursos de skills.
- **Desconocido o no verificable:** la reduccion exacta, la agrupacion de referencias y la recuperacion por runtime hasta que un alcance bilingue las defina y mida.
- **Hecho verificado:** el `README.md` revisado aun nombra `v2.5.0` como estable, mientras la pagina oficial e inmutable de release identifica `v2.6.0`. La evidencia de release gobierna los hechos de release; la discrepancia no cambia los hallazgos arquitectonicos.
- **Decision humana requerida:** aprobar, corregir o rechazar la direccion recomendada en un futuro alcance bilingue; el diseno exacto y la implementacion siguen sin autorizar por separado.

## Recomendacion

**Inferencia respaldada:** el futuro alcance bilingue debe poner precio a una direccion preferida: generar skills de varios archivos con un contrato de entrada pequeno, mover selectivamente mecanica determinista al renderer/checker existente en Python, conservar decisiones visibles e instrucciones en el punto de accion, y probar reduccion de contexto cargado mas alcance de referencias en Claude Code, OpenCode y Pi. Debe rechazar tanto un binario compilado nuevo como el recorte ciego. **Hecho verificado:** esta recomendacion informa `RH-W-0078`; no aprueba diseno, implementacion, cambio de ciclo de vida ni entrega.
