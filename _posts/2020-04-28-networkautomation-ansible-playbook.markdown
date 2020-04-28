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

<img src="{{ '/assets/img/lab3.jpg' | prepend: site.baseurl }}" alt=""> 

<p>Bom, iremos montar nosso playbook para executar a primeira task:</p>

```bash
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

```bash
  tasks:
    - name: Configurando VTP Transparent mode e NTP em todos os switches 
      ios_config: # Módulo de configuração       
        lines:
          - vtp domain ansible
          - vtp mode transparent         
        
      register: print_output # Armazenando os dados executados no módulo acima

    -  debug: var=print_output.stdout_lines # Imprimindo os dados armazenados 
```

<p>Bom, com poucas linhas de código, podemos configurar o VTP e replicar em todos os devices da topologia.</p>

