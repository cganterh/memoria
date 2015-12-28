Resultados
==========

Estado actual del proyecto
--------------------------

En septiembre del 2014 se comenzó con la programación del
proyecto. Hoy, habiendo pasado 16 meses desde el inicio, la
arquitectura general de la aplicación está lista y se han
programado 13 módulos. Faltando aún un módulo para lograr el
objetivo de *producto mínimo viable*.

En el \cref{t_lineas} se muestra la cantidad de lineas de la
aplicación, por tipo (columnas) y por lenguaje de
programación (filas). La columna *Blancas* corresponde a las
líneas dejadas en blanco, la columna *Comentarios*
corresponde a la cantidad de líneas dedicadas a documentar
el proyecto y la columna *Código* corresponde a las líneas
que tienen una función ejecutable. La última fila de la
tabla, corresponde a la suma de todas las anteriores. De
esta forma se puede concluir que, a día de hoy, el proyecto
tiene un total de 4603 lineas de código efectivas.

Lenguaje      Blancas  Comentarios  Código
------------ -------- ------------ -------
Python           1021         1048    2826
make              137            7     474
CoffeeScript      129           24     465
SASS               84           15     443
HTML               45            0     395
Total            1416         1094    4603

Table: Cantidad de lineas del proyecto.\label{t_lineas}

El código existente actualmente provee la suficiente
infraestructura de forma que resulte rápido programar nuevos
módulos.

El proyecto desde sus inicios se ha trabajado como un
proyecto de código abierto. Las actualizaciones se publican
regularmente en la \gls{web} \url{http://github.com}, que es
utilizada como repositorio central. Además, se cuenta con
dos colaboradores. Siendo uno de ellos pagado con los fondos
que se adjudicaron a este proyecto. (más detalles en la
sección [Concurso de Investigación OEA]).

Una versión estable del proyecto es mantenida en
\url{http://aa.cganterh.net} junto con la documentación del
proyecto en \url{http://www.aa.cganterh.net/doc}. Dicha
documentación, aún incompleta, es generada automáticamente
desde los comentarios del proyecto y está completamente
escrita en inglés.

[Concurso de Investigación OEA]: #concurso-de-investigación-oea

Pruebas realizadas
------------------

Durante el desarrollo del proyecto se han hecho pruebas de
manera continua, con el objetivo de encontrar errores de
manera temprana y verificar la estabilidad y rendimiento de
la aplicación. En las siguientes sub-secciones de detallan
las pruebas más importantes que se han llevado a cabo.

###Versión estable de producción

Durante la mayor parte del desarrollo se ha mantenido en
continuo funcionamiento y de manera pública una versión
estable del proyecto. Esto permite mostrar la aplicación,
verificar que no se produzcan errores durante períodos de
ejecución largos y en general probar la aplicación
aleatoriamente en diferentes ubicaciones, con variadas
calidades de conexión a la red.

Utilizando este método se han logrado identificar una
considerable cantidad de errores que hoy ya se encuentran
resueltos.

###Prueba de rendimiento en una clase real

Se ha probado la aplicación durante una clase con
aproximadamente 15 personas. Durante esta prueba se
validaron los mecanismos de acceso y se le pidió a los
alumnos que generaran una considerable cantidad de tráfico,
utilizando el módulo llamado *No entiendo*. Este módulo
permite notificar al profesor cuándo un alumno no entiende.
Presionando repetidamente este botón se puede generar una
cantidad de tráfico.

Esta es más bien una prueba cualitativa, que sirvió para
determinar que alrededor de 15 personas, generando tráfico
constantemente, no son capaces de producir un colapso de las
comunicaciones de la aplicación. Además, el servidor
utilizado en esta prueba estaba localizado en EEUU. Por lo
que se puede asegurar que un servidor local tendría mucho
mejor rendimiento.

###Prueba de RTT en una conexión de mala calidad

Se han capturado datos del tiempo en que un mensaje \gls{ws}
puede ir y volver a un servidor localizado en EEUU, durante
un paseo por los lugares con peor señal móvil de la UTFSM.
El round-trip time fue medido utilizando los mensajes ping y
pong del protocolo \gls{ws}.

Se capturaron $2497$ mediciones, con una media de $351[ms]$,
una desviación estándar de $1583[ms]$, un mínimo de $12[ms]$
y un máximo de $66262[ms]$. Con estos datos se realizó el
histograma de la \cref{f_hist}.

![Histograma del los RTT capturados en un paseo por la
  UTFSM.\label{f_hist}](src/4-resultados/fig/hist.pdf)

Con estos datos se pudo programar un sistema de monitoreo
continuo de la calidad de conexión. Este sistema permite
garantizar la estabilidad de la aplicación y la consistencia
de los datos manejados por ella. Además, se puede garantizar
que la aplicación no perderá conexión el 99,9% del tiempo en
condiciones tan extremas como las simuladas durante las
mediciones.
