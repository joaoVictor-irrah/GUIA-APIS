# 📚 Guia de Fundamentos de APIs - Documentação Completa

> **Objetivo:** Entender os conceitos fundamentais de APIs, redes e comunicação web de forma didática e acessível
>
> **Público:** Equipe de suporte técnico e / ou outras partes interessadas
>
> **Tempo de leitura total:** 90-120 minutos

---

## 🎯 Overview da Documentação

Este guia está organizado em módulos independentes, cada um cobrindo um conceito fundamental para trabalhar com APIs e suporte técnico. Você pode ler na ordem sequencial ou consultar tópicos específicos conforme necessário.

### 📖 Estrutura do Conteúdo

A documentação está dividida em **9 módulos principais**:

1. **DNS** - Entenda como nomes de domínio são convertidos em endereços IP
2. **HTTP** - A linguagem de comunicação da web
3. **WebSocket** - Comunicação em tempo real bidirecional
4. **RESTful API** - Padrões para construir APIs modernas
5. **Webhooks** - Sistema de notificações automáticas
6. **SSL/TLS** - Segurança e criptografia na comunicação
7. **Microserviços** - Arquitetura moderna de sistemas distribuídos
8. **Glossário** - Termos técnicos explicados
9. **Cenários Práticos** - Casos reais de suporte

---

## 🗂️ Índice de Documentações

### Fundamentos de Rede

| Módulo | Arquivo | Tempo | Descrição |
|-------|---------|-------|-----------|
| 1️⃣ **DNS** | [01-DNS.md](./01-DNS.md) | ~15 min | Sistema de nomes de domínio, registros DNS, propagação |
| 2️⃣ **HTTP** | [02-HTTP.md](./02-HTTP.md) | ~20 min | Métodos, códigos de status, cabeçalhos, ciclo request/response |
| 6️⃣ **SSL/TLS** | [06-SSL-TLS.md](./06-SSL-TLS.md) | ~12 min | Certificados digitais, HTTPS, criptografia |

### Comunicação e APIs

| Módulo | Arquivo | Tempo | Descrição |
|-------|---------|-------|-----------|
| 3️⃣ **WebSocket** | [03-WebSocket.md](./03-WebSocket.md) | ~15 min | Comunicação em tempo real, chat, notificações push |
| 4️⃣ **RESTful API** | [04-RESTful-API.md](./04-RESTful-API.md) | ~25 min | Princípios REST, design de API, boas práticas |
| 5️⃣ **Webhooks** | [05-Webhooks.md](./05-Webhooks.md) | ~10 min | Notificações automáticas, eventos, callbacks |

### Arquitetura e Referência

| Módulo | Arquivo | Tempo | Descrição |
|-------|---------|-------|-----------|
| 7️⃣ **Microserviços** | [07-Microservicos.md](./07-Microservicos.md) | ~18 min | Arquitetura distribuída, containers, orquestração |
| 8️⃣ **Glossário** | [08-Glossario.md](./08-Glossario.md) | ~5 min | Dicionário de termos técnicos |
| 9️⃣ **Cenários Práticos** | [09-Cenarios-Praticos.md](./09-Cenarios-Praticos.md) | ~15 min | Casos reais de troubleshooting |

---

## ⚡ Guia Rápido de Consulta

### 🔍 Problemas Comuns e Onde Buscar Solução

#### Cliente não consegue acessar o site

| Sintoma | Possível Causa | Consultar |
|---------|----------------|-----------|
| "Este site não pode ser acessado" | Problema de DNS | [01-DNS.md](./01-DNS.md) - Seção "Problemas Comuns" |
| "Conexão não é segura" | Certificado SSL inválido | [06-SSL-TLS.md](./06-SSL-TLS.md) - Seção "Erros de Certificado" |
| "Erro 404 - Não encontrado" | URL incorreta ou recurso deletado | [02-HTTP.md](./02-HTTP.md) - Seção "Códigos 4xx" |
| "Erro 500 - Erro interno" | Falha no servidor | [02-HTTP.md](./02-HTTP.md) - Seção "Códigos 5xx" |

#### Problemas com API

| Sintoma | Possível Causa | Consultar |
|---------|----------------|-----------|
| "401 Unauthorized" | Token de autenticação inválido | [02-HTTP.md](./02-HTTP.md) + [04-RESTful-API.md](./04-RESTful-API.md) |
| "429 Too Many Requests" | Excedeu limite de requisições | [04-RESTful-API.md](./04-RESTful-API.md) - Rate Limiting |
| "400 Bad Request" | JSON mal formatado | [04-RESTful-API.md](./04-RESTful-API.md) - Tratamento de Erros |
| "API lenta" | Falta de paginação ou cache | [04-RESTful-API.md](./04-RESTful-API.md) - Performance |

#### Comunicação em Tempo Real

| Sintoma | Possível Causa | Consultar |
|---------|----------------|-----------|
| WebSocket não conecta | Firewall bloqueando porta | [03-WebSocket.md](./03-WebSocket.md) - Troubleshooting |
| Mensagens não chegam | Handler não implementado | [03-WebSocket.md](./03-WebSocket.md) - Problemas Comuns |
| Webhook não dispara | URL inválida ou timeout | [05-Webhooks.md](./05-Webhooks.md) - Debugging |
| Conexão cai frequentemente | Falta de heartbeat (ping/pong) | [03-WebSocket.md](./03-WebSocket.md) - Keep-Alive |

---

## 🛠️ Ferramentas Essenciais por Categoria

### Diagnóstico de DNS
- **nslookup** - Consultar registros DNS
- **dig** - Informações detalhadas de DNS
- **DNSChecker.org** - Verificar propagação global

📖 **Ver mais:** [01-DNS.md](./01-DNS.md) - Seção "Ferramentas Para Testar DNS"

### Teste de APIs HTTP/REST
- **Postman** - Cliente visual para APIs
- **curl** - Linha de comando para requisições
- **DevTools** (F12) - Inspeção de rede no navegador

📖 **Ver mais:** [02-HTTP.md](./02-HTTP.md) + [04-RESTful-API.md](./04-RESTful-API.md)

### Teste de WebSocket
- **websocat** - CLI para WebSocket
- **Postman** - Suporte a WebSocket
- **websocket.org/echo.html** - Servidor de teste

📖 **Ver mais:** [03-WebSocket.md](./03-WebSocket.md) - Seção "Ferramentas Para Testar WebSocket"

### Verificação de SSL/TLS
- **SSL Labs** - Análise de certificados
- **Browser DevTools** - Ver detalhes do certificado
- **openssl** - Linha de comando

📖 **Ver mais:** [06-SSL-TLS.md](./06-SSL-TLS.md) - Seção "Ferramentas de Diagnóstico"

---

## 📋 Checklists Rápidos

### ✅ Checklist: Problema de Conectividade

```
□ DNS está resolvendo corretamente? (nslookup)
□ Servidor está respondendo? (ping/curl)
□ Certificado SSL é válido? (verificar no navegador)
□ Firewall está bloqueando? (testar de outra rede)
□ Portas corretas estão abertas? (80 para HTTP, 443 para HTTPS)
```

**📖 Documentação detalhada:** [01-DNS.md](./01-DNS.md) + [06-SSL-TLS.md](./06-SSL-TLS.md)

### ✅ Checklist: Erro em API REST

```
□ Qual o código de status HTTP? (200, 400, 500, etc.)
□ URL está correta e completa?
□ Método HTTP é o esperado? (GET, POST, PUT, DELETE)
□ Token de autenticação é válido?
□ Corpo da requisição está formatado corretamente? (JSON)
□ Cabeçalhos necessários foram enviados?
□ Consegue reproduzir em Postman?
```

**📖 Documentação detalhada:** [04-RESTful-API.md](./04-RESTful-API.md)

### ✅ Checklist: WebSocket com Problemas

```
□ Conexão está abrindo? (status OPEN)
□ URL usa wss:// (para HTTPS) ou ws:// (para HTTP)?
□ Firewall/proxy está bloqueando WebSocket?
□ Handler onmessage está implementado?
□ Heartbeat (ping/pong) está configurado?
□ Reconexão automática funciona?
```

**📖 Documentação detalhada:** [03-WebSocket.md](./03-WebSocket.md)

---

## 🎓 Conceitos-Chave por Módulo

### DNS - Conceitos Essenciais
- **Registro A** → Mapeia domínio para IPv4
- **Registro CNAME** → Cria apelido para domínio
- **TTL** → Tempo que DNS fica em cache
- **Propagação** → Tempo para DNS atualizar globalmente (até 48h)

📖 [Documentação completa](./01-DNS.md)

### HTTP - Conceitos Essenciais
- **GET** → Buscar dados
- **POST** → Criar novo recurso
- **PUT/PATCH** → Atualizar recurso
- **DELETE** → Remover recurso
- **2xx** → Sucesso
- **4xx** → Erro do cliente
- **5xx** → Erro do servidor

📖 [Documentação completa](./02-HTTP.md)

### WebSocket - Conceitos Essenciais
- **Bidirecional** → Cliente e servidor podem iniciar comunicação
- **Tempo real** → Latência mínima
- **Stateful** → Conexão permanece aberta
- **Heartbeat** → Ping/pong para manter conexão viva

📖 [Documentação completa](./03-WebSocket.md)

### REST API - Conceitos Essenciais
- **Stateless** → Cada requisição é independente
- **Recursos** → URLs representam entidades (substantivos)
- **JSON** → Formato padrão de dados
- **Versionamento** → `/v1/`, `/v2/` para compatibilidade
- **Paginação** → Limitar quantidade de dados retornados

📖 [Documentação completa](./04-RESTful-API.md)

### Webhooks - Conceitos Essenciais
- **Push vs Pull** → Servidor notifica cliente (não o contrário)
- **Payload** → Dados enviados na notificação
- **Retry** → Retentar em caso de falha
- **Assinatura** → Validar autenticidade do webhook

📖 [Documentação completa](./05-Webhooks.md)

### SSL/TLS - Conceitos Essenciais
- **Certificado** → Prova que o site é legítimo
- **HTTPS** → HTTP com criptografia
- **CA (Certificate Authority)** → Empresa que emite certificados
- **Handshake** → Processo de estabelecer conexão segura

📖 [Documentação completa](./06-SSL-TLS.md)

### Microserviços - Conceitos Essenciais
- **Desacoplamento** → Serviços independentes
- **API Gateway** → Ponto único de entrada
- **Container** → Empacotamento de aplicação
- **Orquestração** → Gerenciar múltiplos containers

📖 [Documentação completa](./07-Microservicos.md)

---

## 💡 Dicas de Uso

### Para Estudo
- ✅ Leia os módulos na ordem sugerida para construir conhecimento progressivo
- ✅ Pratique com as ferramentas mencionadas em cada módulo
- ✅ Refaça os exemplos práticos apresentados
- ✅ Use o glossário sempre que encontrar um termo desconhecido

### Para Consulta Rápida
- 🔍 Use o guia rápido acima para encontrar soluções por sintoma
- 🔍 Consulte os checklists durante atendimento
- 🔍 Mantenha as ferramentas essenciais instaladas e prontas
- 🔍 Marque (bookmark) as seções de troubleshooting de cada módulo

### Para Troubleshooting
1. Identifique o **sintoma** (o que o cliente reporta)
2. Consulte a **tabela de problemas comuns** acima
3. Acesse a **documentação específica** indicada
4. Siga o **checklist** correspondente
5. Use as **ferramentas** sugeridas para diagnóstico

---

## 📞 Como Reportar Problemas para Desenvolvimento

Quando precisar escalar um problema para o time de desenvolvimento, inclua:

### Informações Essenciais
```
1. SINTOMA
   - O que o cliente reportou
   - Mensagem de erro exata

2. CONTEXTO
   - URL completa
   - Método HTTP usado
   - Timestamp do problema

3. DADOS DA REQUISIÇÃO
   - Headers enviados
   - Body da requisição (se aplicável)
   - Token/autenticação (CENSURAR dados sensíveis)

4. RESPOSTA DO SERVIDOR
   - Código de status HTTP
   - Headers da resposta
   - Body da resposta

5. INVESTIGAÇÃO REALIZADA
   - O que você já testou
   - Ferramentas utilizadas
   - Resultados dos testes

6. REPRODUÇÃO
   - É intermitente ou constante?
   - Afeta todos os usuários ou só alguns?
   - Consegue reproduzir em Postman/curl?
```

📖 **Exemplos práticos:** [09-Cenarios-Praticos.md](./09-Cenarios-Praticos.md)

---

## 📚 Recursos Adicionais

### Ferramentas Online Úteis
- **JSONLint** (jsonlint.com) - Validar JSON
- **Postman** (postman.com) - Testar APIs
- **SSL Labs** (ssllabs.com) - Testar SSL/TLS
- **DNSChecker** (dnschecker.org) - Verificar DNS globalmente
- **HTTPie** (httpie.io) - Cliente HTTP amigável

### Para Aprofundamento
- **MDN Web Docs** - Documentação técnica de web
- **REST API Tutorial** (restapitutorial.com)
- **WebSocket.org** - Especificação e exemplos
- **RFC 2616** - Especificação HTTP (avançado)

---

## 📝 Contribuindo

Encontrou algo que pode melhorar nos docs?

1. Abra issue descrevendo o problema
2. Sugira melhoria específica
3. Se for pequeno, faça PR direto

**Mantenha:**
- Exemplos concisos (5-30 linhas)
- Linguagem clara e direta
- Foco em pragmatismo

---

**Versão:** 1.1.13
**Última atualização:** 2025-11-11

---

## 📝 Notas Finais

Esta documentação foi criada para ser:
- **Didática** - Explicações com analogias do dia-a-dia
- **Prática** - Exemplos reais e ferramentas testáveis
- **Modular** - Cada tópico é independente
- **Consultável** - Guia rápido para uso no dia-a-dia

**Lembre-se:** Não precisa decorar tudo! Use esta documentação como referência durante seu trabalho. Com o tempo e prática, os conceitos vão se tornar naturais.

---

**Bons estudos e bom suporte! 🚀**
