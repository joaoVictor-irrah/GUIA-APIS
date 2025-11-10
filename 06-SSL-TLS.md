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

| Tipo | Validação | Uso | Preço |
|------|-----------|-----|-------|
| **DV** (Domain Validation) | Apenas domínio | Sites pessoais, blogs | Grátis - $50/ano |
| **OV** (Organization Validation) | Domínio + empresa | Sites corporativos | $50 - $200/ano |
| **EV** (Extended Validation) | Validação rigorosa | Bancos, e-commerce | $200 - $1000/ano |
| **Wildcard** | Subdomínios ilimitados | `*.exemplo.com` | $100 - $500/ano |

**Visual no navegador:**

```
DV:
🔒 https://exemplo.com

EV (barra verde - alguns navegadores):
🔒 Exemplo Inc. | https://exemplo.com
```

### 🆚 HTTP vs HTTPS

| HTTP | HTTPS |
|------|-------|
| `http://exemplo.com` | `https://exemplo.com` |
| Porta 80 | Porta 443 |
| ❌ Sem criptografia | ✅ Criptografia TLS |
| 🔓 Dados visíveis | 🔐 Dados protegidos |
| ⚠️ Inseguro | ✅ Seguro |
| ❌ Navegador alerta | ✅ Cadeado verde |
| ❌ Google penaliza SEO | ✅ Google favorece SEO |

**Visual no navegador:**

```
HTTP:
⚠️ Não seguro | http://exemplo.com

HTTPS:
🔒 https://exemplo.com
```

### 🛠️ Obtendo um Certificado SSL/TLS

#### Opção 1: Let's Encrypt (Gratuito, Automatizado)

Let's Encrypt é uma CA que oferece certificados grátis com renovação automática.

**Vantagens:**
- ✅ Totalmente gratuito
- ✅ Automatizado (certbot)
- ✅ Confiável (reconhecido por todos navegadores)
- ✅ Renovação automática a cada 90 dias

**Como instalar (servidor Linux):**

```bash
# 1. Instalar Certbot
sudo apt update
sudo apt install certbot python3-certbot-nginx

# 2. Obter certificado (Nginx)
sudo certbot --nginx -d exemplo.com -d www.exemplo.com

# 3. Certbot configura tudo automaticamente!
# - Obtém certificado
# - Configura Nginx
# - Configura renovação automática

# 4. Testar renovação
sudo certbot renew --dry-run

# 5. Certificado será renovado automaticamente a cada 90 dias
```

**Para Apache:**
```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d exemplo.com
```

#### Opção 2: Certificado Comercial (Pago)

**Quando usar:**
- Precisa de suporte dedicado
- EV certificate (barra verde)
- Garantia financeira
- Wildcard (múltiplos subdomínios)

**Passos:**
1. Comprar em CA (DigiCert, GeoTrust, etc)
2. Gerar CSR (Certificate Signing Request) no servidor
3. Enviar CSR para CA
4. CA valida identidade
5. CA emite certificado
6. Instalar certificado no servidor

### 🔍 Verificando Certificados

#### No Navegador

1. Clicar no cadeado ao lado da URL
2. Ver detalhes do certificado

**Informações exibidas:**
- ✅ Emitido para quem
- ✅ Emitido por qual CA
- ✅ Validade (não expirou?)
- ✅ Algoritmo de criptografia

#### Via Linha de Comando

```bash
# Verificar certificado de um site
openssl s_client -connect exemplo.com:443 -servername exemplo.com

# Ver apenas validade
echo | openssl s_client -connect exemplo.com:443 2>/dev/null | \
  openssl x509 -noout -dates

# Saída:
notBefore=Jan  1 00:00:00 2025 GMT
notAfter=Apr  1 23:59:59 2025 GMT
```

#### Ferramentas Online

- **SSL Labs** (https://www.ssllabs.com/ssltest/)
  - Testa configuração SSL/TLS
  - Dá nota (A+, A, B, C, F)
  - Mostra vulnerabilidades
  
- **WhyNoPadlock** (https://www.whynopadlock.com/)
  - Detecta por que cadeado não aparece
  - Mostra recursos HTTP em página HTTPS

### ⚠️ Problemas Comuns SSL/TLS (Para Suporte)

#### Problema 1: "Certificado Expirado"

**Sintomas:**
- Navegador: "Sua conexão não é particular"
- Erro: `NET::ERR_CERT_DATE_INVALID`

**Causa:**
Certificado passou da data de validade.

**Como verificar:**
```bash
echo | openssl s_client -connect exemplo.com:443 2>/dev/null | \
  openssl x509 -noout -dates
  
# Se notAfter < hoje, expirou!
```

**Solução:**
1. Renovar certificado:
   ```bash
   # Let's Encrypt
   sudo certbot renew
   
   # Reiniciar servidor web
   sudo systemctl restart nginx
   ```

2. Se renovação automática falhou, verificar logs:
   ```bash
   sudo journalctl -u certbot.timer
   ```

#### Problema 2: "Erro de Nome do Certificado"

**Sintomas:**
- Erro: `NET::ERR_CERT_COMMON_NAME_INVALID`
- "O certificado não é válido para este site"

**Causa:**
Certificado foi emitido para `exemplo.com`, mas você está acessando `www.exemplo.com`.

**Solução:**
Obter certificado que cubra ambos:
```bash
sudo certbot --nginx -d exemplo.com -d www.exemplo.com
```

Ou usar certificado wildcard (`*.exemplo.com`).

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
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4a/Web.com_logo.svg/2560px-Web.com_logo.svg.png">

❌ ERRO (HTTP em página HTTPS):
<img src="https://i.ytimg.com/vi/uD6zczawkyU/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLAmmgipLedJYxC7smOKkbBJmwZgjw">
<script src="http://cdn.example.com/script.js"></script>
```

**Como encontrar:**
1. Abrir DevTools (F12)
2. Aba "Console"
3. Ver avisos de "Mixed Content"

**Solução:**
1. Mudar todos os recursos para HTTPS
2. Ou usar URLs relativas/protocol-relative:
   ```html
   <!-- Protocol-relative -->
   <img src="//exemplo.com/foto.jpg">
   <!-- Usa HTTP ou HTTPS automaticamente -->
   ```

#### Problema 4: "Autoridade Certificadora Não Confiável"

**Sintomas:**
- Erro: `NET::ERR_CERT_AUTHORITY_INVALID`
- "O certificado não foi emitido por uma autoridade confiável"

**Causas:**
1. Certificado auto-assinado
2. CA não reconhecida
3. Cadeia de certificados incompleta

**Como verificar:**
```bash
# Verificar cadeia completa
openssl s_client -connect exemplo.com:443 -showcerts
```

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

**Como verificar:**
```bash
# Testar conexão TLS
openssl s_client -connect exemplo.com:443 -tls1_2
openssl s_client -connect exemplo.com:443 -tls1_3
```

**Solução:**
1. Atualizar configuração do servidor (habilitar TLS 1.2+)
2. Configurar ciphers modernos
3. Verificar firewall (porta 443 aberta?)

### 🔧 Configurando TLS no Servidor Web

#### Nginx (Boas práticas)

```nginx
server {
    listen 443 ssl http2;
    server_name exemplo.com www.exemplo.com;
    
    # Certificado
    ssl_certificate /etc/letsencrypt/live/exemplo.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/exemplo.com/privkey.pem;
    
    # Protocolos (só TLS 1.2 e 1.3)
    ssl_protocols TLSv1.2 TLSv1.3;
    
    # Ciphers seguros
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    
    # OCSP Stapling (melhora performance)
    ssl_stapling on;
    ssl_stapling_verify on;
    
    # HSTS (força HTTPS)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    
    # Restante da configuração...
}

# Redirecionar HTTP → HTTPS
server {
    listen 80;
    server_name exemplo.com www.exemplo.com;
    return 301 https://$server_name$request_uri;
}
```

#### Apache

```apache
<VirtualHost *:443>
    ServerName exemplo.com
    ServerAlias www.exemplo.com
    
    # Certificado
    SSLCertificateFile /etc/letsencrypt/live/exemplo.com/cert.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/exemplo.com/privkey.pem
    SSLCertificateChainFile /etc/letsencrypt/live/exemplo.com/chain.pem
    
    # Protocolos
    SSLProtocol all -SSLv2 -SSLv3 -TLSv1 -TLSv1.1
    
    # Ciphers
    SSLCipherSuite HIGH:!aNULL:!MD5
    SSLHonorCipherOrder on
    
    # HSTS
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
</VirtualHost>

# Redirecionar HTTP → HTTPS
<VirtualHost *:80>
    ServerName exemplo.com
    Redirect permanent / https://exemplo.com/
</VirtualHost>
```

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

### 🏆 HSTS (HTTP Strict Transport Security)

HSTS força navegadores a SEMPRE usarem HTTPS.

**Como funciona:**

```http
# Servidor envia cabeçalho:
Strict-Transport-Security: max-age=31536000; includeSubDomains

# Navegador entende:
"Por 1 ano (31536000 segundos), SEMPRE acesse este site via HTTPS,
mesmo se o usuário digitar http://"
```

**Vantagens:**
- ✅ Protege contra downgrade attacks
- ✅ Mais rápido (não tenta HTTP antes)
- ✅ Previne ataques man-in-the-middle

**Configuração:**

```nginx
# Nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;

# Apache
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
```

### 📊 TLS Versions

| Versão | Status | Segurança | Uso |
|--------|--------|-----------|-----|
| SSL 2.0 | ❌ Obsoleto | 🔴 Inseguro | Nunca usar |
| SSL 3.0 | ❌ Obsoleto | 🔴 Inseguro | Nunca usar |
| TLS 1.0 | ⚠️ Depreciado | 🟡 Fraco | Evitar |
| TLS 1.1 | ⚠️ Depreciado | 🟡 Fraco | Evitar |
| TLS 1.2 | ✅ Atual | 🟢 Seguro | Usar |
| TLS 1.3 | ✅ Moderno | 🟢 Muito seguro | Usar |

**Recomendação:** Habilitar apenas TLS 1.2 e 1.3.

---

---

## 📚 Próximos Passos

Continue aprendendo:

➡️ **[Microserviços - Arquitetura Moderna](./07-Microservicos.md)**

---

[← Voltar ao Índice](./README.md)
