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


