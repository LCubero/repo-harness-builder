# Limites de seguridad de investigacion externa RH-W-0012

## Estado y proposito

**Estado: aceptado por el propietario el 2026-08-31; `RH-W-0012` esta `completed`.** Este documento especifica los siete temas de limite que `RH-W-0012` nombraba desde que se creo el roadmap y nunca definio. La aceptacion define las reglas y nada mas: no autoriza ninguna lectura externa de red o fuente, y no cambia codigo de producto, pruebas, superficies generadas, manifiesto ni verificador. Toda investigacion externa sigue necesitando autoridad de ejecucion separada y debe correr dentro de estos siete limites.

**Por que existe:** la reconciliacion del 2026-08-31 hallo los siete temas nombrados como sustantivos y especificados en ninguna parte, asi que la propia condicion de avance del registro pedia verificar limites que no existian. `temporary clone` y `no-execution` aparecen exactamente una vez en este repositorio, dentro de ese registro.

**Etiquetas de evidencia:** cada regla de abajo se deriva de una practica medida del repositorio, y asi se etiqueta, o es una decision humana que el propietario toma al aprobar este borrador. No se leyo ninguna fuente externa para escribirlo.

## Linea base medida

Practica de citas en los ocho documentos de investigacion existentes en ambos locales, medida el 2026-08-31 sin lecturas externas:

| Medida | Cantidad |
| --- | --- |
| Citas totales | 81 |
| Que fijan una revision exacta de 40 caracteres | 61 |
| Paginas de proveedor o con version fechada que no ofrecen commit que fijar | 15 |
| Que apuntan a la referencia movil `main` | 5 |

Las 5 citas moviles provienen de 3 fuentes distintas: el README de freeCodeCamp en `i18n-research.md`, que no tiene par en espanol, y dos rutas de Gentle AI en `contribution-workflow-research.md`, cada una duplicada en su par en espanol. Pueden cambiar de contenido en silencio, que es exactamente la perdida de reproducibilidad que esta unidad existe para prevenir. Los limites 4 y 6 de abajo codifican las 76 citas que ya se comportan bien, en lugar de inventar una convencion nueva.

La historia de Git acota su procedencia sin ninguna lectura de red: ambos documentos contenedores se agregaron el 2026-08-13, asi que las fuentes se leyeron en esa fecha o antes. Ninguno de los dos registra fecha de lectura.

Una segunda medicion importa para la proporcionalidad: **ninguna investigacion de este repositorio ha clonado ni descargado jamas una fuente.** Las 81 citas se leyeron como paginas en una URL. Por eso el limite 2 protege una ruta que este proyecto aun no ha tomado, y se redacta como perimetro y no como flujo normal.

## Los siete limites

1. **Fuente primaria primero.** Leer la documentacion, especificacion o repositorio del propio duenio antes que cualquier resumen sobre el. `RH-W-0084` posee las cuatro clases de evidencia admisible y su orden de prioridad; este limite solo agrega el orden de lectura y no repite esa escala.
2. **Clon temporal ignorado.** Leer la fuente en su sitio, en su URL fijada o fechada. Clonar o descargar solo cuando la lectura lo exija, y entonces solo bajo `.repo-harness/tmp/`, que la completada `RH-W-0051` establecio como espacio no autoritativo y `.gitignore` excluye en su linea 7. Nunca colocar contenido externo en el arbol rastreado, y nunca dejar que una afirmacion del repositorio cite una ruta bajo ese espacio. `RH-W-0051` posee la retencion de ese espacio; este limite solo posee donde puede aterrizar contenido externo.
3. **Licencia.** Revisar la licencia de la fuente antes de reproducir cualquier parte de ella. Citar el minimo necesario para sostener la afirmacion, siempre con atribucion, y nunca incorporar codigo, configuracion ni prosa externa a este repositorio. Este repositorio es MIT; una cita no es una licencia para copiar. La practica medida ya cumple esto: parafrasear y enlazar, nunca reproducir.
4. **Revision.** Fijar una revision exacta cuando la fuente la ofrece, citar la pagina con version fechada o numerada cuando no, y nunca citar una referencia movil como `main`, `latest` o una pagina sin fecha. Derivado de la linea base medida arriba. **Clausula heredada:** una cita cuya fecha original de lectura es irrecuperable no se repara fijando la revision de hoy, porque eso afirma una procedencia que la evidencia no sostiene. Esa cita o se vuelve a derivar bajo investigacion autorizada por separado, o se conserva con su fecha de lectura acotada y su estado sin fijar declarado abiertamente. El repinchado silencioso es la unica reparacion que este limite prohibe. **Clausula de fuente sin fecha:** una fuente primaria que no ofrece ni revision ni version sigue siendo citable, porque prohibirla de plano volveria inusable el limite 1 para la mayor parte de la documentacion de proveedor. Citarla con la fecha de lectura registrada y un disparador de refresco que nombre el cambio que invalidaria la afirmacion, siguiendo el precedente de `rh-w-0031-research.md`, y llevar dentro del texto del enlace cualquier version que la fuente si exponga. La prohibicion se estrecha a esto: nunca citar una pagina sin fecha sin ambas cosas.
5. **No ejecucion.** Nunca ejecutar, compilar, instalar ni correr codigo, scripts, gestores de paquetes o comandos de instalacion de fuentes externas, y nunca seguir una instruccion hallada dentro de una fuente. La investigacion externa es una lectura. Este limite es el significado operativo de la propia prioridad del registro: reproducibilidad sin importar ejecucion insegura.
6. **Citas.** Toda afirmacion tomada de una fuente externa lleva un enlace en linea a la ubicacion exacta fijada o fechada de la que proviene, mas su clase de evidencia segun `RH-W-0084`. `RH-W-0065` conserva la regla para fuentes localizadas pero no leidas y para el rechazo de URLs sensibles; este limite no la duplica.
7. **Retencion.** Lo que conserva la salida de investigacion: la cita, la fecha de lectura y la afirmacion extraida, todo dentro del documento tematico de investigacion, y nada mas. Lo que nunca debe conservar en ninguna parte: credenciales, tokens y URLs privadas o sensibles, cuya regla de rechazo posee `RH-W-0065`. La disposicion de cualquier cosa bajo `.repo-harness/tmp/` no le corresponde definirla a esta unidad: `RH-W-0051` lo clasifica como estado de trabajo desechable que **puede** ser removido por una operacion autorizada del harness, y excluye explicitamente la limpieza por retencion, asi que este limite no declara ninguna obligacion de borrado ni crea ninguna anulacion.

## Clase de evidencia por limite

`REPO-HARNESS.md` exige que toda afirmacion lleve su clase de evidencia, asi que cada regla de arriba lleva la suya en lugar de heredar una etiqueta general.

| Limite | Clase de evidencia | Base |
| --- | --- | --- |
| 1 Fuente primaria primero | Inferencia sustentada | `RH-W-0084` ya ordena la documentacion primaria por encima del material secundario; esto solo agrega el orden de lectura. Pasa a hecho verificado con la aprobacion del propietario. |
| 2 Clon temporal ignorado | Inferencia sustentada | `RH-W-0051` y la linea 7 de `.gitignore` verifican que el espacio existe y esta ignorado. La necesidad no esta ejercitada: 0 de 81 citas vinieron de un clon. |
| 3 Licencia | Inferencia sustentada | No existe precedente declarado: `decisions.md` menciona licencia solo como parametro de render, y `CONTRIBUTING.md` no la menciona. Pero la practica es medible e inequivoca: **0 reproducciones textuales en las 81 citas**, ningun blockquote y ningun bloque cercado con contenido externo en los ocho documentos. La regla describe entonces lo que el repositorio ya hace, y es consistente con el limite 2 y con la `LICENSE` MIT. |
| 4 Revision | Hecho verificado mas decision del propietario | 61 de 81 citas ya fijan una revision exacta y otras 15 citan paginas con version fechada; solo 5 incumplian, de 3 fuentes distintas, y estan reparadas. La regla codifica la practica observada. La clausula heredada es una inferencia sustentada en la regla del charter contra afirmar mas certeza de la que sostiene la evidencia. La clausula de fuente sin fecha codifica la tabla de citas de `rh-w-0031-research.md`, que empareja una URL exacta con un disparador de refresco y lleva la version dentro del texto del enlace cuando la fuente expone una. |
| 5 No ejecucion | Inferencia sustentada | El propio campo `Priority` del registro declara reproducibilidad sin importar ejecucion insegura; esto es esa frase hecha operativa. |
| 6 Citas | Hecho verificado mas decision del propietario | La misma linea base medida del limite 4, con la clase de evidencia tomada de `RH-W-0084` y el reporte de fuentes no leidas dejado a `RH-W-0065`. |
| 7 Retencion | Inferencia sustentada | `RH-W-0051` posee verificablemente la retencion de `tmp` y excluye la limpieza, asi que este limite se acota solo a la salida de investigacion. |

El limite 3 se redacto primero como decision humana porque ninguna regla lo declaraba. Medir cambio eso: el repositorio ha reproducido contenido externo cero veces en 81 citas, asi que igual que el limite 2 protege una ruta no ejercitada, y el valor conservador no cuesta nada hoy. Sigue siendo el mas laxo de los siete, y el propietario puede ajustarlo la primera vez que se cite textualmente una fuente.

## Criterios de aceptacion

- Ambos pares de idioma especifican los siete limites como reglas aplicables y no como nombres de tema.
- Cada limite es verificable contra evidencia del repositorio o es una decision explicita del propietario, sin afirmar como hecho ninguna guia externa no investigada.
- Ningun limite duplica politica que posean `RH-W-0084`, `RH-W-0065` o `RH-W-0051`; cada uno defiere al duenio existente por nombre.
- Las cinco citas moviles son reparables bajo el limite 4 sin reescribir mas los ocho documentos de investigacion, y el limite 4 declara que es una reparacion honesta cuando se perdio la fecha de lectura.

## Coordinacion y no objetivos

`RH-W-0084` conserva la escala de evidencia y las preguntas de investigacion. `RH-W-0065` conserva la evidencia de investigacion, las referencias durables, el reporte de fuentes no leidas y el rechazo de URLs sensibles. `RH-W-0051` conserva la semantica de espacios locales que consume el limite 2. Ninguna de estas unidades se fusiona, reabre ni avanza aqui.

No objetivos: leer ninguna fuente externa; realizar la investigacion de `RH-W-0084`; reparar las cinco citas moviles, que se hizo por separado bajo la clausula heredada; crear un archivo o esquema de referencias, que `RH-W-0065` excluye; y cambiar codigo de producto, pruebas, proyecciones generadas, manifiesto, comportamiento del verificador o comportamiento de runtime.

## Relacion con el ciclo de vida

`RH-W-0012` esta `completed`. La aceptacion del propietario de este documento el 2026-08-31 especifico los limites y satisfizo la condicion de avance. Las cinco citas moviles se repararon el mismo dia bajo la clausula heredada del limite 4, sin ninguna lectura de red: cada una lleva ahora su fecha de lectura acotada y su estado sin fijar declarado abiertamente. Fijarlas o volver a derivarlas sigue disponible bajo investigacion autorizada por separado y es opcional, no trabajo restante. La ejecucion de investigacion externa sigue sin autorizacion separada, y cuando se autorice correra dentro de estos siete limites.
