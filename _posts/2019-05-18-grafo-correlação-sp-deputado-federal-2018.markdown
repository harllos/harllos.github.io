---
title: "Grafo de correlação entre candidatos a deputado federal de SP em 2018"
layout: post
date: 2019-05-18 15:32
image: /assets/images/bolsowitzel.jpg![]
headerImage: true
tag:
- Grafo
- Correlação
- Eleições 2019
- Python
- Gephi
- Análise de Dados

category: blog
author: Harllos Arthur
description: Grafo de correlação entre candidatos a deputado federal de SP em 2018.
---



Para determinar o tamanho de cada nó, utilizei o atributo ["betweenness centrality"](https://en.wikipedia.org/wiki/Betweenness_centrality#Weighted_networks), calculado no próprio Gephi.

Para demonstrar os clusteres da rede, apliquei o algoritmo [Force Atlas](https://github.com/gephi/gephi/wiki/Force-Atlas-2) 2 no Gephi. O grafo é ponderado pela correlação entre os nós.

O algoritmo que calcula a [modularidade](https://github.com/gephi/gephi/wiki/Modularity) padrão do Gephi fez com que as redes do Ciro e do Bolsonaro fossem uma só. Intensificando levemente a intensidade, os grupos ficaram separados.

Intermediação: O critério de cálculo leva em consideração a posição de um nó entre o caminho que conecta todos os outros nós. O nó que tiver maior valor de intermediação é aquele que se encontra mais no meio do caminho entre todos os outros nós possíveis

Como se sabe, correlação não indica causalidade. Nos resta levantar algumas hipóteses a partir dos dados.

Não sabia se meu algoritmo estava correto. Mas, ao analisar o grafo, faz sentido. As comunidades formadas fazem jus à vida real. Os presidenciáveis formaram grupos distintos e aglutinaram parceiros políticos ou candidatos de perfil ideológico e territorialmente parecido. Surgiram algumas coisas esquisitas, como Haddad próximo do Meirelles. Mas isso parece dizer mais sobre os eleitores que os candidatos. Podemos simplesmente supor que Meirelles foi relativamente bem votado em alguns lugares onde Haddad também foi. Com uma lupa sobre os dados, podemos confirmar a hipótese, como fizemos com Witzel e Bolsonaro.

Como era de se esperar, os maiores nós foram os principais presidenciáveis: Bolsonaro, Haddad, Ciro e Alckmin. Em torno deles, se formaram os quatro principais grupos.

Para quem quiser se aprofundar um pouquinho mais sobre as correlaçoes entre os cadidatos, segue, a seguir, uma matriz de confusão com os @@ mais votados. A principal diferença em relação ao grafo, além da quantidade de candidatos plotados, é que a matriz apresenta, também, as correlações negativas.

O grafo colocou alguns personagens em situação curiosa. Marina Silva não conseguiu formar uma rede própria, ficando dentro da rede de Ciro Gomes, mas próxima a Haddad. Henrique Meirelles ficou em vermelho, mas mais próximo de Alckmin, assim como Marco Feliciano, que, apesar de vermelho, ficou próximo a Alckimin e o grupo do Bolsonaro. Os votos para a legenda do PSDB ficaram no grupo do Ciro e muito próximo do grupo do atual presidente. Ivan Valente ficou muito próximo ao PSOL e à Sâmbia Bomfim (PSOL), mas o algoritmo o juntou ao grupo verde.

Mais e melhores análises ficam para o leitor.


![Correlação Geral](/assets/images/sp2018_dep_pr.png){:class="bigger-image"}
<figcaption class="caption">Clique no link para ver a imagem maior: https://harllos.github.io/assets/images/sp2018_dep_pr.png </figcaption>


<iframe width="1500" height="700" src="https://harllos.github.io/network/grafo_sp_2018_depfed_pr.html#" frameborder="0" allowfullscreen></iframe>

