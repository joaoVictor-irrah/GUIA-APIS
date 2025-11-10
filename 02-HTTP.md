# 2️⃣ HTTP - A Linguagem da Web

[← Voltar ao Índice](./README.md)

> **Tempo de leitura:** ~20 minutos  
> **Nível:** Iniciante

---


### 🎯 O Que É HTTP?

**HTTP** significa **HyperText Transfer Protocol** (Protocolo de Transferência de Hipertexto).

**Definição simples:**
HTTP é a "língua" que navegadores e servidores usam para conversar na internet.

**Analogia:**
Imagine que você vai a um restaurante:

```
VOCÊ (Cliente)          GARÇOM (Servidor)
   │                        │
   │  "Quero um suco"      │
   ├───────────────────────>│  (HTTP REQUEST - Pedido)
   │                        │
   │  "Aqui está o suco"   │
   │<───────────────────────┤  (HTTP RESPONSE - Resposta)
   │                        │
```

O HTTP funciona assim:
- Você faz um **pedido** (request)
- O servidor dá uma **resposta** (response)

### 📨 Anatomia de uma Requisição HTTP

Uma requisição HTTP tem 4 partes principais:

```
┌────────────────────────────────────────────┐
│ 1. MÉTODO (O que você quer fazer)          │
│    GET /produtos HTTP/1.1                  │
└────────────────────────────────────────────┘
┌────────────────────────────────────────────┐
│ 2. CABEÇALHOS (Informações adicionais)     │
│    Host: www.loja.com                      │
│    User-Agent: Chrome/120.0                │
│    Accept: application/json                │
└────────────────────────────────────────────┘
┌────────────────────────────────────────────┐
│ 3. LINHA EM BRANCO                         │
│                                            │
└────────────────────────────────────────────┘
┌────────────────────────────────────────────┐
│ 4. CORPO (Dados enviados - opcional)       │
│    { "nome": "Camiseta", "preco": 50 }     │
└────────────────────────────────────────────┘
```

### 🔤 Métodos HTTP (Verbos)

Os métodos HTTP dizem o que você quer fazer. Pense neles como "verbos de ação".

| Método | Ação | Analogia | Exemplo de Uso |
|--------|------|----------|----------------|
| **GET** | Buscar/Ler | "Me mostre o cardápio" | Ver lista de produtos |
| **POST** | Criar | "Quero fazer um pedido novo" | Criar um novo usuário |
| **PUT** | Atualizar (completo) | "Quero trocar todo o pedido" | Atualizar todos os dados de um produto |
| **PATCH** | Atualizar (parcial) | "Quero mudar só a bebida" | Mudar apenas o e-mail do usuário |
| **DELETE** | Deletar | "Cancela meu pedido" | Excluir um produto |
| **HEAD** | Buscar só informações | "Esse prato existe?" | Verificar se um recurso existe |
| **OPTIONS** | Ver o que pode fazer | "O que vocês servem?" | Descobrir métodos permitidos |

**Exemplos práticos:**

```http
# Exemplo 1: Buscar todos os produtos
GET /api/produtos HTTP/1.1
Host: loja.exemplo.com

# Exemplo 2: Criar um novo produto
POST /api/produtos HTTP/1.1
Host: loja.exemplo.com
Content-Type: application/json

{
  "nome": "Notebook",
  "preco": 3000,
  "estoque": 10
}

# Exemplo 3: Atualizar preço de um produto
PATCH /api/produtos/123 HTTP/1.1
Host: loja.exemplo.com
Content-Type: application/json

{
  "preco": 2500
}

# Exemplo 4: Deletar um produto
DELETE /api/produtos/123 HTTP/1.1
Host: loja.exemplo.com
```

### 📊 Códigos de Status HTTP (Respostas)

Quando o servidor responde, ele envia um **código de status** dizendo como foi o pedido.

**Estrutura:**
```
HTTP/1.1 200 OK
   ↑     ↑   ↑
   │     │   └── Descrição
   │     └────── Código numérico
   └──────────── Versão do HTTP
```

**Categorias de códigos (primeiro dígito):**

| Faixa | Significado | Analogia |
|-------|-------------|----------|
| **1xx** | Informacional | "Estou processando seu pedido..." |
| **2xx** | Sucesso | "Tudo certo! Aqui está." |
| **3xx** | Redirecionamento | "Procure em outro lugar" |
| **4xx** | Erro do cliente | "Você fez algo errado" |
| **5xx** | Erro do servidor | "Nós falhamos, desculpe" |

#### Códigos Mais Comuns (DECORAR!)

##### ✅ 2xx - Sucesso

| Código | Nome | Significado | Quando acontece |
|--------|------|-------------|-----------------|
| **200** | OK | Sucesso total | GET que funcionou |
| **201** | Created | Criado com sucesso | POST que criou algo novo |
| **204** | No Content | Sucesso sem retornar dados | DELETE que funcionou |

##### 🔀 3xx - Redirecionamento

| Código | Nome | Significado | Quando acontece |
|--------|------|-------------|-----------------|
| **301** | Moved Permanently | Mudou para sempre | Site mudou de endereço definitivo |
| **302** | Found | Redirecionamento temporário | Site temporariamente em outro lugar |
| **304** | Not Modified | Não foi modificado | Dados em cache ainda são válidos |

##### ❌ 4xx - Erro do Cliente

| Código | Nome | Significado | Quando acontece |
|--------|------|-------------|-----------------|
| **400** | Bad Request | Pedido mal formatado | JSON inválido, faltou campo obrigatório |
| **401** | Unauthorized | Não autenticado | Precisa fazer login |
| **403** | Forbidden | Sem permissão | Logado, mas sem autorização |
| **404** | Not Found | Não encontrado | URL não existe, recurso deletado |
| **405** | Method Not Allowed | Método não permitido | Tentou DELETE em rota que só aceita GET |
| **409** | Conflict | Conflito | Tentou criar algo que já existe |
| **422** | Unprocessable Entity | Dados inválidos | Validação falhou (ex: email inválido) |
| **429** | Too Many Requests | Muitas requisições | Cliente excedeu limite de requisições |

##### 🔥 5xx - Erro do Servidor

| Código | Nome | Significado | Quando acontece |
|--------|------|-------------|-----------------|
| **500** | Internal Server Error | Erro interno | Bug no código do servidor |
| **502** | Bad Gateway | Gateway ruim | Servidor intermediário falhou |
| **503** | Service Unavailable | Serviço indisponível | Servidor sobrecarregado ou em manutenção |
| **504** | Gateway Timeout | Timeout do gateway | Servidor demorou muito para responder |

### 🗂️ Cabeçalhos HTTP (Headers)

Cabeçalhos são "informações extras" enviadas junto com o pedido ou resposta.

**Analogia:**
São como as informações no envelope de uma carta:
- Remetente (quem enviou)
- Destinatário (para quem vai)
- Tipo de conteúdo (carta, documento, pacote)
- Prioridade (urgente, comum)

#### Cabeçalhos de Requisição (Cliente → Servidor)

| Cabeçalho | O Que Faz | Exemplo |
|-----------|-----------|---------|
| **Host** | Informa qual site você quer | `Host: www.exemplo.com` |
| **User-Agent** | Identifica seu navegador/app | `User-Agent: Chrome/120.0` |
| **Accept** | Tipo de resposta que você aceita | `Accept: application/json` |
| **Content-Type** | Tipo de dado que você está enviando | `Content-Type: application/json` |
| **Authorization** | Credenciais de autenticação | `Authorization: Bearer abc123...` |
| **Cookie** | Cookies armazenados | `Cookie: sessionId=xyz789` |

#### Cabeçalhos de Resposta (Servidor → Cliente)

| Cabeçalho | O Que Faz | Exemplo |
|-----------|-----------|---------|
| **Content-Type** | Tipo de dado na resposta | `Content-Type: application/json` |
| **Content-Length** | Tamanho da resposta (bytes) | `Content-Length: 1234` |
| **Set-Cookie** | Define um cookie no navegador | `Set-Cookie: sessionId=xyz789` |
| **Cache-Control** | Como fazer cache da resposta | `Cache-Control: max-age=3600` |
| **Location** | Para onde redirecionar | `Location: /novo-endereco` |

**Exemplo completo de requisição e resposta:**

```http
# ===== REQUISIÇÃO (Cliente → Servidor) =====

POST /api/usuarios HTTP/1.1
Host: api.exemplo.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)

{
  "nome": "Maria Silva",
  "email": "maria@exemplo.com",
  "senha": "senhaSegura123"
}


# ===== RESPOSTA (Servidor → Cliente) =====

HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 156
Location: /api/usuarios/42
Set-Cookie: sessionId=abc123; HttpOnly; Secure

{
  "id": 42,
  "nome": "Maria Silva",
  "email": "maria@exemplo.com",
  "criadoEm": "2025-11-10T14:30:00Z"
}
```

### 🔐 HTTP vs HTTPS

| HTTP | HTTPS |
|------|-------|
| **H**yper**T**ext **T**ransfer **P**rotocol | **H**TTP **S**ecure |
| ❌ Sem criptografia | ✅ Com criptografia |
| 🔓 Dados trafegam "abertos" | 🔐 Dados trafegam "trancados" |
| ⚠️ Vulnerável a interceptação | ✅ Protegido contra interceptação |
| Porta padrão: **80** | Porta padrão: **443** |

**Analogia:**
- **HTTP** = Enviar um cartão postal (qualquer um pode ler)
- **HTTPS** = Enviar uma carta lacrada em envelope (só destinatário abre)

**Visual:**

```
HTTP (Inseguro):
Cliente ──[Dados visíveis]──> Hacker pode ler ──> Servidor
                               ↑ 
                          "Senha: 12345"

HTTPS (Seguro):
Cliente ──[Dados criptografados]──> Hacker vê "!@#$%^&*" ──> Servidor
                                      ↑
                                 Não consegue ler!
```

### 📡 Ciclo Completo de uma Requisição HTTP

Vamos entender o que acontece quando você acessa `https://api.loja.com/produtos`:

```
1. DNS LOOKUP
   Navegador: "Qual o IP de api.loja.com?"
   DNS: "É 203.0.113.50"
   
2. CONEXÃO TCP
   Navegador conecta no servidor 203.0.113.50:443
   
3. HANDSHAKE SSL/TLS (se HTTPS)
   Navegador e servidor trocam certificados e chaves
   
4. ENVIO DA REQUISIÇÃO HTTP
   GET /produtos HTTP/1.1
   Host: api.loja.com
   
5. PROCESSAMENTO NO SERVIDOR
   Servidor busca lista de produtos no banco de dados
   
6. RESPOSTA HTTP
   HTTP/1.1 200 OK
   Content-Type: application/json
   
   [ { "id": 1, "nome": "Notebook" }, ... ]
   
7. RENDERIZAÇÃO
   Navegador exibe os produtos na tela
```

### 🛠️ Ferramentas Para Testar HTTP

#### 1. **Navegador (DevTools)**

**Como acessar:**
- **Chrome/Edge:** F12 → Aba "Network"
- **Firefox:** F12 → Aba "Rede"

**O que você vê:**
- Todas as requisições HTTP
- Códigos de status
- Tempo de resposta
- Cabeçalhos
- Corpo da requisição/resposta

#### 2. **Postman** (Aplicativo)

Ferramenta profissional para testar APIs.

**Passo a passo:**
1. Baixar em: https://www.postman.com
2. Criar uma requisição:
   - Método: GET
   - URL: `https://api.exemplo.com/produtos`
3. Clicar em "Send"
4. Ver resposta

#### 3. **curl** (Linha de comando)

Ferramenta de terminal para fazer requisições HTTP.

```bash
# Requisição GET simples
curl https://api.exemplo.com/produtos

# Ver cabeçalhos da resposta
curl -i https://api.exemplo.com/produtos

# Requisição POST com JSON
curl -X POST https://api.exemplo.com/produtos \
  -H "Content-Type: application/json" \
  -d '{"nome":"Notebook","preco":3000}'
  
# Seguir redirecionamentos
curl -L https://exemplo.com

# Ver tempo de resposta
curl -w "Tempo: %{time_total}s\n" https://api.exemplo.com
```

#### 4. **httpie** (Mais amigável que curl)

```bash
# Instalar
pip install httpie

# GET simples
http GET https://api.exemplo.com/produtos

# POST com JSON
http POST https://api.exemplo.com/produtos nome="Notebook" preco:=3000

# Com autenticação
http GET https://api.exemplo.com/usuarios Authorization:"Bearer TOKEN123"
```

### ⚠️ Problemas Comuns HTTP (Para Suporte)

#### Problema 1: "Erro 404 - Não encontrado"

**Sintomas:**
- Cliente: "A página não existe"
- Erro: `404 Not Found`

**Causas:**
- URL digitada errado
- Recurso foi deletado
- Rota da API mudou

**Como investigar:**
```bash
# Testar se a URL existe
curl -i https://api.exemplo.com/rota-problematica

# Se retornar 404, verificar:
# 1. URL está correta?
# 2. Recurso existe no banco de dados?
# 3. Documentação da API mudou?
```

**Soluções:**
1. Corrigir a URL
2. Verificar se recurso foi deletado
3. Consultar documentação atualizada da API

#### Problema 2: "Erro 401 - Não autorizado"

**Sintomas:**
- Cliente: "Diz que não estou autorizado"
- Erro: `401 Unauthorized`

**Causas:**
- Não enviou token de autenticação
- Token expirou
- Token inválido

**Como investigar:**
```bash
# Testar sem token
curl https://api.exemplo.com/usuarios
# Deve retornar 401

# Testar com token
curl https://api.exemplo.com/usuarios \
  -H "Authorization: Bearer SEU_TOKEN"
```

**Soluções:**
1. Fazer login novamente (obter novo token)
2. Verificar se token está sendo enviado corretamente
3. Verificar validade do token

#### Problema 3: "Erro 500 - Erro interno"

**Sintomas:**
- Cliente: "Algo deu errado no servidor"
- Erro: `500 Internal Server Error`

**Causas:**
- Bug no código do servidor
- Banco de dados fora do ar
- Dependência externa falhou

**Como investigar:**
1. Ver logs do servidor (se tiver acesso)
2. Reproduzir o erro
3. Verificar se o problema é constante ou intermitente

**Soluções:**
1. Reportar ao time de desenvolvimento com detalhes:
   - URL que causou o erro
   - Método HTTP usado
   - Dados enviados
   - Horário do erro
2. Verificar status de dependências externas
3. Se for bug, escalar para dev corrigir

#### Problema 4: "Request Timeout"

**Sintomas:**
- Cliente: "A requisição demora muito e não responde"
- Erro: Timeout ou `504 Gateway Timeout`

**Causas:**
- Servidor sobrecarregado
- Query de banco de dados lenta
- Rede instável
- Processamento pesado

**Como investigar:**
```bash
# Testar tempo de resposta
curl -w "Tempo total: %{time_total}s\n" \
  -o /dev/null \
  https://api.exemplo.com/endpoint-lento
```

**Soluções:**
1. Se > 30 segundos: problema de performance no servidor
2. Verificar se há manutenção programada
3. Escalar para time de infra/dev investigar

### ✅ Checklist de Troubleshooting HTTP

- [ ] **Passo 1:** Qual o código de status HTTP? (200, 404, 500, etc.)
- [ ] **Passo 2:** Qual o método usado? (GET, POST, PUT, DELETE)
- [ ] **Passo 3:** A URL está correta?
- [ ] **Passo 4:** Há autenticação? Token está válido?
- [ ] **Passo 5:** O corpo da requisição está correto? (JSON válido)
- [ ] **Passo 6:** Cabeçalhos necessários foram enviados?
- [ ] **Passo 7:** O problema é intermitente ou constante?
- [ ] **Passo 8:** Consegue reproduzir em Postman/curl?

---

---

## 📚 Próximos Passos

Continue aprendendo:

➡️ **[WebSocket - Conversas em Tempo Real](./03-WebSocket.md)**

---

[← Voltar ao Índice](./README.md)
