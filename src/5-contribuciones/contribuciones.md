Contribuciones del Presente Trabajo
===================================

Proyecto de Investigación OEA Adjudicado
----------------------------------------

Con este proyecto se ha postulado al *Concurso de Proyectos
de Investigación Educativa en Ingeniería y Ciencias, Olivier
Espinosa Aldunate*. Ganando el concurso en su versión 2015.

Los fondos adjudicados por este concurso han permitido
contratar a un programador adicional que podrá continuar con
el desarrollo del proyecto durante el año 2016.

Además, se agradece la colaboración con el departamento de
física. El cual ha brindado su apoyo durante el desarrollo
de la aplicación ofreciendo retroalimentación. Y colaborará
permitiendo realizar pruebas de la aplicación en clases
reales, realizando mediciones del impacto de estas
tecnologías en los alumnos.

Aportes a Proyectos Externos
----------------------------

Bourbon y httplib2 son dos proyectos de código abierto
grandes, de bastante impacto y con comunidades
internacionales de desarrolladores. En el caso de httplib2,
este es mantenido por trabajadores de Google y es
dependencia de oauth2client, la biblioteca para la
autenticación de usuarios de Google en Python. Durante el
desarrollo de la aplicación se han encontrado pequeños
errores en estos proyectos externos. Para poder continuar
con el desarrollo, se ofrecieron soluciones a ambos
problemas, siendo estas aceptadas por las respectivas
comunidades.

Módulos de Software Reutilizables
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

Otro ejemplo, que requeriría un poco más de trabajo para ser
empaquetado es la clase de autenticación con Google. Las
clases que provee Tornado para autenticar usuarios con
Google, están desactualizadas. Incluso los mismos
desarrolladores de Tornado admiten en foros que los
mecanismos de autenticación con Google no están siendo
mantenidos y no han sido probados extensivamente.
Es por eso que en este trabajo se ha tenido que implementar
estos mecanismos desde cero.

Liberación del Código
---------------------

Como ya ha sido mencionado este es un proyecto de código
abierto, lo cual significa que el código está disponible
para la comunidad sin ningún tipo de restricción de copia o
modificación. Esto suele ser beneficioso para este tipo de
proyectos, ya que aumenta la probabilidad de que el código
sea mantenido y de que se forme una comunidad alrededor del
proyecto.

Para este proyecto se ha escogido la licencia de software
libre [*GNU Affero General Public
License*](http://www.gnu.org/licenses/agpl-3.0.html). Esta
es un derivado de la licencia *GNU General Public
License*. Ambas licencias permiten el uso comercial, la
distribución y la modificación del software. Además,
requieren que la distribución de trabajos derivados se haga
con la misma licencia que el trabajo original. Cualquier
copia distribuida bajo estas licencias debe incluir el
código fuente del software.

La diferencia entre *GNU Affero General Public License* y
*GNU General Public License* radica en que, con la segunda,
una persona puede hacer una modificación del software y
usarla de manera privada sin liberar el código. Esto
normalmente no es un problema. Sin embargo, cuando el
software en cuestión está hecho para ejecutarse en un
\gls{servidor}, la persona que modificó el software podría
sacar provecho de sus modificaciones sin publicarlas. En
este caso el software no está siendo distribuido, pero la
persona se beneficia de su modificación sin contribuir a la
comunidad. Debido a este detalle la licencia *GNU Affero
General Public License* incluye una condición que especifica
que, cuando un software publicado bajo esta licencia se
ejecuta en un \gls{servidor} para prestar servicios, se debe
proveer un mecanismo para que los usuarios descarguen el
código fuente.
