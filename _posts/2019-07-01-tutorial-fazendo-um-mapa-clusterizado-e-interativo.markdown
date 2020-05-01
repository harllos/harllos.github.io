---
title: "Tutorial: fazendo um mapa clusterizado e interativo"
layout: post
date: 2019-07-01 21:21
image: /assets/images/mapa_clusterizado.png
headerImage: true
tag:
- Mapas
- Eleições 2018
- Python
- Mapbox
- Análise de Dados
- Javascript

category: blog
author: harllos
description: Veja como fazer um mapa clusterizado e interativo com JavaScript.
---

Se você tem uma base de dados de clientes, entregas, endereços ou algo parecido, você pode querer uma melhor visualização das informações disponíveis. Mapas são excelentes recursos para isso. Há diversas excelentes soluções para o desenvolvimento de web mapas (Leaflet, Plotly, Geopandas, Google MyMaps, Folium…). Qualquer uma dessas ferramentas pode fazer algo do mais simples até o mais complexo.

Neste tutorial, desenvolverei um mapa de clusters dos eleitores do Estado do Rio de Janeiro. Adianto que este não é um mapa simples, mas é um bom exemplo de como uma grande quantidade de dados pode ser visualizada em pontos clusterizados espalhados em um mapa.

## Para tanto, utilizaremos três ferramentas principais:

    * geojson.io (do Mapbox)
    * javascript
    * supercluster js

Antes de continuar, é preciso deixar claro que não há uma ferramenta melhor que a outra. Há ferramentas melhores para cada tipo de necessidade. Talvez, para o seu caso, este tutorial seja mais do que necessite (é provável que seja). Por isso, primeiro, pergunte-se: “o que quero visualizar?” para, então, definir quais ferramentas utilizar.

**Desse modo, o objetivo deste tutorial é apresentar uma boa solução para a visualização de clusters com grande quantidade de pontos no espaço**. Para clusters menos robustos, podemos utilizar uma [versão mais simples disponibilizada também pelo pessoal do Mapbox](https://docs.mapbox.com/mapbox-gl-js/example/cluster/). Se você não tem uma tabela com milhões de linhas, é preferível utilizar esse modelo que, além de mais simples, é mais bonito e mais rápido!

Para clusters onde há vários pontos sobrepostos (mesma latitude e longitude), há uma solução disponibilizada pelo leaflet. Em nosso caso, não temos dois locais de votação no mesmo lugar. Portanto, não precisarei utilizar esse recurso.

Preparado? :))

<div class="breaker"></div>

#  #1 Prepare seu Dataset!

A etapa mais desafiadora do desenvolvimento de um web mapa é a da preparação do dataset a ser utilizado. Muitas vezes, gasta-se mais tempo entendendo e limpando um dataset que o analisando.

A construção do dataset fica a cargo do leitor. Tenha em mente que precisamos da localização geográfica exata para cada ponto que queremos exibir no mapa. Ou seja, para cada item a ser mostrado, precisamos saber qual a latitude e a longitude desse objeto.

Como o meu objetivo é o de mostrar como clusterizar pontos em um mapa, partirei de uma base de dados já tratada. Neste caso, utilizarei um CSV com informações básicas sobre os locais de votação das eleições 2018 no Rio de Janeiro disponibilizado pelo Ministério Público do Rio de Janeiro por meio da plataforma [MP in loco](http://apps.mprj.mp.br/sistema/inloco/).

O CSV do MPRJ com os locais de votação do RJ possui 4921 linhas, incluindo o título, com as seguintes colunas: id; município; zona eleitoral; nome do local de votação; bairro; endereço; total de seções eleitorais do local; total de eleitores aptos e, o mais importante, a latitude e a longitude de cada local de votação.

Se você ainda precisa geolocalizar seus itens, sugiro a utilização da API do Google Maps cuja documentação pode ser encontrada [aqui](https://developers.google.com/maps/documentation/geocoding/start?refresh=1&authuser=0).

# #2 Precisamos de um GeoJSON!

Agora que já temos uma tabela com as linhas geolocalizadas, precisamos convertê-la em um tipo de arquivo ideal para mapas digitais: o GeoJSON. Há várias formas de converter um arquivo CSV para [GeoJSON](https://tools.ietf.org/html/rfc7946). A maneira mais fácil que encontrei foi utilizando uma ferramenta muito simples feita pelo pessoal do [MapBox](https://mapbox.com/), o [geojson.io](https://geojson.io/). Dá até para arrastar seu CSV para o navegador e visualizar os pontos no mapa.

Também é possível fazer upload ou conectar com o GitHub, entre outras opções. Além do GeoJSON, pode-se, a partir do CSV original, obter um Shapefile, KML, entre outros tipos de arquivos que são comumente utilizados para desenvolver mapas. Outra opção bem interessante é a de editar o arquivo final antes de exportá-lo para fazer algumas eventuais correções ou modificações que julgar necessário. Com o uso, descobri que é uma boa maneira de testar nossa tabela: se os pontos aparecerem no mapa, passamos no teste! Caso contrário, é melhor averiguar se está tudo nos conformes.

Após testar algumas alternativas, optei por sempre fazer upload do meu GeoJSON no [Gist](https://gist.github.com/) para obter uma url que alimentará as informações no mapa. Você pode simplesmente abrir seu GeoJSON, copiar e colar no Gist.

# #3 Criando um mapa!

Para criar nosso mapa, vamos utilizar a Mapbox GL JS ([veja a documentação](https://docs.mapbox.com/mapbox-gl-js/overview/)) — uma bilioteca em JavaScript que usa [WebGl](https://pt.wikipedia.org/wiki/WebGL) para renderizar mapas interativos. Recentemente, a Mapbox GL JS adicionou uma nova feature que nos permite clusterizar os objetos no mapa. Para mapas mais pesados como o nosso, vamos trabalhar com uma biblioteca JavaScript chamada [Supercluster](https://github.com/mapbox/supercluster) junto com a Mapbox GL JS. [A Supercluster foi criada por esse cara aqui](https://blog.mapbox.com/clustering-millions-of-points-on-a-map-with-supercluster-272046ec5c97).

**Vou, por partes, explicar o código. Ao final, teremos um único código que poderá gerar nosso mapa.**

# #4 Começando pelo começo!

Abra seu editor de texto preferido e copie e cole o código abaixo. Ele precisa ficar no cabeçalho do nosso html para nos conectar à api do Mapbox e à biblioteca supercluster. Também podemos utilizá-la para alterar alguns aspectos visuais do nosso mapa.

```css
<!DOCTYPE html>
<html>
<head>
 <meta charset='utf-8' />
 <title>Eleitores do RJ 2018</title>
 <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
 <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.js'></script>
 <script src="https://npmcdn.com/@turf/turf/turf.min.js"></script>
 <script src="https://unpkg.com/supercluster@3.0.2/dist/supercluster.min.js"></script>
 <script src="https://cdnjs.cloudflare.com/ajax/libs/chroma-js/1.3.4/chroma.min.js"></script>
 <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.css' ' rel='stylesheet' />
 <link href="https://api.mapbox.com/mapbox-assembly/v0.21.1/assembly.min.css" rel="stylesheet">
 <script async defer src="https://api.mapbox.com/mapbox-assembly/v0.21.1/assembly.js"></script>
 
 <style>
 body {
 margin: 0;
 padding: 0;
 }
#map {
 position: absolute;
 top: 0;
 bottom: 0;
 width: 100%;
 }
 </style>
</head>
```

A seguir, **vamos definir outros aspectos de estilo e localização do mapa**. Também devemos inserir uma caixa no canto superior esquerdo do mapa onde ficará uma caixa de seleção para performar funções com os dados provenientes do GeoJSON. Essa caixa, assim como as funções de soma, min, max e mean, que apresentarei mais tarde, são estritamente opcionais.

```css

<body>
 <div class='viewport-full relative clip'>
 <div class='viewport-twothirds viewport-full-ml relative'>
 <div id='map' class='absolute top left right bottom'></div>
 <div class='absolute top-ml left z1 w-full w300-ml px12 py12'>
 <div class='viewport-third h-auto-ml hmax-full bg-gray-dark round-ml shadow-darken5 scroll-auto'>
 <div class='p24 my12 mx12 scroll-auto color-white'>
 <h3 class='txt-l txt-bold my6 mx6'>Eleitores RJ 2018</h3>
 <h5 class='txt-m txt-bold px12'>Agregar Eleitores:</h5>
 <div class='select-container py12' id="select-container">
 <select class='select' id="select-option">
 <option value="sum">sum</option>
 <option value="count">count</option>
 <option value="avg">avg</option>
 <option value="min">min</option>
 <option value="max">max</option>
 </select>
 <div class='select-arrow'></div>
 </div>
 </div>
 </div>
 </div>
 ```

 É aqui onde precisamos chamar a url do Gist onde armazenei nosso GeoJson lá no passo 2. Também é onde estabelecemos qual variável será utilizada para gerar os clusters — em nosso caso, a quantidade de votos em cada local de votação. Também defini alguns parâmetros iniciais, como o raio dos clusters, zoom máximo e a escala de cor utilizada.

```css
<script>
 var select_el = document.getElementById('select-option')
 var select_value = select_el.value
 var clusterRadius = 40;
 var clusterMaxZoom = 20;
 //Propriedade a ser utilizada para gerar os pontos a serem clusterizados em nosso aquivo GeoJSON. Precisa ser uma variável do tipo NUMÉRICO.
 var propertyToAggregate = "total_aptos"
 //este é o link onde armazenei nosso GeoJSON
 let data_url = "https://gist.githubusercontent.com/harllos/022379b54666103b8f842a18f71bb88a/raw/7442f4648c9f8f9371f21a79593265f9503279a6/locais_2018_mp_clean.geojson"; 
 var mydata;
 var currentZoom;
 var color = 'YlOrRd';
 var clusterData;
 var worldBounds = [-180.0000, -90.0000, 180.0000, 90.0000];
 ```

 A seguir, são definidas funções que utilizaremos para gerar, de fato, nosso mapa mais à frente. **Não é necessário mexer nessas funções** (a não ser que você queira, claro). Para fazer esse e outros mapas, fiz alterações mínimas, mas, na maioria dos casos, deixei como está.

 ```css
 function getFeatureDomain(geojson_data, myproperty) {
 let data_domain = []
 turf.propEach(geojson_data, function(currentProperties, featureIndex) {
 data_domain.push(Math.round(Number(currentProperties[myproperty]) * 100 / 100))
 })
 return data_domain
 }
function createColorStops(stops_domain, scale) {
 let stops = []
 stops_domain.forEach(function(d) {
 stops.push([d, scale(d).hex()])
 });
 return stops
 }
function createRadiusStops(stops_domain, min_radius, max_radius) {
 let stops = []
 let stops_len = stops_domain.length
 let count = 1
 stops_domain.forEach(function(d) {
 stops.push([d, min_radius + (count / stops_len * (max_radius - min_radius))])
 count += 1
 });
 return stops
 }
 ```

**O trecho de código abaixo é descartável**. Ele “preenche” a caixa de funções que adicionamos no canto superior esquerdo lá no cabeçalho do nosso arquivo e realiza cálculos com a propriedade escolhida para visualização. No nosso caso, o código acabará mostrando os locais de votação com menos eleitores, com mais eleitores, a média e a soma. Acredito que ficará mais fácil de entender essa parte quando o mapa estiver pronto e você manusear a caixa de seleção que aparecerá no navegador.

```css
//Supercluster with property aggregation
 var cluster = supercluster({
 radius: clusterRadius,
 maxZoom: clusterMaxZoom,
 initial: function() {
 return {
 count: 0,
 sum: 0,
 min: Infinity,
 max: -Infinity
 };
 },
 map: function(properties) {
 return {
 count: 1,
 sum: Number(properties[propertyToAggregate]),
 min: Number(properties[propertyToAggregate]),
 max: Number(properties[propertyToAggregate])
 };
 },
 reduce: function(accumulated, properties) {
 accumulated.sum += Math.round(properties.sum * 100) / 100;
 accumulated.count += properties.count;
 accumulated.min = Math.round(Math.min(accumulated.min, properties.min) * 100) / 100;
 accumulated.max = Math.round(Math.max(accumulated.max, properties.max) * 100) / 100;
 accumulated.avg = Math.round(100 * accumulated.sum / accumulated.count) / 100;
 }
 });
 ```

 A seguir, **é preciso inserir um token para acessar a plataforma do Mapbox**. É possível utilizar estilos padrão do Mapbox (ou do leaflet) ou, ainda, criar um mapa estilizado no [Mapbox Studio](https://www.mapbox.com/mapbox-studio/). Também adicionamos algumas questões de navegabilidade do mapa.
 
 ```css
mapboxgl.accessToken = 'seu token vai aqui';
 var map = new mapboxgl.Map({
 container: 'map',
 style: "seu map style vai aqui. você poderá criar um estilizado ou utilizar um padrão do Mapbox.
 Também é possível utilizar uma solução do OpenStreet Map ou similares, que não é o escopo deste tutorial.",
 center: [-43.01,-22.495], #escolha uma latitude/longitude inicial para seu mapa
 zoom: 8, #escolha um zoom inicial
 hash: true
 });
 ``` 

 A função abaixo determina algumas condições para quando o mapa é iniciado. É onde, de fato, iniciamos nosso mapa.

 ```css
 map.on('style.load', function() {
 fetch(data_url)
 .then(res => res.json())
 .then((out) => {
 mydata = out;
 // Usando a Supercluster!
 cluster.load(mydata.features);
 // Criando o mapa
 initmap();
 })
 .catch(err => console.error(err));
 });
 ```

 Abaixo, definimos algumas escalas de cor e de raio dos clusters conforme a variável escolhida para visualização. Também determina características tanto para os pontos clusterizados quanto para os “unclustered points”.

 ```css
 var colorStops, radiusStops;
function updateClusters(repaint) {
 currentZoom = map.getZoom();
 clusterData = turf.featureCollection(cluster.getClusters(worldBounds, Math.floor(currentZoom)))
let stops_domain = chroma.limits(getFeatureDomain(clusterData, select_value), 'e', 8)
 //coloração original era definida pela linha abaixo. copiei, colei e utilizei escala logarítimica que tirei daqui: https://github.com/gka/chroma.js/
 var scale = chroma.scale(color).domain(stops_domain).mode('lab')
 colorStops = createColorStops(stops_domain, scale)
 radiusStops = createRadiusStops(stops_domain, 20, 50);
if (repaint) {
 map.setPaintProperty('clusters', 'circle-color', {
 property: select_value,
 stops: colorStops
 });
map.setPaintProperty('clusters', 'circle-radius', {
 property: select_value,
 stops: radiusStops
 });
map.setPaintProperty('unclustered-point', 'circle-color', {
 property: propertyToAggregate,
 stops: colorStops
 });
map.setLayoutProperty('cluster-count', "text-field", "{" + select_value + "}");
 }
 }
 ```

 Aqui, “iniciamos” o mapa com a função initmap e adicionamos os clusters ao mapa — incluindo detalhes do estilo dos clusters, como cor dos números que aparecerão nele ou limites das bordas.

 ```css
 function initmap() {
updateClusters(false);
select_el.addEventListener('change', function(e) {
 // Update selected aggregation on dropdown
 select_value = select_el.value
 updateClusters(true);
 })
map.addSource("locais-2018-mp-clean", {
 type: "geojson",
 data: clusterData,
 buffer: 1,
 maxzoom: 14
 });
map.addLayer({
 id: "clusters",
 type: "circle",
 source: "locais-2018-mp-clean",
 filter: ["has", "point_count"],
 paint: {
 "circle-color": {
 property: select_value,
 stops: colorStops
 },
 "circle-blur": 0.50,
 "circle-radius": {
 property: select_value,
 type: "interval",
 stops: radiusStops
 }
 }
 }, "road-label");
map.addLayer({
 id: "unclustered-point",
 type: "circle",
 source: "locais-2018-mp-clean",
 filter: ["!has", "point_count"],
 paint: {
 "circle-color": {
 property: propertyToAggregate,
 stops: colorStops
 },
 "circle-radius": 2,
 "circle-stroke-width": 2,
 "circle-stroke-color": "#fff"
 }
 }, "road-label");
map.addLayer({
 id: "cluster-count",
 type: "symbol",
 source: "locais-2018-mp-clean",
 filter: ["has", "point_count"],
 layout: {
 "text-field": "{" + select_value + "}",
 "text-font": ["DIN Offc Pro Medium", "Roboto Black"],
 "text-size": 13
 
 },
 paint: {
 "text-halo-color": "black",
 "text-halo-width": 1.5,
 "text-color":"white"
 }
});
map.on('zoom', function() {
 newZoom = map.getZoom();
if (Math.floor(currentZoom) == 0) {
 currentZoom = 1
 };
if (Math.floor(newZoom) != Math.floor(currentZoom)) {
 currentZoom = newZoom
 updateClusters(true);
 map.getSource('locais-2018-mp-clean').setData(clusterData)
 }
 })
 };
 ``` 

 **Pronto! Os códigos acima, juntos, já geram um mapa clusterizado**. Porém, podemos fazer melhor adicionando pop-ups com informações sobre nossos pontos no mapa. Desse modo, podemos enriquerecer as informações contidas nos mapas com basicamente tudo o que acharmos importante!

 # #5 Adicionando pop-ups interativos!

Para gerar nossos pop-ups, basta um pedaço de código muito simples. Optamos pelo pop-up quando clicarmos no ponto do mapa utilizando uma camada do nosso mapa que inserimos no Mapbox Studio (mesmo CSV). Assim, podemos escolher as informações que queremos exibir em .sethtml.

```css
// adding menu but doing nothing yet 
 var popup = new mapboxgl.Popup({
 closeButton: false,
 closeOnClick: true
 });
map.on('click', 'unclustered-point', function(e) {
 // Change the cursor style as a UI indicator.
 map.getCanvas().style.cursor = 'pointer';
var coordinates = e.features[0].geometry.coordinates.slice();
 var description = '<h2>aptos: '+e.features[0].properties.total_aptos+'<h2>local: '+e.features[0].properties.nm_local+'</h2>'+'\n'
 +'<h2>endereço: ' +e.features[0].properties.endereco+'</h2><p>município: '+ e.features[0].properties.municipio + '\n'
 +'</h2><p>bairro: '+e.features[0].properties.bairro + '</h2><p>zona: ' + e.features[0].properties.zona_eleitoral + '\n'
 +'</h2><p>total de seções: '+e.features[0].properties.total_seção 
 ;
 // Ensure that if the map is zoomed out such that multiple
 // copies of the feature are visible, the popup appears
 // over the copy being pointed to.
 while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
 coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
 }
// Populate the popup and set its coordinates
 // based on the feature found.
 popup.setLngLat(coordinates)
 .setHTML(description)
 .addTo(map);
 });
map.on('mouseleave', 'unclustered-point', function() {
 map.getCanvas().style.cursor = '';
 popup.remove();
 });
</script>
 </script>
</body>
</html>
```

**Pronto! Agora temos um mapa que exibe os clusters de eleitores no Estado do RJ**. Com ele, podemos analisar onde está localizado o maior número de eleitores, os maiores locais de votação em número de eleitores aptos, as grandes zonas de eleitores do RJ, além de informações específicas sobre cada local de votação nos pop-ups, como o endereço, zona e seções.

# #6 Compartilhando com os amigos!

Quer que seus colegas vejam seu mapa? Bom, uma maneira muito simples é utilizando o https://bl.ocks.org. Basta subir o código final no gist e trocar o “gist.github” por “bl.ocks.org” na barra de endereços do navegador.

Nosso mapa final está [aqui](https://bl.ocks.org/harllos/raw/2f5ca4c16548b6a70f38aa67896276d7/#8/-22.495/-43.01):


<iframe width="1500" height="700" src="https://bl.ocks.org/harllos/raw/2f5ca4c16548b6a70f38aa67896276d7/#8/-22.495/-43.01" frameborder="0" allowfullscreen></iframe>

É isso. Em breve, devo postar mais mapas. Qualquer dúvida, sugestão ou correção, fala comigo! :)

*O código completo está neste Gist:*

https://gist.github.com/harllos/raw/2f5ca4c16548b6a70f38aa67896276d7/#8/-22.495/-43.01

<div class="breaker"></div>

Ah, deixo meu agradecimento à @Ananda Weingärtner pela revisão do texto.