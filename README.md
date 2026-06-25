# Fundamentos de Rede para Data Center: DNS, Roteamento e IPv6

## Status do projeto

Projeto concluído — 7 etapas finalizadas.

## Objetivo

Este projeto apresenta uma documentação técnica e prática sobre DNS, roteamento, IPv6 e troubleshooting aplicados ao diagnóstico de conectividade em um ambiente simulado de Data Center.

O foco é simular a análise inicial de um técnico de Data Center ao investigar falhas de acesso a servidores, aplicações e serviços críticos.

---

## Cenário simulado

Neste projeto:

- O macOS representa a estação técnica de acesso.
- A VM Linux Ubuntu via UTM representa um servidor em ambiente simulado de Data Center.
- O acesso ao servidor é realizado via SSH.
- Os testes foram executados diretamente na VM Linux.

Cenário principal:

```text
Um servidor de aplicação está ligado, mas os usuários relatam falha de acesso pelo nome da aplicação.
```

A investigação considera possíveis falhas em:

- DNS
- IP
- Gateway
- Roteamento
- IPv6
- Firewall
- Porta
- Serviço da aplicação
- Conectividade física ou lógica

---

## Estrutura do projeto

```text
data-center-network-basics/
│
├── README.md
│
├── docs/
│   ├── dns.md
│   ├── roteamento.md
│   ├── ipv6.md
│   └── troubleshooting-servidor-inacessivel.md
│
└── labs/
    ├── lab-dns.md
    ├── lab-roteamento.md
    ├── lab-ipv6.md
    │
    └── evidências/
        ├── dns/
        ├── roteamento/
        └── ipv6/
```

---

## Conteúdo desenvolvido

### 1. DNS aplicado a Data Center

Arquivo:

```text
docs/dns.md
```

Conteúdo:

- Conceito de DNS
- Registros `A`, `AAAA` e `CNAME`
- Importância do DNS em Data Center
- Diagnóstico de falhas de resolução de nomes
- Aplicação em incidentes de servidor inacessível

---

### 2. Roteamento aplicado a Data Center

Arquivo:

```text
docs/roteamento.md
```

Conteúdo:

- Conceito de roteamento
- Gateway padrão
- Rotas entre redes
- Diagnóstico de conectividade externa
- Separação entre falha de rota e falha de DNS

---

### 3. IPv6 aplicado a Data Center

Arquivo:

```text
docs/ipv6.md
```

Conteúdo:

- Conceito de IPv6
- Diferença entre IPv4 e IPv6
- Endereços link-local e local/global
- Registros DNS `AAAA`
- Diagnóstico básico de IPv6

---

### 4. Lab DNS

Arquivo:

```text
labs/lab-dns.md
```

Testes realizados:

```bash
cat /etc/resolv.conf
nslookup google.com
ping google.com
dig AAAA google.com
```

Resultado:

```text
A VM Linux conseguiu resolver nomes de domínio e testar conectividade pelo nome.
Não foi identificada falha de DNS neste cenário.
```

---

### 5. Lab Roteamento

Arquivo:

```text
labs/lab-roteamento.md
```

Testes realizados:

```bash
ip addr
ip route
ping 8.8.8.8
```

Resultado:

```text
A VM Linux possui IP válido, rota padrão configurada e conectividade externa por IP.
Não foi identificada falha básica de roteamento neste cenário.
```

---

### 6. Lab IPv6

Arquivo:

```text
labs/lab-ipv6.md
```

Testes realizados:

```bash
ip -6 addr
ip -6 route
dig AAAA google.com
```

Resultado:

```text
A VM Linux possui IPv6 configurado, rota IPv6 definida e resolução DNS para registros AAAA funcionando.
```

---

### 7. Troubleshooting de servidor inacessível

Arquivo:

```text
docs/troubleshooting-servidor-inacessivel.md
```

Testes e validações complementares:

```bash
curl -I https://google.com
ss -tulnp
```

Conteúdo:

- Fluxo de diagnóstico inicial
- Checklist de troubleshooting
- Matriz de decisão rápida
- Modelo de registro de incidente
- Exemplo aplicado ao laboratório
- Validação de resposta HTTP/HTTPS
- Verificação de portas e serviços em escuta

---

## Principais comandos utilizados

### DNS

```bash
cat /etc/resolv.conf
resolvectl status
nslookup google.com
dig google.com
dig AAAA google.com
```

### Roteamento

```bash
ip addr
ip route
ping 8.8.8.8
```

### IPv6

```bash
ip -6 addr
ip -6 route
dig AAAA google.com
```

### Serviço e portas

```bash
curl -I https://google.com
ss -tulnp
```

---

## Habilidades demonstradas

Este projeto demonstra conhecimentos iniciais em:

- Diagnóstico de DNS
- Verificação de IP e gateway
- Análise de rota padrão
- Teste de conectividade externa
- Validação de registros DNS `AAAA`
- Identificação de endereços IPv6
- Verificação de portas e serviços
- Teste de resposta HTTP/HTTPS
- Interpretação de resultados em Linux
- Acesso remoto via SSH
- Documentação técnica
- Organização de evidências
- Raciocínio operacional aplicado a Data Center

---

## Aplicação em Data Center

Em um ambiente de Data Center, falhas de conectividade podem impactar servidores, aplicações, monitoramento, backups, APIs, bancos de dados e usuários.

Este projeto simula uma abordagem inicial de diagnóstico para diferenciar falhas de:

```text
DNS
Roteamento
Gateway
IPv6
Firewall
Porta
Serviço
Conectividade geral
```

A proposta é demonstrar não apenas a execução de comandos, mas a capacidade de interpretar resultados, registrar evidências técnicas e seguir uma linha lógica de troubleshooting.

---

## Conclusão

Este projeto consolidou fundamentos de DNS, roteamento, IPv6 e troubleshooting aplicados a um cenário simulado de Data Center.

A VM Linux utilizada no laboratório apresentou:

- DNS funcional
- IP válido
- Gateway configurado
- Rota padrão ativa
- Conectividade externa por IP
- IPv6 configurado
- Resolução DNS para registros `AAAA`
- Resposta HTTP/HTTPS externa
- Serviços em escuta, incluindo SSH

Em um incidente real, após essas validações, a investigação poderia seguir para firewall, portas, serviço da aplicação, logs ou camada de aplicação.

---

## Autor

Joandson Oliveira  
Transição profissional para Infraestrutura, Cloud Computing e Data Center.

LinkedIn: linkedin.com/in/joandson-silva
