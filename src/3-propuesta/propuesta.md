Propuesta
=========

Requerimientos
--------------

###Levantamiento de Requerimientos

La primera parte del trabajo consistió en redactar una
encuesta con el fin de levantar los requerimientos
necesarios para comenzar con el desarrollo de software. Se
realizaron dos tipos de preguntas: de preferencias sobre las
clases y técnicas (uso de diferentes tecnologías). Además,
se agregaron preguntas especiales para profesores y para
alumnos. El público al que estaba orientada la encuesta eran
los usuarios de salas SCALE-UP. Por lo tanto se decidió
encuestar a todos los alumnos y profesores que usaran este
tipo de salas en el período de una semana. Al finalizar la
semana de encuestas se contó la cantidad de personas que
respondieron, resultando ser 609 personas.

También, se realizó una visita a una clase activa de física
con el objetivo de tener un acercamiento a las actividades
que se realizan en ellas.

\pagebreak[4]

###Resultados del Levantamiento

Del análisis de las encuestas y la visita a la clase activa,
se llegó a las siguientes conclusiones:

\pagebreak[3]

1.  La aplicación debe poder mostrar diapositivas y videos.

2.  Los profesores deben poder enviar tareas a los alumnos.

3.  La agilidad del sistema es un atributo muy importante,
    debe desarrollarse con esto en mente.

4.  Es bueno permitir el anonimato de los alumnos.

5.  La aplicación debe permitir a los profesores utilizar de
    mejor manera su tiempo dentro de la clase.

6.  Aunque deba sacrificarse funcionalidad, el software debe
    ser muy estable.

7.  La aplicación debe usar estándares que garanticen el
    correcto funcionamiento en los \glspl{navegador}
    modernos y futura compatibilidad de los
    \glspl{navegador} más atrasados.

8.  El sistema debe ser robusto a las pérdidas de conexión a
    internet.

\pagebreak[2]

Finalmente, se diseñó una serie de módulos mínimos que
cumplieran con las necesidades actuales de las salas
SCALE-UP. Estos módulos serán descritos en detalle en la
sección [Arquitectura de módulos].

Arquitectura del Sistema
------------------------

###Acceso a la Aplicación

Se ha diseñado un sistema que simplifica el acceso a la
aplicación. De forma que los usuarios puedan ingresar
rápidamente sin necesidad de recordar una dirección
\gls{web}. Este sistema consta de códigos \gls{qr} o
\gls{nfc} pegados en las mesas de las salas. Un código
frente a cada asiento. Cada alumno debe escanear el
código correspondiente a su asiento con su teléfono
inteligente. Al hacerlo se iniciará la aplicación. Además,
existe un código en la puerta de la sala, el cual debe ser
utilizado por el profesor para abrir la aplicación y
comenzar la clase.

\pagebreak[0]

En la \cref{f_acceso} se muestra un diagrama que representa
el sistema descrito en el párrafo anterior. El cuadro del
lado izquierdo representa una sala SCALE-UP, con sus mesas
(círculos), los códigos
(\vcenteredinclude{src/3-propuesta/fig/qr.pdf}), los
\glspl{cliente}
(\vcenteredinclude{src/3-propuesta/fig/cliente.pdf}) y su
puerta. Cada \gls{cliente} se conecta inalámbricamente
través de la red con el \gls{servidor}.

![Sistema de acceso a la aplicación.
  \label{f_acceso}](src/3-propuesta/fig/acceso.pdf)

Los códigos almacenan una dirección \gls{web} que permite
iniciar la aplicación indicando el código desde el cual se
ha ingresado. Para lograr esto, la dirección \gls{web}
contenida en cada código tiene un identificador único
compuesto por cinco caracteres alfanuméricos. Desde aquí en
adelante se utilizará la palabra *código* para referirse al
identificador único, a no ser que se indique lo contrario.

\pagebreak[0]

###Arquitectura General

La arquitectura de la aplicación está compuesta por un
conjunto de clases y módulos que atienden diferentes
requerimientos \gls{http} hechos por el \gls{cliente}. En la
\cref{f_arq} se muestra la arquitectura general de la
aplicación. El bloque superior, con etiqueta *Cliente*,
representa al \gls{navegador} utilizado por los usuarios. La
nube, con la etiqueta *Internet*, representa a la red
utilizada para comunicar al \gls{cliente} con el
\gls{servidor}, así como también a servicios de terceros que
puedan encontrarse en esa red. Todos los componentes que se
encuentran por debajo de la nube *Internet* forman parte del
\gls{servidor}.

![Arquitectura general de la aplicación.
  \label{f_arq}](src/3-propuesta/fig/arquitectura.pdf)

La aplicación funciona utilizando módulos[^modulos]. Cada
módulo es una agrupación de clases, pudiendo tener una o más
clases. Existen tres tipos principales de clases que pueden
formar parte de un módulo de la aplicación:

`WSClass`

:   (etiqueta *COM* verde en la \cref{f_arq}) es una clase
    destinada a la lógica y comunicación del módulo. Cada
    objeto perteneciente a esta clase tiene acceso a tres
    canales de comunicación:

    Canal Local

    :   es un canal que comunica mensajes entre
        procedimientos del \gls{servidor}. Para cada
        \gls{cliente} existe un canal local independiente y
        aislado de los demás. Este canal se utiliza
        principalmente para comunicar a los módulos
        asignados a un \gls{cliente} (sin que un módulo
        tenga que importar el código del otro) y, para
        enrutar, filtrar y procesar mensajes.

    Canal \gls{ws}

    :   es un canal que sirve para intercambiar mensajes
        entre un \gls{cliente} y el \gls{servidor}. Al igual
        que con los canales locales, existe un canal
        \gls{ws} para cada \gls{cliente}, son independientes
        y están aislados entre sí.

    Canal de Base de Datos

    :   es un único canal en el que múltiples instancias de
        la aplicación pueden intercambiar mensajes. Es
        posible ejecutar múltiples instancias de la
        aplicación con el fin de escalar horizontalmente el
        sistema. Para que las diferentes instancias trabajen
        como una sola, no solo es necesario que compartan la
        misma base de datos, además es necesario compartir
        mensajes. Esto se logra utilizando una colección
        especial de la base de datos como canal de
        transmisión de mensajes.

    Cada uno de estos canales puede ser accedido mediante un
    adaptador, de clase `PubSub`, el cual cumple la función
    de estandarizar el acceso al canal. Estos adaptadores
    intercambian mensajes, creados utilizando diccionarios
    de Python, de manera análoga a como se intercambian
    objetos \gls{json} en una aplicación \gls{web} a través
    de \gls{http}. El requisito mínimo para enviar un
    diccionario por un adaptador `PubSub` es que contenga la
    clave `'type'`. El objeto, normalmente una cadena de
    texto, asociado a la clave `'type'` tiene como objetivo
    identificar el formato de las otras claves del
    diccionario. Cuando un objeto conoce el formato asociado
    a un tipo de mensaje (`'type'`), puede subscribir
    métodos en el adaptador `PubSub` a ese tipo de mensajes
    y/o enviar mensajes con ese formato a través del
    adaptador. Una vez que el adaptador recibe un mensaje,
    todos los métodos suscritos al tipo de ese mensaje son
    ejecutados.

    El nombre de esta clase, `WSClass`, se ha mantenido
    desde el comienzo del proyecto, cuando estos objetos
    solo tenían acceso al canal \gls{ws}. En futuras
    versiones de la aplicación, debe cambiarse el nombre
    para que sea más descriptivo.

`BoilerUIModule`

:   (etiqueta *UI* azul en la \cref{f_arq}) es una clase que
    hereda de [`tornado.web.UIModule`][uimodule]. La
    función de una clase que hereda de
    `tornado.web.UIModule` es definir un componente gráfico
    (\gls{html}, \gls{css} y \gls{js}) que será insertado
    dentro del documento \gls{html} enviado a un usuario. El
    objetivo de esta clase es facilitar la inclusión de
    archivos \gls{css} y \gls{js} para ser enviados junto
    con el documento \gls{html}, pudiendo ser mantenidos de
    forma independiente del resto de la aplicación. En la
    práctica, cuando se utiliza un `BoilerUIModule` en un
    módulo, es común implementar el módulo como un paquete.
    De esta forma se pueden almacenar los archivos \gls{css}
    y \gls{js} dentro del mismo directorio.

`DBObject`

:   (etiqueta *DB* naranja en la \cref{f_arq}) es una clase
    que permite acceder a documentos almacenados en MongoDB
    como si fueran un objeto en Python. Las clases que
    heredan de DBObject tienen que definir el atributo de
    clase `coll`. De esta forma se indica la colección donde
    se encuentran los documentos que desean ser
    representados. Además, las instancias de esta clase
    funcionan como un caché de la base de datos, manteniendo
    los datos de un documento en memoria mientras se utiliza
    la instancia.

Un \gls{cliente} puede hacer diferentes tipos de
requerimiento \gls{http} a la aplicación: vacío (`/`), de
código (`/7fl3x`), de autenticación (`/singin`) o de
conexión \gls{ws} (`/ws`). Dependiendo del tipo de
requerimiento, el \gls{cliente} es atendido por una
instancia de `GUIHandler`, `LoginHandler` o `MSGHandler`.
Estas tres clases, que serán explicadas en los siguientes
párrafos, aparecen representadas en la \cref{f_arq} debajo
de la nube *Internet*. Y son la interfaz a través de la cual
se comunica el \gls{servidor} con el \gls{cliente}. De esta
forma, los requerimientos vacíos y de código (`/` y
`/l1bs8`) son atendidos por objetos `GUIHandler`; los
requerimientos de autenticación (`/singin`) son atendidos
por objetos `LoginHandler`; y los requerimientos de conexión
\gls{ws} son atendidos por objetos `MSGHandler`.

\pagebreak[0]

La función de `GUIHandler` es enviar el \gls{frontend} a los
\glspl{cliente}. Existen tres formas en las que se puede
cargar el \gls{frontend} en el \gls{cliente}: como profesor,
como alumno o como ninguno (refiriéndose a que el usuario no
participa de ninguna clase y no toma rol de alumno o
profesor). La forma en la que se carga el \gls{frontend}
depende del requerimiento:

*   Si el requerimiento es de código y el código es de sala,
    se carga el \gls{frontend} de profesor.
*   Si el requerimiento es de código y el código es de
    asiento, se carga el \gls{frontend} de alumno.
*   Si el requerimiento es vacío, se carga el \gls{frontend}
    "ninguno".

Para enviar el \gls{frontend}, primero es necesario
componerlo utilizando las clases `BoilerUIModule` definidas
en los módulos de la aplicación (etiqueta *UI* azul en
\cref{f_arq}). Las clases que se utilizan dependen de la
forma en que se desee cargar el \gls{frontend} y esto a su
vez depende del tipo de requerimiento y el código utilizado.
El proceso está representado en la \cref{f_arq} con las
flechas que entran y salen del bloque `GUIHandler`. Estas
conexiones son las únicas de la figura que tienen flechas,
indicando la dirección en la que fluye la información, desde
los módulos pasando por `GUIHandler` y hasta el
\gls{cliente}, a través del protocolo \gls{http}. En
particular, salvo el envío del requerimiento \gls{http}, la
información solo fluye hacia el \gls{cliente}.

El \gls{frontend} de la aplicación está compuesto
principalmente por paneles e indicadores (más detalles en la
sección [Interfaz Gráfica]). Los paneles pueden cargar
componentes gráficos en la sección principal de la
aplicación. Mientras los indicadores solo pueden cargar
componentes gráficos en la parte derecha de la cabecera de
la aplicación. Una vez que el \gls{frontend} está cargado en
el \gls{cliente}, se verifica si en ese \gls{cliente} existe
un identificador de sesión. Si el identificador de sesión no
existe, se muestra el panel `HomeLockingPanel`. Este panel
contiene el título de la aplicación y un mensaje de
bienvenida. Además, provee un botón para poder autenticar al
usuario utilizando una cuenta de Google. El resultado de la
autenticación es un identificador de sesión.

Cuando el usuario aprieta el botón de autenticación con
Google, se envía un requerimiento de autenticación
(`/signin`) a `LoginHandler`. Utilizando los datos enviados
con el requerimiento, `LoginHandler` redirecciona al
\gls{cliente} a una página de autenticación de Google. Una
vez terminada la autenticación, el \gls{cliente} envía un
código generado por Google a `LoginHandler`. Con este
código, `LoginHandler` puede acceder por su cuenta a los
datos del usuario, crear un nuevo identificador de sesión,
firmar el identificador de sesión con el secreto asignado al
usuario y enviarlo al \gls{cliente}. Una vez que el nuevo
identificador de sesión es almacenado en el \gls{cliente},
el \gls{cliente} hace un nuevo requerimiento a `GUIHandler`.
Esta vez, el \gls{frontend} sí encontrará el identificador
de sesión, y en vez de mostrar el panel `HomeLockingPanel`,
se mostrará un panel de carga y comenzará el proceso de
conexión \gls{ws}.

\pagebreak[4]

Cuando se carga el \gls{frontend} en el \gls{cliente}, y ya
existe un identificador de sesión, el \gls{frontend} envía
un requerimiento de conexión \gls{ws}. Este requerimiento es
atendido por una instancia de `MSGHandler`. Cuando la
instancia de `MSGHandler` es creada, esta además crea una
instancia de la clase `WSClass` (etiqueta *COM* verde en
\cref{f_arq}) de cada módulo de la aplicación. De esta forma
el componente gráfico (`BoilerUIModule`, etiqueta *UI* azul
en \cref{f_arq}) de cada módulo en el \gls{frontend}, puede
comunicarse con su contraparte en el \gls{servidor} a través
del nuevo canal \gls{ws}.

El primer mensaje que es intercambiado por el canal \gls{ws}
es de tipo `'session.start'`, y contiene el identificador de
sesión y el código con que se ha ingresado a la aplicación.
De esta forma, el usuario, su rol (profesor alumno o
ninguno) y la ubicación del usuario, quedan asociados a esa
instancia de `MSGHandler`. Si todos los datos enviados en el
mensaje `'session.start'` son correctos, y el identificador
de sesión no ha expirado, el mensaje es respondido con un
mensaje de tipo `'session.start.ok'`. En caso contrario, se
responde con el mensaje de tipo `'logout'`, el cual fuerza
al \gls{frontend} a eliminar el identificador de sesión del
\gls{cliente} y a cerrar la conexión \gls{ws}.

Las instancias de las clases `GUIHandler`, `LoginHandler` y
`WSClass` (etiqueta *COM* verde en \cref{f_arq}) utilizan
varios objetos de la capa de interfaz con la base de datos
(bloque con la etiqueta *DB Interf.* en la \cref{f_arq}).
Algunos de estos objetos, por dar un ejemplo, pertenecen a
las clases `Course`, `Room` y `User`. Las cuales son todas
sub-clases de `DBObject`. Además, como ya se había
mencionado, los módulos de la aplicación pueden definir sus
propias sub-clases de `DBObject`, con el fin de definir
nuevas colecciones para implementar nuevas funcionalidades
en la aplicación. Las nuevas sub-clases de `DBObject` pueden
registrarse en la capa de interfaz con la base de datos,
para que puedan ser usadas desde otros módulos. También,
pueden mantenerse de manera privada dentro del módulo,
dependiendo de lo que se desee implementar.

[^modulos]: No deben confundirse los módulos de Python con
    los módulos de la aplicación. Cada módulo de la
    aplicación es un módulo de Python, pero no todo módulo
    de Python es un módulo de la aplicación.

    En Python existe un tipo especial de módulo llamado
    paquete. Lo más común es que los módulos de la
    aplicación sean paquetes, ya que esto le permite a los
    módulos de la aplicación contener varios archivos.

[uimodule]: http://www.tornadoweb.org/en/stable/web.html#tornado.web.UIModule

###Arquitectura de Módulos

La arquitectura descrita en la sección anterior permite, de
manera abstracta, diseñar grupos de módulos y sus
relaciones. Utilizando los requerimientos obtenidos, se ha
diseñado el conjunto de módulos que se representa en la
\cref{f_mod}. Existen dos módulos de servicio:
*Presentación* y *Control Remoto*. Ambos módulos prestan un
servicio a los demás módulos. El módulo *Presentación*
es un panel que permite mostrar contenidos en pantalla
completa. En él se puede mostrar cualquier elemento
\gls{html}, ya sea texto, imágenes, videos, simulaciones o
juegos. El módulo *Control Remoto* permite a otros módulos
mostrar botones. Su objetivo es proveer un panel desde el
que se pueda acceder a las funciones más importantes de la
aplicación.

![Arquitectura de los módulos de la aplicación.
  \label{f_mod}](src/3-propuesta/fig/modulos.pdf)

\pagebreak[4]

El módulo *Diapositivas* permite cargar diapositivas en
distintos formatos al sistema. Utiliza al módulo
*Presentación* para poder mostrar un archivo de
diapositivas. También utiliza al modulo *Control Remoto*
para instanciar botones que permitan controlar la
presentación. Este módulo solo se carga en el \gls{cliente}
cuando un usuario es profesor. Además, permite mostrar
diapositivas en un computador mientras se utiliza el celular
como control remoto.

Finalmente, utilizando los servicios del módulo *Preguntas*,
el módulo *Diapositivas* permite lanzar preguntas a los
alumnos. El módulo *Preguntas* permite escribir sub-módulos
que implementan diferentes tipos de preguntas. Para este
trabajo se implementará solo el sub-módulo para preguntas de
alternativas. Además, es posible desplegar los resultados de
las preguntas en el módulo *Presentación*, utilizando un
botón instanciado en el módulo *Control Remoto*.

\pagebreak[4]

Existe un conjunto de módulos de sistema que no ha sido
mencionado y no aparece en la \cref{f_mod}. Estos son:

\pagebreak[3]

*   Courses
*   Router
*   Critical
*   Home
*   Lesson Setup
*   Loading
*   Student Setup
*   Connection Indicator
*   Don't Understand
*   User

Estos módulos proveen funciones de sistema y parte de la
interfaz de usuario, pero no son de mayor importancia en lo
que respecta a lo que la aplicación puede hacer. Algunos de
ellos serán descritos en mayor detalle en las secciones
[Lógica de acceso], [Flujo de la aplicación] e [Interfaz
Gráfica].

Organización del Código
-----------------------

El código de la aplicación se maneja utilizando el sistema
de control de versiones Git. Se ha decidido organizar el
desarrollo en tres repositorios Git diferentes.

El primer repositorio se llama TornadoBoiler (disponible en
<https://github.com/cganterh/tornadoBoiler>) y su objetivo
es proveer de una base estandarizada de código para
proyectos hechos con el \gls{framework} Tornado. Aquí se
define una interfaz gráfica básica e infraestructura para
crear módulos gráficos de \gls{frontend}.

Utilizando TornadoBoiler como base de código, se ha creado
TornadoBoxes (disponible en
<https://github.com/cganterh/tornadoBoxes>). Este
repositorio tiene por objetivo entregar una base de código
para aplicaciones \gls{web} modulares, de una sola carga y
que se comuniquen con el \gls{servidor} utilizando \gls{ws}.
Este proyecto define una interfaz gráfica compuesta por un
menú principal, que permite acceder a diferentes paneles con
contenido organizado en bloques (*Boxes*). Además, existen
indicadores en la parte superior derecha de la interfaz.

Finalmente, basándose en TornadoBoxes, se crea el
repositorio en el que se mantiene la aplicación
correspondiente a este trabajo, llamada EduRT (disponible en
<https://github.com/cganterh/EduRT>). El cual implementa
toda la funcionalidad orientada a los usuarios finales.

Las ventajas de este tipo de organización, es que se puede
reutilizar y mantener una misma base de código para
múltiples proyectos. Además, cualquier actualización en un
proyecto superior se puede propagar a proyectos inferiores
de manera casi automática.

Interfaz Gráfica
----------------

La interfaz de la aplicación está organizada en módulos de
diferentes tipos: paneles, paneles bloqueantes e
indicadores. Cada uno de estos módulos se puede instanciar
en un área de la pantalla. Existen principalmente dos áreas.
La cabecera de la aplicación, que comprende una barra
horizontal en la parte superior y el área principal, que
corresponde a todo el espacio inferior que no es utilizado
por la cabecera. Las diferentes áreas se pueden apreciar en
la \cref{f_areas}.

![Áreas de la interfaz gráfica.\label{f_areas}
 ](src/3-propuesta/fig/areas.pdf)

Los paneles y los paneles bloqueantes son mostrados en el
área principal. Estos ocupan toda el área, y por lo tanto,
solo un panel puede ser mostrado a la vez. Suelen organizar
sus contenidos internos en cajas (boxes heredadas de
TornadoBoxes), que son áreas rectangulares pensadas para
separar contenidos. La diferencia entre paneles y paneles
bloqueantes radica en que el usuario puede cambiar entre los
paneles utilizando el menú desplegable principal (captura b
en la \cref{f_interfaz}), en el cual están listados todos
los paneles, pero no lo paneles bloqueantes. Los paneles
bloqueantes solo pueden ser mostrados programáticamente. Por
ejemplo, cuando ocurre un evento, un error o se necesita la
interacción inmediata del usuario. Cuando un panel
bloqueante es mostrado, se esconde el botón que despliega el
menú principal, evitando que el usuario pueda ignorar el
panel (ver captura c en \cref{f_interfaz}). Solo se puede
salir del estado de bloqueo de un panel bloqueante de manera
programática. Por lo tanto esto depende de la programación
del panel bloqueante. Generalmente se requiere que el
usuario complete alguna acción para que el panel bloqueante
desaparezca y se pueda volver a utilizar el menú principal.

![Capturas de la interfaz gráfica.\label{f_interfaz}
 ](src/3-propuesta/fig/ui.pdf)

Los indicadores son principalmente botones que se muestran
dentro de la cabecera de la aplicación a su lado derecho,
como se muestra en todas las capturas de la
\cref{f_interfaz}. Sirven como herramientas de acceso rápido
o de notificación de eventos.

Aparte de los componentes modulares, existe un componente
general del sistema llamado burbuja de notificaciones. Los
diferentes módulos pueden acceder de manera programática a
la burbuja, con el fin de entregar información rápida, pero
sin interrumpir al usuario (ver captura d en
\cref{f_interfaz}). La burbuja tiene dos modos
fundamentales: normal y error. La diferencia entre ellos, es
que en el modo error la burbuja se muestra de color rojo.

Flujo de la Aplicación
----------------------

El *flujo de la aplicación* hace referencia a las
principales maneras que el usuario puede navegar por la
aplicación. En esta sección se describen los dos flujos
principales de la aplicación: flujo de profesor y flujo de
alumno.

El mecanismo de ingreso principal a la aplicación, es
mediante códigos \gls{qr}/\gls{nfc}. La idea detrás de esto
es que los usuarios no tengan que memorizar una dirección
\gls{web} o tener instalada una forma de acceso en sus
dispositivos. Además, al marcar lugares físicos con códigos
\gls{qr}/\gls{nfc}, la aplicación puede aprovechar el
conocimiento sobre el lugar en que está ubicado cada código
para localizar a los usuarios. Existen dos tipos de código,
los de sala y los de asiento. Los códigos de sala se deben
pegar en la puerta de una sala donde se desee utilizar la
aplicación. Y al escanearlos se ingresa a la aplicación en
modo profesor. Mientras que los códigos de asiento deben ser
pegados en cada asiento de una sala, y al escanearlos se
ingresa en modo de alumno.

\pagebreak[0]

###Flujo de Profesor

Cuando un profesor llega a una sala en la que desea dictar
una clase, debe escanear el código ubicado en la puerta de
la sala utilizando su celular. Haciendo esto entrará
automáticamente a la aplicación. Y, suponiendo que ya se
ha autenticado con Google en ese dispositivo, se mostrará
una lista con los cursos que dicta. Si el curso que desea
dictar no existe, es posible agregarlo en ese mismo momento
introduciendo el nombre del curso. Cuando el profesor
selecciona el curso que desea dictar, la aplicación
automáticamente establece que en esa sala se está dictando
el curso seleccionado.

Luego el profesor ingresa a la sala y debe encender el
computador para poder utilizar el proyector. Para cargar la
aplicación en el computador, es necesario ingresar
utilizando la dirección \gls{web} asociada. Al abrir y
autenticarse en la aplicación, utilizando el computador, se
detecta automáticamente que el profesor ya está dictando un
curso. Gracias a esto, la aplicación puede cargarse en modo
profesor y sabe qué curso se está dictando.

Finalmente, el profesor puede cargar un archivo de
presentación en el computador y así utilizar el proyector.
Mientras que en el celular puede cargar el panel de control
remoto para controlar el flujo de la presentación. Además,
si una diapositiva tiene asociada una pregunta, esta puede
ser lanzada utilizando el control remoto.

###Flujo de Alumno

Cuando un alumno llega a la sala y se sienta en su puesto,
puede escanear el código \gls{qr}/\gls{nfc} asociado. Al
hacerlo, y asumiendo que el alumno ya está autenticado en
ese dispositivo con una cuenta de Google, se muestran todos
los cursos que se están dictando actualmente en esa sala
[^multiples_cursos]. Cada curso se muestra con el nombre del
profesor que lo dicta. Al seleccionar un curso de la lista,
el alumno entra a la aplicación y podrá recibir las
preguntas que sean lanzadas en ese curso.

[^multiples_cursos]: La afirmación de que puedan dictarse
    múltiples cursos en una sala puede parecer un error. Sin
    embargo, esta ha sido una decisión de diseño bastante
    meditada y se ha concluido que es una de las mejores
    maneras de resolver varios problemas de diseño.

\pagebreak[4]

Lógica de Acceso
----------------

En la sección [Arquitectura general] se describe cómo el
\gls{frontend} puede ser cargado en diferentes modos:
profesor, alumno o ninguno. Durante el diseño de la
aplicación se ha tomado en consideración las diferentes
posibilidades que puedan surgir cuando un usuario utiliza
varios terminales para ingresar en diferentes modos. Con el
fin de garantizar un funcionamiento consistente se ha
establecido que cada usuario puede estar solo en uno de los
siguientes estados:

*   Ninguno (n)
*   Alumno (s)
*   Alumno asistiendo a un curso (S)
*   Profesor (t)
*   Profesor dictando un curso (T)

Luego se elaboró la máquina de estados que se muestra en la
\cref{f_fst}. En la figura se muestran además las posibles
transiciones. Estas pueden ocurrir como respuesta a uno de
los siguientes eventos:

*   Salir de la aplicación (o)
*   Abrir la aplicación en modo ninguno (n)
*   Abrir la aplicación en modo alumno (s)
*   Abrir la aplicación en modo alumno en una sala distinta
    de la actual (s*)
*   Comenzar a asistir a un curso como alumno (S)
*   Abrir la aplicación en modo profesor (t)
*   Abrir la aplicación en modo profesor en una sala
    distinta de la actual (t*)
*   Comenzar a dictar un curso como profesor (T)

![Máquina de estados finitos para los estados de usuario.
  \label{f_fst}](src/3-propuesta/fig/fsm.pdf)

\pagebreak[4]

Una vez establecidos los estados y los eventos que gatillan
las transiciones, fue necesario crear una tabla de
transiciones y detallar qué tipo de acciones se deben
ejecutar para cada transición. Sin entrar en mayor detalle,
para cada transición es necesario ejecutar una combinación
de las siguientes acciones:

\pagebreak[3]

*   \(DI) Decrease Instances
*   \(II) Increase Instances
*   \(RAC) Room Assing Course
*   \(RDC) Room Deasign Course
*   \(UAC) User Assign Course
*   \(UDC) User Deasign Course
*   \(US) Use seat
*   \(LS) Leave Seat
*   \(LOI) Logout Other Instances
*   \(RTV) Redirect to Teacher View
*   \(LC) Load Course
*   \(LR) Load Room

De esta forma se implementan reglas del tipo: si un usuario
tiene activada la aplicación como alumno y abre la
aplicación en otro dispositivo, la sesión del primer
dispositivo debe cerrarse. O: Cuando un usuario tiene
abierta la aplicación en modo profesor e ingresa en modo
ninguno con un segundo dispositivo, el segundo dispositivo
debe cambiar a modo profesor.

[Arquitectura general]: #arquitectura-general
[Arquitectura de módulos]: #arquitectura-de-módulos
[Lógica de acceso]: #lógica-de-acceso
[Flujo de la aplicación]: #flujo-de-la-aplicación
[Interfaz Gráfica]: #interfaz-gráfica
