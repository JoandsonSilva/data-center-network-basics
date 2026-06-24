# Lab DNS — Diagnóstico de Resolução de Nomes

## Objetivo

Este laboratório tem como objetivo praticar comandos de diagnóstico DNS para identificar se um problema de acesso está relacionado à resolução de nomes.

O foco é simular uma análise inicial feita por um técnico de Data Center ao investigar uma aplicação ou servidor inacessível pelo nome.

Neste laboratório, o macOS representa a estação técnica de acesso, enquanto a VM Linux representa um servidor em ambiente simulado de Data Center. A conexão via SSH permite executar comandos de diagnóstico diretamente no servidor, validando IP, rota, DNS e conectividade.

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

Preencher conforme o ambiente usado:

```text
Sistema operacional:
Rede utilizada:
Tipo de conexão:
Data do teste:
```

Exemplo:

```text
Sistema operacional: Windows 11
Rede utilizada: rede doméstica
Tipo de conexão: Wi-Fi
Data do teste: 24/06/2026
```

---

# Teste 1 — Verificar configuração de rede

## Windows

Comando:

```bash
ipconfig /all
```

## Linux

Comando:

```bash
ip addr
```

E também:

```bash
cat /etc/resolv.conf
```

## Objetivo do teste

Verificar as configurações de rede da máquina, incluindo:

- Endereço IP
- Gateway padrão
- Servidor DNS configurado
- Adaptador de rede ativo

## Resultado observado

Preencher com o resultado do seu teste:

```text
Resultado:
```

## Interpretação técnica

Preencher após executar o comando:

```text
Interpretação:
```

Exemplo:

```text
A máquina possui IP válido, gateway configurado e servidor DNS definido. Isso indica que a configuração básica de rede está presente.
```

---

# Teste 2 — Testar resolução DNS com nslookup

## Windows ou Linux

Comando:

```bash
nslookup google.com
```

## Objetivo do teste

Verificar se o DNS consegue resolver um nome de domínio para um endereço IP.

## Resultado esperado

O comando deve retornar um ou mais endereços IP associados ao domínio consultado.

## Resultado observado

Preencher com o resultado do seu teste:

```text
Resultado:
```

## Interpretação técnica

Exemplo:

```text
O domínio google.com foi resolvido corretamente para endereços IP. Isso indica que a resolução DNS está funcionando neste teste.
```

## Aplicação em Data Center

Em um Data Center, esse teste pode ser usado para validar se uma aplicação interna, servidor ou serviço está sendo resolvido corretamente pelo DNS.

Se o nome não resolver, o problema pode estar no registro DNS, no servidor DNS ou na configuração DNS do cliente.

---

# Teste 3 — Testar conectividade pelo nome

## Windows ou Linux

Comando:

```bash
ping google.com
```

## Objetivo do teste

Verificar se o nome é resolvido e se existe conectividade até o destino.

## Resultado esperado

O comando deve resolver o nome para um IP e tentar enviar pacotes ao destino.

## Resultado observado

Preencher com o resultado do seu teste:

```text
Resultado:
```

## Interpretação técnica

Exemplo:

```text
O nome foi resolvido e houve resposta ao ping. Isso indica que DNS e conectividade básica estão funcionando para este destino.
```

## Atenção

Alguns servidores podem bloquear resposta ao ping. Nesse caso, a ausência de resposta não significa necessariamente que o serviço está fora do ar.

Por isso, é importante combinar esse teste com outros comandos, como `nslookup`, `tracert`, `traceroute` ou `curl`.

---

# Teste 4 — Testar resolução de registro IPv6

## Windows

Comando:

```bash
nslookup -type=AAAA google.com
```

## Linux

Comando:


```bash
dig AAAA google.com
```

## macOS

### Ver IP da interface principal

```bash
ipconfig getifaddr en0
```

## Objetivo do teste

Verificar se o domínio possui registro DNS do tipo `AAAA`, usado para endereços IPv6.

## Resultado esperado

O comando pode retornar um ou mais endereços IPv6.

## Resultado observado

Preencher com o resultado do seu teste:

```text
Resultado:
```

## Interpretação técnica

Exemplo:

```text
O domínio possui registro AAAA, indicando que pode ser acessado por IPv6 caso a rede tenha suporte.
```

## Aplicação em Data Center

Em ambientes modernos de Data Center e cloud, uma falha em IPv6 pode ocorrer mesmo quando IPv4 está funcionando.

Por isso, validar registros `A` e `AAAA` ajuda a separar problemas de IPv4, IPv6 e DNS.

---

# Teste 5 — Comparar acesso por nome e por IP

## Passo 1 — Resolver o nome

Comando:

```bash
nslookup google.com
```

Anote um dos IPs retornados.

## Passo 2 — Testar acesso pelo IP

Comando:

```bash
ping IP_RETORNADO
```

Exemplo:

```bash
ping 142.250.218.78
```

## Objetivo do teste

Comparar se o acesso funciona pelo nome e também pelo IP.

## Resultado observado

Preencher com o resultado do seu teste:

```text
Resultado:
```

## Interpretação técnica

Use este raciocínio:

```text
Se funciona pelo IP, mas não pelo nome, o problema pode estar no DNS.

Se não funciona pelo IP nem pelo nome, o problema pode estar em conectividade, rota, firewall ou indisponibilidade do destino.

Se funciona pelo nome e pelo IP, DNS e conectividade básica estão funcionando para este teste.
```

---

# Checklist de análise DNS

```text
[ ] A máquina possui IP válido?
[ ] Existe servidor DNS configurado?
[ ] O domínio resolve para um IP?
[ ] O IP retornado parece válido?
[ ] O domínio possui registro A?
[ ] O domínio possui registro AAAA?
[ ] O acesso funciona pelo nome?
[ ] O acesso funciona pelo IP?
[ ] A falha ocorre apenas em um domínio?
[ ] A falha ocorre em vários domínios?
```

---

# Registro de evidência do laboratório

Preencha após realizar os testes:

```text
Data:
Sistema operacional:
Rede utilizada:

Teste 1 — Configuração de rede:
Resultado:
Interpretação:

Teste 2 — nslookup:
Resultado:
Interpretação:

Teste 3 — ping por nome:
Resultado:
Interpretação:

Teste 4 — registro AAAA:
Resultado:
Interpretação:

Teste 5 — comparação nome x IP:
Resultado:
Interpretação:

Conclusão geral:
```

---

# Conclusão do laboratório

Este laboratório demonstrou como utilizar comandos básicos para diagnosticar problemas de DNS.

Em uma rotina de Data Center, esses testes ajudam o técnico a identificar se uma falha de acesso está relacionada à resolução de nomes, conectividade, rota, IPv6, firewall ou serviço.

O objetivo não é apenas executar comandos, mas interpretar os resultados e registrar evidências para apoiar a resolução ou escalonamento do incidente.
