# 4️⃣ RESTful API - Construindo APIs Inteligentes

[← Voltar ao Índice](./README.md)

> **Tempo de leitura:** ~25 minutos  
> **Nível:** Intermediário

---


### 🎯 O Que É uma API REST?

**API** = **A**pplication **P**rogramming **I**nterface (Interface de Programação de Aplicações)

**REST** = **RE**presentational **S**tate **T**ransfer

**Definição simples:**
Uma API REST é um conjunto de regras para criar serviços web que sistemas diferentes possam usar para conversar entre si.

**Analogia:**
Imagine um restaurante:

- **Cardápio** = API (lista do que você pode pedir)
- **Garçom** = Interface (quem recebe e entrega seus pedidos)
- **Cozinha** = Servidor/Backend (processa os pedidos)
- **Você** = Cliente (aplicativo que usa a API)

```
VOCÊ               GARÇOM (API)          COZINHA (Servidor)
"Quero um suco" ────────────────────────> [prepara suco]
                 <──────────────────────── [suco pronto]
"Aqui está"
```

### 🏛️ Princípios REST

Uma API para ser considerada RESTful deve seguir 6 princípios:

#### 1. **Cliente-Servidor (Client-Server)**

Cliente e servidor são independentes. Um não precisa conhecer os detalhes internos do outro.

```
Cliente (App Mobile)          Servidor (API)
Não sabe como dados          Não sabe como cliente
são armazenados              vai exibir os dados
      │                            │
      └─────────[ HTTP ]───────────┘
           Interface padronizada
```

#### 2. **Stateless (Sem Estado)**

Cada requisição é independente. O servidor não "lembra" de requisições anteriores.

**Analogia:**
Cada vez que você liga para o SAC, precisa dizer seu nome, CPF e problema novamente - eles não lembram de você.

```
# Requisição 1
GET /produtos/123
Authorization: Bearer TOKEN_ABC

# Requisição 2 (servidor não lembra da 1)
GET /produtos/456
Authorization: Bearer TOKEN_ABC  ← Precisa enviar token de novo!
```

**Vantagem:** Escalável (servidor não precisa guardar estado de milhões de clientes)

#### 3. **Cacheable (Pode fazer Cache)**

Respostas devem dizer se podem ser armazenadas (cache) para uso futuro.

```http
HTTP/1.1 200 OK
Cache-Control: max-age=3600  ← Pode guardar por 1 hora
Content-Type: application/json

{ "produto": "Notebook" }
```

**Vantagem:** Menos requisições = mais rápido + menos carga no servidor

#### 4. **Interface Uniforme**

Usar padrões consistentes:
- URLs claras e padronizadas
- Métodos HTTP corretos (GET, POST, PUT, DELETE)
- Respostas previsíveis

```
✅ BOM (Padrão REST):
GET    /produtos       → Lista produtos
GET    /produtos/123   → Busca produto 123
POST   /produtos       → Cria produto
PUT    /produtos/123   → Atualiza produto 123
DELETE /produtos/123   → Deleta produto 123

❌ RUIM (Não segue padrão):
GET /getProdutos
GET /buscarProdutoPorId?id=123
POST /criarNovoProduto
```

#### 5. **Sistema em Camadas**

Cliente não sabe se está falando direto com o servidor ou com intermediários (proxy, load balancer, cache).

```
Cliente → [ CDN ] → [ Load Balancer ] → [ API Server ] → [ Database ]
         Cache      Distribuidor         Processador      Armazenamento
```

**Vantagem:** Pode adicionar segurança, cache, balanceamento sem cliente saber

#### 6. **Código sob Demanda (opcional)**

Servidor pode enviar código executável para o cliente (ex: JavaScript).

**Exemplo:** API retorna HTML com JavaScript que o navegador executa.

### 🗺️ Estrutura de URLs REST

URLs REST devem ser **substantivos** (não verbos) e seguir hierarquia lógica.

#### ✅ Boas Práticas

| URL | Método | Ação | Descrição |
|-----|--------|------|-----------|
| `/produtos` | GET | Listar | Busca todos os produtos |
| `/produtos` | POST | Criar | Cria um novo produto |
| `/produtos/{id}` | GET | Buscar | Busca produto específico |
| `/produtos/{id}` | PUT | Atualizar | Atualiza produto específico |
| `/produtos/{id}` | PATCH | Atualizar parcial | Atualiza campos específicos |
| `/produtos/{id}` | DELETE | Deletar | Remove produto |

**Recursos aninhados (hierarquia):**

```
GET    /usuarios/42/pedidos        → Pedidos do usuário 42
GET    /usuarios/42/pedidos/7      → Pedido 7 do usuário 42
POST   /usuarios/42/pedidos        → Criar pedido para usuário 42
DELETE /usuarios/42/pedidos/7      → Deletar pedido 7 do usuário 42
```

#### ❌ Más Práticas (Evitar!)

| ❌ Errado | Por Quê | ✅ Correto |
|----------|---------|-----------|
| `/getProdutos` | Verbo na URL | `/produtos` com GET |
| `/produto/deletar/123` | Verbo na URL | `/produtos/123` com DELETE |
| `/api/v1/ObterTodosProdutos` | Longo e com verbo | `/produtos` com GET |
| `/produtos?acao=deletar&id=123` | Ação em query param | `/produtos/123` com DELETE |
| `/produtos/123/deleteProduto` | Redundante | `/produtos/123` com DELETE |

### 📦 Formato de Dados: JSON

APIs REST modernas usam **JSON** (JavaScript Object Notation) para trocas de dados.

**Por quê JSON?**
- ✅ Leve (menos bytes que XML)
- ✅ Fácil de ler por humanos
- ✅ Suportado por todas as linguagens
- ✅ Estrutura clara (chave-valor)

**Exemplo de JSON:**

```json
{
  "id": 123,
  "nome": "Notebook Dell",
  "preco": 3500.00,
  "estoque": 15,
  "disponivel": true,
  "categorias": ["Eletrônicos", "Informática"],
  "fabricante": {
    "nome": "Dell Inc.",
    "pais": "EUA"
  },
  "avaliacoes": [
    { "usuario": "João", "nota": 5, "comentario": "Excelente!" },
    { "usuario": "Maria", "nota": 4, "comentario": "Bom custo-benefício" }
  ]
}
```

**Tipos de dados em JSON:**
- `"texto"` - String (entre aspas)
- `123` ou `45.67` - Número
- `true` ou `false` - Booleano
- `null` - Nulo (vazio)
- `[ ]` - Array (lista)
- `{ }` - Objeto (chave-valor)

### 🎨 Design de API RESTful

#### Versionamento

Sempre versione sua API para evitar quebrar clientes antigos quando fizer mudanças.

**Opções:**

| Método | Exemplo | Prós | Contras |
|--------|---------|------|---------|
| **URL** | `/api/v1/produtos` | Claro, fácil de usar | Muda URL |
| **Header** | `Accept: application/vnd.api+json;version=1` | URL limpa | Menos óbvio |
| **Query** | `/produtos?version=1` | Flexível | Pode ser ignorado |

**✅ Recomendado:** Versão na URL (`/v1/`, `/v2/`)

```
GET https://api.exemplo.com/v1/produtos
GET https://api.exemplo.com/v2/produtos  ← Nova versão
```

#### Paginação

Não retorne milhares de registros de uma vez!

**Padrão - Query Parameters:**

```http
GET /produtos?page=2&limit=20
```

```json
{
  "dados": [
    { "id": 21, "nome": "Produto 21" },
    { "id": 22, "nome": "Produto 22" },
    ...
    { "id": 40, "nome": "Produto 40" }
  ],
  "paginacao": {
    "paginaAtual": 2,
    "itensPorPagina": 20,
    "totalItens": 500,
    "totalPaginas": 25,
    "proximaPagina": "/produtos?page=3&limit=20",
    "paginaAnterior": "/produtos?page=1&limit=20"
  }
}
```

#### Filtros e Busca

Permitir filtrar resultados via query parameters:

```http
# Filtro simples
GET /produtos?categoria=eletronicos

# Múltiplos filtros
GET /produtos?categoria=eletronicos&precoMin=100&precoMax=1000

# Ordenação
GET /produtos?orderBy=preco&order=desc

# Busca por texto
GET /produtos?q=notebook

# Combinado
GET /produtos?categoria=eletronicos&q=dell&orderBy=preco&page=1&limit=20
```

#### Tratamento de Erros

Sempre retorne erros de forma clara e consistente.

**Estrutura de erro padrão:**

```json
{
  "erro": {
    "codigo": "PRODUTO_NAO_ENCONTRADO",
    "mensagem": "O produto com ID 999 não foi encontrado",
    "status": 404,
    "timestamp": "2025-11-10T14:30:00Z",
    "caminho": "/api/v1/produtos/999",
    "detalhes": [
      {
        "campo": "id",
        "mensagem": "ID 999 não existe no banco de dados"
      }
    ]
  }
}
```

**Erros de validação:**

```http
POST /usuarios
{
  "nome": "",
  "email": "email-invalido",
  "idade": 15
}
```

```json
HTTP/1.1 422 Unprocessable Entity

{
  "erro": {
    "codigo": "VALIDACAO_FALHOU",
    "mensagem": "Dados inválidos",
    "status": 422,
    "detalhes": [
      {
        "campo": "nome",
        "mensagem": "Nome não pode ser vazio"
      },
      {
        "campo": "email",
        "mensagem": "E-mail inválido"
      },
      {
        "campo": "idade",
        "mensagem": "Idade mínima é 18 anos"
      }
    ]
  }
}
```

### 🔐 Autenticação em APIs REST

APIs precisam saber **quem** está fazendo a requisição.

#### 1. **API Key** (Mais simples)

Cliente envia uma chave única:

```http
GET /produtos
X-API-Key: sk_live_abc123def456ghi789
```

**Prós:** Simples
**Contras:** Se vazar, comprometeu tudo

#### 2. **Bearer Token (JWT)**

Token temporário que expira:

```http
GET /usuarios/me
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Fluxo:**
```
1. Cliente faz login
   POST /auth/login
   { "email": "user@exemplo.com", "senha": "***" }
   
2. Servidor retorna token
   { "token": "eyJhbG..." }
   
3. Cliente usa token nas próximas requisições
   GET /usuarios/me
   Authorization: Bearer eyJhbG...
```

**Prós:** Token expira (mais seguro)
**Contras:** Precisa renovar quando expira

#### 3. **OAuth 2.0** (Mais complexo, mais seguro)

Usado quando você acessa serviços de terceiros (Google, Facebook, GitHub).

**Exemplo:**
"Entrar com Google" no seu app.

### 📝 Documentação de API

Documentação clara é essencial! Use ferramentas:

#### **Swagger/OpenAPI**

Padrão da indústria para documentar APIs REST.

**Exemplo visual:**
```
GET /produtos/{id}

Busca um produto por ID

Parameters:
  - id (path, required): ID do produto

Responses:
  200 - Sucesso
    {
      "id": 123,
      "nome": "Notebook"
    }
  404 - Não encontrado
    {
      "erro": "Produto não encontrado"
    }
```

**Ferramentas:**
- **Swagger UI** - Interface visual para testar
- **Postman** - Coleções com documentação
- **ReadMe.io** - Documentação hospedada

### ⚠️ Problemas Comuns em APIs REST (Para Suporte)

#### Problema 1: "Erro 400 - Bad Request"

**Sintomas:**
- Cliente: "A requisição não funciona"
- Erro: `400 Bad Request`

**Causas:**
1. JSON mal formatado
2. Faltou campo obrigatório
3. Tipo de dado errado (enviou texto onde deveria ser número)

**Exemplo de JSON inválido:**

```json
// ❌ ERRO: Faltou fechar chave
{
  "nome": "Produto",
  "preco": 100

// ❌ ERRO: Vírgula no último item
{
  "nome": "Produto",
  "preco": 100,
}

// ✅ CORRETO
{
  "nome": "Produto",
  "preco": 100
}
```

**Como ajudar:**
1. Validar JSON em: https://jsonlint.com
2. Verificar documentação da API (campos obrigatórios)
3. Ver detalhes do erro retornado pelo servidor

#### Problema 2: "Erro 422 - Unprocessable Entity"

**Sintomas:**
- JSON válido, mas dados não passam nas validações
- Erro: `422 Unprocessable Entity`

**Exemplos:**
```json
// E-mail inválido
{ "email": "naoéumemail" }

// Idade negativa
{ "idade": -5 }

// Preço zero
{ "preco": 0 }
```

**Como ajudar:**
1. Ver campo `detalhes` na resposta de erro
2. Corrigir cada campo indicado
3. Consultar regras de validação na documentação

#### Problema 3: "API lenta"

**Sintomas:**
- Requisições demoram muito (> 5 segundos)
- Timeout

**Causas:**
1. Busca muitos dados sem paginação
2. Query no banco lenta
3. API de terceiro lenta
4. Servidor sobrecarregado

**Como investigar:**
```bash
# Medir tempo de resposta
curl -w "Tempo: %{time_total}s\n" \
  https://api.exemplo.com/produtos
```

**Soluções:**
1. Usar paginação: `/produtos?limit=20`
2. Filtrar dados desnecessários
3. Verificar status do servidor
4. Escalar para time de dev/infra

#### Problema 4: "Dados não atualizam"

**Sintomas:**
- Cliente mudou dados, mas continua vendo valores antigos

**Causas:**
1. **Cache:** Resposta antiga foi guardada
2. **Problema de sincronia:** Banco de dados replicado não atualizou
3. **Browser cache:** Navegador guardou resposta

**Como investigar:**
```bash
# Forçar busca sem cache
curl -H "Cache-Control: no-cache" \
  https://api.exemplo.com/produtos/123
```

**Soluções:**
1. Limpar cache do navegador (Ctrl+Shift+R)
2. Verificar cabeçalho `Cache-Control` na resposta
3. Aguardar sincronização de banco (se aplicável)

### ✅ Checklist API RESTful Bem Projetada

- [ ] URLs usam substantivos (não verbos)
- [ ] Métodos HTTP corretos (GET, POST, PUT, DELETE)
- [ ] Versionamento implementado (/v1/, /v2/)
- [ ] Paginação para listas grandes
- [ ] Filtros e busca disponíveis
- [ ] Erros retornam JSON estruturado
- [ ] Autenticação implementada
- [ ] Documentação clara (Swagger)
- [ ] Suporta CORS (se API pública)
- [ ] HTTPS habilitado (não HTTP puro)
- [ ] Rate limiting (limite de requisições)
- [ ] Logs de requisições

### 📚 Exemplo Completo de API REST

**Recurso:** Gerenciamento de Pedidos

```http
# 1. Listar todos os pedidos (com paginação)
GET /api/v1/pedidos?page=1&limit=20
Authorization: Bearer TOKEN

Response: 200 OK
{
  "dados": [ ... lista de pedidos ... ],
  "paginacao": { ... }
}


# 2. Buscar pedido específico
GET /api/v1/pedidos/42
Authorization: Bearer TOKEN

Response: 200 OK
{
  "id": 42,
  "cliente": "João Silva",
  "total": 350.00,
  "status": "ENTREGUE",
  "itens": [
    { "produto": "Camiseta", "quantidade": 2, "preco": 50.00 },
    { "produto": "Calça", "quantidade": 1, "preco": 250.00 }
  ]
}


# 3. Criar novo pedido
POST /api/v1/pedidos
Authorization: Bearer TOKEN
Content-Type: application/json

{
  "clienteId": 123,
  "itens": [
    { "produtoId": 456, "quantidade": 2 },
    { "produtoId": 789, "quantidade": 1 }
  ]
}

Response: 201 Created
Location: /api/v1/pedidos/43
{
  "id": 43,
  "cliente": "João Silva",
  "total": 300.00,
  "status": "PENDENTE",
  "criadoEm": "2025-11-10T14:30:00Z"
}


# 4. Atualizar status do pedido
PATCH /api/v1/pedidos/43
Authorization: Bearer TOKEN
Content-Type: application/json

{
  "status": "ENVIADO"
}

Response: 200 OK
{
  "id": 43,
  "status": "ENVIADO",
  "atualizadoEm": "2025-11-10T15:00:00Z"
}


# 5. Cancelar pedido
DELETE /api/v1/pedidos/43
Authorization: Bearer TOKEN

Response: 204 No Content
```

---

---

## 📚 Próximos Passos

Continue aprendendo:

➡️ **[Webhooks - Notificações Automáticas](./05-Webhooks.md)**

---

[← Voltar ao Índice](./README.md)
