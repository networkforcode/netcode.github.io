---
layout: post 
title: "Network Automation - Primeiros passos com Ansible"
date:   2020-04-26
---

<p class="intro"><span class="dropcap">B</span>uenas NetCode,  hoje iremos iniciar os primeiros passos nos conceitos por debaixo de uma ferramenta poderosa para automação chamada Ansible.</p>

<p>Os conceitos descritos abaixo são fundamentais para darmos ênfase no aprendizado em Ansible, pois, esses conceitos não se encaixam apenas em Network Automation:</p>
* Control node
* Managed nodes
* Inventory
* Modules
* Tasks
* Playbooks

#### Control node
<p>É um centro de controle onde você poderá executar o ansible na rede, não necessariamente você poderia executar em apenas uma máquina linux, mas em qualquer outra que tenha acesso aos dispositivos da rede via conexão SSH.</p>

<p>Você pode executar playbooks, inserindo apenas o caminho {%highlight ruby%} /usr/bin/ansibleou/usr/bin/ansible-playbook{%endhighlight%}. Vale ser ressaltado que a máquina deve possuir o python instalado, de preferência a versão 3.</p>

{%highlight ruby%}
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
{%endhighlight%}

#### Managed nodes
<p>São dispositivos de redes que não contém o ansible instalado e que possuí gerências ansible, ou seja, eles está abaixo do domínio ansible baseado em um ponto de control node. Esses devices gerenciáveis são dispositivos de rede em geral. São devices que você gerência via ansible, esses managed nodes são chamados de hosts.</p>

#### Inventory

<p>É uma lista onde contém números de managed nodes, nessa lista contém o endereço IP de cada hosts, um inventário também é considerado “hostfile” por conter um dimensionamento de diversos devices em apenas um inventario. Dentro desse inventário você pode organizar seus managed nodes, criando e alinhando-os em grupos para facilitar a distribuição de automação.</p>

#### Modules

<p>Dentro de um manual você pode invocar apenas um módulo para executar apenas uma tarefa em determinados hosts, ou executar vários módulos diferentes em apenas um manual. Exemplo, quero administrar diversos users em um BD, posso executar apenas um módulo com uma única tarefa ou gerenciar interfaces VLAN em um tipo especifico de dispositivo de rede.</p>

#### Tasks

<p>Você pode executar uma única tarefa uma vez com um comando ad-hoc. Iremos falar sobre como trabalhar com o ad-hoc commands no próximo artigo sobre ansible. Você pode definir inúmeras tasks dentro de um playbook ou via ad-hoc.</p>

#### Playbooks

<p>O playnook nada mais é que, listas ordenadas de tarefas, nessa lista contém tarefas em ordem. Nos playbooks pode incluir variáveis e tarefas, os playbooks são escritos em YAML e são fáceis de ler, escrever, compartilhar e entender.</p>

<p>Agora que demos ênfase nos jargões comuns ao se trabalhar com ansible, iremos abordar conceitos mais aprofundado sobre control node:</p>
* Execução de control node
* Protocolos de comunicação múltiplos
* Módulos organizados pela plataforma de rede
* Escalonamento de privilégios 
* Usando become para escalação de privilégios
* Playbooks herdados: authorize para escalação de privilégios
* Criando seu próprio arquivo hosts
* Criando seu próprio arquivo ansible.cfg

#### Execução do control node
<p>Os módulos dos devices de rede não são executados por eles próprios quando estão sendo utilizados via gerência ansible.</p>

<pEsses módulos trabalham com comandos manuais e funções ad-hoc. Isso acontece pelo fato de que, a maioria dos módulos dos devices de redes não trabalham com python, diferente dos módulos Linux Unix. Esses devices passam a ser executados via próprio módulo de seu SO, porém, é executado direto no control node, os módulos de rede ansible são executados via ansible ou ansible-playbook.</p>

<p>Vale ressaltar que os módulos de rede alocado nos devices managed nodes trabalham com arquivos de backup direto no control node. No caso do linux, o arquivo de backup é gravado no mesmo diretório do próprio arquivo alterado.</p>

#### Protocolos de comunicação múltiplos
<p>Como já sabemos, os módulos de rede são executados no control node e não nos managed nodes. Baseado nisso, o control node suporta protocolos de comunicação para enviar a execução para os managed nodes. São eles:</p>

<img src="{{ '/assets/img/artigo03/img.png' | prepend: site.baseurl }}" alt=""> 

<p>Vale ressaltar que cada módulo de rede suporta um tipo de protocolo de comunicação, alguns módulos suportam apenas um protocolo, alguns oferecem uma escolha de protocolos a serem utilizados.</p>

<p>O protocolo mais comum utilizado é o CLI sobre SSH, para definir um protocolo de comunicação basta inserir o comando ansible_connection que seria uma variável.</p>

<p>Dica: O tipo de conexão local não seria viável perante a versão mais recente do ansible, caso você esteja utilizando a versão 2.5, seria viável declarar essa variável de conexão.</p>

#### Módulos organizados pela plataforma de rede
<p>Vale ressaltar que uma plataforma de rede seria um conjunto de dispositivos de rede com um SO comum entre eles e que pode ser gerenciados por uma coleção de módulos. Os módulos para cada plataforma de rede compartilham um prefixo, por exemplo:</p>

* Arista – eos_
* Cisco – ios_, iosxr_, nxos_
* Zimbro: junos_
* VyOS: vyos_

#### Escalonamento de privilégios 
<p>Isso é comum mas vale a pena ser ressaltado, os devices que pertencem ao grupo de managed nodes  suportam níveis de privilégios, nos devices de rede, isso é chamado de enable mode, o que seria equivalente ao sudo executado em plataformas unix.</p>

#### Usando become para escalação de privilégios
<p>A partir do Ansible 2.6 você pode usar o parâmetro Ansible de nível superior para executar uma tarefa em qualquer plataforma que suporta escalonamento de privilégio. Caso vocẽ esteja fazendo em control node para replicar nos managed nodes, o arquivo será semelhante ao:</p>
* ansible_connection: network_cli
* ansible_network_os: ios
* ansible_become: yes
* ansible_become_method: enable

#### Playbooks herdados: authorize para escalação de privilégios
<p>Caso estiver executando no Ansible 2.5 ou mais antigo, algumas plataformas de rede suportam escalação de privilégios, mas não conexões network_cli ou httpapi.</p>

<pIsso inclui todas as plataformas nas versões 2.4 e mais antigas e conexões HTTPS usandoeapina versão 2.5. Com umalocalconexão, você deve usar um provider dicionário e incluir. Para esse caso de uso, um arquivo se parece com:</p>
{%highlight ruby%}
    ansible_connection: local
    ansible_network_os: eos
    # provider settings
    eapi:
        authorize: yes
        auth_pass: " {{ secret_auth_pass }}"
        port: 80
        transport: eapi
    use_ssl: no
    
    # E utiliza a eapi variávl em suas tarefas:
    tasks:
      - name: provider demo with eos
         eos_banner:
           banner: motd
           text: 
               this is test
               of multiline
               string
           state: present
         provider: "{{ eapi }}"
{%endhighlight%}
<p>Isso já é obsoleto, porém, ainda dar para se utilizar dependendo da versão do ansible no qual você está trabalhando.</p>

#### Criando seu próprio arquivo hosts
<p>Bom, esse arquivo hosts podemos criar em qualquer localização dentro de nossa máquina.. Esse arquivo é chamado de inventário, pois, nele adicionamos os hosts que queremos configurar. Vale ressaltar que o diretório no qual você está executando seu playbook, deverá conter também nesse diretório o arquivo hosts e o arquivo ansible.cfg, ou você pode centralizar esses arquivos em algum diretório e indicar o caminho deles nas variáveis de conexão dentro do playbook.</p>

<p>Abaixo é mostrado o arquivo hosts, note que adicionei dois grupos de hosts:</p>
{% highlight ruby %}
   [ansible_core]
   SW_CORE_1
   SW_CORE_2
   [ansible_access]
   SW_ACCESS_1
   SW_ACCESS_2
   SW_ACCESS_3
   SW_ACCESS_4
{% endhighlight %}
<p>Adicionamos apenas o hostname desses devices, o endereço desses devices estão alocados no arquivo {%highlight ruby%} /etc/hosts {% endhighlight %}:</p>
{%highlight ruby%}
   127.0.0.1   localhost

   # The following lines are desirable for IPv6 capable hosts
   ::1     ip6-localhost ip6-loopback
   fe00::0 ip6-localnet
   ff00::0 ip6-mcastprefix
   ff02::1 ip6-allnodes
   ff02::2 ip6-allrouters

   192.168.36.214 SW_CORE_1
   192.168.36.215 SW_CORE_2

   192.168.36.210 SW_ACCESS_1
   192.168.36.211 SW_ACCESS_2
   192.168.36.212 SW_ACCESS_3
   192.168.36.213 SW_ACCESS_4
{%endhighlight%}
<p>Note que os endereços estão sendo alocados direto no arquivo hosts do linux, arquivo onde é alocado endereços locais. Dessa forma podemos efetuar um ping descrevendo apenas o hostname que irá funcionar por que ele já é um endereço conhecido localmente:</p>
{%highlight ruby%}
    root@thiago:/home/thiago/Documentos/Code/Ansible# ping SW_ACCESS_1
    PING SW_ACCESS_1 (192.168.36.210) 56(84) bytes of data.
    64 bytes from SW_ACCESS_1 (192.168.36.210): icmp_seq=1 ttl=255 time=1.28 ms
    64 bytes from SW_ACCESS_1 (192.168.36.210): icmp_seq=2 ttl=255 time=1.29 ms
{%endhighlight%}

<p>##</p>
