---
title: "Grafo da correlação entre candidatos a Deputado Federal (SP) em 2018"
layout: post
date: 2019-05-18 15:32
image: /assets/images/print_grafo.png
headerImage: true
tag:
- Grafo
- Correlação
- Eleições 2019
- Python
- NetworkX
- Gephi
- Análise de Dados

category: blog
author: harllos
description: Grafo da correlação dos candidatos a Deputado Federal (SP) em 2018.
---

Mais uma da série "brincando com os boletins de urna" do [TSE](http://www.tse.jus.br/eleicoes/estatisticas/repositorio-de-dados-eleitorais-1/repositorio-de-dados-eleitorais-resultado-2014-resultados) :)

Pensando nas melhores formas de entender como os brasileiros votaram em 2018, pensei que uma boa forma de visualizar de forma organizada os resultados das urnas seria por meio de um grafo. Decidi que uma boa opção poderia ser um grafo com base nas correlações (positivas) entre os candidatos mais votados para deputado federal e, também, os candidatos a Presidente. Para testar minha ideia, fiz um grafo desses para o resultado no estado de São Paulo (SP).

<span class="evidence">Antes de mais nada, deixo claro que minha principal intenção com esse texto é a de jogar luz às possibilidades que temos para analisar uma área que é comumente reconhecida como pertencente somente às ciências sociais. Isso não impede de a gente adicionar um pouco de programação no meio. Além disso, não pretendo analisar pormenorizadamente o grafo. Desse modo, neste *teste*, o que me interessa é contribuir um pouquinho para aqueles estudos que tentam unir humanas com exatas. </span>

Dito tudo isso, vamos ao grafo. Com os dados do repositório do TSE, foi possível calcular as correlações entre candidatos ao cardo de Deputado Federal por SP. Para ficar menos confuso, fiz uma matriz de correlação entre os postulantes que obtiveram mais de 46 mil votos (cerca de 0.2% dos votos válidos). Veja o resultado:

![Correlação Geral](/assets/images/sp2018_dep_pr.png){:class="bigger-image"}

A tabela ficou meio grande, mas você pode vê-la em melhor resolução [neste link](https://harllos.github.io/assets/images/sp2018_dep_pr.png). 

Com as correlações e a biblioteca [netwokx](https://networkx.github.io/documentation/stable/) em mãos, pude gerar o grafo com as correlações positivas entre os candidatos. Com a ajuda do [Gephi](https://gephi.org/) e do plugin [Sigma](http://sigmajs.org/), pude gerar um grafo compreensível:  

<iframe width="1500" height="700" src="https://harllos.github.io/network/grafo_sp_2018_depfed_pr.html#" frameborder="0" allowfullscreen></iframe>


Para determinar o tamanho de cada nó, utilizei o atributo ["betweenness centrality"](https://en.wikipedia.org/wiki/Betweenness_centrality#Weighted_networks), calculado no próprio Gephi. Para demonstrar os clusteres da rede, apliquei o algoritmo [Force Atlas](https://github.com/gephi/gephi/wiki/Force-Atlas-2) 2 no Gephi. O grafo é ponderado pela correlação entre os nós.

<div class="breaker"></div>

O algoritmo que calcula a [modularidade](https://github.com/gephi/gephi/wiki/Modularity) padrão do Gephi fez com que as redes do Ciro e do Bolsonaro fossem uma só. Intensificando levemente a intensidade, os grupos ficaram separados. O critério de cálculo leva em consideração a posição de um nó entre o caminho que conecta todos os outros nós. O nó que tiver maior valor de intermediação é aquele que se encontra mais no meio do caminho entre todos os outros nós possíveis - o que utilizei para definir o tamanho de cada um dos nós.

*Como se sabe, correlação não indica causalidade. Nos resta levantar algumas hipóteses a partir dos dados.*

Como era de se esperar, os maiores nós foram os principais presidenciáveis: Bolsonaro, Haddad, Ciro e Alckmin. Em torno deles, se formaram os quatro principais grupos.As comunidades formadas, aparentemente, fazem jus à vida real. Os presidenciáveis formaram grupos distintos e aglutinaram parceiros políticos ou candidatos de perfil ideológico e territorialmente parecidos. Surgiram, à primeira vista, algumas anomalias, como Haddad próximo do Meirelles. Mas isso parece dizer mais sobre os eleitores que o perfil político dos candidatos. Podemos simplesmente supor que Meirelles foi relativamente bem votado em alguns lugares onde Haddad também foi.

O grafo colocou alguns personagens em situação curiosa. Marina Silva não conseguiu formar uma rede própria, ficando dentro da rede de Ciro Gomes, mas próxima a Haddad. Henrique Meirelles ficou em vermelho, mas mais próximo de Alckmin, assim como Marco Feliciano, que, apesar de vermelho, ficou próximo a Alckmin e a Bolsonaro. Os votos para a legenda do PSDB ficaram no grupo do Ciro e muito próximos do grupo do atual presidente. Ivan Valente ficou muito próximo ao PSOL e à Sâmbia Bomfim (PSOL), mas o algoritmo o classifico no grupo verde.

Mais e melhores análises ficam para o leitor.

<div class="breaker"></div>

Uma vez que SP é o maior colégio eleitoral do País e que conseguimos fazer esse grafo e as correlações de forma simples, então podemos, eventualmente, expandi-lo para outras unidades da federação ou para o Brasil inteiro. Acredito que esse tipo de visualização dos dados eleitorais possa contribuir para aqueles cientistas políticos que estudam o comportamento do eleitorado brasileiro. Afinal, a junção entre as humanas e as exatas só pode ser boa, sem prejuízo de pesquisas que não têm o mesmo viés.