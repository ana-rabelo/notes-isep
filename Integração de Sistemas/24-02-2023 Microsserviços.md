---
tag: amqp, rabbitmq, INSIS
---
#### Overview do AMQP 0-9-1 e o modelo AMQP

<span style="color:#69385C"><b>O que é o AMQP 0-9-1?</b></span>
O Advanced Message Queuing Protocolo é um protocolo de comunicação que permite que os aplicativos cliente se comuniquem com os corretores de middleware brokers.

<b>Brokers de messagens</b> recebem mensagens dos <i>publishers</i> (aplicações que as publicam, também conhecidos como producers) e as roteiam para os <i>consumers</i> (aplicações que as recebem). Por ser um protocolo de rede, os publishers, consumers e o broker podem residir em máquinas diferentes.

O Modelo AMQP 0-9-1 funciona da seguinte maneira:
> As mensagens são publicadas pelos <i>publishers</i> e enviadas às *exchanges*. As *exchanges* distribuem cópias das mensagens para filas utilizando regras que são chamadas de *bindings*.
> Então o broker ou envia as mensagens aos *consumers* inscritos nas filas ou os *consumers* "puxam" (*fetch/pull*) as mensagens das filas por demanda.

Quando publicam uma mensagem, *producers* podem especificar vários atributos para ela, também chamados de **metadados da messagem.**

O modelo do AMQP tem a implementação de _acknowledgements_ de mensagens, isso quer dizer que, quando uma mensagem é enviada a um *consumer*, o *consumer* notifica o *broker*, o que pode ser feito automaticamente ou desenvolvido posteriormente. Quando os *acknowledgements* de mensagens estão em uso, o *broker* só irá remover completamente a mensagem da fila quando receber uma notificação para aquela mensagem (ou grupo de mensagens).

Em algumas situações, por exemplo, quando uma mensagem não pode ser roteada, ela pode retornar ao *publisher*, ser *droppada* ou, se o *broker* implementar uma extensão, colocada na "***dead letter queue***". *Publishers* escolhem como lidar com situações como essa publicando as messagens usando certos parâmetros.

<span style="color:#69385C"><b>AMQP 0-9-1 é um Protocolo Programável</b></span>

