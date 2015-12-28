Conclusiones y Trabajo Futuro
=============================

El desarrollo de este trabajo ha tomado más tiempo del
esperado. Sin embargo el proyecto ha crecido para
convertirse más que solo en una aplicación, en un
\gls{framework} dedicado al desarrollo de herramientas de
interacción dentro de la sala y en tiempo real. Ya en el
capítulo que describe la propuesta del proyecto, se puede
notar que los módulos que implementan funciones para los
usuarios son limitados. Y sin embargo se invirtió más tiempo
y esfuerzo en la escalabilidad y robustez de la arquitectura
principal. Esta no fue una decisión que se tomara desde el
comienzo del proyecto, sino que se fue haciendo cada vez más
evidente durante el desarrollo. Hoy, al culminar el
proyecto, se puede notar que el valor real no está en los
módulos de usuario, pero sí en la capacidad de proveer un
ambiente abierto donde nuevos módulos puedan ser
desarrollados dependiendo de las necesidades de la
comunidad.

Aún queda mucho trabajo por hacer:

*   Se deben desarrollar nuevos módulos para complementar
    los que ya existen.

*   Actualmente el módulo de diapositivas soporta un solo
    formato de presentaciones. Sin embargo este módulo es
    extensible. Pudiéndose escribir fácilmente nuevos
    *parsers* para diferentes tipos de archivo.

*   La razón entre las lineas de documentación a las código
    es actualmente de $0,24$. Lo cual es aún muy poco,
    siendo deseable una razón mayor a $1$.

*   Se deben desarrollar nuevos tipos de pregunta para el
    módulo de preguntas, con el fin de sacar provecho a los
    dispositivos móviles actuales.
