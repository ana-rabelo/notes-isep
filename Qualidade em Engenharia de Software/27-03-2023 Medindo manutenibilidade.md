
▹<span style="color:#65B891"><b>Coupling and Structural Erosion </b></span>(CSE) "grande bola de lama": um sinônimo para um código mal entrelaçado que perde toda a sua coesão arquitetural. O termo descreve um sistema que é muito acoplado e tem muitas dependências entre as partes do sistema que não deveriam ser relacionadas.
Uma mudança em uma parte do sistema pode quebrar algo em uma parte completamente não relacionada
Muitas dependências cíclicas, resultando em grandes grupos de ciclo

▹<span style="color:#65B891"><b>Métricas para medir CSE</b></span>
- Média de dependência de componentes, custo de propagação, e métricas relacionadas
- Cyclicity e cyclicity relativa [?]
- Nível de manutenibilidade

<span style="color:#4E878C"><b>Average component dependency</b></span> (ACD) quantos elementos um elemento randomicamente selecionado de um grafo de depedência iria depender, direta ou indiretamente em média (incluindo ele mesmo). Pode ser obtido dividindo a *Cumulative Component Dependecy* pelo número de componentes.
▹Fornece uma **ideia** de qual fortemente acoplado o sistema é, mas precisa ser colcoado em relação com o total de componentes do sistema.

<span style="color:#4E878C"><b>Propagation Cost </b></span>(PC) mede qual acoplado um sistema é.
Alta porcentagem quer dizer alto acoplamento. Pode ser obtido dividindo o ACD mais um vez pelo número de nós, isso basicamente normaliza o ACD para um valor que pode ser facilmente comparado.
`No caso de 50%, isso quer que, toda vez que você modifica algo, uma média de 50% de todos os componentes vão ser afetados por essa mudança.`
No caso de um sistema grande, esse valor seria muito ruim; mas para exemplos pequenos, o valor não é inútil.
- Se o sistema é pequeno (n < 500), altos valores de PC são menos preocupantes.
- Para sistemas médios (500 <= n < 5000), os valores acima de 20% são preocupantes, enquanto valores acima de 50% indicam sérios problemas.
- Se o sistema é grande (n >= 5000), até um valor de 10% é um pouco preocupante.

<span style="color:#4E878C"><b>Cyclicity</b></span>
<span style="color:#4E878C"><b>Relative Cyclicity</b></span>