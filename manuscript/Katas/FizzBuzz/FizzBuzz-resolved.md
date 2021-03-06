# Resolviendo la kata Fizz Buzz

## Enunciado de la kata

Nuestro objetivo será escribir un programa que imprima los números del 1 al 100 de tal manera que:

* si el número es divisible por 3 devuelve **Fizz**.
* si el número es divisible por 5 devuelve **Buzz**.
* si el número es divisible por 3 y 5 devuelve **FizzBuzz**.

## Lenguaje y enfoque

Esta kata la vamos a hacer en Python con `unittest` como entorno de testing. La idea es crear una clase `FizzBuzz`, que tendrá un método `generate` para crear la lista, de modo que se usará más o menos así:

```python
fizzbuzz = Fizzbuzz()
print(fizzbuzz.generate())
```

Para ello creo una carpeta `fizzbuzzkata` y en ella añado el archivo `fixzzbuzz_test.py`.

## Primer test: definir la clase

Lo que nos pide el ejercicio es obtener una lista de los números 1 al 100 cambiando algunos de ellos por las palabras Fizz, Buzz o ambas en caso de cumplirse ciertas condiciones.

Fíjate que no nos pide una lista de cualquier cantidad de números, sino específicamente del 1 al 100. Volveremos a eso dentro de un momento.

Ahora vamos a concentrarnos en ese primer test. Lo menos que podemos hacer es que se pueda instanciar un objeto del tipo FizzBuzz. He aquí un posible primer test:

```python
import unittest


class FizzBuzzTestCase(unittest.TestCase):
    def test_something(self):
        fizzbuzz = FizzBuzz()
        self.assertIsNotNone(fizzbuzz)


if __name__ == '__main__':
    unittest.main()
```

Este primer test debería ser suficiente para fallar, que es lo que dice la segunda ley, y forzarnos a definir la clase para que el test pueda pasar, cumpliendo la tercera ley. En algunos entornos ni siquiera sería necesaria la aserción ya que se puede hacer pasar el test sin ellas. En otros entornos la ausencia de aserciones haría que fallase.

Así que lo lanzamos para ver si es verdad que falla. El resultado, como era de esperar es que el test no pasa, mostrando el siguiente error:

```
NameError: name 'FizzBuzz' is not defined
```

Para hacer que el test pase, tendremos que definir la clase FizzBuzz. Creamos el archivo `fizzbuzz.py` y definimos lo mínimo necesario para que el test pase:

```python
class FizzBuzz:
    pass
```

Y en el test, la importamos:

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):
    def test_can_instantiate(self):
        fizzbuzz = FizzBuzz()
        self.assertIsNotNone(fizzbuzz)


if __name__ == '__main__':
    unittest.main()
```

Al introducir este cambio y ejecutar el test podemos ver que ahora sí pasa y ya estamos en verde.

Hemos cumplido las tres leyes y cerrado nuestro primer ciclo test-código. No hay mucho más que podamos hacer aquí, salvo pasar al siguiente test.

## Segundo test: definir el método generate

La clase `FizzBuzz` no sólo no hace nada, ni siquiera tiene métodos. Hemos dicho que queremos que tenga un método `generate` que devolverá la lista de los números del 1 al 100.

Para forzarnos a escribir el método `generate`, tenemos que escribir un test que lo llame. El método tendrá que devolver algo, ¿no? Específicamente queremos que nos devuelva la lista de números. Pero ahora no hace falta que lo haga con los múltiplos de 3 y 5 convertidos.

El test debe verificar esto, pero debe seguir pasando cuando hayamos desarrollado el algoritmo completo. Lo que podemos verificar es que devuelve una lista de 100 elementos, sin prestar atención a los que contiene exactamente.

Este test nos forzará a crear el método `generate` y que éste devuelva una colección de 100 elementos:

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):
    def test_can_instantiate(self):
        fizzbuzz = FizzBuzz()
        self.assertIsNotNone(fizzbuzz)

    def test_generates_list_of_100_elements(self):
        fizzbuzz = FizzBuzz()
        num_list = fizzbuzz.generate()
        self.assertEqual(100, len(num_list))
        
if __name__ == '__main__':
    unittest.main()

```

Por supuesto, el test falla:

```
AttributeError: 'FizzBuzz' object has no attribute 'generate'
```

Esto nos pone en modo "añadir código de producción" para hacer que el test pase.

```python
class FizzBuzz:
    def generate(self):
        pass
```

Con este código el test sigue sin pasar, pero la clase ya tiene el método requerido:

```
TypeError: object of type 'NoneType' has no len()
```

Ahora mismo, el método devuelve `None`. Nosotros queremos una lista:

```python
class FizzBuzz:
    def generate(self):
        return []
```

Al hacer que generate devuelva una lista, hacemos que el test falle porque no se cumple lo que esperamos: que la lista tenga un cierto número de elementos:

```
AssertionError: 100 != 0
```

Este ya es un error del test. Los anteriores eran básicamente errores equivalentes a los errores de compilación (errores de sintaxis, etc). Por eso es tan importante ver los tests fallar, para utilizar el feedback que nos proporcionan los mensajes de error.

Hacer que el test pase es bastante fácil:

```python
class FizzBuzz:
    def generate(self):
        return [None] * 100
```

Con el test pasando vamos a pensar un poco.

En primer lugar, se puede argumentar que en este test hemos pedido que la respuesta de `generate` cumpla dos condiciones:

* ser de tipo list (o array, o collection)
* tener exactamente 100 elementos

Podríamos haber forzado esto mismo con dos tests aún más pequeños.

A estos pequeños pasos se les suele llamar *baby steps* y lo cierto es que no tienen una medida determinada, sino que dependen de nuestra práctica y experiencia.

Así, por ejemplo, el test que hemos creado es lo bastante pequeño como para no generar un gran salto de código en producción, aunque es capaz de verificar las dos condiciones a la vez.

En segundo lugar, fíjate que hemos escrito tan sólo el código necesario para que se cumpla el test. De hecho devolvemos una lista de 100 elementos `None`, lo cual parece que no tiene mucho sentido pero es suficiente para nuestro objetivo con este test.

En tercer lugar, ya tenemos bastante código escrito, entre test y producción, como para examinarlo y ver si tenemos alguna oportunidad de refactorizar.

La oportunidad más clara de refactor que tenemos ahora mismo es el número mágico 100, que sería una constante de la clase:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        return [None] * self._NUMBERS_IN_LIST
```

Y tenemos una en el código de test. Podríamos instanciar la clase `FizzBuzz` en un único lugar ya que no tenemos que hacer ningún tratamiento especial en cada test.

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

if __name__ == '__main__':
    unittest.main()
```

Hemos terminado un nuevo ciclo en el que ya hemos introducido la fase de refactor.

## El tercer test: generar una lista de números

Nuestra clase `FizzBuzz` ya puede generar una lista de 100 elementos, pero de momento cada uno de ellos es, literalmente, nada. Es hora de escribir un test que nos fuerce a poner números en esa lista.

Para ellos podríamos esperar que la lista generada contiene los números del 1 al 100. Sin embargo tenemos un problema: al final del proceso de desarrollo la lista contendrá los números pero algunos de ellos estarán representados con las palabras **Fizz**, **Buzz** o **FizzBuzz**. Si no tengo esto en cuenta, este tercer test empezará a fallar en cuanto comience a implementar el algoritmo que convierte los números. No parece una buena solución.

Un enfoque más prometedor es: ¿qué números no se verán afectados por el algoritmo? Pues aquellos que no sean múltiplos de 3 o de 5, por lo que podríamos escoger algunos de ellos para verificar que se incluyen en la lista sin transformar.

El más sencillo de todos es el 1, que debería figurar en la primera posición de la lista. Por razones de simetría vamos a hacer que los números se generen como strings.

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('1', num_list[0])

if __name__ == '__main__':
    unittest.main()

```

El test es muy pequeño y falla:

```
AssertionError: '1' != None
```

¿Qué cambio podríamos introducir en el código de producción en este punto para que el test pase? El más obvio podría ser el siguiente:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = [None] * self._NUMBERS_IN_LIST
        number_list[0] = '1'
        return number_list
```

Lo cierto es que es suficiente como para pasar el test, así que nos vale.

En principio, no vemos nada en el código que nos de oportunidad de refactorizar, así que vamos a por el siguiente test:

## El cuarto test: seguimos generando números

En realidad todavía no hemos verificado que el método `generate` nos da una lista de números, así que necesitamos seguir creando nuevos tests que nos fuercen a crear ese código.

Vamos a asegurarnos de que en la segunda posición aparece el número '2' que es el siguiente más sencillo que no es múltiplo de 3 o de 5:

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('1', num_list[0])

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('2', num_list[1])


if __name__ == '__main__':
    unittest.main()
```

Tenemos un nuevo test y también falla, así que vamos a añadir código en producción para que el test pase:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = [None] * self._NUMBERS_IN_LIST
        number_list[0] = '1'
        number_list[1] = '2'
        return number_list
```

Y el test pasa con esta implementación tan fea.

Pero no importa. De momento nada nos ha forzado a realizar una mejor. Seguramente en tu cabeza se está empezando a formar una idea de cómo proseguir, pero es más importante que te concentres en el próximo test que necesitas.

## El quinto test: generalizar la generación de números

Posiblemente estés pensando que podríamos generalizar el algoritmo que coloca los números en su lugar en la lista. Pero hagámoslo con un test más. Hasta ahora sólo tenemos dos casos que apuntan a que podríamos generar la secuencia.

El siguiente número que no es múltiplo de 3 y 5 es 4.

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('1', num_list[0])

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('2', num_list[1])

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('4', num_list[3])


if __name__ == '__main__':
    unittest.main()

```

Por supuesto fallará. Hagamos pasar el test mediante la implementación más tonta posible:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = [None] * self._NUMBERS_IN_LIST
        number_list[0] = '1'
        number_list[1] = '2'
        number_list[3] = '4'
        return number_list
```

Y ahora que vemos un patrón podríamos hacer algún tipo de generalización aprovechando que estamos en verde. Estamos en fase de refactor y con los tests en verde tenemos libertad para cambiar el código sin afectar al comportamiento que tenemos hasta ahora.

En este caso, la idea es bastante obvia:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = [None] * self._NUMBERS_IN_LIST
        for i in range(self._NUMBERS_IN_LIST):
            number_list[i] = str(i + 1)

        return number_list
```

Si ejecutamos el test, vemos que sigue pasando sin problemas.

Claro que existen formas más pythonicas y compactas, como esta:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100
    
    def generate(self):
        return list(map(lambda num: str(num + 1), range(self._NUMBERS_IN_LIST)))
```

Pero debemos tener cuidado, probablemente estamos adelantándonos demasiado con este refactor y seguramente nos generará problemas cuando intentemos avanzar. Por eso, es preferible mantener una implementación más directa e ingenua y dejar las optimizaciones y estructuras más avanzadas para cuando el comportamiento del método esté completamente definido.

## El sexto test: aprendiendo a decir "Fizz"

Es hora de que nuestro `FizzBuzz` sea capaz de convertir el 3 en "Fizz". Un test mínimo para especificarlo sería el siguiente:

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('1', num_list[0])

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('2', num_list[1])

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('4', num_list[3])

    def test_number_three_is_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('Fizz', num_list[2])


if __name__ == '__main__':
    unittest.main()

```

Teniendo un test que falla, veamos qué código de producción mínimo podríamos añadir para que pase:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = [None] * self._NUMBERS_IN_LIST
        for i in range(self._NUMBERS_IN_LIST):
            number_list[i] = str(i + 1)
            if (i+1) == 3:
                number_list[i] = 'Fizz'

        return number_list
```

Hemos añadido un `if` que hace pasar el test. Vemos que hay una pequeña oportunidad de refactorizar. 

Por un lado, podemos usar una forma más *pythónica* de gestionar una lista, inicializándola como objeto `list` y añadiendo elementos en cada iteración del rango. Esto nos obliga a cambiar un poco la estructura del código.

Por otro lado, la función `range` es un generador que devuelve números enteros. En este caso sólo queremos que genere 100 números, de modo que empieza por 0 y genera números consecutivos hasta 99. Esto nos obliga a diferenciar entre el número natural que nos interesa mostrar y el número de posición que ocupa en la lista (que es *zero indexed*, es decir, empieza en cero).

`range` puede aceptar parámetros para indicar el inicio y el final de la generación de números, pero excluye el límite superior. Es decir, para obtener los números de 1 a 100, tendríamos que usar `range(1,100 + 1)`. Con todo, podemos tener una lectura más clara:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = list()
        for number in range(1, self._NUMBERS_IN_LIST + 1):
            if number == 3:
                number_list.append('Fizz')
            else:
                number_list.append(str(number))

        return number_list
```

Y todavía podemos mejorarlo un poco más:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = list()
        for number in range(1, self._NUMBERS_IN_LIST + 1):
            if number == 3:
                number_list.append('Fizz')
                continue
            number_list.append(str(number))

        return number_list
```

## El séptimo test: diciendo "Fizz" cuando toca

Ahora queremos que nos ponga un "Fizz" cuando el número es múltiplo de 3 y no sólo cuando es exactamente 3. Por supuesto, nos toca añadir un test para especificarlo:

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('1', num_list[0])

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('2', num_list[1])

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('4', num_list[3])

    def test_number_three_is_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('Fizz', num_list[2])

    def test_multiples_of_three_are_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('Fizz', num_list[5])


if __name__ == '__main__':
    unittest.main()

```

Para hacer pasar el test el cambio que hay que hacer es bastante pequeño. Tenemos que cambiar la condición para ampliarla a los múltiplos de tres:

```python
class FizzBuzz:
    def generate(self):
        num_list = list()
        for number in range(1, 101):
            if number % 3 == 0:
                num_list.append('Fizz')
                continue
            num_list.append(str(number))

        return num_list
```

En el código de producción no tenemos nada muy interesante que hacer, pero resulta incómoda la forma en que hacemos el test. A medida que introducimos números se hace un poco difícil de leer y raro consultar el número por su posición (que es el número menos 1). Así que vamos a hacer un cambio en el test que nos hará verlo de forma más sencilla.

Crearemos un método que verifique que un número aparece en la lista sin cambios, una especie de `assert` personalizado.

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 1)

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 2)

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 4)

    def test_number_three_is_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('Fizz', num_list[2])

    def test_multiples_of_three_are_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual('Fizz', num_list[5])

    def assertSameNumber(self, theList, number):
        self.assertEqual(str(number), theList[number-1])

if __name__ == '__main__':
    unittest.main()

```

Y hagamos también otro que verifique el número se traduce como Fizz:

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 1)

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 2)

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 4)

    def test_number_three_is_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 3)

    def test_multiples_of_three_are_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 6)

    def assertSameNumber(self, theList, number):
        self.assertEqual(str(number), theList[number-1])

    def assertFizz(self, theList, number):
        self.assertEqual('Fizz', theList[number-1])

if __name__ == '__main__':
    unittest.main()

```

Con estos cambios, el test es ahora mucho más legible, evitándonos la carga cognitiva de traducir sobre la marcha entre números y posiciones.

## El octavo test: aprendiendo a decir "Buzz"

Con el cambio que acabamos de hacer nos damos cuenta de que realmente no necesitamos probar más casos de múltiplos de tres porque no nos darían nueva información. Este test nos permite especificar el nuevo comportamiento. Fíjate que hemos añadido también un método para testear que devuelve 'Buzz':

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 1)

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 2)

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 4)

    def test_number_three_is_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 3)

    def test_multiples_of_three_are_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 6)

    def test_number_five_is_listed_as_Buzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertBuzz(num_list, 5)

    def assertSameNumber(self, theList, number):
        self.assertEqual(str(number), theList[number-1])

    def assertFizz(self, theList, number):
        self.assertEqual('Fizz', theList[number-1])

    def assertBuzz(self, theList, number):
        self.assertEqual('Buzz', theList[number-1])

if __name__ == '__main__':
    unittest.main()
```

Y modificamos el código de producción para lograr que pase el test:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = list()
        for number in range(1, self._NUMBERS_IN_LIST + 1):
            if number % 3 == 0:
                number_list.append('Fizz')
                continue
            if number == 5:
                number_list.append('Buzz')
                continue
            number_list.append(str(number))

        return number_list
```

No hay mucho más que podamos hacer ahora, salvo pasar al siguiente test.

## El noveno test: diciendo "Buzz" cuando toca

A estas alturas el test es bastante obvio, el siguiente múltiple de 5 es 10:

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 1)

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 2)

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 4)

    def test_number_three_is_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 3)

    def test_multiples_of_three_are_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 6)

    def test_number_five_is_listed_as_Buzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertBuzz(num_list, 5)

    def test_multiples_of_five_are_listed_as_Buzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertBuzz(num_list, 10)

    def assertSameNumber(self, theList, number):
        self.assertEqual(str(number), theList[number-1])

    def assertFizz(self, theList, number):
        self.assertEqual('Fizz', theList[number-1])

    def assertBuzz(self, theList, number):
        self.assertEqual('Buzz', theList[number-1])

if __name__ == '__main__':
    unittest.main()

```

Y, de nuevo, el cambio en el código de producción es simple:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = list()
        for number in range(1, self._NUMBERS_IN_LIST + 1):
            if number % 3 == 0:
                number_list.append('Fizz')
                continue
            if number % 5 == 0:
                number_list.append('Buzz')
                continue
            number_list.append(str(number))

        return number_list
```

Llegados a esta punto, podemos ver similitudes en la estructura del código al tratar cada caso. Esto apunta a una posibilidad de refactor bastante potente. Sin embargo, aún nos queda un caso por especificar e implementar como son el 15 y sus múltiplos, que tendrían que salir representados en la lista como *FizzBuzz*.

## El décimo test: aprender a decir "FizzBuzz"

La estructura es exactamente igual. Empecemos por el caso más sencillo: 15 debe devolver FizzBuzz (añadimos también el método assertFizzBuzz):

```php
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 1)

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 2)

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 4)

    def test_number_three_is_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 3)

    def test_multiples_of_three_are_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 6)

    def test_number_five_is_listed_as_Buzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertBuzz(num_list, 5)

    def test_multiples_of_five_are_listed_as_Buzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertBuzz(num_list, 10)

    def test_number_fifteen_is_listed_as_FizzBuzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizzBuzz(num_list, 15)

    def assertSameNumber(self, theList, number):
        self.assertEqual(str(number), theList[number-1])

    def assertFizz(self, theList, number):
        self.assertEqual('Fizz', theList[number-1])

    def assertBuzz(self, theList, number):
        self.assertEqual('Buzz', theList[number-1])

    def assertFizzBuzz(self, theList, number):
        self.assertEqual('FizzBuzz', theList[number-1])

if __name__ == '__main__':
    unittest.main()

```

El nuevo test falla. Hagámoslo pasar:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = list()
        for number in range(1, self._NUMBERS_IN_LIST + 1):
            if number % 3 == 0:
                number_list.append('Fizz')
                continue
            if number % 5 == 0:
                number_list.append('Buzz')
                continue
            if number == 15:
                number_list.append('FizzBuzz')
                continue
            number_list.append(str(number))

        return number_list
```

Pero este test no pasa. Y no lo hace porque en este caso, el orden importa. 15 es múltiplo de 3 y 5 por lo cual se ve afectado por la regla del 3 que se aplica antes. Necesitamos cambiar el orden de modo que el test sí pueda pasar:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = list()
        for number in range(1, self._NUMBERS_IN_LIST + 1):
            if number == 15:
                number_list.append('FizzBuzz')
                continue
            if number % 3 == 0:
                number_list.append('Fizz')
                continue
            if number % 5 == 0:
                number_list.append('Buzz')
                continue
            number_list.append(str(number))

        return number_list
```

## El décimo primer test: decir "FizzBuzz" cuando toca

Necesitamos un test:

```python
import unittest

from fizzbuzzkata.fizzbuzz import FizzBuzz


class FizzBuzzTestCase(unittest.TestCase):

    def setUp(self):
        self.fizzbuzz = FizzBuzz()

    def test_can_instantiate(self):
        self.assertIsNotNone(self.fizzbuzz)

    def test_generates_list_of_100_elements(self):
        num_list = self.fizzbuzz.generate()
        self.assertEqual(100, len(num_list))

    def test_number_one_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 1)

    def test_number_two_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 2)

    def test_number_four_is_listed_as_itself(self):
        num_list = self.fizzbuzz.generate()
        self.assertSameNumber(num_list, 4)

    def test_number_three_is_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 3)

    def test_multiples_of_three_are_listed_as_Fizz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizz(num_list, 6)

    def test_number_five_is_listed_as_Buzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertBuzz(num_list, 5)

    def test_multiples_of_five_are_listed_as_Buzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertBuzz(num_list, 10)

    def test_number_fifteen_is_listed_as_FizzBuzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizzBuzz(num_list, 15)

    def test_multiples_of_fifteen_are_listed_as_FizzBuzz(self):
        num_list = self.fizzbuzz.generate()
        self.assertFizzBuzz(num_list, 30)

    def assertSameNumber(self, theList, number):
        self.assertEqual(str(number), theList[number-1])

    def assertFizz(self, theList, number):
        self.assertEqual('Fizz', theList[number-1])

    def assertBuzz(self, theList, number):
        self.assertEqual('Buzz', theList[number-1])

    def assertFizzBuzz(self, theList, number):
        self.assertEqual('FizzBuzz', theList[number-1])

if __name__ == '__main__':
    unittest.main()
```

Hagámoslo pasar:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    def generate(self):
        number_list = list()
        for number in range(1, self._NUMBERS_IN_LIST + 1):
            if number % 15 == 0:
                number_list.append('FizzBuzz')
                continue
            if number % 3 == 0:
                number_list.append('Fizz')
                continue
            if number % 5 == 0:
                number_list.append('Buzz')
                continue
            number_list.append(str(number))

        return number_list
```

¡Y ya tenemos nuestro "FizzBuzz"!

## Finalizando

Hemos completado el desarrollo del comportamiento especificado de la clase FizzBuzz. De hecho, cualquier test que añadamos ahora nos confirmará que el algoritmo es lo bastante general como para que todos los casos estén cubiertos. Es decir, no hay un test concebible que nos pueda obligar a introducir más código de producción: no hay nada más que debamos hacer.

En un caso de trabajo real este código sería funcional y entregable. Pero ciertamente podemos mejorarlo todavía. Todos los tests pasando indican que el comportamiento deseado está implementado, así que podríamos refactorizar sin miedo.

Por ejemplo, el siguiente refactor nos abre la puerta a la posibilidad de añadir nuevas reglas con mucha facilidad al separar su definición de su aplicación. Hay que señalar que utiliza algunas características específicas de Python, pero si haces la kata en otros lenguajes seguramente encontrarás soluciones equivalentes:

```python
class FizzBuzz:

    _NUMBERS_IN_LIST = 100

    rules = {
        15: 'FizzBuzz',
        3: 'Fizz',
        5: 'Buzz'
    }

    def __init__(self):
        self.number_list = list()

    def generate(self):
        for number in range(1, self._NUMBERS_IN_LIST + 1):
            for divisor in self.rules.keys():
                if number % divisor == 0:
                    self.number_list.append(self.rules[divisor])
                    break
            else:
                self.number_list.append(str(number))

        return self.number_list
```

¿Se podría haber realizado este refactor antes? Seguramente sí, pero es nuestra primera kata y nuestro objetivo era habituarnos a los ciclos fundamentales de TDD. Si ahora repites la kata es posible que tomas otras decisiones en algunas puntos y que encuentres mejores soluciones.

## Qué hemos aprendido con esta kata

* Con esta kata hemos introducido las leyes de TDD
* También hemos introducir el ciclo red->green->refactor
* Hemos aprendido a introducir tests mínimos para hacer avanzar el código de producción
