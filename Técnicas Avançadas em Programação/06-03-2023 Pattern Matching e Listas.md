### Pattern Matching
> O scala tem um padrão de matching geral que permite fazer a correspondência de qualquer tipo de data com uma **<mark style="background: #FFB8EBA6;">política de first-match</mark>**.

```scala
def matchTest(x: Int): String = x match
	case 1 => "one"
	case 2 => "two"
	case 3 => "three"
```
Também pode ter um *guard*, um predicado que permite ou evita a correspondência.
```scala
...
	case _ => "many"
```
O pattern matching também funciona em tuplas ou listas.
```scala
def matchTest(x: Any): String => x match
	case h::t => "list"
	case (_,_) => "pair"
	case (_,_,_) => "tuple"
	case _ => "other"
```
Quando uma correspondência é complexa, uma **função anônima** pode ser usada.
```scala
Map(1 -> 2, 3 -> 4) map(x => x._1 + x._2)

Map(1 -> 2, 3 -> 4) map(case (k, v) => k + v)
```

- [x] Pesquisar Map em Scala ⏫ ✅ 2023-03-13
> Em Scala, `Map` é uma uma estrutura de dados, que é uma coleção de pares chave-valor imutável.
> Temos também a função `map()` que pode ser aplicada em qualquer coleção (Listas, Arrays, Sets, Maps, etc.) e que recebe como parâmetro uma função que é aplicada a cada elemento da coleção, transformando-a em outra coleção.
```scala
colecao.map(funcao)
```

### Listas
> `scala.List` implementa o **compartilhamento estrutural** (*structural sharing*) para otimizar o uso de memória ao criar novas listas a partir de listas existentes.

```scala
val l1 = List(5, 3, 7) 
val l2 = 9 :: l1 //List[Int] = List(9, 5, 3, 7)
val l3 = 1 :: 8 :: l2 //List[Int] = List(1, 8, 9, 5, 3, 7)
```

A função `containsSlice` retorna se uma lista contém uma outra lista.
```scala
val lst = List(1, 2, 3, 4, 5)

lst containsSlice List(2, 3, 4) //boolean = true
lst containsSlice List(2, 5) //boolean = false
```

`patch()` remove `r` valores a partir do índice `s`. Insere `s` no índice `f`.
```scala
def patch(f: Int, s: GenSeq[A], r: Int): List[A]

val lp = List(1, 2, 3, 4, 5)

lp.patch(2, Seq(99), 2) ///List[Int] = List(1, 2, 99, 5)
```
A função `Seq(99)` cria uma sequência com o elemento 99, apenas.

`groupBy()` agrupa uma lista a partir de uma chave em um map.
```scala
def groupBy[k](f: (A) => K): Map[K, List[A]]

val list = List(("one", "i"), 
				("two", "2"), 
				("two", "ii"), 
				("one", "1"), 
				("four", "iv"))
list.groupBy( (k, v) => k)
```

`reduceLeft` reduz uma lista para um único valor do tipo A ou qualquer subtipo.
```scala
def reduceLeft[B >: A](f: (B, A) => B): B

val lp = List(1,2,3,4,5) 
lp.reduceLeft( _ + _ ) // =15
lp.reduceLeft( _ max _ ) // =5
```

`foldLeft` aplica uma operação a todos os elementos de uma coleção e, ao mesmo tempo, acumula um resultado.
```scala
def foldLeft[B](z: B)(op: (B, A) ⇒ B): B

val lp = List(1,2,3) 
lp.foldLeft(0)( _ + _ ) // =6
//O valor inicial do acumulador é 0 e a função de adição é aplicada a cada iteração, de forma a somar todos os números da lista. 
```
```scala
lp.foldLeft(List[Int]()) ((l,e) => l++(1 to e)
```
A expressão acima chama o método `foldLeft` com o valor inicial de uma lista vazia `List[Int]()`. Considerando a lista `lp = List(1, 2, 3)`, funciona da seguinte forma:
1. Na primeira iteração, o elemento da lista `lp` é `1` e o acumulador atual é a lista vazia. O intervalo de números de `1` a `1` é criado usando o método `to`, resultando na lista `[1]`. Essa lista é o novo valor do acumulador.
2. Na segunda iteração, o elemento da lista `lp` é `2`, e o acumulador atual é `[1]`. O intervalo de números de `1` a `2` é criado usando o método `to`, resultando na lista `[1, 2]`. Essa lista é concatenada com o acumulador atual usando o operador `++`, resultando na lista `[1, 1, 2]`. Essa lista é o novo valor do acumulador.
3. Na terceira e última iteração, o elemento da lista `lp` é `3`, e o acumulador atual é `[1, 1, 2]`. O intervalo de números de `1` a `3` é criado usando o método `to`, resultando na lista `[1, 2, 3]`. Essa lista é concatenada com o acumulador atual usando o operador `++`, resultando na lista `[1, 1, 2, 1, 2, 3]`. Essa lista é o novo valor do acumulador.
4. Quando a iteração termina, o valor final do acumulador é retornado como resultado.

