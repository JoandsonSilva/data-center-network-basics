# Fundamentos de Rede para Data Center: DNS, Roteamento e IPv6

## Objetivo do projeto

Este projeto apresenta uma documentação técnica sobre DNS, roteamento e IPv6 aplicados ao diagnóstico de conectividade em ambientes de Data Center.

O foco é simular a visão inicial de um técnico de Data Center ao investigar falhas de acesso a servidores, aplicações e serviços críticos.

## Contexto

Em um ambiente de Data Center, falhas de rede podem impactar aplicações, servidores, sistemas internos, monitoramento, backups e usuários.

Por isso, um técnico precisa saber identificar se uma falha está relacionada a:

- DNS
- Roteamento
- Gateway
- IPv4
- IPv6
- Firewall
- Serviço parado
- Problema físico ou lógico

Este projeto tem como objetivo organizar esses conceitos de forma prática e documentada.

## Estrutura do projeto
data-center-network-basics/
│
├── README.md
├── docs/
│   ├── dns.md
│   ├── roteamento.md
│   ├── ipv6.md
│   └── troubleshooting-servidor-inacessivel.md
│
├── comandos/
│   └── comandos-de-diagnostico.md
│
└── imagens/
    └── topologia-data-center-basica.png

    
## Topologia conceitual

Internet
   |
Firewall / Gateway
   |
Switch Core
   |
------------------------------------------------
|                    |                         |
VLAN Servidores      VLAN Monitoramento        VLAN Administração
|                    |                         |
Servidor Web         Servidor Grafana          Estação Técnica
Servidor DNS         Prometheus                Acesso Suporte

## Comandos utilizados
ip addr
ip route
ping
ping -6
traceroute
dig
nslookup
curl

## Cenário simulado

Um servidor hospedado em um ambiente de Data Center está ligado, mas a aplicação não responde pelo nome de domínio.

Exemplo:

```text
app.empresa.local


## Status do projeto

Em desenvolvimento.

Autor

Joandson Oliveira
Transição profissional para Infraestrutura, Cloud Computing e Data Center.




