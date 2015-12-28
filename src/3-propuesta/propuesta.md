Propuesta
=========

Requerimientos
--------------

###Levantamiento de requerimientos

La primera parte del trabajo consistió en redactar una
encuesta con el fin de levantar los requerimientos
necesarios para comenzar con el desarrollo de software. Se
realizaron dos tipos de preguntas: de preferencias sobre las
clases y técnicas (uso de diferentes tecnologías). Además se
agregaron preguntas especiales para profesores y para
alumnos. El público al que estaba orientada la encesta eran
los usuarios de salas SCALE-UP. Por lo tanto se decidió
encuestar a todos los alumnos y profesores que usaran este
tipo de salas en el período de una semana. Al finalizar la
semana de encuestas se contó la cantidad de personas que
respondieron, resultando ser 609 personas.

También, se realizó una visita a una clase activa de física
con el objetivo de tener un acercamiento a las actividades
que se realizan en ellas.

###Resultados del levantamiento

Del análisis de las encuestas y la visita a la clase activa,
se llegó a las siguientes conclusiones:

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
    correcto funcionamiento en los navegadores modernos y
    futura compatibilidad de los navegadores más atrasados.

8.  El sistema debe ser robusto a las perdidas de conexión a
    internet.

Finalmente, se diseñó una serie de módulos mínimos que
cumplieran con las necesidades actuales de las salas
SCALE-UP. Estos módulos serán descritos en detalle en la
sección [Arquitectura de módulos].

Principios de diseño
--------------------

1.  Debe desarrollarse de forma que el sistema sea ágil,
    computacionalmente y con respecto a las dinámicas
    desarrolladas entre los actores.\label[dp]{dp1}

2.  El sistema debe ser robusto a las pérdidas de conexión.
    \label[dp]{dp2}

3.  Aunque deba sacrificarse funcionalidad, el software debe
    ser muy estable.\label[dp]{dp3}

4.  Debe desarrollarse de forma que los profesores puedan
    utilizar de mejor manera su tiempo dentro de la clase.
    \label[dp]{dp4}

5.  Debe permitirse el anonimato.\label[dp]{dp5}

Tecnologías
-----------

Una aplicación \gls{web} normalmente está constituida por
dos unidades independientes que se comunican entre sí:
\gls{backend} y \gls{frontend}. El \gls{backend} suele
ejecutarse en un \gls{servidor} y usar una base de datos
para organizar la información de los usuarios. El
\gls{frontend} suele ejecutarse en el terminal de un usuario
(\gls{cliente}).

En las siguientes secciones se describirán las tecnologías
que se ejecutan en el \gls{cliente} y en el \gls{servidor}.
Luego se describirán las tecnologías utilizadas en la
comunicación entre \gls{cliente} y \gls{servidor}.
Finalmente se describirán algunas tecnologías de apoyo que
fueron utilizadas en este trabajo.

###Tecnologías del servidor

Las principales tecnologías utilizadas en el \gls{servidor}
son las siguientes:

GNU/Linux

:   es la combinación del núcleo Linux con el sistema
    operativo GNU. Sobre este sistema operativo se ejecutan
    todos los demás programas necesarios para que la
    aplicación funcione. Actualmente el desarrollo de la
    aplicación se lleva a cabo en una distribución de este
    sistema, llamada Ubuntu. También se han hecho pruebas
    sobre la distribución Debian. La aplicación
    probablemente pueda funcionar sobre otros sistemas
    operativos con modificaciones menores.

CPython 3

:   es la implementación oficial y más ampliamente utilizada
    del lenguaje de programación Python.

MongoDB

:   es un sistema de base de datos \gls{nosql} orientado a
    documentos, desarrollado bajo el concepto de código
    abierto. En vez de guardar los datos en tablas como se
    hace en las \glspl{rdb}, MongoDB guarda estructuras de
    datos en documentos tipo \gls{json} con un esquema
    dinámico, haciendo que la integración de los datos en
    ciertas aplicaciones sea más fácil y rápida.

Tornado

:   es un \gls{servidor} \gls{web} y framework para
    aplicaciones \gls{web}. Se destaca por ser escalable,
    \gls{nbloq} y estar escrito en Python.
    
    Mediante el uso \gls{nbloq} de las operaciones de
    entrada y salida de la red, Tornado puede escalar a
    decenas de miles de conexiones abiertas, lo que es ideal
    para \gls{lpolling}, \gls{ws}, y otras aplicaciones que
    requieren una conexión de larga vida a cada usuario
    \cite{tornado}.

Motor

:   Motor presenta una API basada en \glspl{callback} o en
    \glspl{futuro} para el acceso \gls{nbloq} a MongoDB
    desde Tornado o \gls{asyncio} \cite{motor}.

En la \cref{f_stack} se muestra un diagrama de dependencias
de estas tecnologías. Cada flecha representa una relación de
dependencia, donde la tecnología que está en la parte
superior de la flecha depende de la tecnología que está en
la parte inferior. La aplicación desarrollada en este
trabajo está representada en la parte superior con la
etiqueta *Aplicación*.

Además, en el \gls{servidor} se utilizan las siguientes
bibliotecas de menor importancia:

-   httplib2
-   oauth2client
-   pillow
-   PyJWT
-   qrcode

![Principales tecnologías utilizadas en el \gls{servidor}.
  Las flechas indican una relación de dependencia entre
  tecnologías. Una tecnología que se encuentra en la parte
  superior de una flecha, depende de la tecnología que está
  en la parte inferior.
  \label{f_stack}](src/3-propuesta/fig/stack.pdf)

###Tecnologías del cliente

El código del lado del \gls{cliente} ha sido desarrollado
utilizando \gls{html5}, \gls{css3} y \gls{js}.

La biblioteca más importante utilizada en el \gls{cliente} es
*ReconnectingWebSocket*. Esta es una pequeña biblioteca
\gls{js} que decora la API \gls{ws} para proporcionar una
conexión que se vuelva a conectar automáticamente
si es interrumpida \cite{rws}.

Otras bibliotecas de menor importancia utilizadas en el
\gls{cliente} son:

-   Normalize.css
-   TinyColor
-   Unibabel

###Tecnologías de comunicación

Para comunicar el \gls{backend} con el \gls{frontend} se
utiliza HTTP, \gls{ws} y \gls{json}. Primero, el
\gls{cliente} hace un requerimiento HTTP al \gls{servidor}.
Basado en este requerimiento el \gls{servidor} empaqueta la
aplicación y la envía al \gls{cliente}. Cuando la aplicación
llega el \gls{cliente}, esta se ejecuta y crea una conexión
\gls{ws} permanente con el \gls{servidor}, a través de la
que se intercambian mensajes codificados con \gls{json}.

###Tecnologías de apoyo

Además de las tecnologías mencionadas anteriormente, que
tienen relación directa con el funcionamiento de la
aplicación, se han utilizado diversas tecnologías que
facilitan el desarrollo del proyecto:

CoffeeScript

:   es un pequeño lenguaje que se compila a \gls{js}. Es un
    intento de exponer las partes buenas de \gls{js} de una
    manera sencilla. El código se compila uno a uno en
    \gls{js} equivalente, y no hay interpretación en tiempo
    de ejecución. Se puede usar cualquier biblioteca
    \gls{js} existente sin problemas desde CoffeeScript (y
    viceversa). La salida compilada es legible, trabajará en
    cualquier intérprete de \gls{js}, y tiende a ser tan
    rápida o más rápida que el \gls{js} equivalente escrito
    a mano \cite{coffeescript}.

Git

:   es un sistema de control de versiones distribuido, libre
    y de código abierto, diseñado para manejar con rapidez y
    eficiencia desde pequeños hasta grandes proyectos
    \cite{git}.

Sass

:   es una extensión de \gls{css} que añade potencia y
    elegancia al lenguaje \gls{css} básico. Permite el uso
    de variables, reglas anidadas, \glspl{mixin},
    importaciones en línea y mucho más, todo con una
    sintaxis totalmente compatible con \gls{css}. Sass ayuda
    a mantener grandes hojas de estilo bien organizadas, y
    poner en funcionamiento pequeñas hojas de estilo
    rápidamente \cite{sass}. Las hojas de estilo escritas
    con Sass son compiladas a \gls{css} para ser utilizadas
    en la aplicación, resultando ser una opción eficiente
    con respecto de otras soluciones basadas en bibliotecas.

Bourbon

:   es una biblioteca simple y ligera de \glspl{mixin} para
    Sass \cite{bourbon}. Bourbon permite escribir \gls{css}
    con una mayor compatibilidad, ya que maneja
    automáticamente la inclusión de prefijos de navegador.

Sphinx

:   es una herramienta que hace que sea fácil escribir
    documentación. Fue creado originalmente para la nueva
    documentación de Python. Tiene excelentes recursos para
    la documentación de proyectos escritos en Python, pero
    \gls{c} y \gls{cpp} también están soportados
    \cite{sphinx}.

Arquitectura del sistema
------------------------

###Arquitectura general

En la \cref{f_arq} se muestra la arquitectura general de la
aplicación. El bloque superior con etiqueta *Cliente*
representa al navegador utilizado por los usuarios. La nube
con la etiqueta *Internet* representa a la red utilizada
para comunicar al \gls{cliente} con el \gls{servidor}, así
como también a servicios de terceros que puedan encontrarse
en esa red. Todos los componentes que se encuentran por
debajo de la nube *Internet* forman parte del servidor.

![Arquitectura general de la aplicación.
  \label{f_arq}](src/3-propuesta/fig/arquitectura.pdf)

La aplicación funciona utilizando módulos[^modulos]. Cada
módulo es una agrupación de clases, pudiendo tener una o más
clases. Existen tres tipos principales de clases que pueden
formar parte de un módulo de la aplicación:

`WSClass`

:   es una clase destinada a la lógica y comunicación del
    módulo (identificada con la etiqueta *COM* verde en la
    \cref{f_arq}). Cada objeto perteneciente a esta clase
    tiene acceso a tres canales de comunicación:
    
    Canal Local
    
    :   es un canal que comunica mensajes entre
        procedimientos del servidor. Para cada \gls{cliente}
        existe un canal local independiente y aislado de los
        demás. Este canal se utiliza principalmente para
        comunicar a los módulos de un \gls{cliente} (sin que
        tengan que importarse entre ellos) y, para enrutar,
        filtrar y procesar mensajes.
    
    Canal WebSocket
    
    :   es un canal que sirve para intercambiar mensajes
        entre un \gls{cliente} y el servidor. Al igual que con los
        canales locales, existe un canal WebSocket para cada
        \gls{cliente}, son independientes y están aislados entre
        si.
    
    Canal de Base de Datos
    
    :   es un único canal en el que múltiples instancias de
        la aplicación pueden intercambiar mensajes. Los
        mensajes enviados en este canal son guardados
        temporalmente en la base de datos para poder ser
        distribuídos a todas las instancias de la aplicación
        que ocupen una misma base de datos.
    
    Cada uno de estos canales puede ser accedido mediante un
    adaptador, de clase `PubSub`, el cual cumple la función
    de estandarizar el acceso al canal. Estos adaptadores
    intercambian mensajes, creados utilizando diccionarios
    de Python, de manera análoga a como se intercambian
    objetos \gls{json} en una aplicación \gls{web} a través
    de HTTP. El requisito mínimo para enviar un diccionario
    por un adaptador `PubSub` es que contenga la clave
    `'type'`. El objeto, normalmente una cadena de texto,
    asociado a la clave `'type'` tiene como objetivo
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

:   es una clase que hereda de
    [`tornado.web.UIModule`][uimodule] (identificada con la
    etiqueta *UI* azul en la \cref{f_arq}). La función de
    una clase que hereda de `tornado.web.UIModule` es
    definir un componente gráfico (\gls{html}, \gls{css} y
    \gls{js}) que será insertado dentro del documento
    \gls{html} enviado a un usuario. El objetivo de esta
    clase es facilitar la inclusión de archivos \gls{css} y
    \gls{js} para ser enviados junto con el documento
    \gls{html}, pudiendo ser mantenidos de forma
    independiente del resto de la aplicación. En la
    práctica, cuando se utiliza un `BoilerUIModule` en un
    módulo, es común implementar el módulo como un paquete.
    De esta forma se pueden almacenar los archivos \gls{css}
    y \gls{js} dentro del mismo directorio.

`DBObject`

:   es una clase que permite acceder a documentos
    almacenados en MongoDB como si fueran un objeto en
    Python (identificada con la etiqueta *DB* naranja en la
    \cref{f_arq}). Las clases que heredan de DBObject tienen
    que definir el atributo de clase `coll`. De esta forma
    se indica la colección donde se encuentran los
    documentos que desean ser representados. Además, las
    instancias de esta clase funcionan como un caché de la
    base de datos, manteniendo los datos de un documento en
    memoria mientras se utiliza la instancia.

Un \gls{cliente} puede hacer diferentes tipos de
requerimiento a la aplicación: vacío (`/`), de código
(`/7fl3x`), de autenticación (`/singin`) o de conexión
\gls{ws} (`/ws`). Dependiendo del tipo de requerimiento, el
\gls{cliente} es atendido por una instancia de `GUIHandler`,
`LoginHandler` o `MSGHandler`. Estas tres clases aparecen
representadas en la \cref{f_arq} debajo de la nube
*Internet*. Y son la interfaz a través de la cual se
comunica el \gls{servidor} con el \gls{cliente}. De esta
forma, los requerimientos vacíos y de código (`/` y
`/l1bs8`) son atendidos por un `GUIHandler`; los
requerimientos de autenticación (`/singin`) son atendidos
por `LoginHandler`; y los requerimientos de conexión
\gls{ws} son atendidos por `MSGHandler`.

Los códigos a los que hacen referencia los requerimientos de
código, son secuencias de cinco caracteres alfanuméricos y
provienen de códigos \gls{qr} o \gls{nfc} (en las secciones
[Lógica de acceso] y [Flujo de la aplicación] se dan más
detalles sobre el uso de los códigos). Existen dos tipos de
código, los de sala (`'room'`) y los de asiento (`'seat'`).
No es posible distinguir el tipo de código solamente con la
información contenida en ellos, ya que esta información solo
se encuentra como un dato asociado a cada código en la base
de datos.

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
\cref{f_arq}). No todas las clases `BoilerUIModule` son
utilizadas al componer el \gls{frontend}. Las clases que se
utilizan dependen de la forma en que se desee cargar el
\gls{frontend} y esto a su vez depende de el tipo de
requerimiento y el código utilizado. Este proceso está
representado en la \cref{f_arq} con las flechas que entran y
salen del bloque `GUIHandler`. Estas conexiones son las
únicas de la figura que tienen flechas, indicando la
dirección en la que fluye la información, desde los módulos
pasando por `GUIHandler` y hasta el \gls{cliente}, a través
del protocolo HTTP. En este proceso, exceptuando el envío
del requerimiento, la información solo fluye hacia el
\gls{cliente}.

El \gls{frontend} de la aplicación está compuesto
principalmente por paneles e indicadores (más detalles en la
sección [Interfaz Gráfica]). Los paneles pueden cargar
componentes gráficos en la sección principal de la
aplicación. Mientras los indicadores solo pueden cargar
componentes gráficos en la parte derecha de la cabecera de
la aplicación. Una vez que el \gls{frontend} está cargado en
el \gls{cliente}, se verifica si en ese cliente existe
un identificador de sesión. Si el identificador de sesión no
existe, se muestra el panel `HomeLockingPanel`. Este panel 
contiene el título de la aplicación y un mensaje de
bienvenida. Además, provee un botón para poder autenticar al
usuario utilizando una cuenta de Google. El resultado de la
autenticación es un identificador de sesión.

Cuando el usuario aprieta el botón de autenticación con
Google, se envía un requerimiento de autenticación
(`/signin`) a `LoginHandler`. Utilizando los datos enviados
con el requerimiento, `LoginHandler` redirecciona al cliente
a una página de autenticación de Google. Una vez terminada
la autenticación, el \gls{cliente} envía un código generado
por Google a `LoginHandler`. Con este código, `LoginHandler`
puede acceder por su cuenta a los datos del usuario, crear
un nuevo identificador de sesión, firmar el identificador de
sesión con el secreto asignado al usuario y enviarlo al
cliente. Una vez que el nuevo identificador de sesión es
almacenado en el \gls{cliente}, el \gls{cliente} hace un
nuevo requerimiento a `GUIHandler`. Esta vez, el
\gls{frontend} sí encontrará el identificador de sesión, y
en vez de mostrar el panel `HomeLockingPanel`, se mostrará
un panel de carga y comenzará el proceso de conexión
\gls{ws}.

Cuando se carga el \gls{frontend} en el \gls{cliente}, y ya
existe un identificador de sesión, entonces el
\gls{frontend} envía un requerimiento de conexión \gls{ws}.
Este requerimiento es atendido por una instancia de
`MSGHandler`. Cuando la instancia de `MSGHandler` es creada,
esta además crea una instancia de la clase `WSClass`
(etiqueta *COM* verde en \cref{f_arq}) de cada módulo de la
aplicación. De esta forma el componente gráfico
(`BoilerUIModule`, etiqueta *UI* azul en \cref{f_arq}) de
cada módulo en el \gls{frontend}, puede comunicarse con su
contraparte en el \gls{servidor} a través del nuevo canal
\gls{ws}.

El primer mensaje que es intercambiado por el canal \gls{ws}
es de tipo (`'type'`) `'session.start'`, y contiene el
identificador de sesión y el código con que se ha ingresado
a la aplicación. De esta forma, el usuario, su rol (profesor
alumno o ninguno) y la ubicación del usuario, quedan
asociados a esa instancia de `MSGHandler`. Si todos los
datos enviados en el mensaje `'session.start'` son
correctos, y el identificador de sesión no ha expirado, el
mensaje es respondido con un mensaje de tipo
`'session.start.ok'`. En caso contrario, se responde con el
mensaje de tipo `'logout'`, el cual fuerza al \gls{frontend}
a eliminar el identificador de sesión del cliente y a cerrar
la conexión \gls{ws}.

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
pueden mantenerse de manera más privada dentro del módulo,
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

###Arquitectura de módulos

La arquitectura descrita en la sección anterior permite, de
manera abstracta, diseñar grupos de módulos y sus
relaciones. Utilizando los requerimientos obtenidos, se ha
diseñado el conjunto de módulos se representa en la
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

El módulo *Diapositivas* permite cargar diapositivas en
distintos formatos al sistema. Utiliza al módulo
*Presentación* para poder mostrar un archivo de
diapositivas. También utiliza al modulo *Control Remoto*
para instanciar botones que permitan controlar la
presentación. Este módulo solo se carga en el cliente cuando
un usuario es profesor. Además, permite mostrar diapositivas
en un computador mientras se utiliza el celular como control
remoto.

Finalmente, utilizando los servicios del modulo *Preguntas*,
el módulo *Diapositivas* permite lanzar preguntas a los
alumnos. El módulo *Preguntas* permite escribir sub-modulos
que implementan diferentes tipos de preguntas. Para este
trabajo se implementará solo el sub-módulo para preguntas de
alternativas. Además, es posible desplegar los resultados de
las preguntas en el módulo *Presentación*, utilizando un
botón instanciado en el módulo *Control Remoto*.

Existe un conjunto de módulos de sistema que no han sido
mencionados y no aparecen en la \cref{f_mod}. Estos son: 

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

Organización del código
-----------------------

El código de la aplicación se maneja utilizando el sistema
de control de versiones Git. Se ha decidido organizar el
desarrollo en tres repositorios Git diferentes.

El primer repositorio de llama TornadoBoiler y su objetivo
es proveer de una base estandarizada de código para
proyectos hechos con el \gls{framework} Tornado. Aquí se
define una interfaz gráfica básica e infraestructura para
crear módulos gráficos de \gls{frontend}.

Utilizando TornadoBoiler como base de código, se ha creado
TornadoBoxes. Este repositorio tiene por objetivo entregar
una base de código para aplicaciones \gls{web} modulares, de
una sola carga y que se comuniquen con el servidor
utilizando \gls{ws}. Este proyecto define una interfaz
gráfica compuesta por un menú principal que permite acceder
a diferentes paneles con contenido organizado en bloques
(*Boxes*). Además, existen indicadores en la parte superior
derecha de la interfaz.

Finalmente, basándose en TornadoBoxes, se crea el
repositorio en el que se mantiene la aplicación
correspondiente a este trabajo. El cual implementa toda la
funcionalidad orientada a los usuarios finales.

Las ventajas de este tipo de organización, es que se puede
reutilizar y mantener una misma misma base de código para
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
las capturas de la \cref{f_interfaz}.

![Capturas de la interfaz gráfica.\label{f_interfaz}
 ](src/3-propuesta/fig/ui.pdf)

Los paneles y los paneles bloqueantes son mostrados en el
área principal. Solo un panel puede ser mostrado a la vez, y
por lo tanto, ocupan toda el área principal. Suelen
organizar sus contenidos internos en cajas (boxes heredadas
de TornadoBoxes), que son áreas rectangulares pensadas para
separar contenidos. La diferencia entre paneles y paneles
bloqueantes radica en que el usuario puede cambiar entre los
paneles utilizando el menú desplegable principal (captura b
en la \cref{f_interfaz}), en el cual están listados todos
los paneles, pero no lo paneles bloqueantes. Los paneles
bloqueantes solo pueden ser mostrados programáticamente. Por
ejemplo, cuando ocurre un evento, un error o se necesita la
interacción inmediata del usuario. Cuando un panel
bloqueante es mostrado, se esconde el botón que despliega al
menú principal, evitando que el usuario pueda ignorar el
panel (ver captura c en \cref{f_interfaz}). Solo se puede
salir del estado de bloqueo de un panel bloqueante de manera
programática. Por lo tanto esto depende de la programación
del panel bloqueante. Generalmente se requiere que el
usuario complete alguna acción para que el panel bloqueante
desaparezca y se pueda volver a utilizar el menú principal.

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

Flujo de la aplicación
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

###Flujo de profesor

Cuando un profesor llega a una sala en la que desea dictar
una clase, debe escanear el código ubicado en la puerta de
la sala utilizando su celular. Haciendo esto entrará
automáticamente a la aplicación. Y, suponiendo que ya se
haya autenticado con Google en ese dispositivo, se mostrara
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

###Flujo de alumno

Cuando un alumno llega a la sala y se sienta en su puesto,
puede escanear el código el código \gls{qr}/\gls{nfc}
asociado. Al hacerlo, y asumiendo que el alumno ya esta
autenticado en ese dispositivo con una cuenta de Google, se
muestran todos los cursos que se están dictando actualmente
en esa sala [^multiples_cursos]. Cada curso se muestra con
el nombre del profesor que lo dicta. Al seleccionar un curso
de la lista, el alumno entra a la aplicación y podrá recibir
las preguntas que sean lanzadas en ese curso.

[^multiples_cursos]: La afirmación de que puedan dictarse
    múltiples cursos en una sala puede parecer un error. Sin
    embargo, esta ha sido una decisión de diseño bastante
    meditada y se ha concluido que es una de las mejores
    maneras de resolver varios problemas de diseño.

Lógica de acceso
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

Una vez establecidos los estados y los eventos que gatillan
las transiciones, fue necesario crear una tabla de
transiciones y detallar qué tipo de acciones se deben
ejecutar para cada transición. Sin entrar en mayor detalle,
para cada transición es necesario ejecutar una combinación
de las siguientes acciones:

*   (DI) Decrease Instances
*   (II) Increase Instances
*   (RAC) Room Assing Course
*   (RDC) Room Deasign Course
*   (UAC) User Assign Course
*   (UDC) User Deasign Course
*   (US) Use seat
*   (LS) Leave Seat
*   (LOI) Logout Other Instances
*   (RTV) Redirect to Teacher View
*   (LC) Load Course
*   (LR) Load Room

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
