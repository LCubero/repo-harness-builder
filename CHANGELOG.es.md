# Historial de cambios

[`CHANGELOG.md`](CHANGELOG.md) es el unico historial cronologico canonico. Este archivo ofrece una presentacion completa en espanol y se mantiene en la misma unidad de trabajo.

## 2026-08-13

- Se hizo accionable la remediacion ante fallos de transporte: una sesion generada ahora debe comprobar si existe una via de credenciales alternativa y nombrarla, prefiriendo la herramienta del proveedor usada como asistente de credenciales de un solo comando que no altera la configuracion global, y devolviendo el comando exacto a la persona cuando la via restante necesita un secreto que el agente no puede aportar.
- Se ligo la respuesta de las sesiones generadas al idioma preferido declarado por la persona usuaria o, en su defecto, al idioma en que escribe, y se eliminaron los literales en espanol fijos del informe del verificador que imponian un unico idioma sin importar quien leyera.
- El idioma de la documentacion pasa a ser una eleccion declarada y no una suposicion universal: un repositorio es solo en ingles salvo que declare mas idiomas, asi que uno monolingue ya no hereda nueve archivos en espanol, un contrato de mantenimiento i18n y un selector de README que nunca pidio.
- Se registraron en el manifiesto el idioma de conversacion y los idiomas declarados de documentacion, se validan ambos en el verificador generado, y la comprobacion de congruencia los descubre en lugar de exigir que se los indiquen otra vez.
- Se separo el idioma del informe del constructor, que es presentacion y nunca entra en el digest del plan, del idioma de conversacion del harness construido, que es una eleccion del producto que el constructor pregunta.
- Se corrigio la comparacion por secciones para que el documento prometido elija el vocabulario de ambos lados, terminando con un informe falso que nombraba todas las secciones de un documento que demostrablemente las conservaba.
- Se limito la comprobacion de idioma en documentos de propiedad humana al ingles sin traducir en lugar de fijar frases sembradas que hacian que reescribir legitimamente se leyera como deriva, mientras la salida generada conserva su base requerida completa.
- Se incorporaron los marcadores `doc-role` y se actualizaron los envoltorios `repo-harness-help` de este repositorio, dejando en `current` tanto el check del constructor como el verificador generado.

## 2026-08-12

- Se corrigieron seis bloqueos de revision bilingue: plantillas y pares raiz completos en espanol, validacion de completitud por idioma, ayuda localizada integrada con digest estable, planificacion reciproca de README con cierre seguro, mapas con enlaces resolubles y clasificacion normativa de fuentes.
- Se agrego una arquitectura documental completa en ingles y espanol para este producto y los repositorios consumidores.
- Se localizaron los dominios estrictos de aprobacion del constructor y se mantuvieron conversacionales las habilidades diarias.
- Se agregaron controles de alcance previo al commit, consentimiento Git por resultado y diagnosticos limitados de transporte.
- Se corrigieron la preservacion de repositorios existentes, las actualizaciones seguras y los punteros raiz con preservacion exacta de bytes.
- Se agregaron la guia del producto, el harness autoalojado, la licencia MIT y las proyecciones para Claude Code y OpenCode.
