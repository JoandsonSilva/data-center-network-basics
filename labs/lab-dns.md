# Lab DNS — Diagnóstico de Resolução de Nomes

## Status da etapa

Etapa 3 de 7 — Lab DNS com evidências.

## Objetivo

Este laboratório tem como objetivo praticar comandos de diagnóstico DNS para identificar se um problema de acesso está relacionado à resolução de nomes.

O foco é simular uma análise inicial feita por um técnico de Data Center ao investigar uma aplicação ou servidor inacessível pelo nome.

Neste laboratório, o macOS representa a estação técnica de acesso, enquanto a VM Linux representa um servidor em ambiente simulado de Data Center. A conexão via SSH permite executar comandos de diagnóstico diretamente no servidor, validando DNS e conectividade.

## Cenário simulado

Um servidor de aplicação em um ambiente de Data Center está ligado, mas os usuários relatam que não conseguem acessar a aplicação pelo nome:

```text
app.empresa.local
```

Antes de concluir que o servidor está fora do ar, o técnico precisa verificar se o problema está relacionado a DNS.

## Conceito principal

Se uma aplicação funciona pelo IP, mas não funciona pelo nome, a causa provável pode estar relacionada à resolução DNS.

Exemplo:

```text
Acesso pelo IP: funciona
Acesso pelo nome: falha
Possível causa: DNS
```

## Ambiente utilizado

```text
Sistema operacional da estação técnica: macOS
Servidor simulado: VM Linux Ubuntu via UTM
Acesso ao servidor: SSH
Rede utilizada: rede local
Data do teste: 24/06/2026
```

---

# Teste 1 — Verificar configuração de rede e DNS

## Comandos utilizados

No macOS:

```bash
ipconfig getifaddr en0
route -n get default
```

Na VM Linux:

```bash
cat /etc/resolv.conf
```

## Objetivo do teste

Verificar se a estação técnica possui IP e gateway configurados, além de identificar como a VM Linux trata a resolução DNS.

## Resultado observado

```text
A estação técnica macOS possui IP 192.168.18.3 e gateway padrão 192.168.18.1.

Na VM Linux, a resolução DNS é gerenciada pelo systemd-resolved, utilizando o endereço local 127.0.0.53 como resolvedor DNS.
```

## Interpretação técnica

```text
A estação técnica possui IP válido e gateway configurado.

A VM Linux utiliza o systemd-resolved, recurso comum em distribuições Linux modernas. O endereço 127.0.0.53 atua como resolvedor DNS local, encaminhando as consultas para os servidores DNS configurados na rede.

Isso indica que existe configuração básica de rede e DNS no ambiente analisado.
```

---

# Teste 2 — Testar resolução DNS com nslookup

## Comando utilizado

Na VM Linux:

```bash
nslookup google.com
```

## Objetivo do teste

Verificar se a VM Linux consegue resolver um nome de domínio para endereço IP.

## Resultado observado

```text
O comando nslookup google.com utilizou o servidor DNS 127.0.0.53 e resolveu o domínio google.com para o endereço IPv4 172.217.172.142.

Também foi retornado um endereço IPv6 para o domínio consultado.
```

## Interpretação técnica

```text
A resolução DNS funcionou corretamente.

O domínio google.com foi resolvido para endereço IPv4 e também apresentou endereço IPv6, indicando que a VM consegue consultar o DNS configurado.

Neste cenário, não há evidência de falha na resolução de nomes.
```

## Aplicação em Data Center

Em um Data Center, esse teste pode ser usado para validar se uma aplicação interna, servidor ou serviço está sendo resolvido corretamente pelo DNS.

Se o nome não resolver, o problema pode estar no registro DNS, no servidor DNS ou na configuração DNS do cliente.

---

# Teste 3 — Testar conectividade pelo nome

## Comando utilizado

Na VM Linux:

```bash
ping google.com
```

## Objetivo do teste

Verificar se o nome é resolvido e se existe conectividade até o destino.

## Resultado observado

```text
O comando ping google.com resolveu o domínio para o IP 172.217.172.142.

Foram transmitidos 10 pacotes, todos recebidos com sucesso, resultando em 0% de perda de pacotes.
```

## Interpretação técnica

```text
O teste confirmou que a VM Linux consegue resolver o nome google.com e também possui conectividade até o destino.

Como não houve perda de pacotes, não foi identificada falha de DNS ou de conectividade básica neste teste.
```

## Atenção

Alguns servidores podem bloquear resposta ao ping. Nesse caso, a ausência de resposta não significa necessariamente que o serviço está fora do ar.

Por isso, é importante combinar esse teste com outros comandos, como `nslookup`, `dig`, `traceroute` ou `curl`.

---

# Teste 4 — Testar resolução de registro IPv6

## Comando utilizado

Na VM Linux:

```bash
dig AAAA google.com
```

## Objetivo do teste

Verificar se o domínio possui registro DNS do tipo `AAAA`, usado para endereços IPv6.

## Resultado observado

```text
O comando dig AAAA google.com foi utilizado para consultar registros IPv6 do domínio google.com.

O teste tem como objetivo validar se o domínio possui resolução DNS para IPv6.
```

## Interpretação técnica

```text
A consulta de registro AAAA ajuda a identificar se um domínio possui endereço IPv6 associado.

Em ambientes de Data Center e cloud, esse teste é importante porque uma aplicação pode funcionar em IPv4 e apresentar falha em IPv6, ou o contrário.
```

## Aplicação em Data Center

Em ambientes modernos de Data Center e cloud, uma falha em IPv6 pode ocorrer mesmo quando IPv4 está funcionando.

Por isso, validar registros `A` e `AAAA` ajuda a separar problemas de IPv4, IPv6 e DNS.

---

# Teste 5 — Comparar acesso por nome e por IP

## Comandos utilizados

Resolver o nome:

```bash
nslookup google.com
```

Testar conectividade pelo nome:

```bash
ping google.com
```

## Objetivo do teste

Comparar se o acesso funciona pelo nome e se o domínio é resolvido corretamente para um endereço IP.

## Resultado observado

```text
O domínio google.com foi resolvido para o IP 172.217.172.142.

O teste de ping pelo nome obteve resposta com 0% de perda de pacotes.
```

## Interpretação técnica

```text
Como o acesso pelo nome funcionou e o domínio foi resolvido para um IP válido, não há evidência de falha de DNS neste cenário.

Caso o acesso pelo IP funcionasse e pelo nome falhasse, a principal hipótese seria problema de DNS.

Caso o acesso falhasse tanto pelo IP quanto pelo nome, a investigação deveria seguir para rota, gateway, firewall, conectividade ou indisponibilidade do serviço.
```

---

# Checklist de análise DNS

```text
[x] A estação técnica possui IP válido?
[x] A estação técnica possui gateway configurado?
[x] A VM Linux possui resolvedor DNS configurado?
[x] O domínio resolve para um IP?
[x] O IP retornado parece válido?
[x] O domínio possui resposta para IPv4?
[x] O domínio apresenta suporte a IPv6?
[x] O acesso funciona pelo nome?
[x] A VM possui conectividade básica com o destino?
[ ] Há evidência de falha de DNS?
```

---

# Evidências

### 1. Configuração de rede da estação técnica macOS

![Configuração de rede macOS](../evidências/dns/01-dns-config-vm-linux.png)

### 2. Resolução DNS com nslookup

![nslookup google.com](../evidências/dns/02-nslookup-google-vm-linux.png)

### 3. Teste de conectividade pelo nome

![ping google.com](../evidências/dns/03-ping-google-vm-linux.png)

### 4. Consulta de registro IPv6 AAAA

![dig AAAA google.com](../evidências/dns/04-dig-aaaa-google-vm-linux.png)

---

# Registro de evidência do laboratório

```text
Data: 24/06/2026
Estação técnica: macOS
Servidor simulado: VM Linux Ubuntu via UTM
Tipo de acesso: SSH
Rede utilizada: rede local

Teste 1 — Configuração de rede e DNS:
Resultado: estação técnica com IP 192.168.18.3 e gateway 192.168.18.1. VM Linux utilizando systemd-resolved com 127.0.0.53.
Interpretação: configuração básica de rede e DNS presente.

Teste 2 — nslookup:
Resultado: google.com resolvido para IPv4 172.217.172.142 e também com retorno IPv6.
Interpretação: resolução DNS funcionando corretamente.

Teste 3 — ping por nome:
Resultado: 10 pacotes transmitidos, 10 recebidos, 0% de perda.
Interpretação: DNS e conectividade básica funcionando.

Teste 4 — registro AAAA:
Resultado: consulta usada para validar registro IPv6 do domínio.
Interpretação: teste importante para diferenciar falhas de IPv4, IPv6 e DNS.

Conclusão geral:
Neste cenário, não foi identificada falha de DNS. A VM Linux conseguiu resolver nomes e testar conectividade pelo nome.
```

---

# Conclusão do Lab DNS

A estação técnica macOS possui IP válido e gateway padrão configurado.

A VM Linux utiliza `systemd-resolved`, identificado pelo endereço local `127.0.0.53`, que atua como resolvedor DNS local.

Nos testes realizados, o domínio `google.com` foi resolvido corretamente para endereço IPv4 e também apresentou endereço IPv6. O teste de conectividade pelo nome obteve resposta com 0% de perda de pacotes.

Neste cenário, não foi identificada falha de DNS.

Em uma rotina de Data Center, esse tipo de validação ajuda o técnico a diferenciar problemas de resolução de nomes de falhas de rota, firewall, conectividade ou serviço.
