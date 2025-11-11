# 3️⃣ WebSocket - Conversas em Tempo Real

[← Voltar ao Índice](./README.md)

> **Tempo de leitura:** ~15 minutos  
> **Nível:** Intermediário

---


### 🎯 O Que É WebSocket?

**WebSocket** é um protocolo de comunicação que permite **conversas bidirecionais** em tempo real entre cliente e servidor.

**Diferença HTTP vs WebSocket:**

```
HTTP (Tradicional):
Cliente: "Tem mensagem nova?"
Servidor: "Não"
  ... espera 5 segundos ...
Cliente: "Tem mensagem nova agora?"
Servidor: "Não"
  ... espera 5 segundos ...
Cliente: "E agora?"
Servidor: "Sim! Aqui está"

⚠️ Problema: Cliente fica perguntando toda hora (ineficiente!)


WebSocket (Moderno):
Cliente: "Quero ficar conectado. Me avise quando houver novidade!"
Servidor: "Ok, conexão estabelecida"
  ... 2 minutos depois ...
Servidor: "Opa! Chegou mensagem nova!" (envia automaticamente)
Cliente: "Recebi! Obrigado"

✅ Vantagem: Servidor avisa quando há novidade (eficiente!)
```

**Analogia:**

| HTTP | WebSocket |
|------|-----------|
| 📬 Ir ao correio buscar cartas | ☎️ Ter um telefone conectado |
| Você vai lá várias vezes ver se chegou algo | Quando chega algo, o telefone toca |
| Gasta tempo e energia | Eficiente - só atende quando tem algo |

### 🔄 Como Funciona WebSocket

#### Passo 1: Handshake (Aperto de mão inicial)

Começa como HTTP, depois "atualiza" para WebSocket:

```http
# Cliente envia:
GET /chat HTTP/1.1
Host: servidor.exemplo.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13

# Servidor responde:
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

**O que aconteceu:**
- Cliente: "Oi, quero abrir um WebSocket"
- Servidor: "Ok! Vamos mudar de HTTP para WebSocket"
- ✅ Conexão permanece aberta!

#### Passo 2: Comunicação Bidirecional

Depois do handshake, ambos podem enviar mensagens a qualquer momento:

```
┌─────────────────┐         ┌─────────────────┐
│     Cliente     │         │    Servidor     │
└────────┬────────┘         └────────┬────────┘
         │                           │
         │  "Olá!" ─────────────────>│
         │                           │
         │<────────────── "Oi! Tudo bem?"
         │                           │
         │  "Tudo ótimo!" ──────────>│
         │                           │
         │<────────────── "Novo usuário entrou"
         │                           │
         │  (pode enviar a qualquer momento)
         │  (servidor também pode enviar a qualquer momento)
```

### 💡 Casos de Uso do WebSocket

| Aplicação | Por Que WebSocket? |
|-----------|-------------------|
| **Chat em tempo real** | Mensagens aparecem instantaneamente |
| **Notificações** | Avisos chegam na hora (sem ficar consultando) |
| **Jogos online** | Movimentos sincronizados em tempo real |
| **Trading/Bolsa** | Preços atualizados ao vivo |
| **Colaboração** (Google Docs) | Ver o que outros editam ao vivo |
| **Dashboards** | Gráficos atualizam automaticamente |
| **Streaming de dados** | Sensores IoT enviando dados |

### 🆚 WebSocket vs HTTP: Quando Usar Cada Um?

| Situação | Use | Por Quê |
|----------|-----|---------|
| Buscar lista de produtos | HTTP (GET) | Dados não mudam constantemente |
| Criar um pedido | HTTP (POST) | Operação única |
| Chat de atendimento | WebSocket | Mensagens em tempo real |
| Dashboard com gráficos ao vivo | WebSocket | Atualizações frequentes |
| Upload de arquivo | HTTP | Transferência única |
| Notificações push | WebSocket | Servidor precisa avisar cliente |
| API RESTful | HTTP | Padrão para APIs públicas |
| Jogo multiplayer | WebSocket | Sincronização em tempo real |

**Regra prática:**
- ✅ **HTTP:** Cliente puxa informação quando precisa
- ✅ **WebSocket:** Servidor empurra informação quando tem novidade

### 🔐 WebSocket Seguro (WSS)

Assim como HTTP tem HTTPS, WebSocket tem **WSS** (WebSocket Secure).

| WS | WSS |
|----|-----|
| `ws://exemplo.com` | `wss://exemplo.com` |
| Sem criptografia | Com criptografia SSL/TLS |
| ❌ Inseguro | ✅ Seguro |
| Porta padrão: 80 | Porta padrão: 443 |

**⚠️ Sempre use WSS em produção!**

### 📊 Comparação: HTTP vs WebSocket vs Polling

| Característica | HTTP | WebSocket | Polling (HTTP repetido) |
|----------------|------|-----------|------------------------|
| **Conexão** | Fecha após resposta | Permanece aberta | Abre e fecha toda hora |
| **Direção** | Cliente → Servidor | ↔️ Bidirecional | Cliente → Servidor |
| **Latência** | Média | Baixa | Alta |
| **Overhead** | Baixo por requisição | Muito baixo | Alto (muitas requisições) |
| **Servidor pode iniciar?** | ❌ Não | ✅ Sim | ❌ Não |
| **Uso de recursos** | Baixo | Médio | Alto |
| **Casos de uso** | APIs REST, sites | Chat, jogos, dashboards | Quando WebSocket não disponível |

---

---

## 📚 Próximos Passos

Continue aprendendo:

➡️ **[RESTful API - Construindo APIs Inteligentes](./04-RESTful-API.md)**

---

[← Voltar ao Índice](./README.md)
