---
layout: post 
title: "Network Programmability - Manipulamdo dados JSON com Python"
date:   2020-03-30
---

<p class="intro"><span class="dropcap">N</span>o artigo de hoje, iremos falar  dos conceitos por debaixo dos panos quando citamos Json como linguagem de estrutura e formatos de dados. Pegue seu café, se acomode e vamos nessa!</p>

<p>Para darmos ênfase neste artigo, iremos instalar no nosso ambiente python a biblioteca netmiko com o seguinte comando:</p>
{%highlight ruby%}
   $ sudo pip install netmiko
{%endhighlight%}
<p>Neste artigo iremos trabalhar com as bibliotecas netmiko e TextFSM junto com a linguagem Json para estruturar os dados que iremos manipular.</p>

<p>Criaremos um arquivo teste01.py para manipularmos os parâmetros de saída de comandos “show”. Dentro desse arquivo iremos montar uma estrutura básica para alocar a biblioteca netmiko:</p>
{%highlight ruby%}
 import os
 import json
 from netmiko import ConnectHandler

user = os.environ.get('username')
pw = os.environ.get('password')

SW_CORE = {
    'device_type': 'cisco_ios', 
    'host':   '192.168.36.12',
    'username': 'teste',
    'password': 'teste',
    'port' : 22
}
{% endhighlight %}
<p>A classe SW_CORE se refere ao dicionário que estamos criando, dentro desse dicionário iremos atribuir alguns atributos.</p>

<p>device_type - se refere ao módulo do tipo de IOS que iremos interagir, esse módulo faz parte da lista dos módulos da biblioteca netmiko.</p>

<p>host – Inserimos o IP de gerência do appliance que queremos interagir.</p>

<p>username e password – Usuário local do appliance ou qualquer usuário que esteja cadastrado no radius.</p>

<p>port – Iremos utilizar a porta 22 por que queremos acessar esse appliance via SSH.</p>

<p>Depois de montarmos a estrutura básica de conexão, iremos chamar essa classe para os parâmetros acima serem executados:   </p>
{%highlight ruby%}
 try:
	connect = ConnectHandler (**SW_CORE)
{%endhighlight%}

A variável “connect” representa a classe SW_CORE, essa variável será usada como referência ao enviarmos comandos para o appliance. O parâmetro try serve para montarmos uma estrutura em bloco, dentro dessa estrutura iremos setar comandos, quem irá fazer essa execução é a função connect.send_command():
{%highlight ruby%}
output = connect.send_command('show ip int brief', use_textfsm=True)
    print(json.dumps(output, indent=2))
except Exception as e:
    print(e)
{%endhighlight%}
<p>Dentro dessa função, inserimos o comando “show ip int br”, repare que o parâmetro ”use” atribuiu o módulo TextFSM para ser utilizado ao estruturar a saída do comando “show...” em linguagem Json.</p>

<p>Ao inserir a função “json.dumps()” dentro do parâmetro “print”, queremos fazer referência à variável “output” para ser estruturado em Json com o espaçamento entre linhas de 2 (indent=2) para temos uma melhor resolução ao lermos os elementos escritos em Json.</p>

<p>Ao executar o arquivo teste01.py, tivemos a seguinte saída no terminal:</p>
{% highlight ruby %} 
 {
    "intf": "Ethernet0/2",
    "ipaddr": "unassigned",
    "status": "administratively down",
    "proto": "down"
  },
  {
    "intf": "Ethernet0/3",
    "ipaddr": "unassigned",
    "status": "administratively down",
    "proto": "down"
  },
{% endhighlight %}
<p>Dessa forma conseguimos obter uma saída em Json, o que facilita muito ao querermos integrar nossa estrutura de comandos em formatos Json com ferramentas de automação.</p>

<p>Para manipularmos essa estrutura Json, caso eu queira montar um relatório sobre as interfaces de um ou mais appliances, iremos fazer a manipulação dos parâmetros referente ao comando “show...” executado acima:</p>
{%highlight ruby%}
 try:
	connect = ConnectHandler (**SW_CORE)
	interfaces = connect.send_command('show ip int brief', use_textfsm=True)
	for interface in interfaces:
        		if interface['status'] == 'administratively down':             
             		print(f"{interface['intf']} IS DOWN!")
except Exception as e:
   print(e) 
{%endhighlight%}
<p>O script acima se refere que, definimos um atributo “interface” para representar a variável interfaces. Dentro desse atributo iremos brincar apenas com a lógica do parâmetro “if”.  Vale ressaltar que os parâmetros acima devem estar dentro do bloco “try”.</p>

<p>Se o atributo interface que está representando a variável “interfaces” estiver com o parâmetro “status” (que faz parte do comando “show...”) em “administratively down”, printar a variável ”interface” junto com o parâmetro que queremos inserir algum argumento. No caso, queremos que a resposta desse script seja a interface sendo representada pelo argumento que definimos (IS DOWN!). Ao executar este arquivo, tivemos o seguinte resultado:</p>
{%highlight ruby%}
  Ethernet0/1 IS DOWN!
  Ethernet0/2 IS DOWN!
  Ethernet0/3 IS DOWN!
{%endhighlight%}
<p>Essa é a maneira que manipulamos dados Json com python. Neste artigo mostramos como trabalhar com Json e python. E aí, o que achou deste artigo? Dê um pulo em nosso <a href="https://www.linkedin.com/in/thiago-porfirio/">Linkedin</a>Linkedin para ficar por dentro de novas publicações, vai ser legal contar com sua presença por lá.</p>

