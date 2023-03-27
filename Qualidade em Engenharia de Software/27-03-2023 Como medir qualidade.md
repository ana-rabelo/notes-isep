▹<span style="color:#65B891"><b>Atributos de Qualidade</b></span> propriedades testáveis ou mensuráveis de um sistema que são utilizados para indicar o quanto um sistema satisfaz as necessidades dos seus *stakeholders*.
▹A série de padrões [ISO/IEC 25000,](https://www.iso.org/obp/ui/#iso:std:iso-iec:25010:ed-1:v1:en) também conhecidos como <span style="color:#65B891"><b>SQuaRE</b></span> (System and Software Quality Requierements and Evaluation).

##### Cenários
▹<span style="color:#65B891"><b>Cenários de atributos de qualidade</b></span> são uma técnica para especificar atributos de qualidade que descrevem um estímulo recebido pelo sistema e uma resposta mensurável a esse estímulo. Cenários são <mark style="background: #FF5582A6;">hipóteses testáveis</mark> e <mark style="background: #FFB86CA6;">falsificáveis</mark> sobre o **comportamento** dos atributos de qualidade do sistema em consideração.
		<span style="color:#4E878C"><b>Cenários "crus"</b></span> (*raw*) são descrições simples que formam a base para cenários de atributos de qualidade mais precisos.
		<span style="color:#4E878C"><b>Cenários formais</b></span> possui as seis partes de um cenário: **stimulus**, **source**, **artifact**, **response**, **response measure** e **environment context**.

``` Javascript 
"Uma aplicação deve mudar os modos de visualização em menos de 5 segundos quando um usuário pressionar o botão de mudar visualização"
1. source: user
2. stimulus: pressionar o botão de mudar
3. environment: o conjunto de circunstâncias em que o cenário acontece
4. artifact: o sistema inteiro
5. response: mudar o modo de visualização
6. response measure: menos de 5 segundos
```

▹Comece com uma lista de atributos de qualidade <span style="color:#65B891"><b>mensuráveis</b></span>, especifique um <span style="color:#65B891"><b>intervalo</b></span>.
▹Requisitos de atributos de qualidade podem ser especificados como <span style="color:#65B891"><b>cenários</b></span> , pois são testáveis e não ambíguos.
▹<span style="color:#65B891"><b>Prioridades </b></span>podem ser especificadas: prioridade de acordo com a importância para o cliente e prioridade de acordo com o grau de risco técnico.

##### ISO/IEC 25010: Eficiência de performance
- <span style="color:#65B891"><b>Time behaviour: </b></span>grau em que os tempos de resposta e processamento e taxa de transferência de um produto ou sistema, ao executar suas funções, atendem aos requisitos.
- <span style="color:#65B891"><b>Resource utilization: </b></span>grau em que as quantidades e tipos de recursos utilizados por produto ou sistema, ao executar suas funções, atendem os requisitos.
- <span style="color:#65B891"><b>Capacidade: </b></span>grau em que os limites máximos de um parâmetro do produto ou sistemas atendem aos requisitos.

##### Medindo performance
É facilmente medida numericamente e pode ser estimada por modelos. Tipicamente duas medidas são consideradas:
<span style="color:#4E878C"><b>Latência</b></span>: o tempo necessário para complementar um pedaço de trabalho
<span style="color:#4E878C"><b>Throughput</b></span>: número de instâncias de um determinado tipo de carga de trabalho que o sistema pode concluir em um tempo definido 
Por exemplo, se estiver processando mensagens de evento de entrada,  a latência é **quanto tempo leva para processar uma única mensagem**, enquanto o throughput é **quantas mensagens um único processador consegue processar em uma unidade de tempo**.

----------------------
#### JMeter
▹[Elementos de um Test Plan](https://jmeter.apache.org/usermanual/test_plan.html) - especifica o que o JMeter vai executar quando for executado.
<span style="color:#4E878C"><b>Thread groups</b></span> é o ponto inicial de qualquer test plan (<span style="color:#65B891"><b>todos os controllers e samplers devem estar em um thread group</b></span>).
Cada thread representa um <span style="color:#4E878C"><b>usuário</b></span> (e.g. setando uma thread group para 1000 simula 1000 usuários).

<span style="color:#4E878C"><b>Samplers</b></span> dizem para enviar *requests* ao servidor e esperar por uma resposta. Eles fazem o trabalho real no Meter e interagem com o servidor ao serem carregados. (FTP Request, HTTP Request, ...)

<span style="color:#4E878C"><b>Listeners</b></span> consomem a informação produzida pelos samplers. Todos armazenam o mesmo dado, a única diferença é a forma como o dado é apresentado.
Mostram detalhes dos requests e respostas e podem mostrar HTML básico e representações XML da resposta.
<span style="color:#65B891"><b>Graph Results</b></span> plota os tempos de resposta em um gráfico
<span style="color:#65B891"><b>View Results Tree</b></span>

<span style="color:#4E878C"><b>Logic controllers</b></span> regulam o curso do test plan, customizando a lógica que o JMeter usa para decidir quando enviar requests.

<span style="color:#4E878C"><b>Assertions</b></span> permitem assertar fatos sobre respostas recebidas do server sendo testado para teste funcional. Eles podem ser adicionados em qualquer Sampler. Algumas assertions para HTTP Request Sampler são: <span style="color:#65B891"><b>Response code</b></span>, <span style="color:#65B891"><b>Response text</b></span> e <span style="color:#65B891"><b>HTML assertion</b></span>.

<span style="color:#4E878C"><b>Elementos de configuração</b></span> trabalham junto com um Sampler. Eles são usados para indicar valores padrão para outras partes do test plan assim como configurar variáveis.
Exemplos são: 
<span style="color:#65B891"><b>HTTP Request Defaults: </b></span>salvam tempo fazendo muito do HTTP
<span style="color:#65B891"><b>HTTP Cookie Manager: </b></span>usado com HTTP Request samplers para enviar cookies automaticamente com o request.

<span style="color:#4E878C"><b>Timers</b></span> permite que seja atrasado um certo período de tempo antes de cada request que uma thread faz quando um timer é especificado.

<span style="color:#4E878C"><b>JMeter testing process</b></span>
- Cria um test plan usando uma thread Group e Samplers, Timers, e outros elementos, como apropriado, para criar um teste script
- Executa o test plan
- Analisa resultados com os listeners

<span style="color:#4E878C"><b>Padrão step ramp-up</b></span>
- Pode ser útil para simular um **aumento gradual de tráfego** de usuários e identificar o breaking point do sistema.
- Simular diferentes variações no **comportamento do usuário** e testar a performance do sistema em diferentes condições.
- Gradualmente aumentar o **carregamento no sistema** e monitorar sua performance através do tempo.
- Pode ajudar a identificar qualquer problema com o **load balancing**.

<span style="color:#4E878C"><b>Padrão steady ramp-up</b></span>
Ideal para simular um aumento gradual no tráfego ao longo do tempo, ao invés de um aumento repentino.

<span style="color:#4E878C"><b>Padrão peak-rest</b></span>
- Em um plataforma de jogo online que experencia pico de tráfego durante eventos específicos, como lançamentos do jogo
- Um sistema de saúde que experencia pico de tráfego durante períodos específicos, como uma pandemia
- Um sistema de venda de ingressos que experencia pico de tráfego durante eventos específicos, como shows ou jogos de esporte

Outros padrões são <span style="color:#65B891"><b>padrão sine wave ramp-up</b></span> em que o o tráfego pode variar durante o dia (aumenta e diminui em um padrão de onda senoidal); <span style="color:#65B891"><b> padrão sawtooth ramp-up</b></span> para pico de tráfego repentino; <span style="color:#65B891"><b>padrão random ramp-up</b></span>; <span style="color:#65B891"><b>padrão step-up e hold</b></span>, ...