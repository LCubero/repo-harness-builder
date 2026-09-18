# Alcance RH-W-0091 de prioridad de inicio y reanudación primero

**Estado: `completed`; alcance histórico aprobado por el propietario.** `RH-W-0091` está `completed`; su [registro de la hoja de ruta](roadmap.md#rh-w-0091) es el propietario del ciclo de vida. Este documento conserva el contrato observable y los límites aprobados. La implementación y la verificación independiente final están completas bajo autoridad separada; no se afirman commit, push, PR, merge, release ni publicación.

## Propietarios canónicos y límite

El alma del harness/carta del proyecto canónica y la decisión LLM-first permanecen en [decisions.md](decisions.md). Se leen de forma acotada y se referencian aquí; no se copian, se reformulan como una segunda autoridad ni se mueven.

`RH-W-0069` conserva el esquema, seguridad, privacidad y comportamiento vigente de continuidad nativa de backups. `RH-W-0088` conserva la recuperación de Engram como evidencia de disponibilidad. `RH-W-0078` conserva la división de cuerpos de skills generadas y la divulgación progresiva, incluido el tratamiento del contexto de dependencias. `RH-W-0031`, `RH-W-0071`, `RH-W-0082` y `RH-W-0018` conservan la procedencia de idioma y sus semánticas independientes de preferencias. `RH-W-0091` ordena el consumo solo cuando esos propietarios existentes aportan evidencia válida; no crea inferencia, esquema ni propietario de preferencias.

## Resultados observables futuros

### Prioridad exacta de inicio

Para un inicio de sesión pertinente, el contrato futuro debe consumir evidencia en este orden exacto:

1. el alma del harness/carta del proyecto canónica;
2. el idioma de conversación verificado y las preferencias estables activas o restricciones continuas de sus propietarios existentes y declarados;
3. `Where We Left Off` del backup nativo válido más nuevo (presentado al usuario como «En qué quedamos»); y
4. los detalles solo cuando lo requieran la pertinencia para el objetivo, la contradicción, la evidencia de prioridad inválida o ausente, o la reconciliación obligatoria del repositorio.

La evidencia canónica del proyecto permanece autoritativa en todo momento. La evidencia de prioridad inválida, ausente, obsoleta o contradictoria se informa y reconcilia contra su propietario actual del repositorio; nunca se infiere en silencio ni se vuelve autoritativa por el orden anterior.

Cuando Engram es invocable, la recuperación pertinente y con alcance explícito es por sí misma evidencia de disponibilidad. El contrato futuro no debe agregar una prueba ni preflight separado, ni repetir detalle de capacidad exitoso y sin cambios. Los fallos de recuperación y discrepancias pertinentes a la tarea siguen visibles bajo el propietario existente.

### Continuidad nativa con reanudación primero

La continuidad nativa conserva un archivo de backup exclusivo con timestamp UTC por cada pausa o cierre deliberado. Su primera sección futura de reanudación es `Where We Left Off`, presentada al usuario como «En qué quedamos», y contiene exactamente y en este orden:

1. `Next Action`
2. `Pending Work`
3. `Current Blocker`

Cada valor vacío es `None.` Las afirmaciones aplicables conservan las etiquetas de evidencia en línea existentes: `verified fact`, `supported inference`, `unknown or unverifiable` o `human decision required`; este alcance no agrega taxonomía de etiquetas ni regla de inferencia.

Session-start debe validar y seleccionar de forma segura el backup elegible más nuevo, leer ese contenido de reanudación primero antes del detalle posterior, y expandir solo por pertinencia para el objetivo, contradicción, evidencia de prioridad inválida o ausente, o reconciliación obligatoria del repositorio. La evidencia del repositorio permanece autoritativa.

Session-stop debe escribir el contenido de prioridad y reanudación primero antes de la evidencia detallada. La continuidad detallada permanece después y no se vuelve un segundo propietario de ciclo de vida, aprobación, recibo o conflicto.

### Límite de skills generadas

Este alcance establece solo el orden semántico de lectura/escritura. Únicamente `RH-W-0078` decide si y cómo las skills de sesión generadas dividen cuerpos o divulgan dependencias progresivamente. Los registros no se fusionan.

## Escenarios acotados de aceptación futura y categorías RED

Antes de cualquier implementación autorizada por separado, la cobertura RED enfocada debe demostrar estas categorías de comportamiento sin exigir nombres de pruebas aquí:

1. el consumo primero de la carta referencia al propietario canónico sin copiar una carta;
2. el idioma verificado y las preferencias estables activas o restricciones continuas de sus propietarios existentes y declarados preceden la recuperación del backup, mientras datos ausentes, inválidos o inactivos no se infieren;
3. la recuperación Engram acotada aporta evidencia de disponibilidad sin prueba/preflight separado ni detalle repetido de éxito sin cambios;
4. la validación y selección del backup más nuevo rechazan candidatos inseguros o inválidos y preservan la autoridad del repositorio;
5. `Where We Left Off`, presentado al usuario como «En qué quedamos», se lee antes del detalle posterior y contiene solo los tres campos ordenados, con `None.` y las etiquetas de evidencia existentes cuando apliquen;
6. session-start se expande más allá del contenido de reanudación primero solo por los disparadores nombrados de objetivo, contradicción, prioridad inválida/ausente o reconciliación obligatoria;
7. session-stop coloca prioridad y contenido de reanudación primero antes de la continuidad detallada y conserva un backup exclusivo con timestamp por pausa o cierre deliberado; y
8. la división de cuerpos de skills generadas, la semántica de preferencias, la procedencia de idioma y la propiedad de ciclo de vida permanecen con sus registros existentes.

Estas pruebas futuras demuestran comportamiento de contrato renderizado u observable, no continuación automática, inferencia de preferencias, reutilización de autoridad ni cumplimiento de modelo en vivo.

## Exclusiones explícitas

- No copiar ni cambiar el alma del harness/carta del proyecto canónica, la decisión LLM-first, la procedencia de idioma, la semántica o esquemas de preferencias ni los límites de propiedad.
- No cambiar skills Chiki-RO ni globales `wk`, ni copiar desde ellas.
- No cambiar el candidato preexistente de `RH-W-0046`.
- Sin almacenamiento incremental de backup en un único archivo, checkpoint, retención, migración, limpieza, marco generalizado de preferencias, servicio/API de parser, base de datos, cache, daemon, scheduler ni continuación automática de trabajo.
- Sin comportamiento de implementación, pruebas, proyección de runtime, diseño, plan de implementación, escritura de backup o preferencia, reutilización de autoridad, Git, red, entrega ni debilitamiento de compuertas de privacidad, evidencia, Git o red.
- Sin fusión con `RH-W-0078`, `RH-W-0090`, `RH-W-0053` ni los registros separados de preferencias.

## Límite de aprobación

**Alcance aprobado por el propietario.** La implementación y el cierre de ciclo completados preservan este alcance; la entrega sigue sin afirmarse.
