# Lab IPv6 — Diagnóstico de Endereçamento e DNS IPv6

## Status da etapa

Etapa 5 de 7 — Lab IPv6 com evidências.

## Objetivo

Este laboratório tem como objetivo validar configurações básicas de IPv6 em uma VM Linux acessada via SSH.

O foco é simular uma análise inicial feita por um técnico de Data Center ao verificar se um servidor possui endereçamento IPv6, rota IPv6 e resolução DNS para registros `AAAA`.

## Cenário simulado

Neste laboratório:

- O macOS representa a estação técnica de acesso.
- A VM Linux via UTM representa um servidor em ambiente simulado de Data Center.
- O acesso ao servidor é realizado via SSH.

## Conceito principal

IPv6 é usado em ambientes modernos de rede, cloud e Data Center.

Mesmo que uma aplicação funcione corretamente em IPv4, ela pode apresentar falhas em IPv6 por problemas como:

- ausência de endereço IPv6;
- rota IPv6 inexistente;
- firewall bloqueando tráfego IPv6;
- registro DNS `AAAA` incorreto;
- serviço não escutando em IPv6;
- diferença de configuração entre IPv4 e IPv6.

## Ambiente utilizado

```text
Estação técnica: macOS
Servidor simulado: VM Linux Ubuntu via UTM
Acesso: SSH
Rede utilizada: rede local / NAT da VM
Data do teste: 25/06/2026
```

---

# Teste 1 — Verificar endereços IPv6 da VM Linux

## Comando utilizado

```bash
ip -6 addr
```

## Objetivo

Confirmar se a VM Linux possui endereços IPv6 configurados nas interfaces de rede.

## Resultado observado

```text
A VM Linux possui endereços IPv6 configurados na interface enp0s1.

Foi identificado um endereço IPv6 global iniciado com fd5d:6f8:ee82:e828:c0f8:4aff:fea8:6f6a/64.

Também foi identificado um endereço IPv6 link-local iniciado com fe80::c0f8:4aff:fea8:6f6a/64.

A interface enp0s1 está ativa, com estado UP.
```

## Interpretação técnica

```text
A VM Linux possui IPv6 configurado na interface de rede.

O endereço link-local, iniciado por fe80::, permite comunicação dentro do mesmo segmento de rede.

O endereço iniciado por fd5d: indica um endereço IPv6 do tipo Unique Local Address, usado em redes locais ou ambientes internos.

Em uma rotina de Data Center, essa validação ajuda a confirmar se o servidor possui IPv6 ativo antes de investigar problemas de rota, DNS AAAA ou firewall.
```

---

# Teste 2 — Verificar rotas IPv6 da VM Linux

## Comando utilizado

```bash
ip -6 route
```

## Objetivo

Verificar se a VM Linux possui rotas IPv6 configuradas.

A presença de endereço IPv6 não garante, sozinha, conectividade externa por IPv6. Para isso, também é necessário existir rota adequada.

## Resultado observado

```text
A VM Linux possui rota IPv6 para a rede fd5d:6f8:ee82:e828::/64 pela interface enp0s1.

Também possui rota link-local fe80::/64 pela interface enp0s1.

Foi identificada rota padrão IPv6 via gateway link-local fe80::7c3b:2dff:fe0a:df64 pela interface enp0s1.
```

## Interpretação técnica

```text
A VM Linux possui rotas IPv6 configuradas, incluindo uma rota padrão.

Isso indica que existe caminho definido para tráfego IPv6 a partir da VM.

Em ambientes reais de Data Center, a presença de rota IPv6 é importante para validar se o servidor sabe para onde encaminhar tráfego IPv6 destinado a outras redes.
```

---

# Teste 3 — Consultar registro DNS AAAA

## Comando utilizado

```bash
dig AAAA google.com
```

## Objetivo

Verificar se o DNS consegue retornar registros IPv6 para um domínio.

Registros `AAAA` são usados para associar nomes de domínio a endereços IPv6.

## Resultado observado

```text
O comando dig AAAA google.com retornou status NOERROR.

Foi identificado um registro AAAA para google.com apontando para o endereço IPv6 2800:3f0:4001:83a::200e.

A consulta utilizou o servidor DNS local 127.0.0.53.
```

## Interpretação técnica

```text
O DNS retornou corretamente um registro AAAA para google.com.

Isso indica que a resolução DNS para IPv6 está funcionando neste cenário.

Em uma rotina de Data Center, esse teste ajuda a validar se uma falha de acesso pode estar relacionada a DNS IPv6, registro AAAA incorreto ou ausência de suporte IPv6 no destino.
```

---

# Evidências

### 1. Endereços IPv6 da VM Linux

![Endereços IPv6 da VM Linux](evidências/ipv6/01-ipv6-addr-vm-linux.png)

### 2. Rotas IPv6 da VM Linux

![Rotas IPv6 da VM Linux](evidências/ipv6/02-ipv6-route-vm-linux.png)

### 3. Consulta de registro DNS AAAA

![Consulta DNS AAAA](evidências/ipv6/03-dig-aaaa-google-vm-linux.png)

---

# Checklist de diagnóstico IPv6

```text
[x] A VM possui endereço IPv6?
[x] A VM possui endereço link-local?
[x] A VM possui endereço IPv6 local/global na interface?
[x] Existe rota IPv6 configurada?
[x] Existe rota padrão IPv6?
[x] O DNS retorna registro AAAA?
[x] O resolvedor DNS local respondeu à consulta?
[ ] Foi testada conectividade externa via ping IPv6?
[ ] Há evidência de falha em IPv6?
```

---

# Registro de evidência do laboratório

```text
Data: 25/06/2026
Estação técnica: macOS
Servidor simulado: VM Linux Ubuntu via UTM
Tipo de acesso: SSH
Rede utilizada: rede local / NAT da VM

Teste 1 — Endereços IPv6:
Resultado: a VM possui endereço IPv6 na interface enp0s1, incluindo endereço link-local fe80:: e endereço iniciado por fd5d::.
Interpretação: a interface possui IPv6 ativo e reconhecido pelo sistema operacional.

Teste 2 — Rotas IPv6:
Resultado: foram identificadas rotas IPv6 locais e rota padrão via gateway link-local fe80::7c3b:2dff:fe0a:df64.
Interpretação: a VM possui caminho definido para tráfego IPv6.

Teste 3 — Registro DNS AAAA:
Resultado: o comando dig AAAA google.com retornou o IPv6 2800:3f0:4001:83a::200e.
Interpretação: a resolução DNS para IPv6 funcionou corretamente.

Conclusão geral:
Neste cenário, a VM Linux possui IPv6 configurado, rota IPv6 definida e resolução DNS para registros AAAA funcionando.
```

---

# Conclusão do Lab IPv6

A VM Linux possui endereçamento IPv6 configurado na interface `enp0s1`, incluindo endereço link-local e endereço local/global iniciado por `fd5d`.

Também foi identificada rota padrão IPv6 via gateway link-local, indicando que há caminho definido para tráfego IPv6.

A consulta DNS para registro `AAAA` de `google.com` retornou resposta com sucesso, validando que a resolução de nomes para IPv6 está funcionando neste cenário.

Em uma rotina de Data Center, esse tipo de validação ajuda o técnico a diferenciar falhas de IPv4, IPv6, DNS, rota, firewall ou configuração do serviço.

Mesmo quando IPv4 funciona corretamente, IPv6 precisa ser analisado separadamente para evitar diagnósticos incompletos.
