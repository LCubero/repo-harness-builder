# Contribuir a repo-harness-builder

La fuente canonica es [`CONTRIBUTING.md`](CONTRIBUTING.md).

Toda contribucion comienza con un issue y termina con revision del maintainer. Mantenga cada cambio pequeno, verificable y enlazado con el problema que resuelve.

## Ruta rapida

1. Abra o elija un [issue de GitHub](https://github.com/LCubero/repo-harness-builder/issues). Acuerde el alcance con un maintainer antes de implementar.
2. Cree una rama corta y enfocada, como `docs/contribution-workflow` o `fix/projection-drift`.
3. Cambie primero las fuentes canonicas. Para comportamiento del producto, edite `skill/repo-harness-builder/`; trate `.claude/`, `.opencode/` y `.pi/` como proyecciones de entorno.
4. Mantenga implementacion, pruebas y documentacion cercana en la misma unidad de trabajo.
5. Ejecute las comprobaciones pertinentes y revise el diff completo.
6. Abra un PR enfocado que enlace el issue con `Closes #<numero>` e indique alcance, verificacion y riesgos.
7. Resuelva la revision y obtenga aprobacion del maintainer antes del merge.

No haga commits directos a `main`. Las funcionalidades, los cambios de comportamiento y todo lo que toque pruebas o proyecciones de entorno siempre usan un issue y un PR. Los commits directos a `main` son excepciones que solo puede conceder el propietario o una persona que el propietario autorice; cada excepcion debe solicitarse o declararse explicitamente en el momento. Las correcciones documentales directas a `main` aun requieren una entrada en el changelog.

## Reglas de desarrollo

- Use Python 3.10+ y la biblioteca estandar. No agregue dependencias sin un issue acordado y una necesidad clara.
- Siga [REPO-HARNESS.es.md](REPO-HARNESS.es.md). La evidencia del repositorio prevalece sobre la continuidad de Engram o Gentle AI cuando difieren.
- Mantenga los archivos de texto con finales de linea LF canonicos en Linux, macOS y Windows. La politica `.gitattributes` del repositorio los aplica independientemente de la configuracion local de `core.autocrlf`.
- Preserve `.codegraph/` como estado de herramienta ignorado. Nunca lo incluya en un commit o PR.
- Use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/), por ejemplo `docs: add contribution workflow` o `fix(checker): detect stale projection`.
- Haga commits por unidad entregable, no por tipo de archivo. Las pruebas y la documentacion pertenecen al comportamiento que verifican o explican.
- Mantenga los PR faciles de revisar. Unas 400 lineas cambiadas son un objetivo util, no una compuerta automatica de este repositorio. Divida el trabajo cuando una persona no pueda comprenderlo comodamente.

## Verificacion

Ejecute la suite completa de biblioteca estandar para cambios de producto o proyecciones:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

Ejecute la comprobacion de solo lectura para cambios documentales, de fuentes canonicas o proyecciones:

```powershell
python .repo-harness/check.py
```

El checker debe informar `current`, terminar con codigo 0 e informar 0 escrituras. Si cambiaron fuentes canonicas del producto, sincronice las salidas administradas afectadas mediante la vista previa de mantenimiento y el apply con el digest actual, y luego confirme que las proyecciones correspondientes en `.claude/`, `.opencode/` y `.pi/` contienen los mismos bytes previstos. Los cambios solo documentales que no alteran contratos generados no deben reescribir esas proyecciones.

## Empaquetado de release

Para cada entorno, ejecute `git archive --format=zip --prefix=<runtime>/skills/ -o repo-harness-builder-<version>-<name>.zip <commit>:skill`:
- `.claude` → `claude-code`; `.opencode` → `opencode`; `.pi` → `pi`.
- Liste los miembros del archivo y confirme que todos estan bajo `<runtime>/skills/repo-harness-builder/`.
- Extraiga en la raiz de un repositorio y ejecute `<runtime>/skills/repo-harness-builder/scripts/harness_tool.py --help` con Python; registre el SHA-256 de cada zip.
Publicar una release sigue siendo una decision del propietario.

## Lista para el PR

- [ ] Existe un issue y el PR lo enlaza.
- [ ] La rama y el PR contienen una sola unidad enfocada.
- [ ] Los commits siguen Conventional Commits.
- [ ] Se incluyen pruebas y documentacion cuando corresponde.
- [ ] El PR contiene comandos y resultados exactos de verificacion.
- [ ] El diff no contiene secretos, archivos ajenos ni `.codegraph/`.
- [ ] Un maintainer reviso y aprobo el cambio antes del merge.

Este repositorio no tiene actualmente templates de contribucion, labels personalizados de flujo ni una comprobacion automatica de aprobacion. Use la conversacion del issue y del PR como fuente de aprobacion.
