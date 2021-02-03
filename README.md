# Unidad 1: Introducción a la programación


## Código en computación

* Instrucciones escritas **para una computadora** en un **lenguaje de cómputo**   
* Paso por paso hasta una solución.
* El código puede ser muy largo y formar un programa (**software**) entero o de una sóla línea para realizar una única operación.
* Escribir código para que lo ejecuten las computadoras y **comentado** para seres humanos.


## Cómo buscar ayuda (permanentemente)

Una *habilidad* indispensable en bioinformática

El objetivo de esta clase no es darte un manual completo para resolver cualquier cuestión bioinformática, sino darte las habilidades y herramientas para que llegues por tí misma a las soluciones. Para esto hay una habilidad indispensable: **buscar ayuda en internet**. La enorme mayoría de las preguntas que te hagas sobre bioinformática y programación en general, sobre todo al principio, **son las mismas dudas que tuvieron otros antes** y, casi seguro **las respuestas ya están en algún lugar del internet**. Y si nadie a escrito con tu duda, puedes preguntar. 

Las respuestas a muchas de las tareas que dejaré aparecen en los primeros resultados si haces la búsqueda correcta en internet. De hecho, a veces parte de la tarea será que *se te ocurra cómo buscar* la solución en internet. Esto está perfectamente bien y es muuuy distinto a copiarle a un compañero. Saber buscar ayuda e información en interent es una habilidad básica del siglo XXI. 

Por esto estoy 100% de acuerdo con xkcd, no tienes que saberlo todo de memoria. Se vale tener una *Mente extendida* llamada internet:

![Extended Mind](https://imgs.xkcd.com/comics/extended_mind.png)


* Google o Duckgogo o el motor de búsqueda que uses.

* [Stacksoverflow](https://stackoverflow.com/): es un foro de ayuda para programación en general, viene dividido por lenguajes. La gente hace preguntas y contesta. Es como un yahoo answers super pro, lxs programadorxs con muchas preguntas respondidas ganan puntos que son algo muy visible para un CV con ese perfil. Este foro es mejor para dudas de bash, R, python y no de programas genéticos.

* [Biostars](https://www.biostars.org/) es parecido a Stacksoverflow un foro de ayuda especializado en Bioinformática. Aquí sí puedes hacer preguntas enfocadas en datos y software genético, por ejemplo cómo mover de un formato de genomas a otro.

* Grupos de usuarios de un software. Por ejemplo [este de Stacks](https://groups.google.com/forum/#!forum/stacks-users). Son ideales para aclarar detalles específicos y mensajes de error de **ese** software (ie, no de dudas de cómo usar la línea de comando). La comunidad es súper chida y muchas veces lxs autores del software son quiénes responden. 


**Tips sobre cómo pedir/buscar ayuda**

* Busca en inglés

* Piensa bien cuáles son las **palabras clave de tu pregunta** y cómo **generalizar** tu caso a algo que cualquiera entienda y que no sea específico a tu computadora. Por ejemplo "How to list files in a directory using the Terminal?" es mucho mejor mejor que "which files are in Manzanas" (tu y yo sabemos que Manzanas es un directorio, pero no el mundo).

* Si vas a pedir ayuda en un foro, lee las reglas y tipo de preguntas atentidas por el foro antes de preguntar. 

* Sigue estas recomendaciones de Stacksoverflow sobre [cómo redactar una buena pregunta](https://stackoverflow.com/help/how-to-ask)

* Si recibes un mensaje de error en la Terminal, R, etc. Copia-pégalo a tu motor de búsqueda favorito.



## Introducción a la consola y línea de comando de bash y R

Vamos a abrir la terminal. Debe ser un ícono parecido a este. En Ubuntu debe estar por default en tu dock. Si no lo encuentras tanto en Mac como en Ubuntu prueba buscar "Terminal" o "Console".

![](terminalicon.png)

Tu terminal abierta debe verse parecida a esta:

![TerminalEjemplo](01Terminal.png)

La primera línea dice la fecha y hora y "ttys001". ttys viene de "Teletype" ([un poco de historia aquí](http://stackoverflow.com/questions/4532413/what-is-tty-on-freebsd)) y significa que el *input* es nuestro teclado, identificado como ttys001. 

En mi caso la segunda línea me dice que tengo correo. Uds probablemente no tengan esto.

La tercera línea es la más importante: dice el **nombre del equipo** (Alicas-MacBook-Pro en este caso), el **directorio** donde estoy (`~` significa "home directory", lo veremos más adelante) y el **nombre del usuario** con quién estoy en la terminal (en mi caso ticatla). 

Notarás que la tercera línea termina en `$`

`$` significa que la terminal está corriendo con un interpretador Shell o Bash y por un usuario sin mayores privilegios. Si termina en `#` significa que la estás corriendo como **root** que es un "súper usuario" con permisos para desconfigurarlo todo, ten cuidado. 



#### ¿Qué significa que La Terminal tenga un interpretador? 

Que le damos comandos y nos responde. 

El la Terminal no existe el *point and click*. El que funcione como una Línea de Comando significa que tienes que darle comandos de qué hacer línea por línea. 

Por ejemplo:

`date` nos responde la fecha actual

`echo algo` nos responde el texto "algo". También lo puedes utilizar con más de una palabra. 

`echo hello world`

Algo así debe verse tu terminal:

![egTerminal2](02Terminal.png)

Pero por practicidad y para facilitar el copiar-pegar en vez de la imágen de la Terminal pondré los comandos (código) y sus resultados, así:

```
Alicias-MacBook-Pro:~ ticatla$ date
Sat 23 Jan 2016 14:46:06 CST
Alicias-MacBook-Pro:~ ticatla$ echo algo
algo
Alicias-MacBook-Pro:~ ticatla$ echo hello world
hello world
```

Que es lo mismo que:

```
$ date
Sat 23 Jan 2016 14:46:06 CST
$ echo algo
algo
$ echo hello world
hello world
``` 
 
Para facilidad visual, de aquí en adelante utilizaremos la opción con el texto a partir de `$` (al menos que sea pertinente ver lo que hay antes). Este tipo de formato te lo encontrarás en diversos foros de ayuda, libros de textos y manuales.

**Ojo** Se pone `$` solo para **distinguir** que lo que sigue es un comando que debes poner en la Terminal, por lo que **NO** debes copiar los comandos a tu terminal incluyendo el `$`.
 

**Pregunta**: ¿Qué pasa si intentas correr el comando  `algo`?:

La terminal dirá:

    -bash: algo: command not found

Que quiere decir que no existe el comando `algo`. 

De igual forma si corres `$ date` (incluyendo el `$`) te dirá que no existe el comando `$`, aunque `date` sí sea un comando.


La práctica hace al maestro. A continuación veremos algunos de los comandos básicos, pero para practicar más parte de la tarea será resolver los siguientes cursos prácticos:

* Terminar el curso [Learn the Command Line de CodeAcademy](https://www.codecademy.com/learn/learn-the-command-line).

* Adentrarse en el bosque con este [juego de bash](http://web.mit.edu/mprat/Public/web/Terminus/Web/main.html).

### La terminal de R

`R` es un programa que funciona con la línea de comando y por lo tanto puede correrse desde la terminal de varias formas o en su propia terminal.

```{bash}
$ R
R version 3.2.2 (2015-08-14) -- "Fire Safety"
Copyright (C) 2015 The R Foundation for Statistical Computing
Platform: x86_64-apple-darwin13.4.0 (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> 1+1
[1] 2
> quit()
```

Con `quit()` salgo de la Terminal de R y vuelvo a la Terminal de bash.

R también tiene su propia terminal (lo que sale cuando abres R via su ícono). Sin embargo, nosotros ocuparemos RStudio, que es una interfaz visual que integra la terminal de R con varias funcionalidades útiles e intuitivas.

