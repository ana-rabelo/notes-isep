```scala
object MyModule:
```
Declara um objeto singleton, que declara uma classe e sua instância, simultaneamente.

```scala
object MyModule:
	private def abs(n: Int): Int
		if (n < 0) -n else n
```
Método privado só pode ser chamado por outros membros de ``MyModule``.

```scala
import MyModule.formatAbs

object MainModule:
	def main(args: Array[String]): Unit =
```
``Unit`` tem o mesmo propósito de ``void`` utilizado em Java ou C.

```scala
for (x <- 1 to 10) println(formatAbs(-x))

(1 to 10).foreach(x => println(formatAbs(-x)))

1 to 10 foreach(x => println(formatAbs(-x)))
```
Maneiras de iterar.

#### Funções e Valores
Em Scala, nós temos funções que tem parâmetros e um tipo de retorno, que por sua vez, podem também ser funções.
Funções podem ter estruturas de controle:

```scala
def f() =
	if (something)
		"A"
	else
		"B"
	if (somethingElse) 
		"C" 
	else 
		"D"
```
Apenas C e D são possíveis nesse caso. O primeiro ``if`` não tem consequência.

Também podemos definir valores:
```scala
val v = 3 + 5
```
E os valores também podem ser valores:
```scala
val venv: Int => Boolean = (_ % 2 == 0)
```

```scala
def meven(i:Int) = (i % 2 == 0)
```
Classe anônima com um ``apply`` método. Isso será transformando em uma função cada vez que for chamado.

```scala
def deven: Int => Boolean = (_ % 2 == 0)
```
Referência a uma ``Function class``.

```scala
val veven: Int => Boolean = (_ % 2 == 0)
```
Referência a uma ``Function object`` que será instanciado imediatamente.

```scala
lazy val lveven: Int => Boolean = (_ % 2 == 0)
```
Referência a uma ``Function object`` que será instanciada apenas quando chamada.
