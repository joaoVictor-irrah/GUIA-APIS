# 6️⃣ SSL/TLS - Segurança na Comunicação

[← Voltar ao Índice](./README.md)

> **Tempo de leitura:** ~12 minutos  
> **Nível:** Intermediário

---

### 🎯 O Que É SSL/TLS?

**SSL** = **S**ecure **S**ockets **L**ayer (antigo, não mais usado)
**TLS** = **T**ransport **L**ayer **S**ecurity (versão moderna)

**Definição simples:**
TLS é a tecnologia que "tranca" (criptografa) os dados trafegados entre cliente e servidor, impedindo que sejam lidos por terceiros.

**Analogia:**

**SEM TLS (HTTP):**

```
Você escreve uma carta e envia pelos Correios dentro de um CARTÃO POSTAL.
↓
Qualquer pessoa (carteiro, vizinho, funcionário) pode ler o conteúdo.
🔓 "Minha senha é 12345"
```

**COM TLS (HTTPS):**

```
Você escreve uma carta e envia dentro de um ENVELOPE LACRADO.
↓
Apenas o destinatário pode abrir e ler.
🔐 Conteúdo protegido!
```

### 🔐 Por Que TLS É Importante?

Sem TLS, suas informações trafegam "abertas" pela internet:

**Dados vulneráveis sem HTTPS:**

- ❌ Senhas
- ❌ Números de cartão de crédito
- ❌ Dados pessoais (CPF, endereço, telefone)
- ❌ Mensagens privadas
- ❌ Tokens de autenticação

**Com HTTPS (TLS):**

- ✅ Dados criptografados
- ✅ Identidade do servidor verificada
- ✅ Integridade (dados não foram alterados)

### 🤝 Como Funciona o TLS (Handshake)

Quando você acessa um site HTTPS, acontece uma "negociação" (handshake) antes de enviar dados:

```
┌──────────────────────────────────────────────────────┐
│  1. CLIENTE → SERVIDOR                               │
│  "Olá! Quero conexão segura. Suporto estes algoritmos:"
│  - TLS 1.3                                           │
│  - AES-256                                           │
└──────────────────┬───────────────────────────────────┘
                   │
                   ▼
┌──────────────────────────────────────────────────────┐
│  2. SERVIDOR → CLIENTE                               │
│  "Ok! Vamos usar TLS 1.3 e AES-256."                │
│  "Aqui está meu CERTIFICADO (identidade)"           │
└──────────────────┬───────────────────────────────────┘
                   │
                   ▼
┌──────────────────────────────────────────────────────┐
│  3. CLIENTE VALIDA CERTIFICADO                       │
│  - Certificado foi emitido por autoridade confiável? │
│  - Certificado não expirou?                          │
│  - Domínio do certificado bate com o site?           │
│  ✅ Tudo OK!                                          │
└──────────────────┬───────────────────────────────────┘
                   │
                   ▼
┌──────────────────────────────────────────────────────┐
│  4. TROCA DE CHAVES                                  │
│  Cliente e servidor criam uma "chave de sessão"     │
│  compartilhada (matemática complexa)                 │
└──────────────────┬───────────────────────────────────┘
                   │
                   ▼
┌──────────────────────────────────────────────────────┐
│  5. CONEXÃO SEGURA ESTABELECIDA                      │
│  Agora TODOS os dados são criptografados com a      │
│  chave de sessão                                     │
└──────────────────────────────────────────────────────┘
```

**Tempo total:** ~100-300ms

### 📜 Certificados SSL/TLS

Um **certificado** é como uma "carteira de identidade" digital que prova que um site é legítimo.

**O que contém:**

- 🏢 Nome do site/empresa
- 🔑 Chave pública (para criptografia)
- 📅 Validade (data início e fim)
- ✍️ Assinatura da Autoridade Certificadora (CA)

**Exemplo visual:**

```
┌────────────────────────────────────────┐
│     CERTIFICADO SSL                    │
├────────────────────────────────────────┤
│ Emitido para:  www.exemplo.com         │
│ Emitido por:   Let's Encrypt           │
│ Válido de:     2025-01-01              │
│ Válido até:    2025-04-01 (90 dias)    │
│                                        │
│ Chave Pública: [dados da chave]       │
│ Assinatura:    [assinatura da CA]     │
└────────────────────────────────────────┘
```

#### Autoridades Certificadoras (CA)

São empresas confiáveis que "assinam" certificados, garantindo sua validade.

**CAs Famosas:**

- **Let's Encrypt** (gratuita, automatizada)
- **DigiCert**
- **GlobalSign**
- **Comodo**
- **GeoTrust**

**Como funciona a confiança:**

```
1. Navegador tem lista de CAs confiáveis (pré-instaladas)
2. Site apresenta certificado assinado pela CA
3. Navegador verifica assinatura
4. Se CA for confiável, certificado é válido ✅
5. Se CA não for confiável, alerta de segurança ⚠️
```

#### Tipos de Certificados

| Tipo                             | Validação              | Uso                   | Preço            |
| -------------------------------- | ---------------------- | --------------------- | ---------------- |
| **DV** (Domain Validation)       | Apenas domínio         | Sites pessoais, blogs | Grátis - $50/ano |
| **OV** (Organization Validation) | Domínio + empresa      | Sites corporativos    | $50 - $200/ano   |
| **EV** (Extended Validation)     | Validação rigorosa     | Bancos, e-commerce    | $200 - $1000/ano |
| **Wildcard**                     | Subdomínios ilimitados | `*.exemplo.com`       | $100 - $500/ano  |

**Visual no navegador:**

```
DV:
🔒 https://exemplo.com

EV (barra verde - alguns navegadores):
🔒 Exemplo Inc. | https://exemplo.com
```

### 🆚 HTTP vs HTTPS

| HTTP                   | HTTPS                  |
| ---------------------- | ---------------------- |
| `http://exemplo.com`   | `https://exemplo.com`  |
| Porta 80               | Porta 443              |
| ❌ Sem criptografia    | ✅ Criptografia TLS    |
| 🔓 Dados visíveis      | 🔐 Dados protegidos    |
| ⚠️ Inseguro            | ✅ Seguro              |
| ❌ Navegador alerta    | ✅ Cadeado verde       |
| ❌ Google penaliza SEO | ✅ Google favorece SEO |

**Visual no navegador:**

```
HTTP:
⚠️ Não seguro | http://exemplo.com

HTTPS:
🔒 https://exemplo.com
```

#### Ferramentas Online

- **SSL Labs** (https://www.ssllabs.com/ssltest/)
  - Testa configuração SSL/TLS
  - Dá nota (A+, A, B, C, F)
  - Mostra vulnerabilidades
- **WhyNoPadlock** (https://www.whynopadlock.com/)
  - Detecta por que cadeado não aparece
  - Mostra recursos HTTP em página HTTPS

---

### ⚠️ Problemas Comuns SSL/TLS

#### Problema 1: "Certificado Expirado"

**Sintomas:**

- Navegador: "Sua conexão não é particular"
- Erro: `NET::ERR_CERT_DATE_INVALID`

**Causa:**
Certificado passou da data de validade.

**Como verificar:**

```bash
echo | openssl s_client -connect google.com:443 2>/dev/null | \
  openssl x509 -noout -dates

# Se notAfter < hoje, expirou!
```

---

#### Problema 2: "Erro de Nome do Certificado"

**Sintomas:**

- Erro: `NET::ERR_CERT_COMMON_NAME_INVALID`
- "O certificado não é válido para este site"

**Causa:**
Certificado foi emitido para `exemplo.com`, mas você está acessando `www.exemplo.com`.

**Solução:**
Obter certificado que cubra ambos:

---

#### Problema 3: "Conteúdo Misto" (Mixed Content)

**Sintomas:**

- Site HTTPS, mas cadeado não aparece ou aparece com aviso
- Console do navegador: "Mixed Content Warning"

**Causa:**
Página HTTPS carrega recursos HTTP (imagens, scripts, CSS).

**Exemplo:**

```html
<!-- Página: https://img.freepik.com/free-vector/modern-conectivity-logo-template_23-2147934052.jpg -->

✅ OK:
<img
  src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4a/Web.com_logo.svg/2560px-Web.com_logo.svg.png"
/>

❌ ERRO (HTTP em página HTTPS):
<img
  src="https://i.ytimg.com/vi/uD6zczawkyU/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLAmmgipLedJYxC7smOKkbBJmwZgjw"
/>
<script src="http://cdn.example.com/script.js"></script>
```

**Solução:**

1. Mudar todos os recursos para HTTPS
2. Ou usar URLs relativas/protocol-relative

---

#### Problema 4: "Autoridade Certificadora Não Confiável"

**Sintomas:**

- Erro: `NET::ERR_CERT_AUTHORITY_INVALID`
- "O certificado não foi emitido por uma autoridade confiável"

**Causas:**

1. Certificado auto-assinado
2. CA não reconhecida
3. Cadeia de certificados incompleta

**Soluções:**

1. Usar certificado de CA reconhecida (Let's Encrypt, DigiCert, etc)
2. Instalar certificados intermediários no servidor
3. Se auto-assinado, usar apenas para desenvolvimento/testes

#### Problema 5: "SSL Handshake Failed"

**Sintomas:**

- Erro: `SSL_ERROR_HANDSHAKE_FAILURE`
- Não consegue estabelecer conexão segura

**Causas:**

1. Protocolo TLS antigo (TLS 1.0, 1.1 desabilitado)
2. Ciphers incompatíveis
3. Firewall bloqueando porta 443

**Solução:**

1. Atualizar configuração do servidor (habilitar TLS 1.2+)
2. Configurar ciphers modernos
3. Verificar firewall (porta 443 aberta?)

---

#### Problema 5: "API Retornando SSL: CERTIFICATE_VERIFY_FAILED"

**Sintomas:**

- Erro: `SSL: CERTIFICATE_VERIFY_FAILED`, `unable to get local issuer certificate` ou similar  
- Ocorre ao tentar conectar via HTTPS à API  
- Pode acontecer apenas em alguns servidores/ambientes (ex.: só em produção, só em um container)

**Possíveis Causas:**

1. Certificado do servidor inválido, expirado ou com CN/SAN diferente da URL usada  
2. Cadeia de certificados incompleta no servidor (faltando intermediário)  
3. Pacote de certificados raiz (CAs) desatualizado no sistema do cliente  
4. Cliente acessando por IP ou domínio diferente do certificado  
5. Linguagem/ambiente usando CA bundle próprio e desatualizado (Python, Node.js, Java, etc.)  

**Solução:**

1. **Confirmar o erro e o ambiente do cliente**
   - Solicitar a mensagem exata do erro (ex.: `SSL: CERTIFICATE_VERIFY_FAILED`, `unable to get local issuer certificate`)  
   - Identificar linguagem/ambiente: Python (requests), Node.js, Java, cURL, n8n, etc.  
   - Checar se o problema ocorre em todos os ambientes ou apenas em um servidor específico  

2. **Verificar o certificado do lado do servidor**
   - Rodar teste em [SSL Labs](https://www.ssllabs.com/ssltest/) com o domínio da API  
   - Confirmar:
     - Certificado válido (não expirado, CN/SAN correspondendo à URL)  
     - Cadeia de certificados completa  
     - Nota A ou similar no SSL Labs  
   - Se o teste estiver OK:
     - Indicar que o problema provavelmente está no ambiente do cliente (CAs/SO)  

3. **Validar domínio e URL usados pelo cliente**
   - Confirmar se a URL utilizada corresponde exatamente ao certificado:
     - Ex.: `https://api.suaempresa.com` e **não** `https://<ip>` ou outro domínio  
   - Checar se não está usando domínio antigo ou ambiente de teste com certificado diferente  

4. **Atualizar certificados raiz no ambiente do cliente (se o servidor estiver correto)**
   - Orientar o cliente a atualizar o pacote de certificados raiz (CAs) no sistema onde a aplicação roda:  

   - **Linux (Debian/Ubuntu):**
     ```bash
     sudo apt-get update
     sudo apt-get install --reinstall ca-certificates
     sudo update-ca-certificates
     ```

   - **Linux (CentOS/RHEL/Alma/Rocky):**
     ```bash
     sudo yum update ca-certificates -y
     # ou
     sudo dnf update ca-certificates -y
     sudo update-ca-trust enable
     sudo update-ca-trust extract
     ```

   - **Containers Docker:**
     - Garantir que a imagem base está atualizada  
     - Reinstalar `ca-certificates` dentro do container  
     - Fazer rebuild/restart do serviço  

   - **Windows:**
     - Garantir que o sistema está atualizado  
     - Verificar se o certificado raiz/intermediário da autoridade emissora (ex.: Let's Encrypt) está presente em **Trusted Root Certification Authorities**  

5. **Testar via cURL no mesmo servidor da aplicação**
   - Pedir para o cliente rodar no **mesmo host** da aplicação:  
     ```bash
     curl -v https://api.suaempresa.com/saude
     ```
   - Se o cURL também falhar:
     - Confirma que o problema é do sistema (CAs ou rede)  
   - Se o cURL funcionar:
     - O problema é provavelmente da linguagem/biblioteca HTTP da aplicação (ex.: bundle de CAs customizado, proxy, etc.)  

6. **Tratar casos especiais por linguagem/ambiente**
   - **Python (requests):**
     - Verificar se está usando `certifi` muito antigo  
     - Atualizar:  
       ```bash
       pip install --upgrade certifi
       ```

   - **Node.js:**
     - Verificar se existe variável `NODE_EXTRA_CA_CERTS` apontando para CA bundle antigo  
     - Atualizar Node.js ou o bundle de CAs em uso  

   - **Java:**
     - Verificar o keystore `cacerts` da JVM  
     - Atualizar JDK/JRE ou importar certificados raiz recentes no keystore  

7. **Alternativa temporária (com cuidado)**
   - Explicar que desativar a verificação SSL (ex.: `verify=False`, `rejectUnauthorized: false`) é **inseguro**  
   - Permitir apenas:
     - Em ambiente de desenvolvimento  
     - Por tempo limitado para diagnóstico  
   - Reforçar que a solução definitiva é sempre atualizar e corrigir o conjunto de certificados confiáveis  

8. **Quando escalar**
   - Se o SSL Labs indicar problema do lado do servidor:
     - Certificado expirado  
     - Cadeia incompleta  
     - Domínio incorreto no certificado  
   - Escalar para a equipe de infraestrutura com:
     - Domínio afetado  
     - Resultado do SSL Labs  
     - Horário em que o cliente percebeu o problema

---

### ✅ Checklist SSL/TLS

- [ ] Certificado válido (não expirou)
- [ ] Emitido por CA confiável
- [ ] Cobre todos os domínios/subdomínios
- [ ] Protocolo TLS 1.2+ habilitado
- [ ] Ciphers modernos configurados
- [ ] HTTPS forçado (HTTP redireciona)
- [ ] Sem conteúdo misto (mixed content)
- [ ] HSTS configurado
- [ ] Renovação automática (Let's Encrypt)
- [ ] Nota A ou A+ no SSL Labs

### 📊 TLS Versions

| Versão  | Status        | Segurança       | Uso        |
| ------- | ------------- | --------------- | ---------- |
| SSL 2.0 | ❌ Obsoleto   | 🔴 Inseguro     | Nunca usar |
| SSL 3.0 | ❌ Obsoleto   | 🔴 Inseguro     | Nunca usar |
| TLS 1.0 | ⚠️ Depreciado | 🟡 Fraco        | Evitar     |
| TLS 1.1 | ⚠️ Depreciado | 🟡 Fraco        | Evitar     |
| TLS 1.2 | ✅ Atual      | 🟢 Seguro       | Usar       |
| TLS 1.3 | ✅ Moderno    | 🟢 Muito seguro | Usar       |

**Recomendação:** Habilitar apenas TLS 1.2 e 1.3.

---

---

## 📚 Próximos Passos

Continue aprendendo:

➡️ **[Microserviços - Arquitetura Moderna](./07-Microservicos.md)**

---

[← Voltar ao Índice](./README.md)
