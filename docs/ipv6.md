# IPv6 aplicado a Data Center

## Objetivo

Este documento apresenta os conceitos básicos de IPv6 aplicados ao diagnóstico de conectividade em ambientes de Data Center.

O foco é entender como o IPv6 pode impactar servidores, aplicações, DNS, rotas, firewall e comunicação em ambientes modernos de infraestrutura.

## O que é IPv6?

IPv6 é a versão mais recente do protocolo IP.

Ele foi criado para substituir gradualmente o IPv4, principalmente por causa da limitação de endereços disponíveis no IPv4.

Exemplo de IPv4:

```text
192.168.10.20
```

Exemplo de IPv6:

```text
2804:abcd:1234::20
```

## Diferença básica entre IPv4 e IPv6

IPv4 usa endereços de 32 bits.

IPv6 usa endereços de 128 bits.

Na prática, isso permite uma quantidade muito maior de endereços disponíveis e melhora a capacidade de expansão de redes modernas.

## Por que IPv6 é importante em Data Center?

Mesmo que muitas empresas ainda usem IPv4, ambientes modernos de Data Center, cloud e provedores de internet podem utilizar IPv6 para:

- Comunicação entre servidores
- Serviços públicos na internet
- Ambientes híbridos
- Cloud computing
- Escalabilidade de endereçamento
- Novas arquiteturas de rede
- Integração com serviços externos

Um técnico de Data Center não precisa ser especialista em IPv6 no início, mas precisa saber identificar quando ele está envolvido em uma falha de conectividade.

## Tipos comuns de endereço IPv6

### Link-local

Endereços link-local normalmente começam com:

```text
fe80::
```

Eles são usados para comunicação dentro do mesmo segmento de rede.

### Global unicast

São endereços IPv6 roteáveis, usados para comunicação em redes maiores ou na internet.

### Loopback

O endereço de loopback em IPv6 é:

```text
::1
```

Ele representa a própria máquina, semelhante ao `127.0.0.1` no IPv4.

## DNS e IPv6

No DNS, registros IPv4 usam o tipo:

```text
A
```

Registros IPv6 usam o tipo:

```text
AAAA
```

Exemplo:

```text
app.empresa.local -> 2804:abcd:1234::20
```

Se uma aplicação possui registro `AAAA`, os clientes podem tentar acessá-la por IPv6.

## Cenário de falha

### Situação

Uma aplicação em ambiente de Data Center funciona corretamente em IPv4, mas apresenta falha quando acessada por IPv6.

### Possíveis causas

- Endereço IPv6 incorreto
- Ausência de rota IPv6
- Registro DNS AAAA incorreto
- Firewall bloqueando IPv6
- Serviço não escutando em IPv6
- Configuração incompleta no sistema operacional
- Diferença de comportamento entre IPv4 e IPv6

## Comandos úteis no Windows

### Ver endereços IPv6 configurados

```bash
ipconfig
```

### Testar conectividade IPv6

```bash
ping -6 google.com
```

### Verificar registro DNS AAAA

```bash
nslookup -type=AAAA google.com
```

## Comandos úteis no Linux

### Ver endereços IPv6

```bash
ip -6 addr
```

### Ver rotas IPv6

```bash
ip -6 route
```

### Testar conectividade IPv6

```bash
ping -6 google.com
```

### Verificar registro DNS AAAA

```bash
dig AAAA google.com
```

### Testar acesso HTTP usando IPv6

```bash
curl -6 -I https://google.com
```

## Checklist de diagnóstico IPv6

Use este checklist ao investigar uma falha envolvendo IPv6:

```text
[ ] A máquina possui endereço IPv6?
[ ] Existe rota IPv6 configurada?
[ ] O gateway IPv6 responde?
[ ] O DNS possui registro AAAA?
[ ] O endereço IPv6 retornado está correto?
[ ] O firewall permite tráfego IPv6?
[ ] O serviço está escutando em IPv6?
[ ] O problema ocorre apenas em IPv6?
[ ] O IPv4 funciona normalmente?
[ ] Existe diferença de rota entre IPv4 e IPv6?
```

## Raciocínio técnico

Se o IPv4 funciona, mas o IPv6 não funciona, o problema pode estar relacionado à configuração IPv6, rota IPv6, firewall, DNS AAAA ou ao serviço da aplicação.

Se o DNS retorna um registro AAAA incorreto, o cliente pode tentar acessar um endereço IPv6 que não responde.

Se não existe rota IPv6, a máquina pode ter endereço IPv6 configurado, mas não conseguir se comunicar fora da rede local.

## Aplicação em rotina de Data Center

Durante uma análise de conectividade, o técnico precisa saber identificar se o problema ocorre em IPv4, IPv6 ou ambos.

Essa diferenciação ajuda a evitar diagnósticos errados.

Exemplo:

- IPv4 funciona e IPv6 falha: investigar IPv6, rota, firewall ou DNS AAAA
- IPv4 e IPv6 falham: investigar conectividade geral, gateway, firewall, serviço ou rede física
- Nome DNS falha, mas IP funciona: investigar DNS
- Serviço responde ping, mas aplicação não abre: investigar porta, serviço ou aplicação

## Exemplo de registro de evidência

```text
Incidente: Falha de acesso via IPv6

Servidor: app01
Nome DNS: app.empresa.local
IPv4: 192.168.10.20
IPv6 esperado: 2804:abcd:1234::20

Teste realizado: nslookup -type=AAAA app.empresa.local
Resultado: registro AAAA retornou endereço incorreto

Teste realizado: ping -6 app.empresa.local
Resultado: sem resposta

Causa provável: registro AAAA incorreto ou rota IPv6 indisponível
Ação recomendada: validar registro DNS AAAA e rota IPv6 com o time responsável
```

## Conclusão

IPv6 é cada vez mais relevante em ambientes modernos de Data Center e cloud.

Mesmo em ambientes onde IPv4 ainda é predominante, entender comandos básicos, registros DNS AAAA e testes de conectividade IPv6 ajuda o técnico a diagnosticar falhas com mais precisão.
