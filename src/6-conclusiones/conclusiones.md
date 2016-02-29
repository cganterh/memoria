Conclusiones y Trabajo Futuro
=============================

Durante desarrollo de este trabajo el proyecto ha crecido
para convertirse más que solo en una aplicación, en un
\gls{framework} dedicado al desarrollo de herramientas de
interacción dentro de la sala y en tiempo real. Al culminar
el proyecto, se puede notar que el valor real no solo está
en los módulos de usuario, sino que además en la capacidad
de proveer un ambiente abierto donde nuevos módulos puedan
ser desarrollados dependiendo de las necesidades de la
comunidad.

Se ha invertido una considerable cantidad de tiempo en la
escalabilidad y robustez de la arquitectura principal. Lo
que permite tener una herramienta confiable, de fácil
implementación y con pocas barreras para su adopción entre
los usuarios.

Desarrollar nuevos módulos es una tarea sencilla debido a su
naturaleza auto-contenida. También ayuda el sistema de
documentación automático que tiene la aplicación y su
consola. En la consola de la aplicación es posible ejecutar
comandos comunes e inyectar código mientras la aplicación
está en funcionamiento. Haciendo más fácil la depuración de
errores.

\pagebreak[2]

Hoy la aplicación cuenta con los módulos necesarios para
crear y utilizar cursos y presentaciones. Sin embargo queda
mucho por hacer:

\pagebreak[3]

*   Se deben desarrollar nuevos módulos para complementar
    los que ya existen. Como por ejemplo el módulo de
    preguntas, módulos de comunicación entre alumnos o de
    integración con nuevos servicios.

*   Actualmente el módulo de diapositivas soporta un solo
    formato de presentaciones. Sin embargo, este módulo es
    extensible. Pudiéndose escribir fácilmente nuevos
    *parsers* para diferentes tipos de archivo.

*   La razón entre las lineas de documentación a las de
    código es actualmente de $0,24$. Lo cual es aún muy
    poco, siendo deseable una razón mayor a $1$.

\pagebreak[4]

*   Se debe desarrollar el módulo de preguntas y agregar
    tipos nuevos de pregunta, con el fin de sacar provecho a
    los dispositivos móviles actuales. Los tipos de pregunta
    que se podrían implementar son alternativas, de dibujo,
    de selección de áreas en imágenes, preguntas numéricas,
    etc.

*   Se deben ajustar detalles en algunos módulos actuales,
    como por ejemplo: agregar botones para eliminar
    presentaciones y cursos, y actualizar automáticamente la
    lista de cursos en la vista de alumno.
