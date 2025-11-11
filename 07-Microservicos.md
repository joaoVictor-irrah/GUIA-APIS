# 7️⃣ Microserviços - Arquitetura Moderna

[← Voltar ao Índice](./README.md)

> **Tempo de leitura:** ~18 minutos  
> **Nível:** Avançado

---


### 🎯 O Que São Microserviços?

**Microserviços** = Arquitetura onde uma aplicação é dividida em **pequenos serviços independentes**, cada um com uma responsabilidade específica.

**Analogia:**

**Aplicação Monolítica (Antiga):**
```
Imagine um canivete suíço gigante com TODAS as ferramentas:
- Faca
- Abridor de garrafas
- Chave de fenda
- Tesoura
- Lanterna
- ...tudo em UM ÚNICO objeto

Problema: Se quebrar, TUDO para de funcionar.
```

**Microserviços (Moderna):**
```
Imagine uma caixa de ferramentas com ferramentas SEPARADAS:
- Faca separada
- Abridor separado
- Chave de fenda separada
- Tesoura separada
- Lanterna separada

Vantagem: Se uma quebra, as outras continuam funcionando.
```

### 🏗️ Monolito vs Microserviços

#### Aplicação Monolítica

```
┌────────────────────────────────────────────┐
│          APLICAÇÃO MONOLÍTICA               │
│                                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐ │
│  │ Usuários │  │ Produtos │  │ Pedidos  │ │
│  └──────────┘  └──────────┘  └──────────┘ │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐ │
│  │Pagamento │  │ Estoque  │  │Notificação││
│  └──────────┘  └──────────┘  └──────────┘ │
│                                             │
│         TUDO em 1 aplicação                 │
│         1 banco de dados                    │
│         1 deploy                            │
└────────────────────────────────────────────┘
```

**Características:**
- ✅ Simples de desenvolver inicialmente
- ✅ Fácil de testar localmente
- ✅ 1 único deploy
- ❌ Difícil de escalar partes específicas
- ❌ Se uma parte trava, TUDO trava
- ❌ Mudanças pequenas exigem redeploy de tudo
- ❌ Código cresce e fica complexo

#### Arquitetura de Microserviços

```
┌─────────────┐  ┌──────────────┐  ┌─────────────┐
│   Serviço   │  │   Serviço    │  │  Serviço    │
│  Usuários   │  │  Produtos    │  │  Pedidos    │
│             │  │              │  │             │
│ API própria │  │ API própria  │  │ API própria │
│ BD próprio  │  │ BD próprio   │  │ BD próprio  │
└─────────────┘  └──────────────┘  └─────────────┘
       │                │                  │
       └────────────────┼──────────────────┘
                        │
                  ┌─────────┐
                  │ API     │
                  │ Gateway │
                  └─────────┘
                        │
                  ┌─────────┐
                  │ Cliente │
                  └─────────┘
```

**Características:**
- ✅ Cada serviço é independente
- ✅ Pode usar tecnologias diferentes
- ✅ Escala apenas o que precisa
- ✅ Falha isolada (um serviço não derruba tudo)
- ✅ Times autônomos (cada time cuida de seu serviço)
- ❌ Mais complexo de gerenciar
- ❌ Requer infraestrutura robusta
- ❌ Testes integrados mais difíceis
- ❌ Monitoramento distribuído necessário

### 🧩 Princípios de Microserviços

#### 1. **Single Responsibility (Uma Responsabilidade)**

Cada microserviço faz UMA coisa bem feita.

```
✅ BOM (cada um tem 1 responsabilidade):
- Serviço de Autenticação (só login/logout)
- Serviço de Produtos (só CRUD de produtos)
- Serviço de Pedidos (só gerencia pedidos)

❌ RUIM (responsabilidades misturadas):
- Serviço de Produtos que também gerencia pedidos
- Serviço de Usuários que também envia e-mails
```

#### 2. **Autonomia**

Cada serviço:
- Tem seu próprio banco de dados
- Pode ser desenvolvido por time diferente
- Pode usar linguagem/framework diferente
- Pode ser deployado independentemente

```
Serviço Usuários:
- Python + Django
- PostgreSQL
- Deploy às 10h

Serviço Produtos:
- Node.js + Express
- MongoDB
- Deploy às 15h

Serviço Pagamento:
- Java + Spring Boot
- MySQL
- Deploy quando necessário
```

#### 3. **Comunicação via API**

Microserviços conversam entre si via APIs (HTTP/REST, gRPC, mensagens).

```
Serviço Pedidos precisa de dados de Produto:

❌ ERRADO (acesso direto ao banco):
SELECT * FROM produtos_database.produtos WHERE id = 123

✅ CORRETO (via API):
GET https://servico-produtos/api/produtos/123
```

#### 4. **Falha Independente**

Se um serviço cai, os outros continuam funcionando (com graceful degradation).

```
Cenário:
- Serviço de Recomendações CAIU
- Serviço de Produtos continua funcionando
- Site mostra produtos, mas sem recomendações

💡 Site NÃO sai do ar completamente!
```

#### 5. **Monitoramento e Logs Centralizados**

Com muitos serviços, é essencial ter visibilidade.

```
Ferramentas:
- Logs centralizados: ELK Stack (Elasticsearch, Logstash, Kibana)
- Métricas: Prometheus + Grafana
- Tracing distribuído: Jaeger, Zipkin
```

### 🌐 Comunicação Entre Microserviços

#### 1. **Síncrona (REST/HTTP)**

Um serviço chama API de outro e **espera** resposta.

```
Serviço Pedidos ────[GET /produtos/123]────> Serviço Produtos
                <───[ { "nome": "..." } ]──── (responde)
```

**Quando usar:**
- Precisa da resposta imediatamente
- Operações de leitura (GET)
- Validações síncronas

**Ferramentas:**
- HTTP/REST
- gRPC (mais rápido, binary)
- GraphQL

**Problema:**
Se Serviço Produtos cair, Pedidos também falha (acoplamento temporal).

#### 2. **Assíncrona (Mensageria)**

Um serviço publica mensagem em fila e **não espera** resposta.

```
Serviço Pedidos ──[envia: "pedido_criado"]──> Fila (Kafka/RabbitMQ)
                                               
Serviço Estoque ←─[consome mensagem]──────── Fila
Serviço Email   ←─[consome mensagem]──────── Fila
```

**Quando usar:**
- Não precisa de resposta imediata
- Processamento em background
- Desacoplamento (serviços não se conhecem)
- Alta disponibilidade

**Ferramentas:**
- **RabbitMQ** (filas)
- **Apache Kafka** (streaming)
- **Redis Streams**
- **AWS SQS/SNS**

**Vantagem:**
Se Serviço Estoque cair, mensagem fica na fila até ele voltar.

#### 3. **Event-Driven (Eventos)**

Serviços reagem a eventos que ocorrem no sistema.

```
Pedido Criado ──> [ Evento: "PEDIDO_CRIADO" ]
                         │
        ┌────────────────┼────────────────┐
        │                │                │
   [Estoque]        [Email]         [Pagamento]
   (reserva)      (envia conf)    (cobra cartão)
```

**Exemplo prático:**

```
1. Usuário cria pedido
2. Serviço Pedidos publica evento: "PEDIDO_CRIADO"
3. Múltiplos serviços reagem:
   - Estoque: Reserva produtos
   - Email: Envia confirmação
   - Pagamento: Processa cobrança
   - Analytics: Registra métrica
```

**Vantagens:**
- ✅ Desacoplamento total
- ✅ Fácil adicionar novos serviços (só escuta evento)
- ✅ Escalável

**Desvantagem:**
- ❌ Mais complexo de debugar
- ❌ Consistência eventual (não imediata)

### 🛠️ Ferramentas do Ecossistema de Microserviços

#### 1. **API Gateway**

Ponto de entrada único para todos os microserviços.

```
Cliente (App/Web)
       │
       ▼
┌─────────────┐
│ API Gateway │  ← Roteamento, autenticação, rate limiting
└─────────────┘
       │
   ────┼────────────────────────
   │   │              │
   ▼   ▼              ▼
[Users][Products][Orders]
```

**Responsabilidades:**
- 🔀 Roteamento (`/users/*` → Serviço Users)
- 🔐 Autenticação (JWT, OAuth)
- 🚦 Rate Limiting (máx 100 req/min)
- 📊 Logging e métricas
- 🔄 Load balancing

**Ferramentas:**
- **Kong**
- **AWS API Gateway**
- **Nginx** (como proxy reverso)
- **Envoy**
- **Traefik**

#### 2. **Load Balancer**

Distribui requisições entre múltiplas instâncias.

```
Requisições
     │
     ▼
┌─────────────┐
│Load Balancer│  ← Distribui carga
└─────────────┘
     │
 ────┼─────────
 │   │       │
 ▼   ▼       ▼
[P1][P2]   [P3]  ← 3 instâncias do serviço Produtos
```

**Estratégias:**
- **Round Robin:** Reveza entre instâncias (1, 2, 3, 1, 2, 3...)
- **Least Connections:** Envia para instância com menos conexões
- **IP Hash:** Mesmo cliente sempre na mesma instância

**Ferramentas:**
- **Nginx**
- **HAProxy**
- **AWS ELB/ALB**
- **Traefik**

#### 3. **Circuit Breaker**

Previne cascata de falhas quando um serviço cai.

**Analogia:** Disjuntor de casa (se curto-circuito, desliga para proteger).

**Estados:**

```
1. CLOSED (Normal):
   Requisições passam normalmente
   
2. OPEN (Falha detectada):
   Requisições são bloqueadas
   Retorna erro imediatamente (não tenta chamar)
   
3. HALF-OPEN (Testando):
   Após timeout, tenta 1 requisição
   Se sucesso: volta CLOSED
   Se falha: volta OPEN
```

**Visual:**

```
Serviço A → Serviço B (funcionando)
           ✅ Sucesso, ✅ Sucesso, ✅ Sucesso

Serviço A → Serviço B (começou a falhar)
           ❌ Falha, ❌ Falha, ❌ Falha (3 vezes)
           
Circuit Breaker: ABRE (para de tentar)

Serviço A → [Circuit Breaker OPEN]
           ⚡ Retorna erro imediatamente
           (não sobrecarrega Serviço B)

... após 30 segundos ...

Circuit Breaker: HALF-OPEN (testa)
Serviço A → Serviço B
           ✅ Sucesso!
           
Circuit Breaker: FECHA (volta ao normal)
```

**Ferramentas:**
- **Hystrix** (Netflix - depreciado)
- **Resilience4j** (moderna)
- **Polly** (.NET)
- **Istio** (service mesh)

#### 4. **Mensageria**

Para comunicação assíncrona.

**RabbitMQ (Filas):**

```
[Produtor] ──> [Fila: pedidos_criados] ──> [Consumidor]

Características:
- Mensagens processadas 1x
- Ordem garantida (FIFO)
- Ideal para tarefas (job queue)
```

**Apache Kafka (Streaming):**

```
[Produtor] ──> [Tópico: eventos_pedidos] ──> [Consumidor 1]
                                         ──> [Consumidor 2]
                                         ──> [Consumidor 3]

Características:
- Mensagens persistidas (não deletadas após consumo)
- Múltiplos consumidores (broadcast)
- Alta throughput
- Ideal para event sourcing
```

#### 5. **Containers e Orquestração**

**Docker:**
Empacota cada microserviço com suas dependências.

```
┌───────────────────────┐
│  Container (Serviço)  │
│  ┌─────────────────┐  │
│  │  Aplicação      │  │
│  ├─────────────────┤  │
│  │  Node.js        │  │
│  ├─────────────────┤  │
│  │  Bibliotecas    │  │
│  ├─────────────────┤  │
│  │  OS Mínimo      │  │
│  └─────────────────┘  │
└───────────────────────┘

Vantagens:
- Portável (roda em qualquer lugar)
- Isolado (não conflita com outros serviços)
- Leve (compartilha kernel do host)
```

**Kubernetes (K8s):**
Orquestra milhares de containers.

```
Kubernetes Cluster:
┌─────────────────────────────────┐
│  Master Node (Controle)         │
│  - Scheduler (onde deployar)    │
│  - API Server                   │
└─────────────────────────────────┘
         │
    ─────┼─────────────
    │    │           │
┌───────┐ ┌───────┐ ┌───────┐
│Worker1│ │Worker2│ │Worker3│
│       │ │       │ │       │
│[Pod]  │ │[Pod]  │ │[Pod]  │
│[Pod]  │ │[Pod]  │ │[Pod]  │
└───────┘ └───────┘ └───────┘

Pod = 1+ containers
```

**Recursos do K8s:**
- ✅ Auto-scaling (aumenta/diminui instâncias automaticamente)
- ✅ Self-healing (reinicia containers que falham)
- ✅ Load balancing
- ✅ Rolling updates (atualiza sem downtime)
- ✅ Service discovery

### 📊 Padrões de Microserviços

#### 1. **Database per Service**

Cada serviço tem seu próprio banco de dados.

```
✅ CORRETO:
┌──────────┐          ┌──────────┐
│ Serviço  │          │ Serviço  │
│ Usuários │          │ Pedidos  │
└────┬─────┘          └────┬─────┘
     │                     │
┌────▼────┐          ┌────▼─────┐
│  BD     │          │  BD      │
│ Usuários│          │ Pedidos  │
└─────────┘          └──────────┘

❌ ERRADO (banco compartilhado):
┌──────────┐  ┌──────────┐
│ Serviço  │  │ Serviço  │
│ Usuários │  │ Pedidos  │
└────┬─────┘  └────┬─────┘
     └───────┬─────┘
         ┌───▼────┐
         │   BD   │
         │Compartilhado│
         └────────┘
```

**Vantagem:** Isolamento total
**Desvantagem:** Joins entre tabelas de serviços diferentes não são possíveis

#### 2. **API Composition**

Quando precisar de dados de múltiplos serviços, um serviço "compositor" busca de todos e junta.

```
Cliente pede: "Detalhes do pedido 123 com dados do cliente"

API Gateway (ou serviço compositor):
1. GET /pedidos/123        → { "clienteId": 42, "total": 350 }
2. GET /clientes/42        → { "nome": "João", "email": "..." }
3. Junta os dados:
   {
     "pedido": { "id": 123, "total": 350 },
     "cliente": { "nome": "João", "email": "..." }
   }
4. Retorna ao cliente
```

#### 3. **Saga Pattern**

Para transações distribuídas (que envolvem múltiplos serviços).

**Problema:**
```
Criar pedido requer:
1. Reservar produtos (Serviço Estoque)
2. Cobrar cartão (Serviço Pagamento)
3. Criar pedido (Serviço Pedidos)

Se passo 2 falhar, como desfazer passo 1?
```

**Solução - Saga Coreografada (eventos):**

```
1. Serviço Pedidos: Publica "PEDIDO_INICIADO"
2. Serviço Estoque: Escuta, reserva produtos
   - Se sucesso: Publica "ESTOQUE_RESERVADO"
   - Se falha: Publica "ESTOQUE_FALHOU"
   
3. Serviço Pagamento: Escuta "ESTOQUE_RESERVADO"
   - Tenta cobrar cartão
   - Se sucesso: Publica "PAGAMENTO_APROVADO"
   - Se falha: Publica "PAGAMENTO_FALHOU"
   
4. Se "PAGAMENTO_FALHOU":
   Serviço Estoque escuta e FAZ ROLLBACK (libera produtos)
   
5. Se "PAGAMENTO_APROVADO":
   Serviço Pedidos confirma pedido
```

**Solução - Saga Orquestrada (coordenador):**

```
Orquestrador de Saga:
1. Chama Estoque: Reservar produtos
   ✅ OK
2. Chama Pagamento: Cobrar cartão
   ❌ FALHA
3. Compensa: Chama Estoque: Liberar produtos
4. Retorna erro ao cliente
```

#### 4. **CQRS (Command Query Responsibility Segregation)**

Separar operações de **escrita** (commands) e **leitura** (queries).

```
ESCRITA (Commands):
Cliente → [Serviço Write] → [BD Write]
                           ↓
                      [Publica evento]
                           ↓
LEITURA (Queries):    [BD Read] ← [Serviço Read] ← Cliente
```

**Vantagens:**
- Escrita e leitura com modelos diferentes
- Escala separadamente (mais leituras que escritas)
- Otimizações específicas

### ✅ Quando Usar Microserviços?

**✅ USE microserviços quando:**
- Aplicação grande e complexa
- Times múltiplos trabalhando paralelamente
- Precisa escalar partes específicas
- Tecnologias diferentes para partes diferentes
- Deploys independentes são valiosos

**❌ NÃO USE microserviços quando:**
- Aplicação pequena (<3 desenvolvedores)
- Time iniciante (sem experiência com distribuição)
- Infraestrutura limitada
- Latência crítica (requisitos sub-10ms)

**Regra prática:**
> "Comece com monolito, migre para microserviços quando a dor justificar a complexidade."

### 📚 Exemplo: E-commerce em Microserviços

```
┌─────────────────────────────────────────────────────┐
│                  API GATEWAY                        │
│  https://api.loja.com                               │
└──────────────────┬──────────────────────────────────┘
                   │
      ┌────────────┼────────────┐
      │            │            │
┌─────▼────┐  ┌───▼─────┐  ┌──▼──────┐
│Serviço   │  │Serviço  │  │Serviço  │
│Usuários  │  │Produtos │  │Pedidos  │
│          │  │         │  │         │
│Login     │  │Catálogo │  │Carrinho │
│Cadastro  │  │Busca    │  │Checkout │
│Perfil    │  │Estoque  │  │Histórico│
│          │  │         │  │         │
│PostgreSQL│  │MongoDB  │  │MySQL    │
└──────────┘  └─────────┘  └───┬─────┘
                                │
                ┌───────────────┴─────────────┐
                │                             │
         ┌──────▼───────┐            ┌───────▼────────┐
         │Serviço       │            │Serviço         │
         │Pagamento     │            │Notificação     │
         │              │            │                │
         │Stripe API    │            │Email/SMS       │
         │Boleto        │            │Push            │
         │PIX           │            │                │
         └──────────────┘            └────────────────┘
```

**Fluxo de compra:**

```
1. Cliente faz login:
   → Serviço Usuários (valida credenciais)
   
2. Cliente busca produtos:
   → Serviço Produtos (retorna catálogo)
   
3. Cliente adiciona ao carrinho:
   → Serviço Pedidos (salva carrinho)
   
4. Cliente finaliza compra:
   a) Serviço Pedidos valida estoque
      → chama Serviço Produtos
   b) Serviço Pedidos cria pedido
   c) Publica evento: "PEDIDO_CRIADO"
   
5. Serviços reagem ao evento:
   - Serviço Pagamento: Cobra cartão
   - Serviço Produtos: Baixa estoque
   - Serviço Notificação: Envia e-mail confirmação
   
6. Se pagamento aprovado:
   → Serviço Notificação envia código rastreio
   
7. Cliente consulta histórico:
   → Serviço Pedidos (retorna pedidos do cliente)
```

---

---

## 📚 Próximos Passos

Continue aprendendo:

➡️ **[Glossário de Termos](./08-Glossario.md)**

---

[← Voltar ao Índice](./README.md)
