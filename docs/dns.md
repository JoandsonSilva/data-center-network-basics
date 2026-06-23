# DNS aplicado a Data Center

## Objetivo

Este documento apresenta os conceitos básicos de DNS aplicados ao diagnóstico de conectividade em ambientes de Data Center.

O foco é entender como a resolução de nomes pode impactar o acesso a servidores, aplicações, sistemas internos e ferramentas de monitoramento.

## O que é DNS?

DNS significa Domain Name System.

Ele é responsável por traduzir nomes de domínio em endereços IP.

Exemplo:

```text
app.empresa.local -> 192.168.10.20
```

Em vez de acessar um servidor diretamente pelo IP, usuários e sistemas acessam pelo nome.

## Exemplo prático

Um usuário tenta acessar a aplicação:

```text
app.empresa.local
```

Mas a aplicação não abre.

Antes de concluir que o servidor está fora do ar, o técnico precisa verificar se o nome está sendo resolvido corretamente para o IP do servidor.

## Por que DNS é importante em Data Center?

Em ambientes de Data Center, vários serviços dependem de DNS para funcionar corretamente:

- Aplicações web
- APIs
- Bancos de dados
- Sistemas internos
- Ferramentas de monitoramento
- Backups
- Acesso remoto
- Serviços em cloud
- Integrações entre sistemas

Uma falha de DNS pode causar indisponibilidade aparente, mesmo quando o servidor está ligado e o serviço está em execução.

## Registros DNS básicos

### Registro A

O registro `A` aponta um nome para um endereço IPv4.

Exemplo:

```text
app.empresa.local -> 192.168.10.20
```

### Registro AAAA

O registro `AAAA` aponta um nome para um endereço IPv6.

Exemplo:

```text
app.empresa.local -> 2804:abcd:1234::20
```

### Registro CNAME

O registro `CNAME` cria um alias, ou apelido, para outro nome DNS.

Exemplo:

```text
sistema.empresa.local -> app.empresa.local
```

## Cenário de falha

### Situação

O servidor está ligado e possui IP configurado, mas a aplicação não responde pelo nome:

```text
app.empresa.local
```

### Possíveis causas

- Registro DNS inexistente
- Registro DNS apontando para IP incorreto
- Servidor DNS indisponível
- Configuração DNS incorreta no cliente
- Problema de cache DNS
- Falha de conectividade com o servidor DNS
- Registro IPv6 ausente ou incorreto

## Comandos úteis no Windows

### Verificar configuração de rede

```bash
ipconfig /all
```

Esse comando ajuda a verificar:

- IP da máquina
- Gateway
- Servidor DNS configurado
- Máscara de rede
- Adaptadores ativos

### Testar resolução DNS

```bash
nslookup app.empresa.local
```

### Testar conectividade pelo nome

```bash
ping app.empresa.local
```

### Limpar cache DNS

```bash
ipconfig /flushdns
```

## Comandos úteis no Linux

### Verificar servidores DNS configurados

```bash
cat /etc/resolv.conf
```

### Testar resolução DNS

```bash
nslookup app.empresa.local
```

Ou:

```bash
dig app.empresa.local
```

### Testar registro IPv6

```bash
dig AAAA app.empresa.local
```

### Testar conectividade pelo nome

```bash
ping app.empresa.local
```

### Testar conectividade IPv6

```bash
ping -6 app.empresa.local
```

## Checklist de diagnóstico DNS

Use este checklist ao investigar uma falha de acesso por nome:

```text
[ ] O nome do servidor ou aplicação está correto?
[ ] O nome resolve para algum IP?
[ ] O IP retornado está correto?
[ ] O cliente está usando o DNS correto?
[ ] O servidor DNS responde?
[ ] O acesso funciona pelo IP?
[ ] O acesso falha apenas pelo nome?
[ ] Existe registro A para IPv4?
[ ] Existe registro AAAA para IPv6?
[ ] O cache DNS pode estar desatualizado?
```

## Raciocínio técnico

Se o acesso pelo IP funciona, mas pelo nome não funciona, a causa provável está relacionada a DNS.

Se o acesso pelo nome e pelo IP falham, o problema pode estar relacionado a rede, rota, firewall, serviço parado ou indisponibilidade do servidor.

## Aplicação em rotina de Data Center

Durante um incidente em Data Center, o técnico pode usar testes de DNS para identificar rapidamente se a falha está na resolução de nomes ou em outra camada.

Essa análise ajuda a:

- Reduzir o tempo de diagnóstico
- Evitar troca física desnecessária
- Direcionar o chamado para o time correto
- Registrar evidências técnicas
- Apoiar a continuidade do serviço

## Exemplo de registro de evidência

```text
Incidente: Aplicação inacessível pelo nome

Servidor: app01
Nome DNS: app.empresa.local
IP esperado: 192.168.10.20
Teste realizado: nslookup app.empresa.local
Resultado: nome não resolveu
Causa provável: falha de DNS ou registro inexistente
Ação recomendada: validar registro DNS com o time responsável
```

## Conclusão

DNS é uma camada essencial para acesso a serviços em ambientes de Data Center.

Entender como testar e interpretar falhas de DNS ajuda o técnico a diferenciar problemas de resolução de nomes, conectividade, rota, firewall ou serviço.
