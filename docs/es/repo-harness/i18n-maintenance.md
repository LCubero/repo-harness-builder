# Mantenimiento de la documentacion bilingue

`REPO-HARNESS.md`, `CHANGELOG.md` y los documentos durables en ingles son propietarios de los hechos operativos. Los pares en espanol son presentaciones localizadas completas.

## Propiedad canonica
<!-- doc-role:canonical-ownership -->

Actualice primero los propietarios factuales. Si una redaccion localizada contradice un hecho canonico, prevalece el propietario canonico y debe corregirse la traduccion.

## Sincronizacion
<!-- doc-role:translation-sync -->

Actualice cada par afectado en espanol dentro de la misma unidad de trabajo. Se exige significado equivalente, no igualdad literal de encabezados u oraciones. Se permite redaccion natural propia del idioma si hechos, restricciones, comandos, rutas y resultados permanecen equivalentes.

## Enlaces
<!-- doc-role:link-validation -->

Mantenga exactamente un selector reciproco en cada README raiz. Resuelva y valide enlaces relativos desde el archivo que los contiene. Ejecute las pruebas canonicas y `python .repo-harness/check.py` despues de cambiar documentacion.

## Vigencia
<!-- doc-role:freshness -->

`CHANGELOG.md` es el unico propietario cronologico. `docs/repo-harness/current-state.md` es el unico propietario del estado actual. Los pares en espanol presentan esos hechos e identifican su propietario.
