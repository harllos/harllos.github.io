---
title: "Grafo da correlação entre candidatos a Deputado Federal (SP) em 2018"
layout: post
date: 2019-05-18 15:32
image: /assets/images/print_grafo.png
headerImage: true
tag:
- Grafo
- Correlação
- Eleições 2018
- Python
- NetworkX
- Gephi
- Análise de Dados

category: blog
author: harllos
description: Grafo da correlação dos candidatos a Deputado Federal (SP) em 2018.
---

<meta property="og:image"
content="https://harllos.github.io/assets/images/print_grafo.png" />

Mais uma da série "brincando com os boletins de urna" do [TSE](http://www.tse.jus.br/eleicoes/estatisticas/repositorio-de-dados-eleitorais-1/repositorio-de-dados-eleitorais-resultado-2014-resultados) :)

<span class="evidence">Antes de mais nada, deixo claro que minha principal intenção com esse texto é a de jogar luz às possibilidades que temos para analisar uma área que é comumente reconhecida como pertencente somente às Ciências Sociais. Isso não impede de a gente adicionar um pouco de programação no meio. Além disso, não pretendo analisar pormenorizadamente o grafo. Desse modo, neste *teste*, o que me interessa é contribuir um pouquinho para aqueles estudos que tentam unir humanas com exatas. </span>

## Um grafo para ver de cima!

Pensando nas melhores formas de entender como os brasileiros votaram em 2018, imaginei que uma boa maneira de visualizar os resultados das urnas, de forma organizada, seria por meio de um grafo de redes. Decidi que uma boa opção poderia ser um **grafo com base nas correlações (positivas) entre os candidatos mais votados para Deputado Federal mais os candidatos a Presidente**. 

A princípio, deduzi que, dessa forma, o resultado poderia dar uma ideia geral sobre quais foram os principais grupos de candidatos nas eleições de 2018. **A ideia é sair um pouco da análise minuciosa e fazer um sobrevoo sobre os resultados das urnas do ano passado**.

**Para testar a minha ideia, fiz um grafo desses para o resultado no estado de São Paulo (SP).**

## Dados do TSE

Com os dados do repositório do TSE, foi possível calcular as correlações entre candidatos ao cargo de Deputado Federal por SP. Calculei a correlação, urna a urna, de cada par de candidatos que obtiveram mais de 46 mil votos (cerca de 0.2% dos votos válidos). Veja o resultado:

![Correlação Geral](/assets/images/sp2018_dep_pr.png){:class="bigger-image"}

A tabela ficou meio grande, mas **você pode vê-la em melhor resolução [neste link]**(https://harllos.github.io/assets/images/sp2018_dep_pr.png). 

## Ferramentas utilizadas

Com as correlações e a biblioteca [netwokx](https://networkx.github.io/documentation/stable/) em mãos, pude gerar o grafo com as correlações positivas entre os candidatos. Com a ajuda do [Gephi](https://gephi.org/) e do plugin [Sigma](http://sigmajs.org/), pude gerar um grafo compreensível:  

<iframe width="1500" height="700" src="https://harllos.github.io/network/grafo_sp_2018_depfed_pr.html#" frameborder="0" allowfullscreen></iframe>


Para determinar o tamanho de cada nó, utilizei o atributo ["betweenness centrality"](https://en.wikipedia.org/wiki/Betweenness_centrality#Weighted_networks), calculado no próprio Gephi. Para demonstrar os clusteres da rede, apliquei o algoritmo [Force Atlas](https://github.com/gephi/gephi/wiki/Force-Atlas-2) 2 no Gephi. O grafo é ponderado pela correlação entre os nós.

**Alguns ajustes.** O algoritmo que calcula a [modularidade](https://github.com/gephi/gephi/wiki/Modularity) padrão do Gephi fez com que as redes do Ciro e do Bolsonaro fossem uma só. Aumentando levemente a intensidade, os grupos ficaram separados. O critério de cálculo leva em consideração a posição de um nó entre o caminho que conecta todos os outros nós. O nó que tiver maior valor de intermediação é aquele que se encontra mais no meio do caminho entre todos os outros nós possíveis - o que utilizei para definir o tamanho de cada um dos nós.

<div class="breaker"></div>

# Resultados

## Correlação não é causalidade!

**Como se sabe, correlação não indica causalidade. Nos resta levantar algumas hipóteses a partir dos dados.**

## Como era de se esperar, os maiores nós foram os principais presidenciáveis: Bolsonaro, Haddad, Ciro e Alckmin.

Em torno deles, se formaram os quatro principais grupos. As comunidades formadas, aparentemente, fazem jus à vida real, já que eles fazem parte do grupo de candidatos que mais movimentou as redes sociais e a mídia.

## Os presidenciáveis formaram grupos distintos e aglutinaram parceiros políticos ou candidatos de perfil ideológico e territorialmente parecidos.

Ao passear pelas comunidades, podemos observar que candidatos do mesmo partido estão próximos uns dos outros. Mesmo quando são de partidos diferentes, alguns candidatos ficaram próximos, talvez por conta da proximidade de discurso e de histórico (logo, de eleitor), como Sâmia Bomfim (PSOL) e Tábata Amaral (PDT).

## O grafo colocou alguns personagens em situação curiosa.

À primeira vista, pode ser explicada pela disputa de eleitores de uma mesma região. É perfeitamente possível - e esperado! - que Ciro Gomes e Bolsonaro tenham dividido votos em um determinado bairro. Natural. Acredito que esse cenário torne a correlação mais alta e evidencie a forte competição entre dois candidatos.

É o que parece ter acontecido, por exemplo, com Marina Silva, que não conseguiu formar uma rede própria e ficou na rede de Ciro Gomes, mas próxima a Haddad. Podemos simplesmente supor que Marina foi relativamente bem votada em alguns lugares onde Ciro também foi. Outros casos: Henrique Meirelles ficou em vermelho, mas mais próximo de Alckmin, assim como Marco Feliciano, que, apesar de vermelho, ficou próximo a Alckmin e a Bolsonaro. Os votos para a legenda do PSDB ficaram no grupo do Ciro e muito próximos do grupo do atual Presidente. Ivan Valente ficou muito próximo ao PSOL e à Sâmbia Bomfim (PSOL), mas o algoritmo o classifico no grupo verde.




<div class="breaker"></div>


Bem, finalizo dizendo que esta “brincadeira” é apenas uma mostra de como a análise de dados pode ser útil para as áreas de estudos sociais, como a Ciência Política. Com a quantidade e riqueza de dados que o TSE nos oferece, por exemplo, é possível fazer todo o tipo de análise. O céu é o limite! **Precisamos colocar um pouquinho mais de dados e de programação nas ciências humanas!** Vamos juntos!

<div class="breaker"></div>

*Mais e melhores análises ficam para o leitor*.
