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

### 📡 Exemplo Prático: Chat em WebSocket

**Cliente (JavaScript no navegador):**

```javascript
// 1. Conectar no WebSocket
const socket = new WebSocket('wss://chat.exemplo.com');

// 2. Quando conectar
socket.onopen = function() {
  console.log('✅ Conectado no chat!');
  
  // Enviar mensagem de entrada
  socket.send(JSON.stringify({
    tipo: 'entrar',
    nome: 'João'
  }));
};

// 3. Quando receber mensagem do servidor
socket.onmessage = function(event) {
  const mensagem = JSON.parse(event.data);
  
  console.log('📩 Mensagem recebida:', mensagem);
  // { usuario: 'Maria', texto: 'Olá pessoal!' }
  
  // Exibir na tela
  exibirMensagem(mensagem.usuario, mensagem.texto);
};

// 4. Enviar mensagem
function enviarMensagem(texto) {
  socket.send(JSON.stringify({
    tipo: 'mensagem',
    texto: texto
  }));
}

// 5. Quando desconectar
socket.onclose = function() {
  console.log('❌ Desconectado do chat');
};

// 6. Quando houver erro
socket.onerror = function(error) {
  console.error('🔥 Erro no WebSocket:', error);
};
```

**Servidor (exemplo conceitual):**

```
Quando cliente conecta:
  - Adicionar cliente à lista de conectados
  - Enviar histórico de mensagens

Quando cliente envia mensagem:
  - Receber mensagem
  - Enviar para TODOS os clientes conectados
  
Quando cliente desconecta:
  - Remover da lista de conectados
  - Avisar outros usuários
```

### 🔐 WebSocket Seguro (WSS)

Assim como HTTP tem HTTPS, WebSocket tem **WSS** (WebSocket Secure).

| WS | WSS |
|----|-----|
| `ws://exemplo.com` | `wss://exemplo.com` |
| Sem criptografia | Com criptografia SSL/TLS |
| ❌ Inseguro | ✅ Seguro |
| Porta padrão: 80 | Porta padrão: 443 |

**⚠️ Sempre use WSS em produção!**

### ⚠️ Problemas Comuns WebSocket (Para Suporte)

#### Problema 1: "Conexão WebSocket falha"

**Sintomas:**
- Cliente não consegue conectar
- Erro: "WebSocket connection failed"

**Causas comuns:**
1. **URL errada:** Cliente tenta conectar em `ws://` mas servidor só aceita `wss://`
2. **Firewall/Proxy:** Bloqueando conexões WebSocket
3. **Servidor não suporta WebSocket**
4. **Porta bloqueada**

**Como investigar:**
```javascript
// Testar no console do navegador (F12)
const ws = new WebSocket('wss://api.exemplo.com/socket');

ws.onopen = () => console.log('✅ Conectou');
ws.onerror = (e) => console.error('❌ Erro:', e);
```

**Soluções:**
1. Verificar se URL está correta (`wss://` para produção)
2. Testar de outra rede (pode ser firewall corporativo)
3. Verificar se servidor está rodando
4. Verificar logs do servidor

#### Problema 2: "Conexão cai frequentemente"

**Sintomas:**
- WebSocket conecta, mas desconecta sozinho após alguns minutos
- Cliente precisa reconectar toda hora

**Causas:**
1. **Timeout de proxy/firewall:** Fecha conexões inativas
2. **Keep-alive não configurado:** Servidor fecha por inatividade
3. **Instabilidade de rede**

**Soluções:**
1. **Implementar heartbeat (ping/pong):**

```javascript
// Cliente envia ping a cada 30 segundos
setInterval(() => {
  if (socket.readyState === WebSocket.OPEN) {
    socket.send(JSON.stringify({ tipo: 'ping' }));
  }
}, 30000);
```

2. **Reconexão automática:**

```javascript
let socket;
let reconnectInterval = 5000; // 5 segundos

function conectar() {
  socket = new WebSocket('wss://api.exemplo.com/socket');
  
  socket.onopen = () => {
    console.log('✅ Conectado');
    reconnectInterval = 5000; // Resetar intervalo
  };
  
  socket.onclose = () => {
    console.log('❌ Desconectado. Reconectando em', reconnectInterval/1000, 's');
    setTimeout(conectar, reconnectInterval);
    reconnectInterval = Math.min(reconnectInterval * 2, 60000); // Aumentar intervalo (máx 60s)
  };
}

conectar();
```

#### Problema 3: "Mensagens não chegam"

**Sintomas:**
- Cliente está conectado, mas mensagens não aparecem
- Servidor envia, mas cliente não recebe

**Causas:**
1. **Cliente não está escutando:** Faltou `onmessage`
2. **Formato de mensagem errado:** Servidor envia JSON mas cliente espera texto
3. **Erro no handler:** JavaScript com erro e não processa mensagem

**Como investigar:**

```javascript
// Log TODAS as mensagens
socket.onmessage = function(event) {
  console.log('📩 Mensagem bruta:', event.data);
  
  try {
    const dados = JSON.parse(event.data);
    console.log('📦 Dados parseados:', dados);
    // Processar mensagem...
  } catch (error) {
    console.error('🔥 Erro ao parsear:', error);
  }
};
```

#### Problema 4: "Muitas conexões abertas"

**Sintomas:**
- Servidor fica lento
- Muitos WebSockets abertos

**Causas:**
- Cliente não fecha conexão ao sair da página
- Vazamento de conexões

**Solução - Fechar ao sair:**

```javascript
// Fechar WebSocket ao sair da página
window.addEventListener('beforeunload', () => {
  if (socket.readyState === WebSocket.OPEN) {
    socket.close(1000, 'Página fechada');
  }
});
```

### 🛠️ Ferramentas Para Testar WebSocket

#### 1. **Navegador DevTools**

**Chrome/Edge:**
1. F12 → Aba "Network"
2. Filtro "WS" (WebSocket)
3. Clicar na conexão WebSocket
4. Ver mensagens enviadas/recebidas

#### 2. **Postman** (suporta WebSocket)

1. New → WebSocket Request
2. Digite URL: `wss://echo.websocket.org` (servidor de teste)
3. Connect
4. Enviar mensagens

#### 3. **websocat** (CLI)

```bash
# Instalar
cargo install websocat

# Conectar e enviar mensagem
echo "Olá" | websocat wss://echo.websocket.org

# Modo interativo
websocat wss://echo.websocket.org
# Digite mensagens e pressione Enter
```

#### 4. **Sites de Teste Online**

- **websocket.org/echo.html** - Echo server (retorna o que você enviar)
- **PieSocket.com** - Testar WebSocket online
- **WebSocket King** - Cliente visual

### ✅ Checklist de Troubleshooting WebSocket

- [ ] **Passo 1:** Conexão está abrindo? (status = OPEN)
- [ ] **Passo 2:** URL está correta? (`wss://` para HTTPS)
- [ ] **Passo 3:** Servidor suporta WebSocket?
- [ ] **Passo 4:** Firewall/proxy está bloqueando?
- [ ] **Passo 5:** Heartbeat (ping/pong) configurado?
- [ ] **Passo 6:** Cliente tem reconexão automática?
- [ ] **Passo 7:** Handler `onmessage` está implementado?
- [ ] **Passo 8:** Formato de mensagem está correto?
- [ ] **Passo 9:** Conexão fecha ao sair da página?

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
