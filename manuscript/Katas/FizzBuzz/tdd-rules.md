# Las leyes de TDD

Desde la introducción de la metodología TDD por Kent Beck se ha intentado definir un *framework* sencillo que proporcione una guía para aplicarla en la práctica.

Inicialmente, Kent Beck propuso dos reglas muy básicas:

* No escribir una línea de código sin antes tener un test automático que falle.
* Eliminar la duplicación.

Es decir, para poder escribir código de producción, primero debemos tener un test que no pase y que requiera que escribamos ese código, precisamente porque eso es lo necesario para que el test pase.

Una vez que lo hemos escrito y viendo que el test pasa, nuestro esfuerzo se centra en revisar el código escrito y eliminar en lo posible la duplicación. Esto es muy genérico, porque por una parte se refiere al *refactoring* y, por otra parte, al acoplamiento entre el test y el código de producción. Y al ser tan genérico resulta difícil bajarlo en acciones prácticas.

Por otro lado, estas reglas no nos dicen nada acerca de cuan grandes son los saltos de códigos implicados en cada ciclo. Beck sugiere en su libro que los pasos o baby steps pueden ser tan pequeños o tan grandes como nos resulten útiles. En general, recomienda usar pasos pequeños cuando tenemos inseguridad o poco conocimiento del algoritmo, mientras que permite pasos más grandes si por experiencia y conocimientos tenemos claro qué hacer a continuación.

Con el tiempo, y a partir de la metodología aprendida del propio Beck, Robert C. Martin estableció las "tres leyes", que no sólo definen el ciclo acciones en de TDD, sino que también proporcionan criterios sobre cómo de grandes deberían ser los pasos en cada ciclo:

* No se permite escribir ningún código de producción a menos que haga pasar un test unitario que falle
* No se permite escribir más de un test unitario que sea suficiente para fallar; y los errores de compilación son fallos.
* No se permite escribir más código de producción del que sea necesario para hacer pasar un test unitario que falle

Las tres leyes son lo que hace diferente TDD de simplemente escribir tests antes que el código.

Estas leyes imponen una serie de restricciones cuyo objetivo es forzarnos a seguir un determinado orden y ritmo de trabajo. Definen una serie de condiciones que, si se cumplen, generan un ciclo y guían nuestra toma de decisiones. Entender cómo funcionan, nos ayudará a aprovechar al máximo la capacidad de TDD para ayudarnos a generar código de calidad y mantenible.

Estas leyes se tienen que cumplir todas a la vez porque funcionan juntas.

## Las leyes en detalle

### No se permite escribir ningún código de producción a menos que haga pasar un test unitario que falle

La primera ley nos dice que no podemos escribir código de producción si no hace pasar un test unitario existente que actualmente está fallando. Esto implica lo siguiente:

* Tiene que existir un test que describa un aspecto nuevo del comportamiento de la unidad que estamos desarrollando.
* Este test tiene que fallar porque en el código de producción no existe nada que lo haga pasar.

En resumen, la primera ley nos fuerza a escribir un test que defina el comportamiento que vamos a implementar en la unidad de software que estamos desarrollando antes de plantearnos cómo hacerlo.

Ahora bien, ¿cómo tiene que ser el test que escribamos?

### No se permite escribir más de un test unitario que sea suficiente para fallar; y los errores de compilación son fallos.

La segunda ley nos dice que el test debe ser suficiente para fallar y que tenemos que considerar fallos los errores de compilación o su equivalente en lenguajes interpretados. Por ejemplo, entre estos errores estarían algunos tan obvios como que la clase o función no existe o no ha sido definida.

Debemos evitar la tentación de escribir un esqueleto de la clase o la función antes de escribir el primer test. Recuerda que estamos hablando de *Test Driven Development*. Por tanto, son los tests los que nos dicen qué código de producción escribir y no al revés.

Que el test sea suficiente para fallar quiere decir que el test ha de ser muy pequeño en diversos sentidos y es algo que al principio resulta bastante difícil de definir. Con frecuencia se habla del test "más sencillo", del caso más simple, pero no es exactamente así.

¿Qué condiciones tendría que reunir un test en TDD, particularmente el primero?

Pues básicamente forzarnos a escribir el mínimo código posible que se pueda ejecutar. Lo mínimo en OOP sería instanciar la clase que queremos desarrollar sin preocuparnos de más detalles, de momento. El test concreto variará un poco en función del lenguaje y framework de testing que estemos utilizando.

Si lanzásemos este test veríamos que falla por razones obvias: no existe la clase que se pretende instanciar en ninguna parte. El test está fallando por un problema de compilación o equivalente. Por tanto podría ser un test suficiente para fallar.

A lo largo del proceso veremos que este test es redundante y que podemos prescindir de él, pero no nos adelantemos. Todavía tenemos que conseguir que pase.

### No se permite escribir más código de producción del que sea necesario para hacer pasar un test unitario que falle

La primera y la segunda leyes nos dicen que tenemos que escribir un test y cómo debería ser ese test. La tercera ley nos dice cómo tiene que ser el código de producción. Y la condición que nos pone es que haga pasar el test que hemos escrito.

Es muy importante entender que es el test el que nos dice qué código necesitamos implementar y, por tanto, aunque tengamos la certeza de que va a fallar porque ni siquiera tenemos un archivo con el código necesario para definir la clase, debemos ejecutar el test y esperar su mensaje de error.

Es decir: tenemos que ver que el test, efectivamente, falla.

Lo primero que nos dirá al tratar de ejecutarlo es que la clase no existe. En TDD eso no es un problema, sino una indicación de lo que debemos hacer: añadir un archivo con la definición de la clase. Seguramente con las herramientas del IDE podamos generar ese código de manera automática, y es aconsejable hacerlo así. 

En este punto volvemos a ejecutar el test para comprobar si pasa de rojo a verde. En muchos lenguajes este código será suficiente. En algunos casos puedes necesitar algo más.

Si es así, y el test pasa, el primer ciclo está completo y podremos pasar al siguiente comportamiento.

Si no es así, nos fijaremos en el mensaje que nos ha mostrado el test fallido y actuaremos en consecuencia, añadiendo el código mínimo necesario para que el test, finalmente, pase y se ponga en verde.

## El segundo test y las tres leyes

Cuando hemos logrado hacer pasar el primer test aplicando las tres leyes podríamos pensar que no hemos conseguido realmente nada. Ni siquiera hemos abordado los posibles parámetros que podría necesitar la clase para ser construida, ya sean datos o colaboradores en el caso de servicios o use cases.

Sin embargo, es importante ceñirse a la metodología, sobre todo en estas primeras fases. Con la práctica y la ayuda de un buen IDE el primer ciclo nos habrá llevado apenas unos pocos segundos. En esos pocos segundos hemos escrito un código, ciertamente muy pequeño, pero totalmente respaldado por un test.

Nuestro objetivo sigue siendo que los tests nos dicten qué código tenemos que escribir para implementar cada nuevo comportamiento. Como nuestro primer test ya pasa, tendríamos que escribir el segundo.

Aplicando las tres leyes, lo que viene a continuación es:

* Escribir un nuevo test que defina un comportamiento
* Que ese test sea el mínimo posible para obligarnos a hacer un cambio en el código de producción
* Escribir el código de producción mínimo y suficiente que hace pasar el test

¿Cuál podría ser el próximo comportamiento que necesitamos definir? Si en el primer test nos hemos forzado a escribir el código mínimo necesario para instanciar la clase, el segundo test puede llevarnos por dos caminos:

* Forzarnos a escribir el código necesario para validar parámetros del constructor y, por tanto, poder instanciar un objeto con todo lo necesario.
* Forzarnos a introducir el método que ejecuta el comportamiento deseado.

Habiendo llegado a este punto, nos podríamos preguntar: ¿qué pasa si no cumplimos las tres leyes de TDD?

## Violaciones de las tres leyes y sus consecuencias

Obviando la broma fácil de que no acabaremos en la cárcel o con una multa por incumplir las leyes de TDD, lo cierto es que sí tendríamos que apechugar con algunas consecuencias.

### Primera ley: escribir código de producción sin tener un test

La consecuencia más inmediata es que rompemos el ciclo red-green. El código que escribimos ya no está guiado ni cubierto por tests. De hecho, si queremos tener esa parte testeada, tendremos que hacer un test a posteriori (un test de QA).

### Segunda ley: escribir más que un test que falle

Esto podemos interpretarlo de dos formas: escribir varios tests o escribir un test que supone un salto de comportamiento demasiado grande.

Escribir varios tests provocaría varios problemas. Para hacerlos pasar todos necesitaríamos implementar una gran cantidad de código y la guía que nos podrían proporcionar esos mismos tests se desdibuja tanto que es como no tenerla. No contaríamos con una indicación concreta que resolver implementando nuevo código.

Escribir un único test que define un salto de comportamiento demasiado grande tendría un efecto parecido. Tendríamos que escribir demasiado código de producción de una sola vez, con lo que conlleva de inseguridad y espacio para errores.

### Tercera ley: escribir más del código de producción necesario para que pase el test

Se trata quizá de la más frecuente de todas. Llega un momento en que "vemos" el algoritmo con tanta claridad que nuestro impulso es escribirlo ya y terminar el proceso. Sin embargo, esto nos puede lleva a obviar algunas situaciones. Por ejemplo, en una aplicación podríamos "ver" el algoritmo general e implementarlo. Sin embargo, eso podría habernos distraído de un o varios casos particulares y no contemplarlos, lo que una vez incorporado a la aplicación y desplegado podría llevarnos a errores en producción, incluso con pérdidas económicas.

## El ciclo red-green-refactor

Las tres leyes establecen un framework que podríamos llamar "de bajo nivel". Martin Fowler, por su parte, define el ciclo TDD en estas tres fases que estarían en un nivel superior de abstracción:

* Escribe un test para el siguiente fragmento de funcionalidad que deseas añadir.
* Escribe el código de producción necesario para que el test pase.
* Refactoriza el código, tanto el nuevo como el anterior, para que esté bien estructurado.

Estas tres fases define lo que se suele conocer como el ciclo "red-green-refactor", nombrado así en relación al estado de los tests en cada una de las fases del ciclo:

* **Red**: la creación de un test que falla (está en rojo) y que describe la funcionalidad o comportamiento que queremos introducir en el software de producción.
* **Green**: la escritura del código de producción necesario para hacer pasar el test (ponerlo en verde) con lo cual se verifica que se ha añadido el comportamiento especificado.
* **Refactor**: manteniendo los tests en verde, reorganizar el código para estructurarlo mejor, haciéndolo más legible y sostenible sin perder la funcionalidad desarrollada hasta el momento.

En la práctica los ciclos de refactor surgen después de un cierto número de ciclos de las tres leyes. Los pequeños cambios impulsados por éstas se acumulan hasta llegar a un punto en el que comienzan a aparecer *smells* de código que requieren el refactor.

## Qué significa que un test pasa nada más escribirlo

Cuando escribimos un test y pasa sin añadir código de producción puede ser por alguno de esos motivos:

* El algoritmo que hemos escrito es lo bastante general como para cubrir todos los casos posibles: hemos terminado nuestro desarrollo.
* El ejemplo que hemos elegido no es cualitativamente diferente de otros que ya hemos usado y por lo tanto no nos fuerza a escribir código de producción. tenemos que encontrar otro ejemplo.

En la kata FizzBuzz, por ejemplo, llega un momento en que no hay forma de escribir un test que falle porque el algoritmo cubre todos los casos.

La otra posibilidad es que el ejemplo escogido no sea representativo de un nuevo comportamiento, lo que puede venir dado por una mala definición de la tarea o bien por no haber analizado bien los posibles escenarios.

## Referencias

* [The three rules of TDD](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd)
* [The three rules of TDD - video](https://www.youtube.com/watch?v=AoIfc5NwRks)
* [Refactoring the three laws of TDD](http://www.javiersaldana.com/articles/tech/refactoring-the-three-laws-of-tdd)
* [TDD with PHPSpec](https://es.slideshare.net/CiaranMcNulty/tdd-with-phpspec)
* [The 3 Laws of TDD: Focus on One Thing at a Time](https://qualitycoding.org/3-laws-tdd/)
* [Test Driven Development](https://martinfowler.com/bliki/TestDrivenDevelopment.html)
* [The cycles of TDD](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)
