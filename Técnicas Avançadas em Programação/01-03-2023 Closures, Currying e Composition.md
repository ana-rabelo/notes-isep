### Closures
Uma <mark style="background: #BBFABBA6;">closure</mark> é uma **função**, que o valor do **retorno depende do** valor de **uma ou mais variáveis declaradas fora** desta função.
```scala
def func( l: List[Int] ) =
	def innerFunc(f:Int => Boolean) =
		l.filter(f)
	innerFunc
```
A chamada da função ``func()`` resulta em um closure de ``innerFunc`` com respeito a list ``l``.
Retorna uma função em que ``l`` é instanciada.

### Currying
Uma função <mark style="background: #FFB8EBA6;">curried</mark> **transforma** uma função que recebe dois (múltiplos) argumentos em uma função que recebe apenas um (single) argumento.

Uma função regular que soma dois inteiros:
```scala
def plainOldSum(x: Int, y: Int) = x + y
plainOldSum(1, 2)
```
Uma função similar que foi curried:
```scala
def curriedSum(x: Int, y: Int) = x + y
curriedSum(1)(2)
```
Também é possível aplicar a função parcialmente:
```scala
val f = curriedSum(1) _
f(2) // res2 : Int = 3
```
Os últimos parâmetros são substituídos com underscore.

```scala
def curry[A,B,C](f: (A, B) => C): A => (B => C) = 
	(a => ( b => f(a,b)))
```
A ideia por trás do `currying` é tornar mais fácil a composição de funções. Em vez de passar todos os argumentos de uma vez só para uma função, você pode passá-los em várias etapas, o que pode tornar o código mais legível e fácil de entender.

Vamos implementar o ``uncurry``, que reverte a transformação de curry.
Uma vez que => associa com a direita. A => (B => C) pode ser escrito como A => B => C.
```scala
def uncurry[A,B,C](f: A => B => C): (A, B) => C = 
	(a , b) => f(a)(b)
```

### Composition
A composição é uma forma de uma função se juntar com outras funções. Durante a composição uma função guarda a referência de outra função.
Para compose duas funções ``f`` e ``g``, nós simplesmente dizemos:
	**g compose f** (é igual a **g ○ f**)
Também disponibiliza o método ``andThen``
	**f andThen g** é igual a **g compose f**

```scala
def compose[A,B,C](f: B => C, g: A => B): A => C = 
	a => f(g(a))
```
