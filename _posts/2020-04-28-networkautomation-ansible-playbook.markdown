---
layout: post 
title: "Network Automation - Criando um playbook com os módulos ios_config, ios_vlan e ios_l2_interface"
date:   2020-04-28
---

<p class="intro"><span class="dropcap">N</span>o artigo de hoje, iremos trabalhar com módulos do Ansible para criar um playbook onde irá fazer o provisionamento das seguintes tarefas:</p>

✅ Habilitar o VTP transparent mode

✅ Criar VLANS 10, 20, 30 e 40

✅ Habilitar 802.1q nas interfaces

✅ Habilitar trunk mode nas interfaces

✅ Configurar switch virtual interface com HSRP

✅ Dimensionar HSRP load balancing

<p>Iremos utilizar o lab mostrado na imagem abaixo. Este lab encontra-se no meu repositório do <a href="https://www.linkedin.com/company/ccna-student/?viewAsMember=true">GitHub</a> junto com o código usado neste artigo.</p>

<img src="{{ '/assets/img/artigo04.jpg' | prepend: site.baseurl }}" alt=""> 

<p>Bom, iremos montar nosso playbook para executar a primeira e a segunda task:</p>

```yaml
# Habilitar o VTP transparent mode
---
- name: Configuring devices # Nome do manual 
  hosts: all # Irá executar todos os hosts de todos os grupos que estão alocados no arquivo hosts
  gather_facts: false # Recolhe informações do dispositivo e retorna a saída em YAML
  
  vars: # Variável de conexão
    ansible_connection: network_cli
    ansible_network_os: ios
    ansible_user: teste
    ansible_ssh_pass: teste
```

<p>A variável de conexo nada mais é que, o registro de conexão do device remoto, essas variáveis também faz a conexão de usuários cadastrados no tacacs ou radius.</p>

<p>Dica: Ao declarar minhas variáveis de comunicação, utilizei o parâmetro network_cli que se refere no modo de conexão CLI sobre SSH. Vale ressaltar que no so todos os módulos de redes que suportam esse tipo de protocolo de comunicação.</p>

<p>Iremos descrever a task para enviar a configuração que queremos executar no device remoto:</p>

```yaml
  tasks:
    - name: Configurando VTP Transparent mode e NTP em todos os switches 
      ios_config: # Módulo de configuração       
        lines:
          - vtp domain ansible
          - vtp mode transparent         
        
      register: print_output # Armazenando os dados executados no módulo acima

    -  debug: var=print_output.stdout_lines # Imprimindo os dados armazenados 
    
    # Criar VLANS 10, 20, 30 e 40
    - name: Criando VLANS 10, 20, 30 e 40
      ios_vlan: # Módulos contendo apenas parametrização de vlan
        aggregate: # Executa a lista de itens ordenados pelo parâmetro vlan_id
          - vlan_id: 10              
            name: VLAN 10          
            state: active

          - vlan_id: 20              
            name: VLAN 20          
            state: active 

          - vlan_id: 30              
            name: VLAN 30          
            state: active

          - vlan_id: 40              
            name: VLAN 40          
            state: active          
          
      register: print_output
```

<p>Bom, com poucas linhas de código, podemos configurar o VTP e vlan id e replicar em todos os devices da topologia.</p>

<p>Agora iremos montar a terceira task:</p>

<img src="{{ '/assets/img/artigo04/img1.png' | prepend: site.baseurl }}" alt=""> 

<p>Essa task consiste em habilitar o 802.1q nas interfaces dos core que conecta com os switches de acessos. Agora iremos executar a qaurta task, a próxima task irá habilitar o trunk mode para ocorrer distribuição de vlans pelo link.</p>

```yaml
    - name: HABILITANDO TRUNK MODE NAS INTERFACES ATIVAS DOS SWITCHES CORE
      ios_l2_interface:
        aggregate:
          - name: Ethernet0/0
            mode: trunk
            trunk_allowed_vlans: 1-4094
          - name: Ethernet0/1
            mode: trunk
            trunk_allowed_vlans: 1-4094
          - name: Ethernet0/2
            mode: trunk
            trunk_allowed_vlans: 1-4094
          - name: Ethernet0/3
            mode: trunk
            trunk_allowed_vlans: 1-4094
            
      register: print_output
```

<p>A quarta task foi descrita dentro da variável de conexão da task anterior, pois, a quarta task irá executar comandos no mesmo grupo de devices.</p>

<p>Iremos executar a terceira e a quarta task novamente, porém, alterando as interfaces e o grupo de devices, iremos aplicar as configuração no grupo de switches de acessos.</p>
 
```yaml 
  # Habilitando 802.1q nas interfaces
- name: Configuring 802.1q in the interfaces in access switches
  hosts: ansible_access
  gather_facts: false

  vars:
    ansible_connection: network_cli
    ansible_network_os: ios
    ansible_user: teste
    ansible_ssh_pass: teste

  tasks:    
    - name: Habilitando 802.1q nas interfaces dos switches access
      ios_config:        
        parents: "interface {{ item.interface }}"
        lines:
          - switchport trunk encapsulation dot1q
      with_items:
        - { interface : Ethernet1/0 }
        - { interface : Ethernet1/1 }  

      register: print_output
      
     - name: HABILITANDO TRUNK MODE NAS INTERFACES ATIVAS DOS SWITCHES ACCESS
      ios_l2_interface:
        aggregate:
          - name: Ethernet1/0
            mode: trunk
            trunk_allowed_vlans: 1-4094
          - name: Ethernet1/1
            mode: trunk
            trunk_allowed_vlans: 1-4094          
            
      register: print_output
```
