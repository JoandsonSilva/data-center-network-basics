# Troubleshooting — Servidor Inacessível

## Status da etapa

Etapa 6 de 7 — Procedimento de troubleshooting aplicado a Data Center.

## Objetivo

Este documento apresenta um procedimento inicial de diagnóstico para casos em que um servidor ou aplicação em ambiente de Data Center está inacessível.

O foco é organizar uma sequência lógica de verificação envolvendo DNS, IP, rota, gateway, IPv6, firewall, serviço e conectividade externa.

## Cenário simulado

Um servidor de aplicação em um ambiente de Data Center está ligado, mas os usuários relatam que não conseguem acessar o sistema pelo nome:

```text
app.empresa.local
```

O técnico precisa identificar se o problema está relacionado a:

- DNS
- IP
- Gateway
- Roteamento
- IPv6
- Firewall
- Serviço parado
- Porta não exposta
- Conectividade física ou lógica

## Mentalidade operacional

Em ambiente de Data Center, o objetivo não é apenas executar comandos.

O objetivo é:

- reduzir impacto no serviço;
- identificar a camada provável da falha;
- evitar ações desnecessárias;
- registrar evidências;
- comunicar corretamente;
- escalar para o time certo quando necessário.

---

# Fluxo inicial de diagnóstico

## 1. Validar se o problema é por nome ou por IP

### Comandos

```bash
ping app.empresa.local
```

```bash
nslookup app.empresa.local
```

```bash
dig app.empresa.local
```

### Interpretação

```text
Se o acesso pelo IP funciona, mas pelo nome falha, a causa provável está relacionada a DNS.

Se o acesso falha pelo nome e pelo IP, a investigação deve seguir para rede, rota, firewall, serviço ou servidor.
```

---

# 2. Verificar configuração de rede do servidor

## Comandos

```bash
ip addr
```

```bash
ip route
```

## O que verificar

```text
- A interface de rede está ativa?
- O servidor possui IP válido?
- Existe rota padrão?
- O gateway foi identificado?
- A rede está correta?
```

## Interpretação

```text
Se o servidor não possui IP válido, pode haver problema de configuração de rede, DHCP, interface, VLAN ou conectividade local.

Se não existe rota padrão, o servidor pode não conseguir acessar redes externas.
```

---

# 3. Testar conectividade com o gateway

## Comando

```bash
ping IP_DO_GATEWAY
```

Exemplo:

```bash
ping 192.168.64.1
```

## Interpretação

```text
Se o gateway responde, a conectividade local até a saída da rede está funcionando.

Se o gateway não responde, o problema pode estar em VLAN, switch, cabo, interface, firewall local, gateway ou configuração IP.
```

---

# 4. Testar conectividade externa por IP

## Comando

```bash
ping 8.8.8.8
```

## Interpretação

```text
Se o ping para IP externo funciona, existe conectividade externa sem depender de DNS.

Se o ping para IP externo falha, o problema pode estar em rota, gateway, NAT, firewall ou bloqueio de saída.
```

---

# 5. Testar resolução DNS

## Comandos

```bash
nslookup google.com
```

```bash
dig google.com
```

```bash
dig AAAA google.com
```

## Interpretação

```text
Se o DNS resolve corretamente, a camada de resolução de nomes está funcionando.

Se o DNS não resolve, o problema pode estar no resolvedor local, servidor DNS, registro DNS, cache ou conectividade com o DNS.

Se o registro AAAA retorna IPv6, o técnico também deve considerar possíveis diferenças entre IPv4 e IPv6.
```

---

# 6. Validar IPv6 quando necessário

## Comandos

```bash
ip -6 addr
```

```bash
ip -6 route
```

```bash
dig AAAA app.empresa.local
```

## Interpretação

```text
Se IPv4 funciona e IPv6 falha, a investigação deve focar em endereço IPv6, rota IPv6, firewall IPv6 ou registro DNS AAAA.

Se o domínio possui registro AAAA incorreto, clientes podem tentar acessar um endereço IPv6 que não responde.
```

---

# 7. Testar serviço da aplicação

## Comandos

```bash
curl -I http://app.empresa.local
```

ou:

```bash
curl -I https://app.empresa.local
```

Para verificar portas e serviços em escuta:

```bash
ss -tulnp
```

ou, sem exibir processo:

```bash
ss -tuln
```

## Interpretação

```text
Se o servidor responde ping, mas a aplicação não abre, o problema pode estar no serviço, porta, firewall local ou aplicação.

Se a porta não está em escuta, o serviço pode estar parado ou mal configurado.

Se o curl retorna cabeçalhos HTTP, significa que houve resposta de um serviço web.
```

---

# Checklist de troubleshooting

```text
[x] O servidor possui IP válido?
[x] A interface de rede está ativa?
[x] Existe rota padrão?
[x] O gateway foi identificado?
[x] O acesso por IP externo funciona?
[x] O DNS resolve corretamente?
[x] Existe registro A?
[x] Existe registro AAAA?
[x] IPv4 e IPv6 foram diferenciados?
[x] Foi testado acesso HTTP/HTTPS com curl?
[x] Foram verificadas portas e serviços em escuta?
[ ] Há evidência de firewall ou bloqueio?
[ ] O incidente precisa ser escalado?
```

---

# Matriz de decisão rápida

```text
Sintoma: funciona por IP, mas não por nome
Hipótese provável: DNS

Sintoma: não pinga o gateway
Hipótese provável: rede local, VLAN, cabo, switch, firewall local ou gateway

Sintoma: pinga gateway, mas não acessa IP externo
Hipótese provável: rota, NAT, firewall ou bloqueio de saída

Sintoma: DNS resolve, ping responde, mas aplicação não abre
Hipótese provável: serviço parado, porta bloqueada ou falha na aplicação

Sintoma: curl retorna cabeçalhos HTTP
Hipótese provável: existe resposta de serviço web

Sintoma: porta não aparece em escuta no ss
Hipótese provável: serviço parado ou aplicação não inicializada

Sintoma: IPv4 funciona, IPv6 falha
Hipótese provável: rota IPv6, registro AAAA, firewall IPv6 ou suporte IPv6 incompleto

Sintoma: IPv4 e IPv6 falham
Hipótese provável: conectividade geral, serviço, firewall ou indisponibilidade do servidor
```

---

# Modelo de registro de incidente

```text
Incidente: Servidor ou aplicação inacessível

Data:
Ambiente:
Servidor:
IP:
Nome DNS:
Serviço impactado:
Impacto percebido:
Horário de início:
Responsável pela análise:

Testes realizados:
1.
2.
3.

Resultados:
- DNS:
- IP:
- Gateway:
- Rota:
- IPv6:
- HTTP/HTTPS:
- Portas/serviços:

Causa provável:

Ação tomada:

Status:
[ ] Em análise
[ ] Resolvido
[ ] Escalado

Time acionado:

Lições aprendidas:
```

---

# Exemplo aplicado ao laboratório

```text
Incidente: Validação de conectividade em servidor simulado

Data: 25/06/2026
Ambiente: VM Linux Ubuntu via UTM
Acesso: SSH
IP do servidor: 192.168.64.8
Gateway: 192.168.64.1
Estação técnica: macOS

Testes realizados:
1. ip addr
2. ip route
3. ping 8.8.8.8
4. nslookup google.com
5. dig AAAA google.com
6. ip -6 addr
7. ip -6 route
8. curl -I https://google.com
9. ss -tulnp

Resultados:
- IP: servidor possui IP válido na interface enp0s1.
- Gateway: gateway identificado como 192.168.64.1.
- Rota: rota padrão configurada corretamente.
- Conectividade externa: ping para 8.8.8.8 funcionou com 0% de perda.
- DNS: resolução de nomes funcionando via 127.0.0.53.
- IPv6: servidor possui endereço IPv6 e rota IPv6 configurada.
- DNS IPv6: registro AAAA retornado com sucesso para google.com.
- HTTP/HTTPS: curl retornou HTTP/2 301, indicando resposta válida de serviço web externo.
- Portas/serviços: ss mostrou serviços em escuta, incluindo DNS local na porta 53 e SSH na porta 22.

Causa provável:
Não foi identificada falha de DNS, roteamento, IPv6 ou conectividade externa neste cenário.

Ação tomada:
Foram registrados comandos, resultados e evidências para validar conectividade básica do servidor simulado.

Status:
Resolvido / Validado em laboratório.

Conclusão:
O servidor simulado possui IP válido, rota padrão, DNS funcional, IPv6 configurado, conectividade externa e resposta HTTP/HTTPS. Em um incidente real, caso uma aplicação específica continuasse inacessível, a investigação seguiria para firewall, porta, serviço da aplicação, logs ou camada de aplicação.
```

---

# Evidência complementar

O teste complementar com `curl` e `ss` validou:

```text
- Resposta HTTP/HTTPS externa com status HTTP/2 301.
- Serviços em escuta na VM Linux.
- Porta 22 aberta para SSH.
- Porta 53 utilizada por serviços locais de DNS.
```

---

# Conclusão

Este procedimento organiza uma análise inicial de servidor inacessível em ambiente de Data Center.

A sequência ajuda o técnico a evitar diagnósticos precipitados, separar falhas de DNS, rota, IPv6, firewall, porta e serviço, além de registrar evidências para resolução ou escalonamento.

Em ambientes críticos, a documentação do diagnóstico é tão importante quanto a execução dos comandos.
