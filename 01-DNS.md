# 1️⃣ DNS - O Sistema de Endereços da Internet

[← Voltar ao Índice](./README.md)

> **Tempo de leitura:** ~15 minutos  
> **Nível:** Iniciante

---


### 🎯 O Que É DNS?

**DNS** significa **Domain Name System** (Sistema de Nomes de Domínio).

**Analogia simples:**
Imagine que você quer visitar um amigo. Você pode usar:
- ✅ **Endereço descritivo:** "Rua das Flores, 123" (fácil de lembrar)
- ❌ **Coordenadas GPS:** "-23.5505, -46.6333" (difícil de decorar)

O DNS faz exatamente isso na internet! Ele transforma nomes amigáveis como `www.google.com` em endereços numéricos que os computadores entendem: `142.250.185.78`

### 📖 Por Que Precisamos do DNS?

Computadores se comunicam usando **endereços IP** (números como `192.168.0.1`), mas para nós, humanos, é muito mais fácil lembrar de:
- ✅ `www.facebook.com`
- ❌ `157.240.241.35`

**O DNS é como uma agenda telefônica gigante da internet!**

### 🔍 Como Funciona o DNS (Passo a Passo)

Vamos entender o que acontece quando você digita `www.exemplo.com` no navegador:

```
┌──────────────────────────────────────────────────────────┐
│  1. VOCÊ DIGITA: www.exemplo.com                         │
└─────────────────┬────────────────────────────────────────┘
                  │
                  ▼
┌──────────────────────────────────────────────────────────┐
│  2. SEU COMPUTADOR PERGUNTA:                             │
│  "Cache local, você sabe o IP de www.exemplo.com?"       │
│                                                           │
│  Cache diz: "Não sei" OU "Sim, é 192.0.2.1"             │
└─────────────────┬────────────────────────────────────────┘
                  │ (Se não souber)
                  ▼
┌──────────────────────────────────────────────────────────┐
│  3. PERGUNTA AO SERVIDOR DNS DO PROVEDOR DE INTERNET:    │
│  "Você sabe o IP de www.exemplo.com?"                    │
└─────────────────┬────────────────────────────────────────┘
                  │
                  ▼
┌──────────────────────────────────────────────────────────┐
│  4. SERVIDOR DNS CONSULTA A HIERARQUIA:                  │
│                                                           │
│  a) Root Server (servidor raiz): ".com está em X"        │
│  b) TLD Server (.com): "exemplo.com está em Y"           │
│  c) Authoritative Server: "www.exemplo.com = 192.0.2.1"  │
└─────────────────┬────────────────────────────────────────┘
                  │
                  ▼
┌──────────────────────────────────────────────────────────┐
│  5. RESPOSTA VOLTA:                                       │
│  "O IP de www.exemplo.com é 192.0.2.1"                   │
└─────────────────┬────────────────────────────────────────┘
                  │
                  ▼
┌──────────────────────────────────────────────────────────┐
│  6. SEU NAVEGADOR CONECTA NO IP 192.0.2.1                │
│  e carrega o site!                                        │
└──────────────────────────────────────────────────────────┘
```

### 🏗️ Componentes do DNS

#### 1. **Registros DNS (DNS Records)**

São as "fichas" na agenda do DNS. Principais tipos:

| Tipo | Nome Completo | O Que Faz | Analogia |
|------|---------------|-----------|----------|
| **A** | Address Record | Aponta um domínio para um endereço IPv4 | O endereço residencial da casa |
| **AAAA** | IPv6 Address | Aponta um domínio para um endereço IPv6 | Endereço residencial novo (mais longo) |
| **CNAME** | Canonical Name | Cria um "apelido" para outro domínio | "Zé" é apelido de "José" |
| **MX** | Mail Exchange | Define onde os e-mails devem ser entregues | Endereço da caixa de correio |
| **TXT** | Text Record | Armazena texto (usado para verificações) | Anotações na agenda |
| **NS** | Name Server | Define quais servidores gerenciam o domínio | O síndico do prédio |

**Exemplos práticos:**

```
# Registro A (mais comum)
www.exemplo.com.    A    192.0.2.1

Significado: "O site www.exemplo.com está no servidor 192.0.2.1"

# Registro CNAME
blog.exemplo.com.   CNAME   www.exemplo.com.

Significado: "blog.exemplo.com é um apelido de www.exemplo.com"

# Registro MX
exemplo.com.        MX     10 mail.exemplo.com.

Significado: "E-mails para @exemplo.com devem ir para mail.exemplo.com"
```

#### 2. **TTL (Time To Live)**

É o tempo que uma informação DNS pode ficar "guardada" (em cache) antes de precisar ser atualizada.

**Analogia:**
Imagine que você anota o telefone de uma pizzaria. O TTL é quanto tempo você confia que esse número ainda é válido antes de verificar novamente.

- **TTL baixo** (300 segundos = 5 minutos): Atualiza rápido, mas faz mais consultas
- **TTL alto** (86400 segundos = 24 horas): Demora para atualizar, mas economiza consultas

**Exemplo:**
```
www.exemplo.com.    300    A    192.0.2.1
                    ↑
                    TTL = 300 segundos (5 minutos)
```

### 🛠️ Ferramentas Para Testar DNS

#### 1. **nslookup** (Windows, Mac, Linux)

Consulta informações DNS de um domínio.

**Como usar:**
```bash
# Windows: Abra o Prompt de Comando (cmd)
# Mac/Linux: Abra o Terminal

nslookup www.google.com

# Resposta:
# Nome:    www.google.com
# Endereço:  142.250.185.78
```

#### 2. **dig** (Mac, Linux - mais detalhado)

Fornece informações completas sobre DNS.

```bash
dig www.google.com

# Mostra:
# - Tempo de resposta
# - Servidores DNS consultados
# - TTL
# - Registros encontrados
```

#### 3. **Ferramentas Online**

- **DNSChecker.org** - Verifica DNS em vários países
- **WhatIsMyDNS.net** - Verifica propagação de DNS
- **MXToolbox.com** - Testa registros MX e DNS

### ⚠️ Problemas Comuns de DNS (Para Suporte)

#### Problema 1: "Site não carrega"

**Sintomas:**
- Cliente diz: "Não consigo acessar nosso site"
- Erro no navegador: "Este site não pode ser acessado" ou "DNS_PROBE_FINISHED_NXDOMAIN"

**Diagnóstico:**
```bash
# 1. Teste o DNS
nslookup www.siteproblema.com

# Se retornar erro:
# "servidor não pode encontrar www.siteproblema.com"
# → Problema de DNS!
```

**Causas comuns:**
- ✅ Domínio não foi configurado corretamente
- ✅ Registro DNS foi deletado acidentalmente
- ✅ DNS ainda está propagando (após mudanças recentes)
- ✅ Servidor DNS do cliente está com problemas

**Soluções:**
1. Verificar se o domínio está registrado e ativo
2. Confirmar que os registros DNS existem
3. Aguardar propagação (pode levar até 48h após mudanças)
4. Pedir ao cliente para usar DNS público (Google: 8.8.8.8)

#### Problema 2: "DNS propagation" (Propagação de DNS)

**O que é:**
Quando você muda um registro DNS (ex: trocar o IP do servidor), essa mudança não é instantânea. Leva tempo para se espalhar por todos os servidores DNS do mundo.

**Analogia:**
É como mudar de endereço. Você atualiza seu endereço nos Correios, mas leva alguns dias até todos os carteiros saberem do novo endereço.

**Tempo típico:**
- ⏱️ Mínimo: 5 minutos (se TTL for baixo)
- ⏱️ Comum: 4-24 horas
- ⏱️ Máximo: 48 horas

**Como explicar ao cliente:**
> "Imagine que mudamos o endereço do nosso servidor. Essa informação precisa se espalhar por milhares de servidores DNS ao redor do mundo. É como atualizar uma agenda de contatos gigante - leva algum tempo até todos terem a nova informação."

#### Problema 3: Cache DNS local

**Sintomas:**
- Você mudou o DNS, mas o cliente ainda vê o site antigo
- Outros lugares já veem o site novo, mas o cliente não

**Causa:**
O computador do cliente guardou (fez cache) da informação DNS antiga.

**Solução:**

**Windows:**
```cmd
# Abrir Prompt de Comando como Administrador
ipconfig /flushdns

# Mensagem de sucesso:
# "Cache do DNS Resolver foi liberado com êxito."
```

**Mac:**
```bash
# Terminal
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```

**Linux:**
```bash
# Ubuntu/Debian
sudo systemd-resolve --flush-caches

# Verificar
sudo systemd-resolve --statistics
```

### 📊 Entendendo a Hierarquia DNS

```
                    . (Root - Raiz)
                    │
        ┌───────────┼───────────┐
        │           │           │
       .com        .org        .br (TLDs - Top Level Domains)
        │           │           │
    ────┼────   ────┼────   ────┼────
    │       │   │       │   │       │
 google  amazon  wikipedia  gov   com.br
    │                                │
    │                                │
   www                           exemplo
                                    │
                                   www
```

**Exemplo completo:**
```
www.exemplo.com.br
│   │      │   │
│   │      │   └── TLD (Top Level Domain) - País
│   │      └────── SLD (Second Level Domain) - Tipo
│   └───────────── Domínio - Empresa
└───────────────── Subdomínio - Aplicação específica
```

### 🎓 Conceitos Importantes

#### 1. **Servidor DNS Autoritativo**

É o servidor que tem a resposta "oficial" sobre um domínio.

**Analogia:**
É como o cartório que tem o registro oficial de uma propriedade.

#### 2. **Servidor DNS Recursivo**

É o servidor que busca a resposta para você, consultando outros servidores.

**Analogia:**
É como um detetive que pergunta a várias pessoas até encontrar a resposta.

#### 3. **DNS Público vs DNS do Provedor**

| DNS do Provedor | DNS Público (ex: Google, Cloudflare) |
|----------------|-------------------------------------|
| ✅ Automaticamente configurado | ⚙️ Precisa configurar manualmente |
| ⚠️ Pode ser lento | ✅ Geralmente mais rápido |
| ⚠️ Pode ter problemas técnicos | ✅ Mais confiável |
| ❌ Pode bloquear alguns sites | ✅ Neutro |

**DNS Públicos famosos:**
- **Google:** 8.8.8.8 e 8.8.4.4
- **Cloudflare:** 1.1.1.1 e 1.0.0.1
- **OpenDNS:** 208.67.222.222 e 208.67.220.220

### ✅ Checklist de Troubleshooting DNS

Para suporte técnico, use esta sequência:

- [ ] **Passo 1:** O domínio está registrado e não expirou?
- [ ] **Passo 2:** O DNS aponta para o IP correto? (use `nslookup`)
- [ ] **Passo 3:** O problema é local ou global? (teste de outros lugares)
- [ ] **Passo 4:** Houve mudança recente? (aguardar propagação)
- [ ] **Passo 5:** Cache DNS foi limpo? (instrua o cliente)
- [ ] **Passo 6:** TTL está adequado? (verificar registros)
- [ ] **Passo 7:** Servidor DNS do cliente funciona? (testar com 8.8.8.8)

---

---

## 📚 Próximos Passos

Continue aprendendo:

➡️ **[HTTP - A Linguagem da Web](./02-HTTP.md)**

---

[← Voltar ao Índice](./README.md)
