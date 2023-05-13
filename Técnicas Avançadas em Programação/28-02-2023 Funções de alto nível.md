> Funções que recebem outras funções como parâmetros ou retornam outras funções como resultado são chamadas de <mark style="background: #FFB86CA6;">funções de alto nível</mark>.

``` scala
object MyModule:
	private def formatAbs(x: Int) =
		 val msg = "The absolute value of %d is %d."
		 msg.format(x, abs(x))

	private def formatFactorial(n: Int) =
		 val msg = "The factorial of %d is %d."
		 msg.format(n, factorial(n))

	def main(args: Array[String]): Unit =
		 println(formatAbs(-42))
		 println(formatFactorial(7))
```

As funções ``formatAbs`` e ``formatFactorial`` são quase idênticas, logo, podem ser organizadas em uma única função, ``formatResult``, que recebe como argumento a função a ser aplicada.

``` scala
object MyModule:
	def formatResult(name: String, n: Int, f: Int => Int) = 
		val msg = "The %s of %d is %d." msg.format(name, n, f(n))
```

``formatResult`` é uma função de alto nível (HOF) que recebe outra função, chamada ``f``, que é do tipo ``int to int``, que indica que ``f`` espera um argumento do tipo inteiro e também vai retornar um inteiro.

``` scala
/**  
 * Finds the index of the first element for which p is true 
 **/
 def findFirst[A](as: Array[A], p: A => Boolean): Int = {  
	@annotation.tailrec  
	def loop(n: Int): Int =  
	    if (n >= as.length) -1  
	    else if (p(as(n))) n  
	    else loop(n + 1)  
	loop(0)  
}
findFirst(Array(1, 2, 3, 4), (p: Int) => p== 1)
```

O comando ``annotation.tailrec`` faz com que o compilador retorne um erro se o método não conseguir ser otimizado em um loop.

``` scala
/**  
 * Check if an array has an ordering of type ordered 
 */
 def isSorted[A](as: Array[A], ordered: (A,A) => Boolean): Boolean = {  
	@annotation.tailrec  
	def loop(n: Int): Boolean =  
	    if (n >= as.length - 1) true  
	    else if (!ordered(as(n), as(n + 1))) false  
	    else loop(n + 1)  
	loop(0)  
}  
isSorted(Array(1, 2, 3, 4), (x: Int, y: Int) => x < y)
```

Com HOF, as funções são usadas como parâmetros, o que leva à criação de muitas funções pequenas. Não há necessidade de definir (e nomear) essas funções utilizando ``def``.

Funções declaradas sem nome são chamadas de <mark style="background: #FFB86CA6;">funções anônimas</mark>.

- Uma forma de escrever uma função literal é ocultar os **tipos dos parâmetros**:
``` scala
val someNumbers = List(-11, -10, -5, 0, 5, 10) 
someNumbers.filter((x: Int) => x > 0)
```
Já que a função ``filter()`` é chamada sobre uma lista de objetos do tipo ``Int``, o compilador Scala sabe que ``x`` é um inteiro:
``` scala
someNumbers.filter(x => x > 0)
```

- Para torná-la mais concisa, podemos utilizar **underscores como placeholders** para um ou mais parâmetros, desde que cada parâmetro **apareça apenas uma vez**.
```scala
val someNumbers = List(-11, -10, -5, 0, 5, 10)
someNumbers.filter(_ > 0)
```
O underscore é como um "branco" que precisa ser "preenchido". O espaço em branco será preenchido com um argumento da função cada vez que for invocado.
O método ``filter`` vai substituir o branco em ``_ > 0`` com ``-11``, então com ``-10``, depois com ``-5``  e sucessivamente até o final da lista.

##### O que isso faz?

```scala
0 to 3 map(_ + 1) reduce(_ + _)
```
Para cada elemento de 0 a 3, é somado 1. Então cada elemento é somado um ao outro. O resultado é **10**.

``` scala
val s = 0 to 100 filter( _ % 2 == 0 )
s.reduce( _ + _ )/s.size
```
O valor ``s`` guarda todos os valores pares de 0 a 100. Então soma todos esses valores e divide pela quantidade (média).

Algumas vezes, pode ser necessário aplicar parcialmente a função. E.g.:
```scala
def sum(x: Int, y: Int, z: Int): Int = x + y + z
```
Pode ser aplicada parcialmente:
```scala
val v1 = sum(3, _ : Int, 12)

v1(14) // = 29
```
