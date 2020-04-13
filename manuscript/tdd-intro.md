# ¿Qué es TDD y por qué debería importarme?

Test Driven Development es una metodología de desarrollo de software en la que se escriben tests para guiar la escritura del código de producción.

Los tests especifican de manera formal, ejecutable y mediante ejemplos, los comportamientos que debe realizar el software que estamos programando, definiendo pequeños objetivos que, al ir siendo superados, nos permiten construir el software de forma progresiva, segura y bien estructurada.

Aunque hablemos de tests, no estamos hablando de *Quality Assurance* (en adelante: QA), aunque al trabajar con metodología TDD conseguimos el efecto secundario de hacernos con una suite de tests unitarios que es válida y que tiene la máxima cobertura posible. De hecho, lo normal es que una parte de los tests creados en TDD sean innecesarios para una buena cobertura de test de regresión.

Es decir, tanto TDD como QA se basan en la utilización de los tests como herramientas, pero este uso se diferencia en varios aspectos. Específicamente, en TDD:

* El primer test se escribe antes de que el software siquiera exista.
* Los tests son muy pequeños y su objetivo es forzar que sea necesario escribir código de producción.
* Los tests guían el desarrollo del código y el proceso contribuye al diseño.

En TDD los tests se definen como especificaciones ejecutables del comportamiento de la unidad de software considerada, mientras que en QA el test es una herramienta de verificación de ese mismo comportamiento.

## La metodología Test Driven Development

Aunque a lo largo del libro vamos a desarrollar este apartado en profundidad vamos a presentar brevemente lo esencial de la metodología.

En TDD los tests se escriben en una forma que podríamos considerar como un diálogo con el código de producción. Este diálogo, las normas que lo regulan y los ciclos que esta forma de interactuar con el código genera los practicaremos con la primera kata del libro: FizzBuzz.

### Escribir un test que falle

Una vez que tenemos claro la pieza de software en la que vamos a trabajar y la funcionalidad que queremos implementar, lo primero es definir un primer test muy pequeño que fallará sin remedio porque ni siquiera existe un archivo que contenga el código de producción necesario para que se pueda ejecutar. Aunque es algo que trataremos en todas las katas, en la kata NIF profundizaremos en algunas estrategias para decidir los primeros tests.

```

```

Aunque podemos predecir que el test ni siquiera podrá compilarse o interpretarse, lo intentaremos ejecutar igualmente. En TDD es fundamental ver que los tests efectivamente fallan.

```

```

El mensaje de error nos indicará qué es lo que tenemos que hacer a continuación. Nuestro objetivo a corto plazo es hacer desaparecer ese mensaje de error y los que vengan después uno por uno. 

```

```

Podría ocurrir incluso que sea un mensaje inesperado, como que hemos querido cargar la clase `Book` y hemos creado un archivo `brok` por error. Por eso es tan importante lanzar el test y ver si falla y cómo falla exactamente.

```

```

### Escribir el código de producción necesario para hacer pasar el test

Como respuesta, se escribe el código de producción necesario para que el test pase, pero nada más.

Tras el primer test podemos empezar creando el archivo que contendrá la unidad bajo test. Podríamos incluso volver a lanzar el test ahora, lo cual seguramente provocará que el compilador o intérprete nos devuelva un mensaje de error distinto. Aquí ya dependemos un poco de circunstancias, como las convenciones del lenguaje en que estamos desarrollando, el IDE con el que trabajamos, etc.

En todo caso, se trata de ir dando pequeños pasos hasta que el compilador o intérprete quede conforme y pueda ejecutar el test. En principio, el test debería ejecutarse y fallar indicando que el resultado recibido de la unidad de software no coincide con el esperado.

En este punto hay que hacer una salvedad porque dependiendo del lenguaje, del framework y de algunas prácticas en testing, la forma concreta de este primer test puede ser un poco distinta. Por ejemplo, hay frameworks de test en los que basta conque la ejecución del test no arroje errores o excepciones para considerar que pasa, por lo que un test que simplemente instancia un objeto es suficiente. En otros casos, es necesario que el test incluya una aserción y si no se hace ninguna considera que el test no pasa.

En cualquier caso, el objetivo de esta fase es lograr que el test se ejecute con éxito.

Con la kata Prime Factors estudiaremos el modo en que puede cambiar el código de producción para incorporar nueva funcionalidad.

### Refactorizar si es posible

Cuando se ha logrado hacer pasar cada test debemos examinar el trabajo realizado hasta el momento y comprobar si es posible refactorizar tanto el código de producción como el de test. Aquí aplicamos los principios habituales: si detectamos cualquier *smell*, dificultad para entender lo que ocurre, duplicación de conocimiento, etc. debemos refactorizar el código para ponerlo en mejor estado antes de continuar.

Para ello debemos mantener todos los tests que puede haber pasando. Si alguno alguno de los tests se pone en rojo tendríamos una regresión y habríamos, por así decir, estropeado la funcionalidad.

Tras el primer ciclo es normal no encontrar oportunidades de refactor, pero no te fíes: siempre hay otra manera de ver y hacer las cosas. Por regla general, cuanto antes detectes oportunidades de reorganizar y limpiar el código y lo hagas, más fácil será el desarrollo.

Para profundizar en todo lo que tiene que ver con el refactor al trabajar con la kata Bowling Game.

### Repetir el ciclo varias veces hasta terminar

Una vez que el código de producción hace pasar el test y está lo mejor organizado posible, es el turno de escoger un nuevo aspecto de la funcionalidad y crear un nuevo test que falle para describirlo.

Este nuevo test falla porque el código existente no realiza la nueva funcionalidad deseada y es necesario introducir un cambio. Por tanto, nuestra misión ahora es poner este nuevo test en verde haciendo las transformaciones necesarias en el código que serán pequeñas si hemos sabido dimensionar correctamente nuestros tests anteriores.

Una vez que conseguimos que el nuevo test pase, buscamos las oportunidades de refactor para tener un mejor diseño del código. A medida que avancemos en el desarrollo de la pieza de software veremos que los refactors posibles van siendo más significativos.

En los primeros ciclos comenzaremos con cambios de nombres, extracción de constantes y variables, etc. Luego pasaremos a introducir métodos privados o extraer ciertos aspectos a funciones. En algún momento descubriremos la necesidad de extraer funcionalidad a clases colaboradoras, etc.

Cuando estemos satisfechas con el estado del código repetimos el ciclo mientras nos queda funcionalidad por añadir.

### Cuándo terminar el desarrollo en TDD

La respuesta obvia podría ser: cuando toda la funcionalidad está implementada.

Pero, ¿cómo sabemos esto?

Kent Beck proponía hacer una lista con todos los aspectos que habría que conseguir para considerar completa la funcionalidad. Cada vez que se consigue alguno se tacha de la lista. Es una buena recomendación.

Existe una forma más formal de asegurarnos de que una funcionalidad está completa. Básicamente consiste en no ser capaz de crear un nuevo test que falle. En efecto, si un algoritmo está completamente implementado será imposible crear un test nuevo que pueda fallar.

## Qué no es Test Driven Development

El resultado o outcome de Test Driven Development no es crear un software libre de defectos, aunque se previenen muchos de ellos; ni generar una suite de tests unitarios, aunque en la práctica se obtiene una con una gran cobertura que puede llegar al 100%, aunque como contrapartida puede presentar redundancia. Pero nada de esto es el objetivo de TDD, en todo caso es un efecto colateral.

Los que TDD nos proporciona es una herramienta que:

* Guía el desarrollo del software de una forma sistemática y progresiva.
* Nos permite realizar afirmaciones contrastables sobre si la funcionalidad requerida ha sido implementada o no.
* Nos ayuda a evitar la necesidad de diseñar todos los detalles de implementación anticipadamente, ya que es sí misma es una herramienta de ayuda al diseño de los componentes del software. 

## Beneficios

Varios estudios han mostrado evidencias que apuntan a favor de que la aplicación de TDD tiene beneficios en los equipos de desarrollo. No son evidencias concluyentes, pero las investigaciones realizadas tienden a coincidir en que con TDD:

* Se escriben más cantidad de tests
* El software tiene menos defectos
* La productividad no se ve disminuida, incluso puede aumentar

Es bastante difícil cuantificar el beneficio de usar TDD en cuanto a productividad o velocidad, sin embargo subjetivamente se pueden experimentar varios beneficios.

Uno de ellos es que la metodología TDD puede bajar la cargar cognitiva del desarrollo. Esto es así porque favorece dividir el problema en tareas pequeñas con un foco muy definido, lo que nos permite ahorrar la limitada capacidad de nuestra memoria de trabajo.

## Referencias

* [Test Driven Development](https://en.wikipedia.org/wiki/Test-driven_development)
* [Why Test-driven Development](http://derekbarber.ca/blog/2012/03/27/why-test-driven-development/)
* [Test driven development: empirical body of evidence](https://pdfs.semanticscholar.org/ad0f/dd36aa09d25b739b1649bfa5e20c9e46eb65.pdf)
* [Does Test-Driven Development Really Improve Software Design Quality](https://digitalcommons.calpoly.edu/cgi/viewcontent.cgi?referer=&httpsredir=1&article=1027&context=csse_fac)
* [6 Misconceptions about TDD – Part 1. TDD Brings Little Business Value and Isn’t Worth it](https://www.thedroidsonroids.com/blog/pros-of-tdd-test-driven-development-for-business)