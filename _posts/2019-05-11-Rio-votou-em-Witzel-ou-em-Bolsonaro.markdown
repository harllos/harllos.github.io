---
title: "Rio votou em Witzel ou em Bolsonaro?"
layout: post
date: 2019-05-11 15:41
image: /assets/images/bolsowitzel.jpg![]
width: 300x300
headerImage: true
tag:
- Bolsonaro
- Witzel
- Python
- Plotly
- Eleições
- Análise de Dados

category: blog
author: Harllos Arthur
description: Análise da correlação de votos entre Bolsonaro e Witzel no estado do Rio.
---

## Como estão relacionados, urna a urna, os votos de Witzel e de Bolsonaro?
---

<span class = "evidence"> Disclaimer: texto escrito por um não-matemático e não-estatístico. Alguns leitores já apontaram inconsistências que serão corrigidas em breve. Mais contribuições são muito bem vindas! :) </span>

Dias antes do primeiro turno, as pesquisas ainda mostravam o candidato do PSC, Wilson Witzel, em terceiro lugar. Após surpreendente vitória, questionou-se o que havia motivado a subida repentina do ex-juiz nas pesquisas e a sua consequente ida ao segundo turno.

As análises dos jornais sugeriram forte correlação entre os votos de Witzel com os resultados de Jair Bolsonaro, tendência que continuou após o resultado do segundo turno, que elegeu Witzel governador do Rio de Janeiro. Witzel subiu dos parcos 1% das primeiras pesquisas de intenção de voto direto para o segundo turno, ocasião em que superou Eduardo Paes (DEM) em mais de 1.5 milhão de votos.

> "É a resposta clara da população pela renovação, que se reflete nacionalmente na votação expressiva do presidente Jair Bolsonaro" (Wilson Witzel (PSC) após vitória no primeiro turno).

<div class="breaker"></div>

Neste breve texto, não quero explorar a estratégia publicitária de Witzel para colar sua figura à imagem da família Bolsonaro. Meu objetivo é o de comprovar, em uma análise urna a urna, a alta correlação positiva do chamado voto BolsoWitzel.

Vamos, então, ao que interessa! :)

<div class="breaker"></div>

# Quem votou em Witzel votou em Bolsonaro?

<div class="side-by-side">
    <div class="toleft">
        <img class="image" src="http://localhost:4000/assets/images/corr_witzel_bolso.png" alt="Alt Text">
        <figcaption class="caption">Gráfico mostra alta correlação positiva entre os votos de Witzel e de Bolsonaro. Cada ponto representa uma das mais de 30 mil urnas. Os números nos eixos horizontal e vertical representam a quantidade de votos em cada urna.</figcaption>
    </div>

    <div class="toright">

        <p> O coeficiente de Pearson, amplamente utilizado para estimar correlações, varia entre -1e 1. Quanto mais próximo de algum dos extremos, maior a correlação, que pode ser positiva ou negativa.
        <br> <br> 
        O gráfico ao lado mostra uma alta correlação positiva (0.91ρ) entre os votos de Bolsonaro e de Witzel no primeiro turno. Podemos afirmar, grosso modo, que quem votou em Witzel, votou também em Bolsonaro. Nas urnas em que Bolsonaro teve muitos votos, Witzel também teve. Já naquelas urnas em que o Presidente eleito não foi tão bem, Witzel também não foi.</p>
    </div>
</div>

Para comparar, gerei o gráfico da correlação entre os votos de Márcia Tiburi, candidata a governadora pelo PT, e de Fernando Haddad. Note que, embora alta, a correlação entre os petistas é menor (0.78ρ).


{% highlight raw %}
![Markdowm Image][/image/url]
<figcaption class="caption">Gráfico mostra alta correlação positiva entre os votos de Tiburi e de Haddad. Cada ponto representa uma das mais de 30 mil urnas. Os números nos eixos horizontal e vertical representam a quantidade de votos em cada urna.</figcaption>
{% endhighlight %}

<div class="breaker"></div>

# Que urnas são essas?

Ao chegar aos gráficos acima, cheguei à seguinte questão: afinal, onde fica cada uma dessas bolinhas (urnas)? Para responder a essa dúvida, utilizei um pouquinho de Python, do Pandas e do Plotly. Para disponibilizar os gráficos para o mundo, coloquei tudo em um HTML simples no https://bl.ocks.org/.

Abaixo, um print do gráfico interativo. Você pode clicar aqui e passar o mouse sobre as bolinhas e ir mais a fundo na correlação dos votos BolsoWitzel.


{% highlight raw %}
![Markdowm Image][/image/url]
<figcaption class="caption">Cada ponto corresponde a uma urna. Veja a versão interativa do gráfico acima aqui.</figcaption>
{% endhighlight %}

Já adianto que uma das maiores coincidências de resultados ocorreu na zona 23, seção 583, em um bairro chamado… Vila Militar! Nessa seção, Bolsonaro teve 352 votos e Witzel 169.

Se achar 30 mil urnas muita informação, você também pode passear pelos resultados por zona eleitoral do Rio de Janeiro clicando aqui. Também antecipo que os leitores barrenses foram os que mais digitaram 17 e 20 no primeiro turno.

