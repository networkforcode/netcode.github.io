---
layout: post 
title: "Network Automation - Automatize sua rede com o pyATS (DEMO) Part 1"
date:   2020-05-11
---

<p class="intro"><span class="dropcap">B</span>uenas NetCode… Neste artigo iremos dar os primeiros passos com o pyATS e Genie, iremos desmistificar o conceito por trás dessas duas ferramentas que de certa forma ajuda bastante nos diagnósticos de validação do status da rede.</p>

<p>O que é pyATS e Genie?</p>

<p>Com a estrutura de verificação do pyATS podemos simplificar nossa estrutura de redes e otimizar nossas operações diárias. Com o Genie, podemos aplicar filtros para fazer a aprendizagem de inúmeros serviços que estão operando na rede.</p>

<p>Usando essas duas ferramentas em conjunto, podemos fazer uma comparação clara sobre o status da rede antes e depois de determinada configuração foi aplicada.</p>

<p>O genie serve como analisador de rede. Compara o antes e o depois da rede baseado no objeto que você deseja analisar.</p>

<p>Dica: O objeto analisador se refere ao tipo de protocolo que você deseja aprender ou o próprio comando em específico, a saída do comando é baseada em dicionário python, dessa forma podemos  manipular os parâmetros armazenados na saída do comando aprendido.</p>

<p>A ideia de usar o genie para automatizar o diagnostico da rede, é fazer com que, você possa instanciar a grande maioria ou os principais serviços que rodam no seus dispositivos de rede em geral, servidor, router, firewall, switch, entre outros.</p>

<p>Dessa forma podemos pegar a configuração do antes relacionados aos objetos e apenas gravar
o depois quando alguma alteração nesses serviços (objetos) instanciados vieram a ter falhas, com isso você pode comparar o antes e o depois da alteração para identificar o erro.</p>

<p>Dica: O genie atua na solução pyats como validador da estrutura de automação, ele valida aquilo
que foi feito, por exemplo, uma configuração que não tinha no device.</p>

<p>Já o pyATS pode ser usado para automação de testes, exemplo, desejo validar se a interface Ethernet0/2 está DOWN:</p>

```javascript
log.info(banner("Executando parser para filtrar parâmetros..."))
#Executando o parâmetro parse para retornar dados.
pre_output = device.parse("show interface Ethernet0/2")
# Filtrando parâmetros para efetuar possíveis verificações
pre_status = pre_output['Ethernet0/2']['oper_status']
if pre_status == 'down':
log.info("\nPASS: Interface Ethernet0/2 está DOWN\n")
else:
log.error("\nFAIL: Interface Ethernet0/2 está UP\n")
exit()
```
