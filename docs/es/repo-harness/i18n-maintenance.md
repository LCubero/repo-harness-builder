# Mantenimiento de la documentacion bilingue

`REPO-HARNESS.md`, `CHANGELOG.md` y los documentos durables en ingles son propietarios de los hechos operativos. Los pares en espanol son presentaciones localizadas completas.

## Propiedad canonica
<!-- doc-role:canonical-ownership -->

Actualice primero los propietarios factuales. Si una redaccion localizada contradice un hecho canonico, prevalece el propietario canonico y debe corregirse la traduccion.

## Sincronizacion
<!-- doc-role:translation-sync -->

Actualice cada par afectado en espanol dentro de la misma unidad de trabajo. Se exige significado equivalente, no igualdad literal de encabezados u oraciones. Se permite redaccion natural propia del idioma si hechos, restricciones, comandos, rutas y resultados permanecen equivalentes.

## Correspondencia de entradas
<!-- doc-role:entry-mapping -->

En una lista que el lector cuenta, como una seccion fechada de `CHANGELOG.md`, el par en espanol corresponde una entrada a una entrada. Quien compare ambas versiones no debe tener que adivinar si una entrada ausente es una decision de traduccion o un hecho perdido.

Existe una sola excepcion, y debe declararse donde aplica en vez de inferirse. Una seccion historica puede consolidar varias entradas en ingles en menos entradas tematicas en espanol solo cuando cada hecho sobrevive en alguna entrada y la propia seccion declara que esta consolidada. Una diferencia de conteo sin declarar es un defecto, no una eleccion de estilo. Las secciones nuevas son siempre una a una.

## Enlaces
<!-- doc-role:link-validation -->

Mantenga exactamente un selector reciproco en cada README raiz. Resuelva y valide enlaces relativos desde el archivo que los contiene. Ejecute las pruebas canonicas y `python3 .repo-harness/check.py` (`python` en Windows) despues de cambiar documentacion.

## Vigencia
<!-- doc-role:freshness -->

`CHANGELOG.md` es el unico propietario cronologico. `docs/repo-harness/current-state.md` es el unico propietario del estado actual. Los pares en espanol presentan esos hechos e identifican su propietario.
