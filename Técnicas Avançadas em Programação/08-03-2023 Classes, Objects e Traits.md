#### Objetos Singleton
> As classes em Scala não podem ter membros estáticos. Em vez disso, existem <mark style="background: #FFB86CA6;">objetos singleton</mark>.

◈ A definição de um objeto *singleton* parece com a definição de uma classe.
◈ Métodos e valores que **não estão associado com instâncias individuais** de uma classes pertencem a objetos *singleton*, que são acessíveis através do próprio objeto.

#### Traits
> Uma <mark >trait</mark> encapsula métodos e definições de campo, que podem ser reutilizados **os misturando** em classes.

◈ Diferente de herança, em que cada classe deve herdar de apenas uma superclasse, uma **classe pode se misturar em qualquer quantidade de traits**.
◈ Traits são uma forma de herdar de **múltiplos construtores**.
 ```scala
 class Animal { print("Animal-") }
 trait Furry extends Animal { print("Furry-") }
 trait HasLegs extends Animal { print("HasLegs-") }
 trait FourLegged extends HasLegs { print("FourLegged-") }
 class Cat extends Animal with Furry with FourLegged {
	  print("Cat-") 
 }
new Cat
```
◈ Uma *trait* pode ter métodos concretos ou abstratos.
◈ *Traits* podem ser parametrizadas com tipos genéricos

#### Case classes
> São classes regulares que **exportam os parâmetros dos seus construtores** e que **fornecem um mecanismo de decomposição recursiva** via pattern matching.

```scala
sealed trait Expr[+A] 
final case class Sum[A](a:Expr[A], b:Expr[A]) extends Expr[A] 
final case class Sub[A](a:Expr[A], b:Expr[A]) extends Expr[A] 
final case class Mul[A](a:Expr[A], b:Expr[A]) extends Expr[A] 
final case class Div[A](a:Expr[A], b:Expr[A]) extends Expr[A] 
final case class Val[A](v: A) extends Expr[A]
```
📢 Ao definir uma classe selada (`sealed`) precisamos garantir que todas as suas sub-classes sejam definidas no mesmo arquivo e, portanto, conhecidas em tempo de compilação. Assim, precisamos tornar todas as sub-classes (`Sum`, `Sub`, `Mul`, ...) da classe em seladas ou finais.

Decompomos usando pattern matching:
```scala
def eval(e: Expr[Int]):Int = e match { 
case Sum(a,b) => eval(a)+eval(b) 
case Sub(a,b) => eval(a)-eval(b) 
case Mul(a,b) => eval(a)*eval(b) 
case Div(a,b) => eval(a)/eval(b) 
case Val(v) => v }

val(Sum(val(1), val(2)))
```

#### Companion Objects
> São objetos *singleton*, que são definidos por uma classe, por convenção com o mesmo nome.
> 💭 Ou seja, se tivermos uma classe chamada `MinhaClasse`, podemos definir um *companion object* com o mesmo nome `object MinhaClasse` no mesmo arquivo e escopo.

> Uma classe compartilha todos os seus acessos com seus companion objects e vice-versa. Podem ser usados para definir construtores alternativos para a classe.
> Uma classes e seus objetos companion deve ser definidas em um mesmo file.

>A principal diferença entre um *companion object* e uma classe normal é que um *companion object* não pode ser instanciado diretamente com o operador `new`. Em vez disso, os métodos e valores definidos no *companion object* podem ser acessados ​​usando o nome da classe como um namespace. 
>Por exemplo, se você definir um método `meuMetodo()` no companion object `MinhaClasse`, você pode acessá-lo usando `MinhaClasse.meuMetodo()`.

```scala
sealed trait Level
case object Full extends Level
case object HalfFull extends Level
case object Empty extends Level

case class Rocket private (fuel: Level) //classe
object Rocket: //companion object
	def from(fuel: Level): Rocket =
		Rocket(fuel)
		
	def fly(r: Rocket) =
		if(r.fuel == Empty) "Cannot Fly!"
		else "Vrummm"
		

val r = Rocket.from(Empty)
Rocket.fly(r)
```
