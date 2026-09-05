# Uso del harness del repositorio
<!-- doc-role:overview -->

Use las tres habilidades generadas como un ciclo: recupere contexto, mantenga vigente la documentacion durable y deje una transferencia verificada.

## Flujo diario
<!-- doc-role:daily-workflow -->

1. Inicie o reanude con `repo-harness-session-start` y declare el objetivo. Reconcilia contexto con evidencia del repositorio e inspecciona Git local en solo lectura. La comprobacion remota aplica solo cuando sincronizar es pertinente y seguro. Las afirmaciones conservan su clase de evidencia.
2. Use `repo-harness-documentation` cuando cambien hechos durables o el estado visible.
3. Cierre con `repo-harness-session-stop`, que verifica evidencia, documentacion y continuidad requeridas, usando Engram cuando esta disponible. Solo el trabajo Git solicitado activa revision de rutas y propuestas exactas bajo la politica del repositorio; no se repite una autorizacion sin cambios.

## Disponibilidad integrada
<!-- doc-role:availability -->

El modo integrado compone Gentle AI y Engram de forma independiente; cualquiera puede seguir siendo util sin el otro. El inicio reutiliza la recuperacion de Engram como evidencia de disponibilidad y comprueba Gentle AI solo en limites de coordinacion habilitados. Informe fallos sin prometer garantias ausentes; bloquee solo capacidades requeridas por la operacion. Un fallo de memoria opcional no impide el informe del repositorio. Standalone no usa ninguno de los dos componentes. La coordinacion administrada habilitada recibe contexto y comprobaciones del repositorio, no flujos paralelos; commit, push y PR siguen bajo politica del repositorio, sin nueva compuerta de entrega.

## Congruencia y propiedad
<!-- doc-role:ownership -->

Ejecute `python3 .repo-harness/check.py` (`python` en Windows) despues de cambiar fuentes canonicas o proyecciones. El archivo raiz `.gitattributes` preserva bytes LF mediante las reglas exactas `.repo-harness/check.py text eol=lf` y `.repo-harness/manifest.json text eol=lf`. El control demuestra paridad en disco, no la vigencia de contenido registrado en el proceso activo.

## Limites de aprobacion
<!-- doc-role:approval-boundaries -->

Las habilidades diarias son de solo lectura por defecto y usan aprobacion conversacional. La aplicacion del constructor es distinta y estricta unicamente durante una ejecucion explicitamente activa de la habilidad `repo-harness-builder`, declarada con `--activation builder`: en ingles exige exactamente `APPROVE` o `DENY`; en espanol exige exactamente `APROBAR` o `DENEGAR`. La CLI recibe explicitamente el idioma resuelto y exige tanto el digest vigente del plan como la aprobacion exacta del dominio activo. Con el valor predeterminado `--activation maintenance`, el dominio estricto no aparece en la salida, `--approval` se rechaza y aplicar solo requiere el digest coincidente. Las llamadas directas a la CLI usan ingles de forma predeterminada. La identidad del repositorio, el autoalojamiento, el mantenimiento ordinario del repositorio, la propiedad de archivos generados, las habilidades de sesion y las preguntas directas no activan esta interfaz. Fuera de una ejecucion activa de la habilidad del constructor, use reglas ordinarias de aprobacion conversacional.

Una corrida del constructor que lleva un contexto de autoridad tambien imprime digests que puede revisar sin leer nada privado: `authorityContextSha256` para saber de que trataba la aprobacion, `targetBindingSha256` para que destino nombro, `planSha256` para los cambios, y `generationSha256` que los liga a todos. Los valores de premisa y los enunciados de decision aparecen como marcadores explicitos de redaccion, y cada evidencia aparece como un locator relativo al repositorio y un digest. Verifique que esos locators sean los archivos que espera y recien entonces apruebe. Si algo estaba mal, el host debe refrescar el contexto con una revision material nueva y volver a previsualizar, porque una respuesta corregida con un plan identico conserva el mismo `planSha256` y obtiene un `generationSha256` distinto, de modo que la aprobacion anterior no se puede reutilizar.
