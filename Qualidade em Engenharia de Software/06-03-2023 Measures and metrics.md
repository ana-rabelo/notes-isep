- CMMI: *<span style="color:#8380B6"><b>Software measurement</b></span>* é requerido
	<mark style="background: #FFB8EBA6;">Prática básica</mark> de engenharia de software

- Definição de **entidade**, **atributo** e **medição**.

Medimos coisas no nosso dia-a-dia  e usamos escalas para isso.

Na engenharia de software, as medições são utilizadas no processo de desenvolvimento e do produto, com os objetivos de **entender**, **controlar** e **melhorar**. 

- Quer dizer que eu não tenho problemas ou que não encontrei problemas? (erros)
-  Objetivo x Subjetivo
		Julgamento humano -> subjetivo
		Mesmo subjetivo, essa métrica é **útil**?
			As métricas devem ser bem escolhidas
			Permite tirar algumas conclusões

▹<mark style="background: #BBFABBA6;">Produto</mark>: resultado final ou intermédio do processo
▹<mark style="background: #ABF7F7A6;">Processo</mark>: analisa várias fases

#### Métrica

▹Métricas subjetivas podem ser úteis, desde que entendemos sua <mark style="background: #FFB86CA6;">imprecisão</mark>
▹Uma métrica é um número ou símbolo designado a uma entidade a mapeando para que caracterize um atributo.
▹<span style="color:#8380B6"><b>Nominal scale type</b></span>
▹<span style="color:#8380B6"><b>Ordinal scale type</b></span>: nós temos classes e categorias, e cada entidade pertence a uma categoria, baseada no valor do atributo. No entanto, nós também temos informação sobre a ordenação de classes ou categorias criando uma escala ordinal.
▹<span style="color:#8380B6"><b>Interval scale type</b></span> é mais poderosa que a nominal ou ordinal; a interpretação da diferença entre valores é a mesma; preserva diferenças e não proporções.
▹<span style="color:#8380B6"><b>Ratio scale type</b></span> fornece mais precisão que as últimas, pois tem zero pontos e um valor pode ser representado como múltiplo do outro. Podemos medir o comprimento de um programa com LOC, milhares ed LOC, o número de caracteres presentes no programa, o número de comandos executáveis, e mais.
▹<span style="color:#8380B6"><b>Absolute scale types</b></span> possui apenas uma transformação admissível: a transformação identidade. Ou seja, tem apenas uma forma da medição ser feita, na forma de `número de ocorrências de x em uma entidade.`

Uso de forma consistente
Termo mais específico
<mark style="background: #FFB8EBA6;">Combinação de métricas</mark>, visão mais global
Categorização

##### Measurement Pitfalls
- Cada projeto tem necessidades de informações únicas (e.g., objetivos, riscos, problemas)
- A única exceção é que, em alguns casos com linhas consistentes de produtos, processos e necessidades de informação, um pequeno conjunto de métricas pode ser definida para uso em uma organização.
- Para ser efetiva, medições precisam ser perfomadas continuamente, incluindo identificação periódica e priorização de necessidades de informação e métricas associadas.

#### Measurement Process Overview
- Um processo de medição é composto de 4 atividade:
1. estabelecer e manter o compromisso
2. plano de medição
3. performar medição
4. avaliar medição
Foi adotado pelo CMMI e por sistemas internacionais e padrões de engenharia de software, como ISO/IEC/IEEE 15939.
- [ ] [Systems and software engineering - Measurement process](https://www.iso.org/obp/ui/#iso:std:iso-iec-ieee:15939:ed-1:v1:en)
