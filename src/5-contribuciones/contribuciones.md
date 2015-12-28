Contribuciones del Presente Trabajo
===================================

Concurso de Investigación OEA
-----------------------------

Con este proyecto se ha postulado al *Concurso de Proyectos
de Investigación Educativa en Ingeniería y Ciencias, Olivier
Espinosa Aldunate*. Ganando el concurso en su versión 2015.

Los fondos adjudicados por este concurso han permitido
contratar a un programador adicional que podrá continuar con
el desarrollo del proyecto durante el año 2016.

Además, se destaca la colaboración con el departamento de
física. El cual ha apoyado durante el desarrollo de la
aplicación ofreciendo retroalimentación y colaborará
permitiendo realizar pruebas de la aplicación en clases
reales, realizando mediciones del impacto de estas
tecnologías en los alumnos.

Aportes a proyectos externos
----------------------------

Durante el desarrollo de la aplicación se han encontrado dos
pequeños errores en proyectos de código abierto externos.
Para poder continuar con el desarrollando, se ofrecieron
soluciones a ambos problemas, siendo estas aceptadas por las
respectivas comunidades de desarrolladores.

Los proyectos en los que se encontraron estos errores fueron
Bourbon y httplib2. Ambos son proyectos grandes, de bastante
impacto y con comunidades internacionales de
desarrolladores. En el caso de httplib2, esta es mantenida
por trabajadores de Google y es dependencia de oauth2client,
la biblioteca para la autenticación de usuarios de Google en
Python.

Módulos de software reutilizables
---------------------------------

Como ya se ha mencionado en secciones anteriores, el código
de TornadoBoiler y TornadoBoxes (bases para la aplicación
desarrollada en este trabajo) es completamente reutilizable.
Pero además, existen módulos dentro de la aplicación que
podrían ser empaquetados y distribuidos de manera
independiente.

Este es el caso, por ejemplo, del módulo que implementa los
adaptadores PubSub. PubSub es un patrón de diseño común que,
si bien tiene algunas implementaciones en Python, ninguna de
ellas funciona con el bucle de eventos que provee Tornado.
Como Tornado es una librería que puede manejar conexiones a
múltiples recursos de red, el módulo PubSub sería un buen
candidato para implementar este patrón de diseño en otros
proyectos basados en Tornado.

Otro ejemplo, que requeriría un poco mas de trabajo para ser
empaquetado es la clase de autenticación con Google. Las
clases que provee Tornado para autenticar usuarios con
Google, están desactualizadas. Incluso los mismos
desarrolladores de Tornado admiten en foros que los
mecanismos de autenticación con Google no están siendo
mantenidos y no han sido probados extensivamente.
Es por eso que en este trabajo se han tenido que implementar
estos mecanismos desde cero.

Liberación del código
---------------------

Como ya ha sido mencionado, este es un proyecto de código
abierto. Lo cual significa que el código está disponible
para la comunidad sin ningún tipo de restricción de copia o
modificación. Esto suele ser beneficioso para este tipo de
proyectos, ya que aumenta la probabilidad de que el código
sea mantenido y de que se forme una comunidad alrededor del
proyecto. Finalmente, beneficiando a la comunidad.