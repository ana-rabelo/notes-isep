O for...yield é uma expressão em Scala que combina a funcionalidade do loop for com a criação de uma coleção de dados usando a palavra-chave yield. Quando usamos a expressão for...yield, o loop itera sobre uma sequência de elementos (por exemplo, uma lista, um array, um conjunto etc.), realiza alguma operação em cada elemento e, em seguida, cria uma nova coleção contendo o resultado dessas operações.

Aqui está um exemplo simples de como usar o for...yield em Scala:

java
Copy code
val numbers = List(1, 2, 3, 4, 5)
val squares = for (n <- numbers) yield n * n
Neste exemplo, temos uma lista chamada numbers contendo cinco elementos (1, 2, 3, 4, 5). Em seguida, criamos uma nova lista chamada squares usando o for...yield. Dentro do loop, declaramos uma variável n que assume o valor de cada elemento da lista numbers a cada iteração. Dentro do loop, multiplicamos cada elemento por si mesmo e usamos a palavra-chave yield para adicionar o resultado à nova lista squares.

No final, a nova lista squares conterá os quadrados de cada elemento da lista numbers. Portanto, se imprimirmos a lista squares, obteremos [1, 4, 9, 16, 25].

A expressão for...yield também pode incluir cláusulas adicionais, como filtros e junções, para realizar operações mais complexas sobre os elementos da sequência.
