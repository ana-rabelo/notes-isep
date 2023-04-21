▹Muitas abordagens para modelar um domínio existem.
<span style="color:#65B891"><b>Functional Domain Modeling</b></span> vai usar funções como abstrações principais do comportamento do modelo de domínio e tipos de dados abstratos como as entidades e valores de objetos do modelo de domínio.

1. <span style="color:#AD2831"><b>Domain Modeling com Types</b></span>
Mesmo que cada modelo de domínio seja diferente, muitos padrão comuns aparecem repetidamente.
<span style="color:#AD2831">1.1. Simple Values</span>
São os blocos básicos de construção representados por tipos primitivos como string e integers. Mas note que eles não são strings e integers, na verdade. Um expert em domínio não pensa em termos de int e string, ao invés disto, pensa em termos de  conceitos de `OrderId` e `ProductCode`.
<span style="color:#4E878C">▹O conceito de opaque type e extension method serão utilizados para modelar simple values.</span>

<span style="color:#AD2831">1.2. Product Types</span>
Existem grupos de dados relacionados, bem semelhante aos campos de uma tabela. Por exemplo, um `Cliente` terá várias características, como Nome, Endereço, etc.
<span style="color:#4E878C">▹O conceito de final case class será utilizado para modelar os product types.</span>

<span style="color:#AD2831">1.3. Sum Types</span>
Sum Types representam uma escolha em nossa domínio. Por exemplo, um `Request` pode ser uma `Order` ou um `Quote`.
<span style="color:#4E878C">▹O conceito de sealed trait será utilizado para modelar sum types. 
O conceito de enum também pode ser utilizado.</span>

<span style="color:#AD2831">1.4. Processes</span>
Processes que tem *inputs* e *outputs*.
<span style="color:#4E878C">▹O conceito de função será utilziado para modela processes.</span>

2. <span style="color:#AD2831"><b>Domain Modeling Exercise</b></span>
O exercício é um modelo de uma parte de uma máquina de vendas.
A máquina de vendas pega a soma de dinheiro para comprar um produto. Baseado no dinheiro recebido e no preço do produto, se o dinheiro recebido é menor que o preço do produto, a máquina deve emitir um erro de domínio, senão, ela emite o troco.
O processo de emitir o troco é baseado na quantidade de cada moeda que está na máquina. Se o troco não pode ser entregue, ou por dinheiro insuficiente na máquina ou por incapacidade de produzir o troco exato, um erro de domínio deve ser criado.
O retorno positivo do troco vai ser um conjunto (set) de quantidades de cada denominação, o que produz o troco exato.

Modele o processo de dar o troco, baseado na quantidade de cada moeda dentro da máquina, a moeda inserida e o preço do produto. O processo deve retornar ou um erro de domínio ou a quantidade de cada denominação que representa o troco exato.

O resultado esperado é o modelo de domínio, em que estados impossíveis não são representados (e.g. dinheiro negativo), assim como o processo de produzir o troco correto ou o erro de domínio correto.