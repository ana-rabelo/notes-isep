---
tag: amqp, rabbitmq, INSIS
---
#### Overview do AMQP 0-9-1 e o modelo AMQP

<span style="color:#8380B6"><b>O que é o AMQP 0-9-1?</b></span>
O Advanced Message Queuing Protocolo é um protocolo de comunicação que permite que os aplicativos cliente se comuniquem com os corretores de middleware brokers.

<b>Brokers de messagens</b> recebem mensagens dos <i>publishers</i> (aplicações que as publicam, também conhecidos como producers) e as roteiam para os <i>consumers</i> (aplicações que as recebem). Por ser um protocolo de rede, os publishers, consumers e o broker podem residir em máquinas diferentes.

> [!info] O Modelo AMQP 0-9-1 funciona da seguinte maneira
> As mensagens são publicadas pelos <i>publishers</i> e enviadas às *exchanges*. As *exchanges* distribuem cópias das mensagens para filas utilizando regras que são chamadas de *bindings*.
> Então o broker ou envia as mensagens aos *consumers* inscritos nas filas ou os *consumers* "puxam" (*fetch/pull*) as mensagens das filas por demanda.

Quando publicam uma mensagem, *producers* podem especificar vários atributos para ela, também chamados de **metadados da mensagem.**

O modelo do AMQP tem a implementação de _acknowledgements_ de mensagens, isso quer dizer que, quando uma mensagem é enviada a um *consumer*, o *consumer* notifica o *broker*, o que pode ser feito automaticamente ou desenvolvido posteriormente. Quando os *acknowledgements* de mensagens estão em uso, o *broker* só irá remover completamente a mensagem da fila quando receber uma notificação para aquela mensagem (ou grupo de mensagens).

Em algumas situações, por exemplo, quando uma mensagem não pode ser roteada, ela pode retornar ao *publisher*, ser *droppada* ou, se o *broker* implementar uma extensão, colocada na "***dead letter queue***". *Publishers* escolhem como lidar com situações como essa publicando as mensagens usando certos parâmetros.

<span style="color:#8380B6"><b>AMQP 0-9-1 é um Protocolo Programável...</b></span>
...no sentido que as entidades e esquemas de roteamento são inicialmente definidos pelas aplicações, não pelo administrador *broker*.
Consequentemente, provisão é feita para operações de protocolo que declaram filas e trocas, definam ligações entre elas, se inscrevam em filas e assim por diante.

A aplicação dsenvolve muita liberdade mas também requere que eles fiquem alertas para potenciais conflitos de definição. Na prática, esses conflitos de definição são raros e geralmente indicam uma má configuração.

Aplicações declaram as entidades de AMQP 0-9-1 que eles precisam, definem os esquemas de roteamento necessários e podem escolher deletar as entidades AMPQ 0-9-1 que não estão mais sendo usadas.

<span style="color:#8380B6"><b>Exchanges e Tipos de Exchanges</b></span>
*Exchanges* são entidades AMQP 0-9-1 para onde as mensagens são enviadas. Elas pegam a mensagem e roteam em zero ou mais filas. O algoritmo de roteamento usado depende do ***tipo de exchange*** e regras chamadas ***bindings***. Existem quatro tipos:
| **Tipo exchange** |   **Nomes padrão pré-declarados**    |
|:-----------------:|:------------------------------------:|
|  Direct exchange  |     (Empty string) e amq.direct      |
|  Fanout exchange  |              amq.fanout              |
|  Topic exchange   |              amq.topic               |
| Headers exchange  | amq.match (e amq.headers no RabbitMQ |                                          

Além do tipo, exchanges são declaradas com um número de atributos, os mais importantes são:
- Nome
- Durabilidade (exchanges sobrevivem ao restart do broker)
- Auto-delete (exchange são excluídas quando a ultima fila é vinculada a ela)
- Argumentos (opcional, usados por plugins e por recursos específicos do broker)

Exchanges podem ser duráveis ou transitórias. Exchanges duráveis sobrevivem ao reínicio do broker enquanto as transitórias não. Nem todos os cenários e casos de uso requerem as exchanges para serem duráveis.

<span style="color:#8380B6"><b>Exchanges Padrão</b></span>
A exchange padrão é uma exchange direta sem nome (string vazia) pré-declarada pelo broker. Tem uma propriedade especial que a torna muito útil para aplicações simples: toda fila que é criada é automaticamente vinculada à exchange por uma chave de roteamento que é a mesma do nome da fila.

Por exemplo, quando você declara uma fila com o nome de "search-indexing-online", o broker do AMQP 0-9-1 vai se vincular à exchange padrão usando "search-indexing-online" como chave de roteamento. Além disso, uma mensagem publicada na exchange padrão com a chave "search-indexing-online" vai ser roteada para a fila "search-indexing-online". Em outras palavras, a exchange padrão faz parecer que é possível entregar mensagens diretamente à filas, mesmo que não seja isso, tecnicamente.

<span style="color:#8380B6"><b>Direct Exchange</b></span>
Entrega mensagens para filas baseadas na chave de roteamento da mensagem. Uma exchange direta é ideal para mensagens de roteamento unicast. Funciona assim:
- Uma fila vincula-se à exchange com uma chave de roteamento K
- Quando uma mensagem com a chave R chega na exchange direta, a exchange rotea a mensagem para a fila se K = R

São usualmente utilizadas para distribuir tarefas entre múltiplos workers (instâncias da mesma aplicação) em um round robin manner. Quando faz isso, é importante entender isso, no AMQP 0-9-1, mensagens são carregadas balanceando entre *consumers* e não entre filas.

<span style="color:#8380B6"><b>Fanout Exchange</b></span>
Rotea as mensagens para todas as filas que estão vinculadas à ela e a chave de roteamento é ignorada. Se N filas estão vinculadas à exchange, quando uma mensagem é publicada na exchange, uma cópia da mensagem é entregue à todas as N filas. São ideias para mensagens de broadcast.

Porque entrega uma cópia da mensagem para cada fila, seus usos são bem similares:
- Jogos MMP podem usar para atualizar o quadro de líderes ou eventos globais
- Sites de notícia de esportes utilizam para atualizar score para cliente mobile em tempo real
- Sistemas distribuídos podem broascast vários estados e atualizações de configuração
- Chats em grupo podem distribuir mensagens entre participantes. (XMPP pode ser uma escolha melhor)

<span style="color:#8380B6"><b>Topic Exchange</b></span>
Roteam mensagens para uma ou mais filas baseado em combinar uma chave de roteamento e um padrão que é usado para vincular uma fila à uma exchange. É usualmente utilizada para implementar variações de padrão de publish/subscribe. Também utilizadas para multicast mensagens.

Sempre que um problema envolve múltiplos consumers/aplicações que escolhem seletivamente qual tipo de mensagem eles querem receber, o topic exchange deve ser considerado. E.g.:
- Distribuir dados relevantes para localizações geográficos específicos, por exemplo, pontos de venda.
- Tarefas processadas em segundo plano por múltiplos workers, cada um capaz de lidar com um conjunto específico de tarefas
- Atualização de estoque de preços
- Atualização de notícias que envolvem categorização ou tagging
- Orquestração de serviçoes de diferentes tipos na nuvem
- Arquitetura distribuída/software específico para OS

<span style="color:#8380B6"><b>Headers Exchange</b></span>
É ideal para roteamento de múltiplos atributos que são mais facilmente expressados como headers de mensagem do que uma chave de roteamento. Eles ignoram o atributo de chave. Em invés disso, os atributos usados para roteamento são pegos do atributo headers. Uma mensagem é considerada combinada se o valor da header é igual ao valor especificado na ligação.

É possível ligar uma fila a uma header exchange usando mais que uma header para combinar. Nesse caso, o broker precisa de mais uma informação da aplicação desenvolvedora, ou seja, deve considerar mensagens com qualquer dos headers correspondentes, ou todos eles?
Isso é pra que serve a ligação ``x-match``. Quando ``x-match`` é definido para ``any``, apenas um valor correspondente de header é suficiente. Alternativamente, definir o ``x-match`` para ``all`` significa que todos os valores devem combinar.

Para ``any`` e ``all``, headers começando com a string ``x-`` não vão ser utilizadas para avaliar combinações. Definindo ``x-match`` para ``any-with-x`` ou ``all-with-x`` também vão usar as headers que começam com a string `x-` para avaliar combinações.

Headers exchanges podem ser consideradas como "direct exchanges com esteróides". Porque suas rotas são baseadas nos valores da header, eles podem ser usadas como direct exchanges quando a chave de roteamento não precisa ser uma string; pode ser um inteiro ou um hash(dicionário), por exemplo.

<span style="color:#8380B6"><b>Filas (Queues)</b></span>
Filas no AMQP 0-9-1 armazenam mensagens que são consumidas pelas aplicações. Filas compartilham algumas propriedades com exchanges, mas também tem algumas propriedades adicionais:
- Nome
- Durabilidade
- Exclusividade
- Auto-delete
- Argumentos
Antes de uma fila ser utilizada, ela precisa ser declarada. Declarar uma fila vai fazer com que ela seja criada se ela não existir. A declaração não vai ter efeito se a fila já existe e seus atributos são os mesmo utilizados na declaração. Quando os atributos de uma fila existente não são os mesmos que na declaração, uma exceção com códido 406 (`PRECONDITION_FAILED`) vai ser lançada.
<span style="color:#8380B6"><b>Nomes das filas</b></span>
Aplicações devem escolher os nomes das filas ou pedir para o broker gerar um nome à elas. Os nomes das filas podem ter até 255 bytes de UTF-8 caracteres.
Um broker AMQP 0-9-1 pode gerar um nome único para a fila. Pra usar este recurso, passe uma string vazia como argumento de nome da fila.

Nome de filas começando com "amq." são reservadas para uso interno do broker. Tentativas de declarar uma fila com um nome que viole essa regra vai resultar é uma exceção com o código 403 (`ACCESS_REFUSED`).
<span style="color:#8380B6"><b>Durabilidade das filas</b></span>
Filas podem ser declaradas como duráveis ou transitórias. Os metadados de uma fila durável são armazenados no disco, enquanto os da fila transitória são armazenadas na memória quando possível.

Em ambientes e casos de uso onde a durabilidade é importante, aplicações devem usar filas duráveis e se certificar de que a marca de publicação tenha publicado as mensagens como persistidas.

<span style="color:#8380B6"><b>Bindings</b></span>
São regras que as exchanges usam (além de outras coisas) para rotear mensagens para filas. Para instruir uma exchange E para rotear messagens para uma fila Q, Q deve ser ligada a E. Bindings devem ter uma chave de roteamente opcional usada por alguns tipos de exchange.
O propósito da chave de roteamento é selecionar certos publishers para uma exchange ser roteada para a fila ligada. Em outras palavras, a chave de roteamento é como um filtro.

Pra ilustrar uma analogia:
- Fila é como o seu destino em Nova York.
- Exchange é como o aeroporto JFK.
- Bindings são os caminhos do JFK para o destino. Podem existir zero ou mais caminhos para chegar.

Ter essa camada de indireção permite rotear os cenários que são impossíveis ou muito difíceis de implementar usando publicação direta às filas e também elimina certa quantidade de trabalho duplicado que os desenvolvedores da aplicação precisam fazer.
Se uma mensagem não puder ser roteada para nenhuma fila ( por exemplo, porque não há vinculações para a troca que ela foi publicada para ) ou ela é excluída ou retorna ao publisher, dependendo dos atributos da mensagem que o editor definiu.

<span style="color:#8380B6"><b>Consumers</b></span>
Armazenando mensagens em filas é inútil a menos que as aplicações possam consumi-las. No modelo AMQP 0-9-1, existem duas formas das aplicações fazerem isso:
1. Se inscrever para receber messagens entregues para elas ("push API"): essa é a opção recomendada
2. Polling ("pull API"): essa forma é **altamente ineficiente** e **deve ser evitada** na maioria dos casos.
Com o `push API`, as aplicações tem que indicar interesse em consumir mensagens de uma fila particular. Quando fazem isso, dizemos que elas se *registraram um consumer* ou, de forma mais simples, *se inscreveram em uma fila*. É possível tem mais que um consumer por fila ou registrar um consumer exclusivo.

Cada consumer (inscrição) tem um identificador chamado *consumer tag*. E pode ser usado para cancelar a inscrição de mensagens. As *consumer tag* são apenas strings.

<span style="color:#8380B6"><b>Message Acknowledgments</b></span>
Aplicações consumers (aplicações que recebem e processam mensagens) podem ocasionalmente falhar em processar mensagens individuais ou vão crashar algumas vezes. Também tem a possibilidade de problemas de conexão causando problemas. Isso levanta uma questão: **Quando o broker deve remover as mensagens das filas?** O AMQP 0-9-1 dá aos consumers controle sobre isso. Existem dois modos de acknowledgement:
- Depois que o broker envia uma mensagem para uma aplicação (usando ou o método `basic.deliver` ou `basis.get-ok`)
- Depois que a aplicação envia de volta um acknowledgement (usando o método `basic.ack`)





