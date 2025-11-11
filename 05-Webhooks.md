# 5️⃣ Webhooks - Notificações Automáticas

[← Voltar ao Índice](./README.md)

> **Tempo de leitura:** ~10 minutos  
> **Nível:** Intermediário

---


### 🎯 O Que São Webhooks?

**Webhook** = "Telefone de retorno" onde você pede para ser avisado quando algo acontecer.

**Definição simples:**
Webhooks são notificações automáticas que um sistema envia para outro quando um evento ocorre.

**Analogia:**

Imagine que você está esperando uma encomenda:

**SEM Webhook (Você fica perguntando):**
```
Você: "Meu pacote chegou?"
Correios: "Não"
  ... 2 horas depois ...
Você: "E agora, chegou?"
Correios: "Não"
  ... 2 horas depois ...
Você: "Chegou agora?"
Correios: "Sim!"
```

**COM Webhook (Eles te avisam):**
```
Você: "Me avise quando o pacote chegar"
Correios: "Ok, anotado!"
  ... quando chegar ...
Correios: *TELEFONA* "Seu pacote chegou!"
Você: "Obrigado!"
```

### 🔄 Webhook vs API vs WebSocket

| Método | Quem inicia | Conexão | Uso típico |
|--------|-------------|---------|------------|
| **API** | Cliente puxa dados | Fecha após resposta | Cliente busca quando precisa |
| **WebSocket** | Ambos (bidirecional) | Permanece aberta | Chat, dashboards em tempo real |
| **Webhook** | Servidor empurra dados | Fecha após entrega | Notificações assíncronas |

**Visual:**

```
API (Pull):
Cliente ────[pedido]────> Servidor
Cliente <───[resposta]─── Servidor

WebSocket (Bidirecional):
Cliente ←────[dados]────→ Servidor
        (conexão aberta)

Webhook (Push):
Servidor ────[notificação]────> Cliente
         (quando evento ocorre)
```

### 📡 Como Funcionam Webhooks

#### Passo 1: Você Registra uma URL

Você diz ao serviço: "Quando algo acontecer, envie uma notificação HTTP POST para esta URL".

```http
POST /api/webhooks/cadastrar
{
  "url": "https://seu-sistema.com/webhook/pagamento",
  "eventos": ["pagamento.aprovado", "pagamento.cancelado"]
}
```

#### Passo 2: Evento Ocorre

Algo acontece no sistema (ex: pagamento aprovado).

#### Passo 3: Webhook É Disparado

O serviço envia um HTTP POST para a sua URL:

```http
POST https://seu-sistema.com/webhook/pagamento
Content-Type: application/json
X-Webhook-Signature: sha256=abc123...

{
  "evento": "pagamento.aprovado",
  "id": "PAG-123",
  "valor": 350.00,
  "cliente": "João Silva",
  "timestamp": "2025-11-10T14:30:00Z"
}
```

#### Passo 4: Seu Sistema Processa

```
1. Recebe o POST
2. Valida assinatura (segurança)
3. Processa o evento (ex: liberar acesso, enviar e-mail)
4. Responde 200 OK (confirma recebimento)
```

### 💡 Casos de Uso de Webhooks

| Serviço | Evento | Webhook Enviado Para |
|---------|--------|----------------------|
| **Gateway de Pagamento** (Stripe) | Pagamento aprovado | Seu e-commerce |
| **GitHub** | Push em repositório | Sistema de CI/CD |
| **Slack** | Mensagem em canal | Bot de atendimento |
| **Twilio** | SMS recebido | Sistema de chat |
| **Shopify** | Novo pedido criado | Sistema de estoque |
| **Mailchimp** | Usuário se inscreveu | CRM |
| **Zapier** | Qualquer evento | Sistema conectado |

**Exemplo prático - E-commerce:**

```
1. Cliente compra produto (R$ 100)
2. Gateway de pagamento processa
3. ✅ Pagamento aprovado!
4. Gateway envia webhook para seu sistema:
   POST https://loja.com/webhook/pagamento
   { "status": "aprovado", "pedidoId": 42 }
5. Seu sistema:
   - Marca pedido como pago
   - Envia e-mail de confirmação
   - Atualiza estoque
   - Notifica logística
```

### 🔐 Segurança em Webhooks

**Problema:** Qualquer pessoa pode enviar um POST para sua URL. Como saber se é legítimo?

#### 1. **Assinatura HMAC**

Serviço calcula um "hash" dos dados usando uma chave secreta e envia no cabeçalho.

**Como funciona:**

```
Serviço (ex: Stripe):
1. Pega os dados: {"evento":"pagamento.aprovado"}
2. Calcula HMAC: hash = HMAC-SHA256(dados, chave_secreta)
3. Envia:
   X-Webhook-Signature: sha256=abc123def456...
   
Seu sistema:
1. Recebe dados e assinatura
2. Calcula HMAC dos dados com SUA chave secreta
3. Compara:
   - Se igual: ✅ Legítimo
   - Se diferente: ❌ Fraude!
```

#### 2. **HTTPS Obrigatório**

Recomende sempre usar `https://` para receber webhooks (nunca `http://`).

#### 3. **IP Whitelist**

Alguns serviços publicam lista de IPs. Aceite apenas requisições desses IPs.

```
Stripe IPs:
- 3.18.12.63
- 3.130.192.231
- ...

Firewall:
ACEITAR requisições de IPs acima
BLOQUEAR todos os outros
```

#### 4. **Verificação Secreta**

Incluir parâmetro secreto na URL:

```http
POST https://loja.com/webhook?secret=abc123xyz789

Se secret não bater, rejeitar.
```

**⚠️ IMPORTANTE - Processamento Assíncrono:**

Webhooks têm timeout curto (5-30 segundos). Se você demorar para responder, o serviço pode reenviar.

```
❌ ERRADO (processamento síncrono):
1. Receber webhook
2. Atualizar banco de dados (2s)
3. Enviar e-mail (5s)
4. Notificar outros sistemas (10s)
5. Responder 200 OK
Total: 17 segundos → TIMEOUT!

✅ CORRETO (processamento assíncrono):
1. Receber webhook
2. Adicionar em fila (Redis, RabbitMQ)
3. Responder 200 OK
Total: 0.1 segundos

Worker separado processa a fila:
- Atualizar banco
- Enviar e-mail
- Notificar sistemas
```

### 🔄 Retry (Reenvio)

Se seu sistema não responder 200 OK, o serviço tenta novamente.

**Estratégia típica:**

```
Tentativa 1: Imediatamente
  ❌ Falhou (status 500)
  
Tentativa 2: Após 1 minuto
  ❌ Falhou (timeout)
  
Tentativa 3: Após 5 minutos
  ❌ Falhou
  
Tentativa 4: Após 15 minutos
  ✅ Sucesso (status 200)
```

**Problema:** Seu sistema pode receber o MESMO webhook múltiplas vezes!

**Solução - Idempotência:**

```python
# Guardar IDs já processados
webhooks_processados = set()

def processar_webhook(webhook_id, dados):
    # Verificar se já processou este webhook
    if webhook_id in webhooks_processados:
        print(f"Webhook {webhook_id} já foi processado. Ignorando.")
        return
    
    # Processar
    atualizar_pedido(dados)
    
    # Marcar como processado
    webhooks_processados.add(webhook_id)
```

### 🧪 Testando Webhooks

#### Problema: Como testar localmente?

Webhooks precisam de URL pública, mas seu computador está na rede local (`localhost`).

#### Solução 1: **ngrok** (Túnel temporário)

Cria uma URL pública que redireciona para seu `localhost`.

```bash
# 1. Instalar ngrok
# Baixe em: https://ngrok.com

# 2. Iniciar aplicação local
python app.py  # Roda na porta 5000

# 3. Criar túnel
ngrok http 5000

# Saída:
Forwarding: https://abc123.ngrok.io → http://localhost:5000

# 4. Usar URL do ngrok para cadastrar webhook
https://abc123.ngrok.io/webhook/pagamento
```

#### Solução 2: **Webhook.site** (Inspetor de webhooks)

Site que cria uma URL temporária e mostra todos os webhooks recebidos.

```
1. Acesse https://webhook.site
2. Copie a URL gerada (ex: https://webhook.site/abc-123)
3. Cadastre essa URL no serviço
4. Veja os webhooks chegando na tela
```

#### Solução 3: **Ferramentas de replay**

Stripe, GitHub e outros oferecem páginas para "reenviar" webhooks antigos.

### ⚠️ Problemas Comuns com Webhooks (Para Suporte)

#### Problema 1: "Webhook não chega"

**Sintomas:**
- Evento ocorreu, mas webhook não foi recebido

**Causas:**
1. URL cadastrada errada
2. Firewall bloqueando
3. Endpoint retornou erro (500, 404)
4. HTTPS com certificado inválido

**Como investigar:**

```bash
# 1. Verificar se URL é acessível
curl -X POST https://seu-sistema.com/webhook \
  -H "Content-Type: application/json" \
  -d '{"teste": true}'

# Se retornar erro, problema no seu sistema
# Se retornar sucesso, problema no serviço externo
```

**Soluções:**
1. Verificar URL cadastrada (typo?)
2. Verificar logs do servidor (erro 500?)
3. Testar com ngrok (bypass firewall)
4. Verificar certificado SSL válido

#### Problema 2: "Webhook recebido múltiplas vezes"

**Sintomas:**
- Mesmo evento processado 2, 3, 4 vezes
- Dados duplicados no banco

**Causas:**
1. Seu sistema demorou para responder (timeout)
2. Serviço reenviou por falha de rede
3. Falta de idempotência

**Solução:**

Implementar idempotência para evitar processar o mesmo webhooks multiplas vezes.

#### Problema 3: "Webhook com dados errados"

**Sintomas:**
- Webhook chega, mas com dados incorretos/faltando

**Causas:**
1. Versão antiga da API
2. Payload mudou (breaking change)
3. Filtro configurado errado

**Soluções:**
1. Verificar documentação da API (mudanças?)
2. Comparar payload recebido com esperado
3. Atualizar código para nova estrutura

#### Problema 4: "Webhook muito lento"

**Sintomas:**
- Webhooks causando timeout
- Serviço reenviando porque demorou

**Causas:**
1. Processamento síncrono pesado
2. Consultas lentas ao banco
3. Chamadas a APIs externas no meio do processamento

**Solução**

Usar filas e processamento assíncrono se aplicável.

### ✅ Checklist Webhook Endpoint

- [ ] Aceita apenas POST
- [ ] Valida assinatura/autenticação
- [ ] HTTPS habilitado (não HTTP)
- [ ] Responde em < 5 segundos
- [ ] Processamento assíncrono (usa fila)
- [ ] Idempotente (não processa duplicado)
- [ ] Logs completos (debug)
- [ ] Tratamento de erros
- [ ] Testes com ngrok/webhook.site

### 📊 Webhook vs Polling

**Polling (Cliente pergunta repetidamente):**

```
00:00 - Cliente: "Pagamento foi aprovado?"
        Servidor: "Não"
00:05 - Cliente: "E agora?"
        Servidor: "Não"
00:10 - Cliente: "Agora?"
        Servidor: "Sim!"

⚠️ 2 requisições desperdiçadas
⚠️ Atraso de até 5 minutos
```

**Webhook (Servidor avisa quando acontece):**

```
00:00 - Cliente registra webhook
00:03 - [Pagamento aprovado]
00:03 - Servidor: POST /webhook {"status": "aprovado"}
        Cliente: "Recebi!"

✅ 0 requisições desperdiçadas
✅ Notificação instantânea
```

| Característica | Polling | Webhook |
|----------------|---------|---------|
| **Latência** | Alta (espera próxima consulta) | Baixa (instantâneo) |
| **Requisições** | Muitas (a maioria vazia) | Poucas (só quando há evento) |
| **Carga no servidor** | Alta | Baixa |
| **Complexidade** | Baixa | Média |
| **Quando usar** | Servidor não suporta webhooks | Sempre que possível |

---

---

## 📚 Próximos Passos

Continue aprendendo:

➡️ **[SSL/TLS - Segurança na Comunicação](./06-SSL-TLS.md)**

---

[← Voltar ao Índice](./README.md)
