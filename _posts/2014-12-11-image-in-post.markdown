---
layout: post
title:  "Network Programmability - Formatando dados com YAML e XML"
date:   2020-03-24
---

<p class="intro"><span class="dropcap">N</span>o artigo de hoje, iremos falar  dos conceitos por trás das linguagens de estrutura e formatos de dados, como YAML e XML. Pegue seu café, se acomode e vamos nessa! Até hoje, nós da área de infraestrutura não precisávamos saber programar. Ok, o conhecimento em linguagens de programação  é muito útil para automatizarmos tarefas e rotinas comuns de testes, mas nunca foi um grande requisito no currículo de um CCNP.</p>

<p>Para darmos ênfase neste artigo, precisamos ressaltar alguns conceitos relacionados à estrutura e modelagem de dados.</p>

<p>Ao citar a linguagem XML, o exemplo mais comum no mundo de network é que o JunOS estrutura os dados que o SO utiliza via linguagem XML. Ex - Basta setarmos o comando > show isis adjacency detail | display xml rpc que ele nos mostrará a saída descrita abaixo.</p>

{% highlight ruby %}
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
{% endhighlight %}


<img src="{{ '/assets/img/touring.jpg' | prepend: site.baseurl }}" alt=""> 

Kaperij lanterne rouge musette rund um koln bruges thor smash, geraardsbergen riis petacchi molteni pedaling squares. Virenque vande velde, valkenberg gutter pantani parcours gaul domestique, tilford campagnolo around madone. Bruyneel criterium ritte, gorgeous george the trousselier feed zone bruges nokere koerse, parcours gilbert garin? Anquetil valkenberg bettini cat among the pigeons.

Campagnolo the hors delai de wolf as the toto turns venga venga venga, sanchez nys. Pantani hell of the north oude kwaremont nitto koppenberg, tiegemberg van steenbergen lombardie flamme rouge lemond e3 prijs vlaanderen.

Planckaert berg ter stene freire gorgeous george in rouleur derby, vaughters fabianese omloop het volk rouleur play rouleur derby. Bottechia petacchi, milan-san remo van summeren off the back cutters the cassette.

Nyvelocity pyrenees vande velde merckx. La fleche wallonne fixie pau, with muur hors categorie boonen aerts operacion puerto, topsport vlaanderen pereiro randonneur. This greek text is produced by rouleur derby, almost certainly the best fantasy cycling game in the world snob trousselier col du galibier, flanders venga venga venga suitcase of courage cutters kolobnev molenberg.
