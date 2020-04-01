---
layout: post 
title: "Network Programmability - Manipulando dados JSON com Python"
date:   2020-03-31
---

<p class="intro"><span class="dropcap">N</span>o artigo de hoje, {%highlight ruby%} sudo pip install netmiko {%endhighlight%} iremos falar  dos conceitos por debaixo dos panos quando citamos Json como linguagem de estrutura e formatos de dados. Pegue seu café, se acomode e vamos nessa!</p>

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
sec = os.environ.get('secret')

SW_CORE = {
    'device_type': 'cisco_ios', 
    'host':   '192.168.36.12',
    'username': 'teste',
    'password': 'teste',
    'secret': 'teste', 
    'port' : 22
}
{% endhighlight %}
<p>A classe SW_CORE se refere ao dicionário que estamos criando, dentro desse dicionário iremos atribuir alguns atributos.</p>

<p>device_type - se refere ao módulo do tipo de IOS que iremos interagir, esse módulo faz parte da lista dos módulos da biblioteca netmiko. Alocamos o módulo na estrutura de dicionário como um dos parâmetros que descreve o dispositivo que iremos interagir mais adiante.</p>
{%highlight ruby%}
 CLASS_MAPPER_BASE = {
    "a10": A10SSH,
    "accedian": AccedianSSH,
    "alcatel_aos": AlcatelAosSSH,
    "alcatel_sros": NokiaSrosSSH,
    "apresia_aeos": ApresiaAeosSSH,
    "arista_eos": AristaSSH,
    "aruba_os": ArubaSSH,
    "avaya_ers": ExtremeErsSSH,
    "avaya_vsp": ExtremeVspSSH,
    "brocade_fastiron": RuckusFastironSSH,
    "brocade_netiron": ExtremeNetironSSH,
    "brocade_nos": ExtremeNosSSH,
    "brocade_vdx": ExtremeNosSSH,
    "brocade_vyos": VyOSSSH,
    "checkpoint_gaia": CheckPointGaiaSSH,
    "calix_b6": CalixB6SSH,
    "ciena_saos": CienaSaosSSH,
    "cisco_asa": CiscoAsaSSH,
    "cisco_ios": CiscoIosSSH,
    "cisco_nxos": CiscoNxosSSH,
    "cisco_s300": CiscoS300SSH,
    "cisco_tp": CiscoTpTcCeSSH,
    "cisco_wlc": CiscoWlcSSH,
    "cisco_xe": CiscoIosSSH,
    "cisco_xr": CiscoXrSSH,
    "cloudgenix_ion": CloudGenixIonSSH,
    "coriant": CoriantSSH,
    "dell_dnos9": DellForce10SSH,
    "dell_force10": DellForce10SSH,
    "dell_os6": DellDNOS6SSH,
    "dell_os9": DellForce10SSH,
    "dell_os10": DellOS10SSH,
    "dell_powerconnect": DellPowerConnectSSH,
    "dell_isilon": DellIsilonSSH,
    "endace": EndaceSSH,
    "eltex": EltexSSH,
    "eltex_esr": EltexEsrSSH,
    "enterasys": EnterasysSSH,
    "extreme": ExtremeExosSSH,
    "extreme_ers": ExtremeErsSSH,
    "extreme_exos": ExtremeExosSSH,
    "extreme_netiron": ExtremeNetironSSH,
    "extreme_nos": ExtremeNosSSH,
    "extreme_slx": ExtremeSlxSSH,
    "extreme_vdx": ExtremeNosSSH,
    "extreme_vsp": ExtremeVspSSH,
    "extreme_wing": ExtremeWingSSH,
    "f5_ltm": F5TmshSSH,
    "f5_tmsh": F5TmshSSH,
    "f5_linux": F5LinuxSSH,
    "flexvnf": FlexvnfSSH,
    "fortinet": FortinetSSH,
    "generic_termserver": TerminalServerSSH,
    "hp_comware": HPComwareSSH,
    "hp_procurve": HPProcurveSSH,
    "huawei": HuaweiSSH,
    "huawei_vrpv8": HuaweiVrpv8SSH,
    "ipinfusion_ocnos": IpInfusionOcNOSSSH,
    "juniper": JuniperSSH,
    "juniper_junos": JuniperSSH,
    "juniper_screenos": JuniperScreenOsSSH,
    "keymile": KeymileSSH,
    "keymile_nos": KeymileNOSSSH,
    "linux": LinuxSSH,
    "mikrotik_routeros": MikrotikRouterOsSSH,
    "mikrotik_switchos": MikrotikSwitchOsSSH,
    "mellanox": MellanoxMlnxosSSH,
    "mellanox_mlnxos": MellanoxMlnxosSSH,
    "mrv_lx": MrvLxSSH,
    "mrv_optiswitch": MrvOptiswitchSSH,
    "netapp_cdot": NetAppcDotSSH,
    "netscaler": NetscalerSSH,
    "nokia_sros": NokiaSrosSSH,
    "oneaccess_oneos": OneaccessOneOSSSH,
    "ovs_linux": OvsLinuxSSH,
    "paloalto_panos": PaloAltoPanosSSH,
    "pluribus": PluribusSSH,
    "quanta_mesh": QuantaMeshSSH,
    "rad_etx": RadETXSSH,
    "ruckus_fastiron": RuckusFastironSSH,
    "ruijie_os": RuijieOSSSH,
    "ubiquiti_edge": UbiquitiEdgeSSH,
    "ubiquiti_edgeswitch": UbiquitiEdgeSSH,
    "vyatta_vyos": VyOSSSH,
    "vyos": VyOSSSH,
}
{%endhighlight%}
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
<p> Iremos analisar outro bloco try:</p>
{%highlight ruby%}
 try: 
  net_connect = ConnectHandler (**SW_CORE)
  
  stps = net_connect.send_command('show spanning-tree', use_textfsm=True)
  print(json.dumps(stps, indent=2))
  for stp in stps:
     print(' ') # Espaçamento das linhas
        # printando os parâmetros da saída da CLI. queremos definir a vlan que a interface pertence e se a interface está em modo de designated.
     print(f"{stp['interface']}.{stp['vlan_id']} is currently in role {stp['role']} ")
    
except Exception as e: 
    print(e)
{%endhighlight%}
<p>No bloco "try" acima, queremos printar os parâmetros da CLI baseado no comando "show spanning-tree". Queremos que esse bloco nos traga o retorno de que, Deverá ser definido a VLAN que a interface pertence e também trazer a informação de que a interface está em modo designated.</p>
{%highlight ruby%}
  Et0/0.1 is currently in role Desg 

  Et0/1.1 is currently in role Desg 

  Et0/2.1 is currently in role Desg 

  Et0/3.1 is currently in role Desg 
{%endhighlight%}
<p> Esse retorno acima trás os parâmetro que queremos ver no comando "show spanning-tree".</p>

<p>Essa é a maneira que manipulamos dados Json com python. Neste artigo mostramos como trabalhar com Json e python. E aí, o que achou deste artigo? Dá um pulo lá no nosso <a href="https://www.linkedin.com/company/ccna-student/?viewAsMember=true">Linkedin</a> para ficar por dentro de novas publicações, vai ser legal contar com sua presença por lá.</p>





