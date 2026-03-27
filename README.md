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

> **⚠️ IMPORTANTE:** Para cada atividade, **copie a saída completa do terminal** (texto ou captura de tela) como evidência. Identifique cada evidência com o número da atividade correspondente.

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

**📝 Registre:**
- O nome e o IP do servidor DNS padrão da sua máquina.
- Este servidor é público (ex.: Google `8.8.8.8`, Cloudflare `1.1.1.1`) ou privado/institucional?

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

**📝 Registre:**
- Os endereços IP retornados para cada domínio.
- Todas as respostas foram "Non-authoritative"? Por quê?

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

**📝 Registre:**
- A saída de cada comando acima.
- Identifique pelo menos 2 servidores MX do Gmail e suas respectivas prioridades.
- Qual o servidor primário (SOA) do domínio `google.com`?

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

**📝 Registre:**
- O valor de TTL na primeira e na segunda consulta.
- O TTL diminuiu entre as duas consultas? Explique o motivo.
- Se o TTL de um registro é 300 segundos, o que acontece quando esse tempo expira no cache do resolver?

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

**📝 Registre:**
- O nome retornado para cada IP consultado.
- O nome retornado na consulta reversa é sempre idêntico ao nome consultado originalmente? Explique por que isso pode ocorrer.
- O que é o domínio especial `in-addr.arpa` que aparece nas respostas?

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

**📝 Registre:**
- A lista completa dos servidores raiz retornados.
- O IP do servidor `a.root-servers.net`.

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

**📝 Registre:**
- A saída de cada um dos 3 passos.
- Em qual passo a resposta mudou de "Non-authoritative" para "Authoritative"?
- Desenhe um diagrama (pode ser à mão ou digital) mostrando o caminho percorrido: `Cliente → Raiz → TLD → Autoritativo`.

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

**📝 Registre:**
- A diferença de conteúdo entre as duas respostas.
- Por que na consulta 8.1 você recebe diretamente o IP, enquanto na 8.2 você recebe referências a outros servidores?
- Qual modelo (recursivo ou iterativo) é mais comum na perspectiva do usuário final? E na perspectiva do resolver?

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

**📝 Registre:**
- Os servidores NS de `usp.br`.
- A diferença visual na saída entre uma resposta autoritativa e uma não autoritativa.
- Para o segundo domínio escolhido: quais são os servidores NS e qual IP foi retornado?

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

**📝 Registre:**
- Os IPs retornados por cada resolver para `www.google.com`.
- Os resultados foram idênticos? Se não, por que provedores diferentes podem retornar IPs diferentes para o mesmo domínio? (Dica: pense em CDNs, Anycast e geolocalização.)

---

### Atividade 11 — Investigando a Delegação de Zonas

**Objetivo:** Rastrear a cadeia de delegação DNS de um domínio com múltiplos níveis.

Consulte os NS de cada nível da hierarquia para o domínio `www.ime.usp.br`:

```bash
nslookup -type=NS br
nslookup -type=NS usp.br
nslookup -type=NS ime.usp.br
```

**📝 Registre:**
- Os servidores NS em cada nível.
- Existe delegação de `usp.br` para `ime.usp.br` (ou seja, `ime.usp.br` possui seus próprios servidores NS)?
- O que aconteceria se o servidor autoritativo de `usp.br` ficasse fora do ar? A resolução de `www.ime.usp.br` seria afetada?

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

**📝 Registre:**
- O conteúdo do registro SPF de `google.com` (procure o registro que começa com `v=spf1`).
- O conteúdo do registro DMARC de `google.com`.
- O domínio `usp.br` possui registro DMARC configurado?

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

O aluno deve entregar um **relatório** contendo os seguintes itens:

### 1. Evidências de Execução
- Capturas de tela ou cópia textual da saída de **todas as 12 atividades**.
- Cada evidência deve estar identificada com o número da atividade (ex.: "Atividade 7 — Passo 2").

### 2. Respostas às Questões
- Respostas completas e fundamentadas para **todas as perguntas** marcadas com 📝 ao longo do roteiro.

### 3. Tabela de Resumo
- A tabela de tipos de registro DNS (seção anterior) devidamente preenchida com exemplos reais obtidos durante a prática.

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


