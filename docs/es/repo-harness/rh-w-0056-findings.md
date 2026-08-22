# Hallazgos de RH-W-0056: revision de harness externo

<!-- doc-role:research -->

La fuente factual canonica es [`../../repo-harness/rh-w-0056-findings.md`](../../repo-harness/rh-w-0056-findings.md). Este es su espejo humano completo en espanol. El propietario del ciclo de vida es [RH-W-0056](roadmap.md#rh-w-0056).

## Alcance y metodo

Inspeccion de solo lectura de un harness externo que el propietario construyo y posee, realizada el 2026-08-22. Nada del repositorio revisado se modifico, ejecuto ni copio aca; su arbol de trabajo y su `HEAD` se verificaron sin cambios despues de la revision.

Al harness revisado se lo llama **el harness externo** en todo el documento. Su identidad de proyecto, rutas absolutas, hosts, credenciales, nombres del equipo, datos de cliente y numeros de dominio estan deliberadamente ausentes, porque un patron portable no los necesita y este repositorio no los posee.

### Que se leyo

La superficie completa del harness: el rulebook raiz y los tres archivos puntero de la raiz; los seis archivos de reglas a los que enruta; las seis habilidades del pack de runtime, con todas sus referencias y assets de banner; el documento indice de controles; el docstring de cada script de control mas las dos implementaciones de control mas grandes completas; los archivos de ignore y atributos; la guia de instalacion del pack; el registro de habilidades generado; y las secciones de convenciones y comandos de sesion del README del producto.

### Que no se leyo, y por que

Las referencias de destino de despliegue, los cuerpos de los flujos de auditoria y respaldo mas alla de sus reglas y peligros, las secciones de plataforma de datos del README del producto, y la documentacion de negocio, notebooks y codigo de aplicacion. Eso es detalle operativo de dominio, no patrones de harness. Donde una leccion de esos archivos llego a una regla, a un archivo de peligros o al indice de controles, se leyo ahi. **Ninguna afirmacion de este documento se apoya en esos archivos, y ninguna afirmacion de ausencia los cubre.**

### Como leer un hallazgo

Cada hallazgo es un **patron**, reformulado en el vocabulario de este repositorio. Ninguno es copia de la implementacion revisada y ninguno queda adoptado por este documento. La adopcion es una decision separada del propietario, y cada patron adoptado necesita su propia unidad de trabajo con diseno, pruebas y evidencia de entrega propios.

**Clasificacion**: `integrar` significa que este repositorio deberia poseer el patron; `inspirar` significa que la idea es valiosa pero el mecanismo aca seria distinto; `ya presente` marca un patron al que este repositorio llego de forma independiente, registrado porque la convergencia es evidencia en si misma; `rechazar` significa que no pertenece; `fuera de alcance` significa que atiende un problema que este producto no tiene.

## Las tres ideas estructurales

**1. Una fuente, N copias que nadie sincroniza.** El harness externo declara que casi todos sus controles atrapan esta unica forma, y llego a esa conclusion de manera independiente. Es la misma forma que posee el checker generado de este repositorio. La convergencia independiente la eleva de preferencia de diseno a inferencia respaldada sobre la clase de problema.

**2. Una nota no es un control; un control vive en la ruta ejecutable.** Varios de sus controles existen porque una regla escrita en prosa fallo en silencio durante semanas mientras todos creian que se cumplia. Un caso vale declararlo exacto: un arreglo se documento en memoria como la respuesta, y despues la misma falla volvio a ocurrir en una sesion posterior, porque esa documentacion vivia solo en memoria y nunca llego al archivo que corre. Este repositorio defiende varias obligaciones con prosa, y `RH-W-0038` ya registro que una regla en prosa no se puede exigir mecanicamente. La respuesta del harness externo no es debilitar la regla sino encontrar la mitad mecanizable y mecanizar esa mitad.

**3. Un peligro que puede repetirse necesita una guarda, y la segunda ocurrencia es el disparador.** Un riesgo documentado lo lee quien ya sospecha de el, nunca quien esta por repetirlo. En la segunda ocurrencia, dejar de arreglar el caso y buscar que permite al sistema habilitarlo.

## Grupo A: controles y verificacion

| # | Patron | Clase de evidencia | Recomendacion |
|---|---|---|---|
| A1 | **Corredor de controles por descubrimiento.** Un corredor recorre el arbol de herramientas y ejecuta cada `test_*.py` que encuentra. Agregar un control cuesta un lugar en vez de cinco. Origen medido: existian once archivos de test y nada corria ninguno, sin CI, sin configuracion de corredor, sin script. Diez de los once pasaban a mano. | Hecho verificado en el codigo del corredor | `integrar` |
| A2 | **Cero descubiertos no es un pase.** Un corredor que no encuentra tests sale distinto de cero: un resultado vacio contesta por la sonda, no por el mundo. | Hecho verificado en el codigo del corredor | `integrar` |
| A3 | **Detector que se prueba a si mismo.** Un checker de contenido corre su propio detector contra una cadena que sabe presente y se niega a reportar limpio si el detector falla. Origen: una verificacion de shell que devolvio cero sobre un archivo que tenia dos, porque la shell no expandia el escape buscado. | Hecho verificado en el docstring del checker | `integrar` |
| A4 | **Veredicto tolerante que no es una venda.** Un control que falla separa una libreria de terceros ausente, que es diferencia de maquina, de un modulo ausente que el repositorio posee, que es rotura real. Sin esa division, mover un archivo interno se reporta como sano. | Hecho verificado en el codigo del corredor | `inspirar` |
| A5 | **Controles por cadencia, informados y nunca exigidos.** Un control cuyo disparador es el tiempo y no un cambio de archivo lleva un archivo de sello versionado, imprime solo cuando esta vencido o nunca corrio, y jamas cambia el codigo de salida. El sello va versionado a proposito porque la cadencia es del equipo y no de una laptop. Solo se sella una corrida limpia: registrar una que encontro algo reiniciaria el reloj sobre hallazgos que nadie atendio. | Hecho verificado en el corredor y el indice de controles | `inspirar` |
| A6 | **La salida que falla se imprime completa, nunca resumida.** Un veredicto sin su razon manda al lector a reproducirlo a mano. | Hecho verificado en el codigo del corredor | `integrar` |
| A7 | **Los controles viven en el repositorio, no en el pack distribuido.** Un pack instalado en otro lado debe informar que el script no esta y caer a una revision manual. | Hecho verificado en el indice de controles | `ya presente` |
| A8 | **Un codigo de salida no prueba que una corrida haya tenido exito.** Un wrapper reporto salida 0 mientras su log guardaba el traceback de una corrida que murio antes de escribir nada. | Hecho verificado en el archivo de peligros de respaldo | `integrar` |
| A9 | **Un control ausente se informa y se saltea, nunca falla.** El pack llega a proyectos que no tienen carpeta de controles. | Hecho verificado en la habilidad de cierre | `ya presente` |
| A10 | **Un control que no corrio nunca se reporta como aprobado.** Declarado como regla de correccion y no como preferencia de formato: esa confusion exacta permitio que un chequeo de documentacion reportara limpio durante meses sin revisar nada. Tres de sus controles pueden fallar al *correr*, que es distinto de fallar. | Hecho verificado en la habilidad de cierre y el indice de controles | `integrar` |
| A11 | **Un barrido limpio informa una linea.** Nombrar un control individual solo cuando encontro algo, arreglo algo o no pudo correr. Un muro de lineas verdes es ruido, y el ruido es lo que ensena a saltear la unica linea que importaba. | Hecho verificado en la habilidad de cierre | `integrar` |
| A12 | **Un mapa de anclas lleva su propio autochequeo.** El mapa de documentacion abre con un fragmento ejecutable que lista cualquier ancla que dejo de resolver, se corre antes de usar el mapa, y ordena arreglar el mapa como parte del cambio ya en curso. | Hecho verificado en el mapa de documentacion | `integrar` |

## Grupo B: proyeccion y packs

Incide directamente sobre la abierta `RH-W-0057`.

| # | Patron | Clase de evidencia | Recomendacion |
|---|---|---|---|
| B1 | **El chequeo de proyeccion cubre comandos, no solo habilidades.** Se comparan contra la copia de runtime tanto el directorio de habilidades como el de comandos de cada pack. Esa es exactamente la brecha que `RH-W-0057` registra aca. | Hecho verificado en el checker de packs | `integrar` |
| B2 | **Cinco veredictos, no dos.** `MISSING`, `STALE`, `EXTRA`, `PACK` cuando los packs discrepan entre si, y `HARNESS` cuando un puntero raiz dejo de apuntar al rulebook. `EXTRA` se informa y nunca se toca: ahi viven los experimentos locales. | Hecho verificado en el checker de packs | `integrar` |
| B3 | **La direccion siempre es fuente hacia runtime, y el apply nunca borra.** Un archivo de runtime distinto no puede decir por contenido solo si la fuente avanzo o si alguien edito el runtime sin portar. La regla decide; el lado perdedor se preserva. | Hecho verificado en el checker de packs | `ya presente` |
| B4 | **Los archivos raiz propiedad del harness se siembran una vez y nunca se sobrescriben.** El runtime reescribe su propio archivo puntero, asi que reemplazarlo destruye lo que se acumulo ahi. Lo unico exigido es que siga apuntando al rulebook. | Hecho verificado en el checker de packs | `ya presente` |
| B5 | **El vocabulario por runtime vive en datos, no en ramas de codigo.** Un runtime llama `prompts/` a su carpeta de comandos; el checker lo lleva como tupla por pack. | Hecho verificado en el checker de packs | `ya presente` |
| B6 | **Las carpetas de runtime estan gitignoreadas; solo el pack fuente viaja por git.** Declarado como consecuencia en el rulebook: una habilidad editada solo en el runtime no esta versionada y se pierde en un clon nuevo. | Hecho verificado en el archivo de ignore y el rulebook | `inspirar` |
| B7 | **La falla medida que creo el control.** Los tres packs coincidian en una habilidad mientras dos de los tres runtimes seguian con una version anterior. Nadie lo noto porque quien edita ve su propio runtime funcionando. | Hecho verificado en el checker de packs | `integrar` |
| B8 | **Un archivo de harness se compara por encabezado, y la reparacion solo agrega.** La copia del pack es el *minimo* que el runtime debe contener: se copia entera cuando falta, y despues se compara solo por `## encabezado`, de modo que una seccion requerida ausente se agrega al final. El runtime puede llevar secciones extra. Nada se reemplaza. | Hecho verificado en el flujo de inicio | `integrar` |
| B9 | **Solo se revisan los runtimes efectivamente instalados, y el veredicto los nombra.** Un runtime que esta maquina no tiene se omite, no es error. | Hecho verificado en el flujo de inicio | `integrar` |
| B10 | **La deriva se informa y la reparacion se ofrece, nunca se aplica sin pedirlo.** El lado perdedor puede llevar trabajo sin portar, asi que decide la persona. | Hecho verificado en la habilidad de inicio | `ya presente` |
| B11 | **Cuando no hay fuente canonica designada, la paridad se prueba por hash y se declara asi.** La referencia de una habilidad lleva un fragmento explicito de paridad por hash entre sus tres copias de pack, mas la instruccion de no introducir una cuarta fuente, no etiquetar un pack como generado y no afirmar una sincronizacion automatica que no existe. | Hecho verificado en el flujo de auditoria | `inspirar` |

## Grupo C: transporte, SSH y multiplataforma

Incide directamente sobre la abierta `RH-W-0046` y sobre los gates de sesion generados.

| # | Patron | Clase de evidencia | Recomendacion |
|---|---|---|---|
| C1 | **Deteccion automatica de transporte con respaldo ordenado, para fetch y para push.** Resolver en orden y detenerse en el primero que funciona: el helper de credenciales del CLI del proveedor cuando esta autenticado y el host coincide, luego Git nativo, y si no fallar ruidoso con un banner que nombra el comando exacto. Un push que completo por cualquier transporte es exito. | Hecho verificado en ambos flujos de sesion | `integrar` |
| C2 | **La ruta del proveedor debe pasar una URL HTTPS explicita.** Sobrescribir la URL del remoto por configuracion no vence de forma confiable a un origin SSH existente y cae a SSH en silencio. | Hecho verificado en ambos flujos de sesion | `integrar` |
| C3 | **Todo paso de red es no interactivo, porque un agente no tiene TTY.** Una llave con passphrase o una credencial sin cache abre un prompt que nadie puede contestar, y la sesion se cuelga sin error, sin banner y sin timeout. Modo batch para SSH, interruptor del prompt de terminal para Git y timeout de conexion acotado. En el cierre es peor: la memoria ya se guardo y el commit puede existir, asi que la persona ve una sesion congelada en lugar de un banner. | Hecho verificado en ambos flujos de sesion | `integrar` |
| C4 | **Nunca escribir una asignacion de entorno como prefijo de comando.** Solo POSIX; falla en PowerShell antes de que el comando corra, en la plataforma exacta que la guarda protege. | Hecho verificado en ambos flujos de sesion | `ya presente` |
| C5 | **La forma corta del upstream debe ir entre comillas.** Sin comillas, PowerShell puede parsearla como expresion de hashtable. Este repositorio la usa en prosa sin declarar el riesgo. | Hecho verificado en ambos flujos de sesion | `integrar` |
| C6 | **Estar al dia es exito, nunca falla.** A la par del upstream, o adelantado con commits sin pushear, no necesita integracion. | Hecho verificado en ambos flujos de sesion | `ya presente` |
| C7 | **Los limites conocidos del mecanismo se documentan junto a el.** La sobrescritura del comando SSH se ignora cuando la variable de entorno correspondiente esta puesta, y el interruptor del prompt de terminal no silencia el prompt grafico de un helper de credenciales. | Hecho verificado en ambos flujos de sesion | `integrar` |
| C8 | **La integracion es un merge local del upstream ya traido, nunca un pull que vuelve a traer.** Un pull pelado vuelve a buscar por el transporte por defecto y falla para quien solo tiene la ruta del proveedor. | Hecho verificado en el flujo de cierre | `integrar` |
| C9 | **Los riesgos multiplataforma se registran como incidentes medidos, no como advertencias.** Una herramienta de checksum cuyo formato de salida difiere entre dos shells y ya produjo un falso positivo; un sistema operativo que normaliza nombres de archivo distinto, nombrado como el entorno no probado en vez de asumido seguro; un CLI de nube que en Windows resuelve a un shim por lotes, de modo que una llamada de subproceso pelada revienta antes del primer pedido; y un alias de shell en Windows que abre otro subsistema en vez del interprete esperado. | Hecho verificado en el indice de controles y los peligros de respaldo | `inspirar` |
| C10 | **Un prefijo de zona horaria no es portable y falla en silencio.** Tomar la fecha con un prefijo `TZ=` una shell de Windows lo ignora sin error y devuelve UTC, que despues de las 18:00 local ya es manana, asi que todo queda fechado un dia adelante y nadie lo nota leyendo. La regla es un `date` local pelado mas una conversion explicita. | Hecho verificado en los acuerdos de trabajo | `integrar` |
| C11 | **La configuracion de fin de linea puede pelear con el archivo de atributos y fingir un cambio de contenido.** Con autocrlf activo, un exportador que normaliza finales de linea emite una actualizacion espuria por archivo afectado; verificar contra el blob commiteado antes de registrar un cambio de contenido. | Hecho verificado en los peligros de respaldo | `integrar` |
| C12 | **Revisar el estado del repositorio incluye las ramas, no solo si el arbol esta limpio.** Medido el mismo dia: una auditoria que cubrio documentacion, codigo, harness, controles y despliegue nunca listo las ramas, y una rama fusionada muerta hacia quince dias seguia apareciendo y sembrando duda. Lo que zanja el borrado es un chequeo de ascendencia contra las dos puntas, local y remota, nunca el nombre ni la fecha. | Hecho verificado en los acuerdos de trabajo | `integrar` |

## Grupo D: ciclo de vida de la documentacion

| # | Patron | Clase de evidencia | Recomendacion |
|---|---|---|---|
| D1 | **Un documento canonico por decision; los demas pueden mencionarla pero deben enlazar.** Apuntar esta permitido; repetir sin apuntar es lo que se pudre. Origen medido: el estado de una decision se reescribio en quince archivos con las palabras de cada uno, y tres indicaban trabajo que ya no hacia falta. | Hecho verificado en el checker de estado de decisiones | `integrar` |
| D2 | **Un estado ambiguo falla en vez de adivinarse.** Adivinar es como un item todavia abierto paso por cerrado. | Hecho verificado en el checker de backlog | `ya presente` |
| D3 | **Un item abierto ausente de la tabla resumen que la gente si lee es una falla.** Estado en dos lugares dentro de un documento es la misma forma de una fuente, a escala de documento. | Hecho verificado en el checker de backlog | `ya presente` |
| D4 | **Una afirmacion que no se puede comprobar falla en vez de advertir.** Un item que afirma un estado sin citar la evidencia que lo zanjaria no esta mal, es incomprobable. La mitad comprobable la zanja una comparacion de ascendencia por maquina, no la prosa. | Hecho verificado en el checker de backlog | `integrar` |
| D5 | **Una copia generada avisa en su propia primera linea que es generada y donde esta su fuente.** | Hecho verificado en el indice de controles | `ya presente` |
| D6 | **Un chequeo de sincronizacion debe cubrir la forma de deriva que se lee perfecta.** Tres formas: una copia editada a mano, una copia ausente, y una copia que sobrevive despues de que su fuente fue retirada. La tercera es la peor: sin contradiccion visible, se lee correcta, y afirma algo que el equipo dejo de sostener. | Hecho verificado en el indice de controles | `integrar` |
| D7 | **Un indice grande se greppea, nunca se lee entero, y el rulebook declara su costo en tokens.** | Hecho verificado en el rulebook externo | `inspirar` |
| D8 | **Nunca crear un archivo cuyo unico trabajo sea apuntar a documentos que viven en otro lado.** Un cartel parece navegacion y es en realidad un segundo indice que nadie actualiza. El suyo sobrevivio a varias mudanzas describiendo una disposicion vieja. Mover los documentos y borrar el cartel. | Hecho verificado en los peligros de documentacion | `integrar` |
| D9 | **Una mudanza rompe enlaces en las dos direcciones, y la etiqueta visible es la tercera falla.** Los enlaces relativos del propio archivo movido se rompen aunque el archivo nunca se edito. Cada enlace se resuelve contra su propia fuente y se recalcula, nunca se sustituye texto. Un enlace correcto bajo una etiqueta que miente es peor que uno roto, porque nada lo reporta. | Hecho verificado en los peligros de documentacion | `integrar` |
| D10 | **El registro de decisiones es una estructura de maquina, no prosa.** Registrar una decision nueva es agregar una fila a un diccionario dentro del checker. | Hecho verificado en la habilidad de documentacion | `integrar` |
| D11 | **Barrer de nuevo despues de construir, no solo despues de investigar.** Cuando se construye algo que los documentos daban por bloqueado, todo documento que declaraba el estado viejo quedo mal. Anteponer un aviso de superado no alcanza: la fila del indice repetia el plan viejo en la misma celda que el aviso. | Hecho verificado en los peligros de documentacion | `integrar` |
| D12 | **Una duplicacion deliberada se marca como deliberada para que una limpieza futura no la borre como deriva.** Exactamente una regla se duplica a proposito en los archivos puntero de runtime, porque un agente puede actuar sobre ella antes de leer el rulebook. La excepcion se nombra a si misma y declara su limite. | Hecho verificado en los peligros de documentacion | `integrar` |
| D13 | **Un conteo que aparece en mas de un documento se greppea y se actualiza en todos lados, en los dos idiomas, en el mismo cambio.** El mapa nombra los conteos concretos que ya mordieron. | Hecho verificado en el mapa de documentacion | `ya presente` |
| D14 | **"Sin cambio de texto" es un resultado valido y explicito de una revision de congruencia.** La tabla de gates declara cuando la revision es obligatoria y permite concluir que las paginas siguen siendo exactas. | Hecho verificado en la habilidad de documentacion | `integrar` |
| D15 | **El mapa de documentos guarda solo anclas que no se mueven, y dice por que cada una es segura de fijar.** Todo lo demas se resuelve por el indice canonico. Una ruta escrita en un mapa es una cosa mas que se pone obsoleta, y un mapa que apunta a un nombre viejo es peor que no tener mapa. | Hecho verificado en el mapa de documentacion | `integrar` |

## Grupo E: ciclo de vida de la sesion

| # | Patron | Clase de evidencia | Recomendacion |
|---|---|---|---|
| E1 | **Las rutas sensibles nunca se preparan solas, diga lo que diga la persona.** Una lista explicita de patrones fuerza una pausa y una pregunta por ruta. | Hecho verificado en el flujo de cierre | `ya presente` |
| E2 | **Los archivos append-only se resuelven por union, nunca a mano.** Dropear una entrada deja registros huerfanos y rompe la importacion para todo el equipo. Ante cualquier duda, reconstruir el indice desde lo que hay en disco. | Hecho verificado en el flujo de cierre | `integrar` |
| E3 | **Solo el directorio de estado en la raiz del repositorio es valido.** Nunca una copia anidada bajo packs, carpetas privadas, temporales o bundles copiados. | Hecho verificado en ambos flujos de sesion | `integrar` |
| E4 | **La identidad de autor se deriva, con cadena de respaldo documentada y aviso cuando es inusable.** Sale de la configuracion de control de versiones, cae por tres fuentes segun sistema operativo, y avisa cuando el valor parece un marcador. Nunca fabricarla: un hilo sin atribucion es honesto y recuperable, una direccion inventada no es ninguna de las dos. | Hecho verificado en el flujo de inicio | `inspirar` |
| E5 | **Tres formas fijas de salida, y el vocabulario se exige.** Bloqueado es para gates de seguridad, fallado es solo para errores reales de comando o herramienta, y la palabra "error" se reserva para lo segundo. Nunca ocultar la salida real. | Hecho verificado en ambas habilidades de sesion | `ya presente` |
| E6 | **Un mensaje de exito no es evidencia de que algo se haya guardado.** Su llamada de memoria acepta exactamente dos nombres de parametro; un cuerpo pasado con otro nombre se deserializa vacio, saltea la validacion y aun asi contesta "guardado". La regla es pasarlo explicito, releer el cuerpo guardado y confirmar que no esta vacio antes de continuar. Esa falla ocurrio, se documento, y volvio a ocurrir, porque la documentacion vivia solo en memoria. | Hecho verificado en el flujo de cierre | `integrar` |
| E7 | **El barrido de documentacion corre despues de guardar memoria y antes del commit.** Un chequeo que falla nunca debe costarle a una sesion su memoria. | Hecho verificado en la habilidad de cierre | `integrar` |
| E8 | **El barrido de cierre cierra lo que puede y pregunta solo por lo que no puede.** La deriva con un arreglo obvio se arregla y se informa; la pausa se reserva para un juicio que es de la persona. Un cierre que interroga en cada corrida es un cierre que la gente aprende a apurar, y entonces no atrapa nada. | Hecho verificado en la habilidad de cierre | `integrar` |
| E9 | **Cada ruta que el resumen referencia se valida contra el arbol de trabajo.** Las ausentes se muestran y nunca bloquean. Atrapa un resumen que dice haber creado un archivo que nunca llego al disco. | Hecho verificado en el flujo de cierre | `integrar` |
| E10 | **Un bloqueo antes de la red declara que quedo invisible.** El bloqueo ocurre antes del fetch, asi que las habilidades y comandos nuevos nunca se revisaron, y el banner lo dice en vez de dejar que el lector asuma que el chequeo paso. | Hecho verificado en la habilidad de inicio | `integrar` |
| E11 | **El inicio de sesion lee el rulebook y nada mas.** Los archivos de reglas enrutados nunca se cargan al inicio; se abre uno cuando su gate se dispara durante el trabajo. Eso es lo que mantiene plano el costo de inicio a medida que las reglas crecen. | Hecho verificado en el flujo de inicio | `integrar` |
| E12 | **El ranking de busqueda no es ranking por fecha.** Su busqueda de memoria ordena por relevancia, asi que el registro mas nuevo suele estar ausente de los primeros resultados; cuando un resultado parece implausiblemente viejo, contrastar con una segunda interfaz antes de concluir. | Hecho verificado en el flujo de inicio | `integrar` |
| E13 | **El emparejamiento de hilos se ancla en una linea literal, nunca en "el cuerpo lo menciona".** Una mencion incidental de un companero en el registro de otra persona se cuela por un match de subcadena. | Hecho verificado en el flujo de inicio | `inspirar` |

## Grupo F: disciplina de evidencia y afirmaciones

| # | Patron | Clase de evidencia | Recomendacion |
|---|---|---|---|
| F1 | **Los avisos, las acciones pendientes y las preguntas van en un bloque que sobrevive al escaneo.** Un banner, caja, linea en negrita o lista, nunca dentro de un parrafo. Declarado para las habilidades y para el chat por igual. Una pregunta enterrada en prosa es una pregunta que no existe. | Hecho verificado en los acuerdos de trabajo | `integrar` |
| F2 | **Cada regla se remonta a un incidente medido, y los incidentes viven en un documento aparte.** El archivo de reglas lleva el imperativo; el archivo de evidencia lleva cuanto costo cada una, con fechas y numeros, y el archivo de reglas ordena leerlo antes de discutir una regla. Cada habilidad repite la division: la habilidad declara el imperativo, un archivo de peligros guarda la falla detras, para leerse antes de debilitar una regla. | Hecho verificado en los acuerdos de trabajo y los archivos de peligros | `integrar` |
| F3 | **Una medicion no es una conclusion. Revisar tambien la causa.** | Hecho verificado en los acuerdos de trabajo | `integrar` |
| F4 | **Entregar el alcance junto al numero.** Nombrar el grano y el filtro, o una medicion correcta de una cosa se vuelve una afirmacion falsa sobre todo. | Hecho verificado en los acuerdos de trabajo | `integrar` |
| F5 | **Una herramienta puede estar equivocada.** Cuando una contradice una creencia, medir con una segunda antes de creerle a cualquiera. | Hecho verificado en los acuerdos de trabajo | `integrar` |
| F6 | **Un documento se verifica contra la maquina, nunca contra si mismo. N copias que coinciden no prueban nada.** Las reglas de despliegue lo reformulan como consecuencia dura: seis copias de un comando en un mismo documento de operaciones coincidian perfecto y a las seis les faltaba un volumen. Que las copias coincidan entre si no es evidencia. | Hecho verificado en los acuerdos de trabajo y la habilidad de despliegue | `integrar` |
| F7 | **Un hallazgo de la forma "X e Y no se superponen" debe nombrar que lo dejaria de ser cierto.** Son los hallazgos peligrosos: no llevan verbo que ordene actuar, otorgan permiso, y el lector los toma como permanentes. Uno seguia otorgando permiso despues de que un cambio de datos lo revirtiera, a traves de tres pasadas de revision. | Hecho verificado en los peligros de documentacion | `integrar` |
| F8 | **Poner una regla donde de verdad se lee.** Una regla archivada solo detras de un gate que nunca se dispara nunca corre. | Hecho verificado en los acuerdos de trabajo | `integrar` |
| F9 | **Una guia para retomar trabajo ya terminado es el peor documento obsoleto que existe, porque esta escrita para obedecerse.** Retirarla el dia que el trabajo aterriza. | Hecho verificado en los acuerdos de trabajo | `integrar` |
| F10 | **La taxonomia de estados separa "el chequeo fallo" de "el chequeo no pudo correr".** Cuatro estados: verificado, parcial, fallado y no verificable, donde las fallas de recoleccion, consulta y autenticacion son no verificable con bloqueo nombrado y nunca fallado. Una casilla marcada significa verificado y nada mas. | Hecho verificado en el flujo de auditoria | `integrar` |
| F11 | **Un bundle puede resumir conteos pero nunca lleva un estado global unico.** Cada conclusion declara el suyo. | Hecho verificado en el flujo de auditoria | `integrar` |
| F12 | **Publicar evidencia pasa por un gate de sanitizacion: un escaneo ejecutable de secretos e identidad, luego revision manual, y si ninguno puede probar que es seguro la evidencia no se publica.** El escaneo nombra lo que busca: tokens, asignaciones de clave y secreto, cadenas de conexion, identificadores, rutas de directorio personal y direcciones de correo. | Hecho verificado en el flujo de auditoria | `integrar` |
| F13 | **Cada conclusion en vivo registra como reproducirla.** Comando sanitizado, parametros, huella del objetivo, hora de ejecucion, version de la herramienta, codigo de estado o salida, hash del resultado, fuente y fecha de recuperacion, y la revision de fuente cuando se usa evidencia local. La evidencia cacheada o de una sesion previa nunca es actual. | Hecho verificado en el flujo de auditoria | `inspirar` |
| F14 | **El presupuesto del rulebook declara su metodo de medicion, un objetivo, un techo, y que pasar el objetivo no es un hallazgo.** Medido como conteo de palabras por una constante, nunca a ojo, porque una estimacion por caracteres reporto un numero 17 por ciento alto el dia que importaba. Crecer mas alla del techo mueve una seccion abajo; el techo nunca sube. Su propio registro: la nota que explicaba el presupuesto es lo que rompio el presupuesto. | Hecho verificado en la regla de mantenimiento del rulebook | `ya presente` |
| F15 | **La linea de "ultima actualizacion" es una fecha y una pista, nunca un segundo inventario.** Habia crecido hasta ser un inventario de controles ya escrito en dos documentos canonicos, que es la falla de estado duplicado que su propio checker existe para atrapar. | Hecho verificado en la regla de mantenimiento del rulebook | `integrar` |
| F16 | **Nombrar cual de varios entornos produjo un numero.** Tres entornos que nunca son intercambiables, y todo informe dice de cual vino. | Hecho verificado en los acuerdos de trabajo | `inspirar` |
| F17 | **Un resolvedor, no un nombre, decide la identidad del proyecto; lo que aparezca bajo otro nombre es un alias obsoleto, ignorado con una linea que lo diga.** | Hecho verificado en la regla de memoria | `integrar` |
| F18 | **Una tabla explicita declara que va donde**: una decision y su razon a memoria, lo que se entrego al changelog, el estado documental al indice, como se trabaja aca al rulebook. | Hecho verificado en la regla de memoria | `ya presente` |

## Grupo G: autoridad e identidad de artefactos

Incide directamente sobre la activa `RH-W-0054`.

| # | Patron | Clase de evidencia | Recomendacion |
|---|---|---|---|
| G1 | **La tupla de autoridad viaja como argumentos explicitos del comando: sin archivo de estado, sin variable heredada.** Declarado como la razon y no como el mecanismo: la autoridad para cambiar la maquina tiene que ser visible en el comando que la cambia. Cinco campos, cada uno nombrado, cada uno con su rol. | Hecho verificado en la habilidad de despliegue | `integrar` |
| G2 | **La identidad viene de los bytes, no de un nombre.** Un hash de contenido de un manifiesto normalizado es la identidad; la revision de origen es un ancla de ascendencia y explicitamente no una identidad. Un arbol sucio puede producir un candidato precisamente porque la identidad no depende de que el arbol este limpio. | Hecho verificado en la habilidad de despliegue | `integrar` |
| G3 | **Un nombre que coincide nunca otorga autoridad.** Inspeccionar el artefacto por su identificador inmutable y exigir que declare su propio digest y procedencia. Una etiqueta que coincide no es autoridad. | Hecho verificado en la habilidad de despliegue | `integrar` |
| G4 | **Listar lo que una operacion va a publicar antes de publicarlo, y decirlo en voz alta.** Una operacion entrega todo lo commiteado, no el cambio que el operador tiene en la cabeza. Hacerlo despues solo informa lo que ya paso. | Hecho verificado en la habilidad de despliegue | `integrar` |
| G5 | **Sellar el artefacto con la revision desde la que se construyo, y leer ese sello para contestar la regla anterior la proxima vez.** Sin el, la unica forma de saber que corre es adivinar por fechas, que ya dio una respuesta equivocada. | Hecho verificado en la habilidad de despliegue | `inspirar` |
| G6 | **Probar que el cambio llego: una cadena nueva de este cambio esta ausente antes y presente despues, dentro del artefacto que corre.** Un build verde no prueba que la fuente nueva llego. Elegir una sonda que sobreviva a la transformacion, y cuando una sonda vuelve vacia, descartar la transformacion antes de concluir que el codigo no viajo. | Hecho verificado en la habilidad de despliegue | `inspirar` |
| G7 | **Entregar agrega archivos, nunca los borra.** Listar lo que existe en el destino y no en la fuente antes de construir, y mover los sobrantes a un lado. Un archivo renombrado deja un gemelo obsoleto que rompe el build o gana la resolucion en silencio sobre el archivo que lo reemplazo. | Hecho verificado en la habilidad de despliegue | `inspirar` |
| G8 | **Un bundle generado nunca se sobrescribe.** Un nombre que colisiona toma un sufijo o devuelve bloqueado. | Hecho verificado en el flujo de auditoria | `integrar` |
| G9 | **Ordenar antes de escribir un manifiesto.** Medido: los mismos registros sin cambios produjeron un diff de 544 lineas sin ordenar y de 153 ordenado, y solo el ordenado muestra que se movio. Un respaldo cuyo diff es ilegible no esta haciendo su trabajo. | Hecho verificado en los peligros de respaldo | `integrar` |
| G10 | **Un artefacto generado lleva una instruccion de restauracion, no solo una copia.** Cada archivo respaldado se clasifica como autorado o generado, por lo que los generadores escriben de verdad y nunca por el nombre del archivo, porque restaurar un archivo generado desde un respaldo reintroduce exactamente la deriva que su generador existe para matar. | Hecho verificado en la habilidad y los peligros de respaldo | `integrar` |
| G11 | **Nunca normalizar un artefacto cuyos bytes son el contrato.** Las reglas de normalizacion aplican solo a payloads estructurados; reformatear un prompt cambia lo que el modelo recibe, que es lo unico que un respaldo nunca debe hacer. | Hecho verificado en la habilidad de respaldo | `inspirar` |
| G12 | **Informar las dos mitades por separado.** Un total combinado esconde cual fuente se alcanzo de verdad, y las dos fallan de forma independiente. | Hecho verificado en los peligros de respaldo | `integrar` |
| G13 | **Una corrida verde puede igual dejar el repositorio mal, y el defecto es la afirmacion, no la copia.** Una exportacion limpia con cero fallas se reporto como haber vuelto a sincronizar el repositorio mientras una copia mas vieja seguia declarandose fuente de verdad con contenido obsoleto. Lo nunca aceptable es un archivo cuyo encabezado le dice al lector que es autoritativo mientras una exportacion mas fresca lo desmiente. | Hecho verificado en los peligros de respaldo | `integrar` |

## Lo que esta revision explicitamente no encontro

Se declara para que la ausencia no se confunda despues con evidencia. Cada afirmacion esta acotada a la superficie nombrada en **Que se leyo**.

- **Ningun modelo de empaquetado o distribucion para tomar.** El harness externo se instala copiando un directorio a mano: sin renderer, sin manifiesto de salidas generadas, sin plan ligado por digest, sin ruta de actualizacion, y la referencia de una de sus habilidades declara abiertamente que no hay fuente canonica designada entre sus tres copias de pack. Este repositorio va adelante en ese eje y nada de aca deberia cambiarse por eso.
- **Ningun vocabulario de clases de evidencia que abarque todo el harness.** Su habilidad de auditoria tiene una taxonomia de cuatro estados muy filosa, pero esta acotada a esa habilidad; no hay equivalente de una clasificacion a nivel repositorio de hecho verificado, inferencia respaldada, desconocido y decision humana.
- **Ningun modelo de aprobacion o consentimiento para operaciones del repositorio.** El trabajo ocurre directo sobre la rama principal con commits al cierre de sesion. Es otra postura de riesgo y no es portable aca.
- **Ningun contrato de documentacion bilingue.** Los documentos mezclan idiomas por archivo en vez de mantener pares localizados, asi que no hay nada que aprender para el contrato de localizacion que posee este repositorio.
- **Ningun mecanismo para que un consumidor agregue un control que el harness vaya a correr.** El corredor por descubrimiento es un control del repositorio, y el pack que se distribuye explicitamente no puede llevarlo.

## Resumen de reconciliacion

Noventa y cuatro hallazgos.

| Recomendacion | Cantidad |
|---|---|
| `integrar` | 64 |
| `inspirar` | 14 |
| `ya presente` | 16 |
| `rechazar` | 0 |
| `fuera de alcance` | 0 |

`integrar`: A1, A2, A3, A6, A8, A10, A11, A12, B1, B2, B7, B8, B9, C1, C2, C3, C5, C7, C8, C10, C11, C12, D1, D4, D6, D8, D9, D10, D11, D12, D14, D15, E2, E3, E6, E7, E8, E9, E10, E11, E12, F1, F2, F3, F4, F5, F6, F7, F8, F9, F10, F11, F12, F15, F17, G1, G2, G3, G4, G8, G9, G10, G12, G13.

`inspirar`: A4, A5, B6, B11, C9, D7, E4, E13, F13, F16, G5, G6, G7, G11.

`ya presente`: A7, A9, B3, B4, B5, B10, C4, C6, D2, D3, D5, D13, E1, E5, F14, F18.

Los conteos y la membresia se derivan de las tablas de arriba parseandolas, no a mano. Un borrador anterior de este documento afirmo un total que las tablas no sostenian; ahora el conteo se verifica contra las filas.

**Decision humana requerida.** Esta tabla es una recomendacion, no una adopcion. Agrupamientos propuestos, cada uno su propia unidad de trabajo:

1. **Endurecimiento de transporte**: C1, C2, C3, C5, C7, C8, C10, C11, C12. La mas urgente, porque el gate de sesion generado aca promete un banner de remediacion al que no puede llegar cuando hay una llave protegida por passphrase.
2. **Proyeccion de comandos**: B1, B2, B7, B8, B9. Se pliega sobre la ya abierta `RH-W-0057` y la corrobora.
3. **Disciplina de controles**: A1, A2, A3, A6, A8, A10, A11, A12.
4. **Invariantes de documentacion**: D1, D4, D6, D8, D9, D10, D11, D12, D14, D15.
5. **Disciplina de evidencia y afirmaciones**: F1 a F12, F15, F17.
6. **Autoridad e identidad de artefactos**: G1, G2, G3, G4, G8, G9, G10, G12, G13. G1 a G3 inciden sobre la activa `RH-W-0054` y conviene leerlas antes de su Tarea 2.
7. **Ciclo de vida de la sesion**: E2, E3, E6, E7, E8, E9, E10, E11, E12.

Este documento no autoriza ninguna adopcion, implementacion ni cambio de comportamiento generado.
