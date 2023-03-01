> Programação funcional significa focar nas funções, que é um valor de mesmo estado.

### Expressões condicionais
Scala tem uma expressão condicional ``if-else``, que é utilizado para expressões.
```scala
def abs(x: Int) = if (x >= 0) x else -x
```
`` x >= 0``  é um predicado, que é uma função do tipo ``Boolean``. Um ``if/else`` tem um valor.
``Expressões booleanas`` podem ser compostas de constantes (``true`` e ``false``), negação (``!b₁``), conjunção (``b₁ && b₂``), disjunção (``b₁ || b₂``) e as operações de comparação usuais: ``e₁ <= e₂``, ``e₁ >= e₂``, ``e₁ < e₂``, ``e₁ > e₂``, ``e₁ == e₂``, ``e₁ != e₂``, em que ``b₁`` e ``e₁`` são, respectivamente, expressões booleanas e expressões gerais.

O ``else`` não é obrigatório. A sequência de expressões é válida:
``` scala
val x = 2
if (x > 0) 1 // equivalente a "if (x > 0) 1 else ()"
```

1. Escreva uma função ``lessThan`` que recebe dois argumentos que retorna ``true`` se o primeiro argumento é menor que o segundo, caso contrário retorna ``false``. É necessário usar ``if-else``?
```scala
def lessThan(a: Int, b: Int) = (a < b)
```

2. Sem usar || e &&, escreva as funções ``and`` e ``or``.
```scala 
def and(x: Boolean, y: Boolean) = if (x == y) x else false

def or(x: Boolean, y: Boolean) = if (x == y) x else true
```

### Recursão
Uma função recursiva chama ela mesma. O tipo do retorno precisa ser declarado explicitamente. Deve ser utilizadas ao invés de loops.
Exemplo de uma função fatorial recursiva:
``` scala
def fatorial(n: Long): Long =
	if (n == 0) 1 else n * fatorial (n - 1)
```

### Exercícios básicos

1. Escreva uma função recursiva ``sumDown`` com dois argumentos do tipo Int que soma todos os valores entre o valor recebido e zero.
```scala
def sumDown(x: Int, sum: Int): Int = 
	if (x == 0) sum else sumDown(x - 1, sum + x)
```

2. Escreva uma função ``nSymbol`` com três argumentos: o primeiro indica q quantidade de vezes que um símbolo (o segundo argumento) deve ser retornado.
``` scala
def nSymbol(i: Int, c: Char, s: String): String =
	if (i == 0) s else nSymbol(i - 1, c, s + c)
```

3. Escreva uma função ``mult`` que recebe dois argumentos que retorna a multiplicação de dois valores. A multiplicação deve ser computada usando somas. Por exemplo, 4 * 3 = 4 + 4 + 4 = 3 + 3 + 3 + 3
4. O MMC de dois inteiros é definido como o maior número que divide os dois inteiros sem resto. Por exemplo, o MMC de 16 e 28 é 4.
	O algoritmo de Euclide é baseado na observação que, se r é o resto quando a é dividido por b, então o divisor comun de a e b são precisamente o mesmo que b e r. Além disso podemos usar a 