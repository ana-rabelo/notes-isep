### Scala Test

- [x] PL 02 📅 2023-03-07 ⏫ ⏳

Usaremos o ``AnyFunSuite``, um estilo de teste.
1. Clone ``emptyScala3Project``. Mude o nome em *build.sbt* para "lab02".
Vamos executar um teste simples.
2. Em *src/main/scala* crie um pacote chamado *tap*. Dentro dele, crie um **Scala Object**.
3. Crie a função a seguir:
```scala
package tap 

object StringFunctions: 
	def removeDigits(s: String): String = s.filter(!"0123456789".contains(_)) 
	
	def incDigits(s: String,i: Int) = s.map(x => if (x.isDigit) (x+i).toChar else x)
```

``!`` No IntelliJ, para criar um teste para a função, colocamos o cursor em cima do objeto e pressionamos CTRL+SHIFT+T. Escolhemos "*Create New Test...*" e escolhemos a biblioteca ScalaTest, adicionando a função *removeDigits*.7

4. Agora temos uma *StringFunctionsTest* em *src/test/scala/tap*.
```scala
package tap 
import scala.language.adhocExtensions 
import org.scalatest.funsuite.AnyFunSuite 

class StringFunctionsTest extends AnyFunSuite: 
	test("removeDigits on a blank string should result in a blank string") { 
		assert(StringFunctions.removeDigits("")==="") 
	} 
		
	test("removeDigits on abc95def0ghi should result in abcdefghi ") { 
		assert(StringFunctions.removeDigits("abc95def0ghi")==="abcdefghi")
	}
```
Além do ``assert``, dois outros assertions podem ser usados com ScalaTest: ``assertResult`` para diferenciar esperados para valores concretos e ``intercept`` para assegurar que um pedaço de código throws uma exceção esperada.
O === disponibiliza mais informações na mensagem de erro.

Os testes podem ser executados com **sbt** na janela do terminal. No sbt, podemos enviar **compile**, que compila o projeto. O comando **test** testa todos os testes do projeto.
Ainda temos o comando **console**. Que carrega o projeto e espera pelos comandos. Qualquer função do projeto pode ser executada assim. 

5. Vamos executar: ``tap.StringFunctions.removeDigits("a1b2c3")``.

O modo console pode ser terminado com o comando CTRL-d. Para finalizar o *sbt* usamos o comando **exit**.

### High Order Functions
Funções são valores que podem ser passadas à variáveis e como argumentos para funções. Essas funções são chamadas de <mark style="background: #FFB8EBA6;">funções de alto nível</mark>.
```scala
def resultToString(name: String, n: Int, f: Int=>Int): String = 
	val msg = "The %s of %d is %d." 
	msg.format(name, n, f(n)) 
	
def double(x: Int) = x * 2 
def triple(x: Int) = x * 3 
def quadruple(x: Int) = x * 4 

assert(resultToString("double", 5, double)=="The double of 5 is 10.") 
assert(resultToString("triple", 5, triple)=="The triple of 5 is 15.") 
assert(resultToString("quadruple", 5, quadruple)=="The quadruple of 5 is 20.")
```
A função ``resultToString`` é uma função de alto nível que recebe uma função como seu terceiro argumento ``(f: Int=>Int)``.
A => B recebe um argumento A e retorna um B. 

## Exercise
1. Usando o projeto *lab02*, criamos as funções ``isPrime`` e ``hof``.

``` scala
def hof( f :Int => Boolean, m: Int => Int, xs :List[Int] ): List[Int] = { 
	xs.filter(f).map(m) 
}
```
A função ``hof`` recebe uma função filter ``f`` e uma função mapping ``m``. 
```scala
hof(isEven, square, l)
```
Ela recebe ``l`` (uma lista de inteiros) e filtra os valores pares `isEven` e então aplica a função `square` nestes valores.

### Funções Anônimas
É possível escrever uma função sem dar um nome a ela = função anônima.
Por exemplo ``(x: Int, y: Int) => x + y`` é uma função anônima que soma dois inteiros e retorna um resultado.

#### Exercise
```scala
test("isOdd") {
	assert(( 1 to 15 ).filter((_:Int) % 2 != 0 ) === List(1, 3, 5, 7, 9, 11, 13, 15))
}
test("square") {
    assert(( 1 to 6 ).map( (y:Int) => y*y ) === List( 1, 4, 9, 16, 25, 36))
}
```

## Currying
Currying é uma forma de escrever funções com múltiplos parâmetros listas e é uma poderosa técnica da programação funcional.
Por exemplo,
``def f(x: Int)(y: Int)(z: String)``
é uma função curried com três parâmetros.

Exemplo de uma função não-curried que soma dois parâmetros Int:
```scala
def nonCurriedSum(x: Int, y: Int) = x + y
```
Aqui uma função similar, ams curried:
```scala
def curriedSum(x: Int)(y: Int) = x + y
```
Podemos utilizar a notação *placeholder* para usar o ``curriedSum`` em uma expressão parcialmente aplicada sem definir explicitamente o segundo parâmetro:
```scala
def sum2 = curriedSum(2) _
def sum3 = curriedSum(3) _

sum2(3) //res4: Int = 5
sum3(3) //res5: Int = 6
```
#### Exercise
Considerando a seguinte função:
```scala
def multiply(a: Int)(b: Int):Int = a * b
```
Defina duas funções ``multiply_by_2`` e ``multiply_by_3`` e use a notação placeholder para que o segundo parâmetro não seja explicitamente indicado. Use o ScalaTset para testar as funções.

## Consolidando tudo
Matematicamente, a função que receber um inteiro como argumento e retorna um boolean indicando se o inteiro dado pertence a um *set* é chamado de função característica do set.
Por exemplo, nós podemos caracterizar o set de inteiros negativos pela função característica ``(x: Int) => x < 0``.

Nós escolhemos representar um conjunto por sua função característica e definir um tipo de alias para essa representação:
```scala
type Set = Int => Boolean
```
Usando essa representação, nós definimos uma função que testa a presença de um valor no conjunto:
```scala
def contains(s: Set, elem: Int): Boolean = s(elem)
```

#### Exercises: Funções básicas com *Sets*
1. Defina uma função que cria um *singleton* set a partir de um valor inteiro: o set representa o set do dado elemento.
```scala
def singletonSet(elem: Int): Set = Set(elem)
```
2. Defina as funções ``union``, ``intersect`` e ``diff``, que recebem dois sets e retornam, respectivamente, sua união, interseção e diferença. ``diff(s, t)`` retorna um set que contém todos os elementos do set ``s`` que não estão no set ``t``.
```scala
def union(s: Set, t: Set): Set = s(x) 
```
