# Roteamento aplicado a Data Center

## Objetivo

Este documento apresenta os conceitos básicos de roteamento aplicados ao diagnóstico de conectividade em ambientes de Data Center.

O foco é entender como os pacotes trafegam entre redes diferentes e como um técnico pode identificar falhas relacionadas a gateway, rotas, VLANs, firewall ou conectividade externa.

## O que é roteamento?

Roteamento é o processo de encaminhar pacotes de dados entre redes diferentes.

Quando um servidor precisa se comunicar com outro dispositivo dentro da mesma rede, essa comunicação pode acontecer diretamente.

Quando o servidor precisa acessar outra rede, outra VLAN, a internet ou um serviço externo, ele depende de um gateway ou roteador.

## Exemplo simples

Um servidor está na rede:

```text
192.168.10.0/24
```

O servidor possui o IP:

```text
192.168.10.20
```

E o gateway padrão é:

```text
192.168.10.1
```

Quando esse servidor precisa acessar uma rede diferente, ele envia o tráfego para o gateway `192.168.10.1`.

## Por que roteamento é importante em Data Center?

Em ambientes de Data Center, normalmente existem várias redes separadas para organizar, proteger e controlar o tráfego.

Exemplos:

- Rede de servidores
- Rede de banco de dados
- Rede de armazenamento
- Rede de backup
- Rede de monitoramento
- Rede de administração
- Rede de usuários
- Rede de acesso à internet

O roteamento permite que essas redes se comuniquem de forma controlada.

Sem roteamento correto, um servidor pode estar ligado e funcionando, mas não conseguir acessar outros sistemas, aplicações, APIs, bancos de dados ou serviços externos.

## Gateway padrão

O gateway padrão é o caminho usado por um servidor quando ele precisa acessar uma rede que não está diretamente conectada a ele.

Exemplo:

```text
Servidor: 192.168.10.20
Gateway: 192.168.10.1
```

Se o gateway estiver incorreto, o servidor pode até se comunicar com dispositivos da mesma rede, mas não conseguirá acessar redes externas.

## Cenário de falha

### Situação

Um servidor em um Data Center está ligado e responde localmente, mas não consegue acessar sistemas externos ou a internet.

### Possíveis causas

- Gateway incorreto
- Rota padrão ausente
- Firewall bloqueando tráfego
- VLAN incorreta
- Problema no switch
- Problema no roteador
- Falha na rota até o destino
- Política de acesso bloqueando comunicação
- Problema de DNS confundido com problema de rota

## Comandos úteis no Windows

### Verificar configuração de IP

```bash
ipconfig /all
```

Esse comando ajuda a verificar:

- IP
- Máscara de rede
- Gateway padrão
- Servidor DNS
- Adaptador de rede ativo

### Ver tabela de rotas

```bash
route print
```

Esse comando mostra as rotas conhecidas pela máquina.

### Testar o gateway

```bash
ping 192.168.10.1
```

### Testar rota até um destino

```bash
tracert 8.8.8.8
```

O comando `tracert` mostra por quais saltos o tráfego passa até chegar ao destino.

## Comandos úteis no Linux

### Ver endereços IP

```bash
ip addr
```

### Ver rotas configuradas

```bash
ip route
```

### Testar o gateway

```bash
ping 192.168.10.1
```

### Testar rota até um destino

```bash
traceroute 8.8.8.8
```

Se o `traceroute` não estiver instalado, em algumas distribuições pode ser necessário instalar o pacote correspondente.

### Testar acesso HTTP

```bash
curl -I https://google.com
```

Esse comando ajuda a verificar se existe resposta de um serviço web.

## Checklist de diagnóstico de roteamento

Use este checklist ao investigar uma falha de conectividade:

```text
[ ] O servidor possui IP válido?
[ ] A máscara de rede está correta?
[ ] O gateway padrão está configurado?
[ ] O servidor consegue pingar o gateway?
[ ] A tabela de rotas possui rota padrão?
[ ] O tráfego chega até o destino?
[ ] O problema ocorre apenas para uma rede específica?
[ ] O problema ocorre para todos os destinos externos?
[ ] Existe firewall bloqueando o tráfego?
[ ] A VLAN está correta?
[ ] O problema pode estar relacionado a DNS?
```

## Raciocínio técnico

Se o servidor não consegue pingar o gateway, o problema pode estar na rede local, VLAN, cabo, switch, configuração IP ou firewall local.

Se o servidor consegue pingar o gateway, mas não acessa redes externas, o problema pode estar em rota, firewall, roteador ou política de acesso.

Se o servidor acessa IP externo, mas não acessa pelo nome, o problema provavelmente está relacionado a DNS, não a roteamento.

## Aplicação em rotina de Data Center

Durante um incidente em Data Center, o técnico precisa identificar rapidamente se a falha é local, de rede, de rota ou de serviço.

Testes de roteamento ajudam a entender:

- Se o servidor está corretamente conectado à rede
- Se o gateway responde
- Se a rota padrão existe
- Se o tráfego chega até o destino
- Se existe bloqueio em algum ponto do caminho
- Se o incidente deve ser escalado para rede, firewall, sistema operacional ou aplicação

## Exemplo de registro de evidência

```text
Incidente: Servidor sem acesso externo

Servidor: app01
IP: 192.168.10.20
Gateway esperado: 192.168.10.1

Teste realizado: ping 192.168.10.1
Resultado: gateway respondeu normalmente

Teste realizado: tracert 8.8.8.8
Resultado: tráfego parou após o gateway

Causa provável: bloqueio ou falha de rota após o gateway
Ação recomendada: escalar para o time de redes/firewall com evidências dos testes
```

## Conclusão

Roteamento é essencial para a comunicação entre redes em ambientes de Data Center.

Entender gateway, rotas e testes de conectividade ajuda o técnico a diagnosticar falhas com mais precisão, reduzir tempo de indisponibilidade e direcionar o incidente para o time correto.
