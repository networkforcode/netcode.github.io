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

<p>Vale ressaltar que existe inúmeros formatos de estrutura de dados a nossa disposição, devemos entender o propósito de cada modelo de dados, cada um foi criado para um caso diferente.</p>

<p>Depois de entender os formatos de dados nos quais irei detalhar melhor a seguir, é importante compreender quais tipos de dados podem ser representados pelos formatos dos quais serão ditos neste artigo. O objetivo das linguagens de estruturação de dados é fazer com que, números, palavras e até complexos objetos de instância de software se comuniquem.</p>

#### String 

<p>Indiscutivelmente, é o tipo de dado mais fundamental. Esse parâmetro é a maneira mais comum de representar uma sequência de números, letras ou símbolos. Caso você queira representar alguma frase em um determinado idioma em um dos formatos de estrutura de dados, provavelmente usará uma string para fazer isso. No python, você pode representar uma string através do parâmetro str ou unicode.</p>

#### Integer

<p>O parâmetro integer representa tipos de dados que contém valores numéricos, mas para maioria das pessoas ao discutir valores numéricos se baseiam em números inteiros. O número inteiro é exatamente aquilo que você aprendeu na escola – Um número inteiro, positivo ou negativo. Existe outro tipo de dado como float que você pode usar ao descrever valores não inteiros. Python utiliza o parâmetro int para declarar valores do tipo inteiro.</p>

#### Boolean

<p>Um dos tipos de dados mais simples é o booleano, um valor simples que é verdadeiro ou falso. Esse é um tipo muito popular usado quando um programador deseja saber o resultado de uma operação ou se dois valores são iguais um ao outro, por exemplo. Isso é conhecido como o tipo bool no python.</p>

<p>Dica: Os Network Developers obtém uma estrutura de automação poderosa quando combinam scrpts python com formatos de dados JSON, XML ou YAML junto com as bibliotecas de automação.</p>

### Linguagens de formatação de dados

#### YAML

<p>Esse formato de estrutura de dados é muito amigável aos olhos dos seres humanos. Se você comparar o YAML com outros formatos de dados que discutiremos como XML ou JSON, parece fazer a mesma coisa - Representa construções como listas, pares de valores-chave, sequências de caracteres e números inteiros.</p>

<p>No entanto, como você verá em breve, o YAML faz isso de uma maneira excepcionalmente legível para humanos. O YAML é muito fácil de ler e escrever se você entender os tipos de dados básicos discutidos anteriormente.</p>

<p>Esse é um grande motivo pelo qual um número crescente de ferramentas está usando o YAML como um método para definir um fluxo de trabalho de automação ou fornecer um conjunto de dados para trabalhar (como uma lista de VLANs). É muito fácil usar o YAML para ir de zero a um funcional fluxo de trabalho de automação ou para definir os dados que você deseja enviar para um dispositivo de rede.</p>

<p>Na imagem descrita abaixo, percebe-se que no início contém três hifens, isso significa que todo início de um código .yml deve conter os três hifens.</p>

{% highlight ruby %}
    ---
    - core switch
    - 6500
    - false
    - ['switchport', 'mode', 'access'] 
{% endhighlight %}

<p>Os três hifens também servirá para você declarar outras instâncias e assim indicar vários documentos dentro de um arquivo ou fluxo de dados.</p>

<p>Percebe-se que o YAML imita a arquitetura do python, dessa forma obtemos vantagem ao trabalhar com os dois juntos. No exemplo acima, temos uma lista que indica quatro itens, cada item e um tipo totalmente exclusivo.</p>

<p>O primeiro item, core switch, é um tipo de string. O segundo, 6500, é interpretado como um número inteiro. O terceiro é interpretado como um booleano. Essa “interpretação" é executada por um intérprete YAML, como PyYAML. PyYAML, em específico, faz um bom trabalho ao se referir no tipo de dados que o usuário está tentando comunicar.</p>

<p>Por exemplo, você pode escrever False, como no exemplo acima, ou você poderia escrever não, desligado ou simplesmente n. Todos eles acabam significando a mesma coisa: um valor booleano falso. Esta é uma grande razão pela qual o YAML é frequentemente usado como uma interface humana para muitos softwares.</p>

<p>O exemplo a seguir iremos nos referir nos formatos de dicionário que contém nessa linguagem.</p>

{% highlight ruby %}
    ---
    - core switch
    - 6500
    - false
    - ['switchport', 'mode', 'access'] 
    
    ---
    {juniper: EX9200, cisco: 6500,
     VMware: ['esxi', 'vcenter']}
{% endhighlight %}


<img src="{{ '/assets/img/touring.jpg' | prepend: site.baseurl }}" alt=""> 
