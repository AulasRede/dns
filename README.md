# 🔍 Laboratório Prático: Resolução de Nomes com DNS e NSLookup

**Disciplina:** Redes de Computadores  
**Duração estimada:** 90–120 minutos  
**Pré-requisitos:** Acesso a um terminal (Windows, Linux ou macOS) com conexão à Internet

---

## 📋 Objetivos de Aprendizagem

Ao final desta atividade, o aluno será capaz de:

1. Compreender o funcionamento hierárquico do Sistema de Nomes de Domínio (DNS).
2. Diferenciar consultas **recursivas** e **iterativas** na prática.
3. Realizar consultas diretas e **reversas** utilizando o `nslookup`.
4. Identificar e consultar **servidores raiz**, **TLD** e **autoritativos**.
5. Interpretar o significado do **TTL** (Time to Live) e seu impacto no cache.
6. Reconhecer os principais **tipos de registros DNS** (A, AAAA, MX, CNAME, NS, SOA, PTR, TXT).
7. Analisar a **delegação de zonas** e a cadeia de autoridade do DNS.

---

## 🧰 Ferramentas Utilizadas

| Ferramenta | Finalidade |
|---|---|
| `nslookup` | Consultas DNS interativas e não interativas |
| `Terminal / Prompt de Comando` | Execução dos comandos |
| Ferramenta de captura de tela | Registro de evidências |

> **Nota:** O `nslookup` está disponível nativamente no Windows, Linux e macOS. Todos os comandos deste roteiro são compatíveis com os três sistemas operacionais. Pequenas variações na saída são normais.

---

## 📖 Conceitos Fundamentais

Antes de iniciar as atividades práticas, revise os conceitos abaixo. Eles serão explorados em cada exercício.

### Hierarquia DNS

O DNS é organizado como uma árvore invertida:

```
                    . (raiz)
                    |
        +-----------+-----------+
        |           |           |
       com         org          br
        |                       |
    +---+---+               +---+---+
    |       |               |       |
  google  amazon          com      gov
                            |
                          usp
```

### Consulta Recursiva vs. Iterativa

- **Recursiva:** O cliente pede ao servidor DNS resolver que faça *todo o trabalho* — percorrendo a hierarquia até obter a resposta final. É o modelo típico entre um computador e o resolver configurado (ex.: `8.8.8.8`).
- **Iterativa:** O servidor consultado responde com uma *referência* (referral) ao próximo servidor da cadeia, devolvendo ao cliente a responsabilidade de continuar a busca. É o modelo típico entre resolvers e servidores autoritativos.

### TTL (Time to Live)

Valor em segundos que indica por quanto tempo uma resposta DNS pode ser armazenada em cache antes de precisar ser consultada novamente. Um TTL baixo favorece atualizações rápidas; um TTL alto reduz o tráfego DNS.

---

## 🧪 Atividades Práticas

> **⚠️ IMPORTANTE:** Cada atividade contém itens de evidência identificados por códigos (ex.: **E1.1**, **E2.1**). O aluno deve entregar **todas** as evidências listadas, utilizando os códigos correspondentes para identificação no relatório.
>
> **Tipos de evidência:**
> - 📸 **Captura** = Captura de tela ou cópia textual da saída do terminal
> - 💬 **Resposta** = Resposta escrita (texto dissertativo ou objetivo)
> - 📋 **Tabela** = Preenchimento de tabela

---

### Atividade 1 — Conhecendo o Ambiente DNS Local

**Objetivo:** Identificar o servidor DNS configurado na sua máquina.

Execute o comando abaixo para entrar no modo interativo do `nslookup` e verificar o servidor DNS padrão:

```bash
nslookup
```

A primeira linha da saída mostra o **Default Server** (servidor DNS resolver) e o endereço IP dele.

Agora saia do modo interativo:

```
> exit
```

**� Evidências da Atividade 1:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E1.1** | 📸 Captura | Saída completa do comando `nslookup` mostrando o servidor DNS padrão |
| **E1.2** | 💬 Resposta | Nome e IP do servidor DNS padrão da sua máquina |
| **E1.3** | 💬 Resposta | Este servidor é público (ex.: Google `8.8.8.8`, Cloudflare `1.1.1.1`) ou privado/institucional? Justifique |

---

### Atividade 2 — Consulta Direta Simples (Registro tipo A)

**Objetivo:** Resolver um nome de domínio para seu endereço IPv4.

```bash
nslookup www.google.com
```

Observe na saída:
- **Non-authoritative answer**: indica que a resposta veio do *cache* do resolver, e não diretamente do servidor autoritativo do domínio.
- **Name** e **Address**: o nome canônico e o(s) IP(s) associados.

Repita para outros domínios:

```bash
nslookup www.usp.br
nslookup www.gov.br
```

**� Evidências da Atividade 2:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E2.1** | 📸 Captura | Saída do `nslookup` para `www.google.com`, `www.usp.br` e `www.gov.br` |
| **E2.2** | 💬 Resposta | Endereços IP retornados para cada domínio |
| **E2.3** | 💬 Resposta | Todas as respostas foram "Non-authoritative"? Explique por quê |

---

### Atividade 3 — Tipos de Registros DNS

**Objetivo:** Consultar diferentes tipos de registros DNS usando a opção `type` ou `querytype`.

#### 3.1 — Registro MX (Mail Exchange)

```bash
nslookup -type=MX gmail.com
```

Os registros MX indicam os servidores responsáveis por receber e-mails para o domínio. O número antes do nome do servidor é a **prioridade** (menor = mais prioritário).

#### 3.2 — Registro AAAA (IPv6)

```bash
nslookup -type=AAAA www.google.com
```

#### 3.3 — Registro CNAME (Alias / Nome Canônico)

```bash
nslookup -type=CNAME www.microsoft.com
```

Um CNAME é um "apelido" que redireciona para outro nome de domínio.

#### 3.4 — Registro NS (Name Server)

```bash
nslookup -type=NS google.com
```

Lista os servidores de nomes autoritativos para o domínio.

#### 3.5 — Registro SOA (Start of Authority)

```bash
nslookup -type=SOA google.com
```

O SOA contém informações administrativas da zona: servidor primário, e-mail do administrador, número serial, e temporizadores (refresh, retry, expire, minimum TTL).

#### 3.6 — Registro TXT

```bash
nslookup -type=TXT google.com
```

Registros TXT são frequentemente usados para verificação de domínio (SPF, DKIM, DMARC) e outros metadados.

**� Evidências da Atividade 3:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E3.1** | 📸 Captura | Saída de cada um dos 6 comandos (MX, AAAA, CNAME, NS, SOA, TXT) |
| **E3.2** | 💬 Resposta | Identifique pelo menos 2 servidores MX do Gmail e suas respectivas prioridades |
| **E3.3** | 💬 Resposta | Qual o servidor primário (SOA) do domínio `google.com`? |

---

### Atividade 4 — TTL e Cache DNS

**Objetivo:** Observar o valor de TTL e entender o mecanismo de cache.

Para visualizar o TTL no `nslookup`, ative o modo debug:

```bash
nslookup -debug www.google.com
```

Na saída detalhada, procure pelo campo **`ttl`** (em segundos). Ele aparece junto a cada registro retornado.

Agora execute o mesmo comando duas vezes seguidas, com um intervalo de cerca de 10 segundos:

```bash
nslookup -debug www.google.com
# aguarde ~10 segundos
nslookup -debug www.google.com
```

**� Evidências da Atividade 4:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E4.1** | 📸 Captura | Saída das duas execuções de `nslookup -debug www.google.com` (com intervalo de ~10s) |
| **E4.2** | 💬 Resposta | Valor de TTL na primeira e na segunda consulta |
| **E4.3** | 💬 Resposta | O TTL diminuiu entre as duas consultas? Explique o motivo |
| **E4.4** | 💬 Resposta | Se o TTL de um registro é 300 segundos, o que acontece quando esse tempo expira no cache do resolver? |

---

### Atividade 5 — Consulta Reversa (Registro PTR)

**Objetivo:** Descobrir o nome de domínio associado a um endereço IP (resolução reversa).

Primeiro, descubra o IP de um domínio:

```bash
nslookup www.google.com
```

Anote um dos IPs retornados e faça a consulta reversa:

```bash
nslookup <IP_OBTIDO>
```

Por exemplo:

```bash
nslookup 142.250.218.68
```

Repita o processo para o servidor DNS público da Cloudflare:

```bash
nslookup 1.1.1.1
```

E para um servidor público do Google:

```bash
nslookup 8.8.8.8
```

**� Evidências da Atividade 5:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E5.1** | 📸 Captura | Saída das consultas reversas para o IP do Google, `1.1.1.1` e `8.8.8.8` |
| **E5.2** | 💬 Resposta | Nome retornado para cada IP consultado |
| **E5.3** | 💬 Resposta | O nome retornado na consulta reversa é sempre idêntico ao nome consultado originalmente? Explique por que isso pode ocorrer |
| **E5.4** | 💬 Resposta | O que é o domínio especial `in-addr.arpa` que aparece nas respostas? |

---

### Atividade 6 — Explorando os Servidores Raiz

**Objetivo:** Identificar os servidores raiz (root servers) do DNS.

```bash
nslookup -type=NS .
```

Este comando consulta os registros NS da zona raiz (representada pelo ponto `.`), retornando a lista dos 13 root servers (de `a.root-servers.net` até `m.root-servers.net`).

Agora, consulte o endereço IP de um deles:

```bash
nslookup a.root-servers.net
```

**� Evidências da Atividade 6:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E6.1** | 📸 Captura | Saída do comando `nslookup -type=NS .` e de `nslookup a.root-servers.net` |
| **E6.2** | 💬 Resposta | Lista completa dos servidores raiz retornados |
| **E6.3** | 💬 Resposta | IP do servidor `a.root-servers.net` |

---

### Atividade 7 — Simulando a Resolução Iterativa (Passo a Passo)

**Objetivo:** Percorrer manualmente a cadeia de resolução DNS, simulando o comportamento iterativo que um resolver executa automaticamente.

Vamos resolver o domínio `www.usp.br` passo a passo.

#### Passo 1 — Consultar um servidor raiz

Escolha um servidor raiz (ex.: `a.root-servers.net`) e pergunte sobre `www.usp.br`:

```bash
nslookup www.usp.br a.root-servers.net
```

O servidor raiz **não** conhece a resposta final. Ele retorna uma **referência (referral)** para os servidores do TLD `.br`.

#### Passo 2 — Consultar o servidor TLD (.br)

Use um dos servidores NS retornados no passo anterior (ex.: `a.dns.br`):

```bash
nslookup www.usp.br a.dns.br
```

O servidor do TLD `.br` também não tem a resposta final. Ele faz referral para os servidores autoritativos da zona `usp.br`.

#### Passo 3 — Consultar o servidor autoritativo

Use um dos servidores NS retornados (ex.: `ns1.usp.br` ou o servidor indicado):

```bash
nslookup www.usp.br ns1.usp.br
```

Agora a resposta é **autoritativa** (authoritative answer) — ela vem diretamente do servidor responsável pelo domínio.

**� Evidências da Atividade 7:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E7.1** | 📸 Captura | Saída do Passo 1 — consulta ao servidor raiz |
| **E7.2** | 📸 Captura | Saída do Passo 2 — consulta ao servidor TLD (.br) |
| **E7.3** | 📸 Captura | Saída do Passo 3 — consulta ao servidor autoritativo |
| **E7.4** | 💬 Resposta | Em qual passo a resposta mudou de "Non-authoritative" para "Authoritative"? |

---

### Atividade 8 — Consulta Recursiva vs. Iterativa na Prática

**Objetivo:** Comparar o comportamento quando se usa o resolver padrão (recursivo) versus consultar diretamente servidores autoritativos (iterativo).

#### 8.1 — Consulta recursiva (comportamento padrão)

```bash
nslookup www.usp.br
```

O resolver faz todo o trabalho e retorna a resposta final.

#### 8.2 — Consulta iterativa (especificando o servidor)

```bash
nslookup www.usp.br a.root-servers.net
```

Ao especificar o servidor raiz, você força uma consulta direta a ele, que responde apenas com referrals.

**� Evidências da Atividade 8:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E8.1** | 📸 Captura | Saída da consulta recursiva (8.1) e da consulta iterativa (8.2) |
| **E8.2** | 💬 Resposta | Diferença de conteúdo entre as duas respostas |
| **E8.3** | 💬 Resposta | Por que na consulta 8.1 você recebe diretamente o IP, enquanto na 8.2 você recebe referências a outros servidores? |
| **E8.4** | 💬 Resposta | Qual modelo (recursivo ou iterativo) é mais comum na perspectiva do usuário final? E na perspectiva do resolver? |

---

### Atividade 9 — Identificando o Servidor Autoritativo de um Domínio

**Objetivo:** Localizar e consultar diretamente o servidor autoritativo de um domínio.

```bash
nslookup -type=NS usp.br
```

Identifique os servidores autoritativos. Depois, consulte diretamente um deles:

```bash
nslookup www.usp.br <SERVIDOR_NS_RETORNADO>
```

Observe que agora a resposta **não** contém a linha "Non-authoritative answer" — ela é autoritativa.

Repita o processo para outro domínio à sua escolha (ex.: `amazon.com.br`, `gov.br`).

**� Evidências da Atividade 9:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E9.1** | 📸 Captura | Saída de `nslookup -type=NS usp.br` e da consulta direta ao servidor autoritativo |
| **E9.2** | 💬 Resposta | Servidores NS de `usp.br` |
| **E9.3** | 💬 Resposta | Diferença visual na saída entre uma resposta autoritativa e uma não autoritativa |
| **E9.4** | 📸 Captura | Saída das consultas NS e direta para o segundo domínio escolhido |
| **E9.5** | 💬 Resposta | Para o segundo domínio: quais são os servidores NS e qual IP foi retornado? |

---

### Atividade 10 — Trocando o Servidor DNS Resolver

**Objetivo:** Comparar respostas de diferentes resolvers públicos.

Execute a mesma consulta usando três resolvers diferentes:

```bash
nslookup www.google.com 8.8.8.8
nslookup www.google.com 1.1.1.1
nslookup www.google.com 9.9.9.9
```

| Resolver | Provedor |
|---|---|
| `8.8.8.8` | Google Public DNS |
| `1.1.1.1` | Cloudflare DNS |
| `9.9.9.9` | Quad9 (com filtro de segurança) |

**� Evidências da Atividade 10:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E10.1** | 📸 Captura | Saída dos 3 comandos `nslookup` usando os resolvers `8.8.8.8`, `1.1.1.1` e `9.9.9.9` |
| **E10.2** | 📋 Tabela | Preencha: Resolver → IP retornado para `www.google.com` |
| **E10.3** | 💬 Resposta | Os resultados foram idênticos? Se não, por que provedores diferentes podem retornar IPs diferentes para o mesmo domínio? (Considere CDNs, Anycast e geolocalização) |

---

### Atividade 11 — Investigando a Delegação de Zonas

**Objetivo:** Rastrear a cadeia de delegação DNS de um domínio com múltiplos níveis.

Consulte os NS de cada nível da hierarquia para o domínio `www.ime.usp.br`:

```bash
nslookup -type=NS br
nslookup -type=NS usp.br
nslookup -type=NS ime.usp.br
```

**� Evidências da Atividade 11:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E11.1** | 📸 Captura | Saída dos comandos `nslookup -type=NS` para `br`, `usp.br` e `ime.usp.br` |
| **E11.2** | 💬 Resposta | Servidores NS em cada nível da hierarquia |
| **E11.3** | 💬 Resposta | Existe delegação de `usp.br` para `ime.usp.br` (ou seja, `ime.usp.br` possui seus próprios servidores NS)? |
| **E11.4** | 💬 Resposta | O que aconteceria se o servidor autoritativo de `usp.br` ficasse fora do ar? A resolução de `www.ime.usp.br` seria afetada? |

---

### Atividade 12 — DNS e Segurança: Consulta TXT para SPF/DMARC

**Objetivo:** Verificar registros de segurança de e-mail configurados via DNS.

```bash
nslookup -type=TXT google.com
nslookup -type=TXT _dmarc.google.com
```

Os registros **SPF** (Sender Policy Framework) definem quais servidores estão autorizados a enviar e-mails em nome do domínio. O **DMARC** define políticas de autenticação de e-mail.

```bash
nslookup -type=TXT _dmarc.usp.br
```

**� Evidências da Atividade 12:**

| Código | Tipo | Descrição |
|--------|------|-----------|
| **E12.1** | 📸 Captura | Saída dos comandos TXT para `google.com`, `_dmarc.google.com` e `_dmarc.usp.br` |
| **E12.2** | 💬 Resposta | Conteúdo do registro SPF de `google.com` (registro que começa com `v=spf1`) |
| **E12.3** | 💬 Resposta | Conteúdo do registro DMARC de `google.com` |
| **E12.4** | 💬 Resposta | O domínio `usp.br` possui registro DMARC configurado? |

---

## 📊 Tabela de Resumo dos Tipos de Registro

Ao final das atividades, preencha a tabela abaixo com base nas suas observações:

| Tipo | Nome Completo | Finalidade | Exemplo observado |
|---|---|---|---|
| A | Address | Mapeia nome → IPv4 | |
| AAAA | Address (IPv6) | Mapeia nome → IPv6 | |
| CNAME | Canonical Name | Alias / apelido para outro nome | |
| MX | Mail Exchange | Servidor de e-mail do domínio | |
| NS | Name Server | Servidor autoritativo da zona | |
| SOA | Start of Authority | Informações administrativas da zona | |
| PTR | Pointer | Mapeia IP → nome (resolução reversa) | |
| TXT | Text | Metadados (SPF, DKIM, DMARC, etc.) | |

---

## ✅ Entregáveis

O aluno deve entregar um **relatório** (utilize o documento modelo fornecido) contendo **todos** os itens abaixo.

> **Formato de entrega:** Documento `.docx` ou `.pdf` seguindo o modelo disponibilizado junto ao roteiro.

### 1. Evidências de Execução (📸 Captura)

Todas as evidências do tipo **📸 Captura** listadas nas atividades, identificadas pelo código correspondente.

| Atividade | Evidências de Captura |
|-----------|-----------------------|
| 1 | E1.1 |
| 2 | E2.1 |
| 3 | E3.1 |
| 4 | E4.1 |
| 5 | E5.1 |
| 6 | E6.1 |
| 7 | E7.1, E7.2, E7.3 |
| 8 | E8.1 |
| 9 | E9.1, E9.4 |
| 10 | E10.1 |
| 11 | E11.1 |
| 12 | E12.1 |

**Total: 15 capturas**

### 2. Respostas às Questões (💬 Resposta)

Todas as evidências do tipo **💬 Resposta**, com respostas completas e fundamentadas.

| Atividade | Evidências de Resposta |
|-----------|------------------------|
| 1 | E1.2, E1.3 |
| 2 | E2.2, E2.3 |
| 3 | E3.2, E3.3 |
| 4 | E4.2, E4.3, E4.4 |
| 5 | E5.2, E5.3, E5.4 |
| 6 | E6.2, E6.3 |
| 7 | E7.4 |
| 8 | E8.2, E8.3, E8.4 |
| 9 | E9.2, E9.3, E9.5 |
| 10 | E10.3 |
| 11 | E11.2, E11.3, E11.4 |
| 12 | E12.2, E12.3, E12.4 |

**Total: 25 respostas**

### 3. Tabelas (📋 Tabela)

| Código | Descrição |
|--------|-----------|
| **E10.2** | Tabela comparativa de IPs retornados por cada resolver |
| **Tabela Final** | Tabela de resumo dos tipos de registro DNS (seção anterior) preenchida com exemplos reais |

**Total: 2 tabelas**

### Resumo Geral de Evidências

| Tipo | Quantidade |
|------|------------|
| 📸 Capturas | 15 |
| 💬 Respostas | 25 |
| 📋 Tabelas | 2 |
| **Total** | **42 itens** |

---

## 📚 Referências e Recursos Complementares

- **RFC 1035** — Domain Names: Implementation and Specification  
- **RFC 8484** — DNS Queries over HTTPS (DoH)  
- **RFC 7858** — DNS over Transport Layer Security (DoT)  
- [Root Servers — root-servers.org](https://root-servers.org/)  
- [IANA — Root Zone Database](https://www.iana.org/domains/root/db)  
- [NIC.br — Sobre o DNS](https://nic.br/pagina/sobre-dns/108)  
- [Google Public DNS — Documentação](https://developers.google.com/speed/public-dns)  
- [Cloudflare DNS — 1.1.1.1](https://1.1.1.1/)  

---


