---
layout: post
title:  "Network Programmability - Formatando dados com YAML e XMLs"
date:   2020-03-24
---

<p class="intro"><span class="dropcap">N</span>o artigo de hoje, iremos falar  dos conceitos por trás das linguagens de estrutura e formatos de dados, como YAML e XML. Pegue seu café, se acomode e vamos nessa! Até hoje, nós da área de infraestrutura não precisávamos saber programar. Ok, o conhecimento em linguagens de programação  é muito útil para automatizarmos tarefas e rotinas comuns de testes, mas nunca foi um grande requisito no currículo de um CCNP.</p>

<p>Para darmos ênfase neste artigo, precisamos ressaltar alguns conceitos relacionados à estrutura e modelagem de dados.</p>

<p>Ao citar a linguagem XML, o exemplo mais comum no mundo de network é que o JunOS estrutura os dados que o SO utiliza via linguagem XML. Ex - Basta setarmos o comando > show isis adjacency detail | display xml rpc que ele nos mostrará a saída descrita abaixo.</p>

{% lowlight ruby %}
    <rpc-reply xmlns:junos="http://xml.juniper.net/junos/16.1R1/junos">
        <rpc>
            <get-isis-adjacency-information>
                <detail/>
            </get-isis-adjacency-information>
        </rpc>
        <cli>
            <banner></banner>
        </cli>
    </rpc-reply>
{% endlowlight %}

<p>Vale ressaltar que existe inúmeros formatos de estrutura de dados a nossa disposição, devemos entender o propósito de cada modelo de dados, cada um foi criado para um caso diferente.</p>

<p>Depois de entender os formatos de dados nos quais irei detalhar melhor a seguir, é importante compreender quais tipos de dados podem ser representados pelos formatos dos quais serão ditos neste artigo. O objetivo das linguagens de estruturação de dados é fazer com que, números, palavras e até complexos objetos de instância de software se comuniquem.</p>

#### String
<p>Indiscutivelmente, é o tipo de dado mais fundamental. Esse parâmetro é a maneira mais comum de representar uma sequência de números, letras ou símbolos. Caso você queira representar alguma frase em um determinado idioma em um dos formatos de estrutura de dados, provavelmente usará uma string para fazer isso. No python, você pode representar uma string através do parâmetro str ou unicode.</p>

#### Integer
<p>O parâmetro integer representa tipos de dados que contém valores numéricos, mas para maioria das pessoas ao discutir valores numéricos se baseiam em números inteiros. O número inteiro é exatamente aquilo que você aprendeu na escola – Um número inteiro, positivo ou negativo. Existe outro tipo de dado como float que você pode usar ao descrever valores não inteiros. Python utiliza o parâmetro int para declarar valores do tipo inteiro.</p>

#### Boolean
<p>Um dos tipos de dados mais simples é o booleano, um valor simples que é verdadeiro ou falso. Esse é um tipo muito popular usado quando um programador deseja saber o resultado de uma operação ou se dois valores são iguais um ao outro, por exemplo. Isso é conhecido como o tipo bool no python.</p>

<p>Dica: Os Network Developers obtém uma estrutura de automação poderosa quando combinam scrpts python com formatos de dados JSON, XML ou YAML junto com as bibliotecas de automação.</p>

<img src="{{ '/assets/img/touring.jpg' | prepend: site.baseurl }}" alt=""> 

Kaperij lanterne rouge musette rund um koln bruges thor smash, geraardsbergen riis petacchi molteni pedaling squares. Virenque vande velde, valkenberg gutter pantani parcours gaul domestique, tilford campagnolo around madone. Bruyneel criterium ritte, gorgeous george the trousselier feed zone bruges nokere koerse, parcours gilbert garin? Anquetil valkenberg bettini cat among the pigeons.

Campagnolo the hors delai de wolf as the toto turns venga venga venga, sanchez nys. Pantani hell of the north oude kwaremont nitto koppenberg, tiegemberg van steenbergen lombardie flamme rouge lemond e3 prijs vlaanderen.

Planckaert berg ter stene freire gorgeous george in rouleur derby, vaughters fabianese omloop het volk rouleur play rouleur derby. Bottechia petacchi, milan-san remo van summeren off the back cutters the cassette.

Nyvelocity pyrenees vande velde merckx. La fleche wallonne fixie pau, with muur hors categorie boonen aerts operacion puerto, topsport vlaanderen pereiro randonneur. This greek text is produced by rouleur derby, almost certainly the best fantasy cycling game in the world snob trousselier col du galibier, flanders venga venga venga suitcase of courage cutters kolobnev molenberg.
