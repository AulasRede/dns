# Relatório — Laboratório Prático: Resolução de Nomes com DNS e NSLookup

**Disciplina:** Redes de Computadores  
**Aluno 1(a):**
**Aluno 2(a):**

---

## Atividade 1 — Conhecendo o Ambiente DNS Local

### E1.1 — Captura: Saída do comando `nslookup`

> Cole aqui a saída do terminal ou insira a captura de tela:

```
(cole a saída aqui)
```

### E1.2 — Resposta: Nome e IP do servidor DNS padrão

| Campo | Valor |
|-------|-------|
| Nome do servidor DNS | |
| Endereço IP | |

### E1.3 — Resposta: Servidor público ou privado?

> Resposta:

---

## Atividade 2 — Consulta Direta Simples (Registro tipo A)

### E2.1 — Captura: Saídas do `nslookup`

**`nslookup www.google.com`:**

```
(cole a saída aqui)
```

**`nslookup www.usp.br`:**

```
(cole a saída aqui)
```

**`nslookup www.gov.br`:**

```
(cole a saída aqui)
```

### E2.2 — Resposta: Endereços IP retornados

| Domínio | Endereço(s) IP |
|---------|----------------|
| www.google.com | |
| www.usp.br | |
| www.gov.br | |

### E2.3 — Resposta: Non-authoritative answer

> Resposta:

---

## Atividade 3 — Tipos de Registros DNS

### E3.1 — Capturas dos 6 comandos

**3.1 — Registro MX (`nslookup -type=MX gmail.com`):**

```
(cole a saída aqui)
```

**3.2 — Registro AAAA (`nslookup -type=AAAA www.google.com`):**

```
(cole a saída aqui)
```

**3.3 — Registro CNAME (`nslookup -type=CNAME www.microsoft.com`):**

```
(cole a saída aqui)
```

**3.4 — Registro NS (`nslookup -type=NS google.com`):**

```
(cole a saída aqui)
```

**3.5 — Registro SOA (`nslookup -type=SOA google.com`):**

```
(cole a saída aqui)
```

**3.6 — Registro TXT (`nslookup -type=TXT google.com`):**

```
(cole a saída aqui)
```

### E3.2 — Resposta: Servidores MX do Gmail

| Servidor MX | Prioridade |
|-------------|------------|
| | |
| | |

### E3.3 — Resposta: Servidor primário (SOA) do `google.com`

> Resposta:

---

## Atividade 4 — TTL e Cache DNS

### E4.1 — Capturas das duas execuções com debug

**Primeira execução (`nslookup -debug www.google.com`):**

```
(cole a saída aqui)
```

**Segunda execução (~10s depois):**

```
(cole a saída aqui)
```

### E4.2 — Resposta: Valores de TTL

| Execução | Valor de TTL (segundos) |
|----------|-------------------------|
| Primeira consulta | |
| Segunda consulta | |

### E4.3 — Resposta: O TTL diminuiu? Explique

> Resposta:

### E4.4 — Resposta: Expiração do TTL

> Se o TTL de um registro é 300 segundos, o que acontece quando esse tempo expira no cache do resolver?  
> Resposta:

---

## Atividade 5 — Consulta Reversa (Registro PTR)

### E5.1 — Capturas das consultas reversas

**Consulta reversa para IP do Google:**

```
(cole a saída aqui)
```

**Consulta reversa para `1.1.1.1`:**

```
(cole a saída aqui)
```

**Consulta reversa para `8.8.8.8`:**

```
(cole a saída aqui)
```

### E5.2 — Resposta: Nomes retornados

| IP consultado | Nome retornado |
|---------------|----------------|
| (IP do Google) | |
| 1.1.1.1 | |
| 8.8.8.8 | |

### E5.3 — Resposta: Consulta reversa vs. nome original

> Resposta:

### E5.4 — Resposta: O que é `in-addr.arpa`?

> Resposta:

---

## Atividade 6 — Explorando os Servidores Raiz

### E6.1 — Capturas

**`nslookup -type=NS .`:**

```
(cole a saída aqui)
```

**`nslookup a.root-servers.net`:**

```
(cole a saída aqui)
```

### E6.2 — Resposta: Lista dos servidores raiz

> Liste todos os servidores raiz retornados:

### E6.3 — Resposta: IP do `a.root-servers.net`

> Resposta:

---

## Atividade 7 — Simulando a Resolução Iterativa

### E7.1 — Captura: Passo 1 — Consulta ao servidor raiz

```
(cole a saída aqui)
```

### E7.2 — Captura: Passo 2 — Consulta ao servidor TLD (.br)

```
(cole a saída aqui)
```

### E7.3 — Captura: Passo 3 — Consulta ao servidor autoritativo

```
(cole a saída aqui)
```

### E7.4 — Resposta: Em qual passo a resposta tornou-se autoritativa?

> Resposta:

---

## Atividade 8 — Consulta Recursiva vs. Iterativa na Prática

### E8.1 — Capturas

**8.1 — Consulta recursiva (`nslookup www.usp.br`):**

```
(cole a saída aqui)
```

**8.2 — Consulta iterativa (`nslookup www.usp.br a.root-servers.net`):**

```
(cole a saída aqui)
```

### E8.2 — Resposta: Diferença de conteúdo

> Resposta:

### E8.3 — Resposta: Por que as respostas são diferentes?

> Resposta:

### E8.4 — Resposta: Modelo mais comum

> Resposta:

---

## Atividade 9 — Identificando o Servidor Autoritativo

### E9.1 — Capturas para `usp.br`

**`nslookup -type=NS usp.br`:**

```
(cole a saída aqui)
```

**Consulta direta ao servidor autoritativo:**

```
(cole a saída aqui)
```

### E9.2 — Resposta: Servidores NS de `usp.br`

> Resposta:

### E9.3 — Resposta: Diferença visual entre resposta autoritativa e não autoritativa

> Resposta:

### E9.4 — Captura: Segundo domínio escolhido

**Domínio escolhido:** ______________________

**`nslookup -type=NS <domínio>`:**

```
(cole a saída aqui)
```

**Consulta direta ao servidor autoritativo:**

```
(cole a saída aqui)
```

### E9.5 — Resposta: Servidores NS e IP do segundo domínio

> Resposta:

---

## Atividade 10 — Trocando o Servidor DNS Resolver

### E10.1 — Capturas dos 3 resolvers

**`nslookup www.google.com 8.8.8.8`:**

```
(cole a saída aqui)
```

**`nslookup www.google.com 1.1.1.1`:**

```
(cole a saída aqui)
```

**`nslookup www.google.com 9.9.9.9`:**

```
(cole a saída aqui)
```

### E10.2 — Tabela comparativa

| Resolver | Provedor | IP(s) retornados para `www.google.com` |
|----------|----------|----------------------------------------|
| 8.8.8.8 | Google Public DNS | |
| 1.1.1.1 | Cloudflare DNS | |
| 9.9.9.9 | Quad9 | |

### E10.3 — Resposta: Resultados idênticos?

> Resposta:

---

## Atividade 11 — Investigando a Delegação de Zonas

### E11.1 — Capturas

**`nslookup -type=NS br`:**

```
(cole a saída aqui)
```

**`nslookup -type=NS usp.br`:**

```
(cole a saída aqui)
```

**`nslookup -type=NS ime.usp.br`:**

```
(cole a saída aqui)
```

### E11.2 — Resposta: Servidores NS em cada nível

| Nível | Servidores NS |
|-------|---------------|
| br | |
| usp.br | |
| ime.usp.br | |

### E11.3 — Resposta: Delegação de `usp.br` para `ime.usp.br`

> Resposta:

### E11.4 — Resposta: Impacto de falha no servidor autoritativo

> Resposta:

---

## Atividade 12 — DNS e Segurança: Consulta TXT para SPF/DMARC

### E12.1 — Capturas

**`nslookup -type=TXT google.com`:**

```
(cole a saída aqui)
```

**`nslookup -type=TXT _dmarc.google.com`:**

```
(cole a saída aqui)
```

**`nslookup -type=TXT _dmarc.usp.br`:**

```
(cole a saída aqui)
```

### E12.2 — Resposta: Registro SPF de `google.com`

> Resposta:

### E12.3 — Resposta: Registro DMARC de `google.com`

> Resposta:

### E12.4 — Resposta: DMARC de `usp.br`

> Resposta:

---

## Tabela de Resumo dos Tipos de Registro DNS

| Tipo | Nome Completo | Finalidade | Exemplo observado na prática |
|------|---------------|------------|------------------------------|
| A | Address | Mapeia nome → IPv4 | |
| AAAA | Address (IPv6) | Mapeia nome → IPv6 | |
| CNAME | Canonical Name | Alias / apelido para outro nome | |
| MX | Mail Exchange | Servidor de e-mail do domínio | |
| NS | Name Server | Servidor autoritativo da zona | |
| SOA | Start of Authority | Informações administrativas da zona | |
| PTR | Pointer | Mapeia IP → nome (resolução reversa) | |
| TXT | Text | Metadados (SPF, DKIM, DMARC, etc.) | |

---

## Checklist de Entrega

Antes de enviar, verifique se o seu relatório contém todos os itens:

- [ ] **15 capturas** (E1.1, E2.1, E3.1, E4.1, E5.1, E6.1, E7.1, E7.2, E7.3, E8.1, E9.1, E9.4, E10.1, E11.1, E12.1)
- [ ] **25 respostas** (E1.2, E1.3, E2.2, E2.3, E3.2, E3.3, E4.2, E4.3, E4.4, E5.2, E5.3, E5.4, E6.2, E6.3, E7.4, E8.2, E8.3, E8.4, E9.2, E9.3, E9.5, E10.3, E11.2, E11.3, E11.4, E12.2, E12.3, E12.4)
- [ ] **2 tabelas** (E10.2, Tabela de Resumo dos Tipos de Registro)
- [ ] Todas as evidências identificadas com os códigos corretos
- [ ] Nome, RA e data preenchidos no cabeçalho
