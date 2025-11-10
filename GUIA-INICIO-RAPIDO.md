# 🚀 Guia de Início Rápido

> **Objetivo:** Começar a usar a documentação em menos de 5 minutos

---

## 👋 Bem-vindo!

Você tem acesso a uma documentação completa sobre APIs e fundamentos de rede. Este guia vai te ajudar a começar rapidamente.

---

## 📍 Onde Estou?

Você está no diretório `GUIA-APIS/` que contém:

- **1 arquivo principal** → `README.md` (começa aqui!)
- **9 módulos de conteúdo** → Tópicos específicos
- **1 glossário** → Dicionário de termos
- **1 guia de cenários práticos** → Casos reais

---

## 🎯 Começando Agora

### Opção 1: Sou Iniciante Total
```
1. Abra: README.md
2. Leia a seção "Overview da Documentação"
3. Siga o "Roteiro de Aprendizado Sugerido"
4. Comece por: 01-DNS.md
```

### Opção 2: Preciso Resolver um Problema Agora
```
1. Abra: README.md
2. Vá direto para "Guia Rápido de Consulta"
3. Encontre seu sintoma na tabela
4. Clique no link da documentação indicada
```

### Opção 3: Quero Entender um Tópico Específico
```
1. Escolha o arquivo do tópico:
   - DNS? → 01-DNS.md
   - HTTP? → 02-HTTP.md
   - WebSocket? → 03-WebSocket.md
   - REST API? → 04-RESTful-API.md
   - Webhooks? → 05-Webhooks.md
   - SSL/TLS? → 06-SSL-TLS.md
   - Microserviços? → 07-Microservicos.md
   
2. Leia o módulo específico
```

### Opção 4: Não Entendo um Termo Técnico
```
1. Abra: 08-Glossario.md
2. Busque o termo (Ctrl+F)
3. Leia a definição simples
```

---

## 📖 Estrutura de Cada Documento

Todos os módulos seguem esta estrutura:

```
┌────────────────────────────┐
│ HEADER                     │
│ - Título                   │
│ - Link para voltar         │
│ - Tempo de leitura         │
│ - Nível                    │
└────────────────────────────┘

┌────────────────────────────┐
│ CONTEÚDO                   │
│ - Explicações              │
│ - Exemplos práticos        │
│ - Ferramentas              │
│ - Troubleshooting          │
│ - Checklists               │
└────────────────────────────┘

┌────────────────────────────┐
│ FOOTER                     │
│ - Próximos passos          │
│ - Link para voltar         │
└────────────────────────────┘
```

---

## 🔍 Exemplo Prático: "Cliente não consegue acessar o site"

### Passo 1: Consulta Rápida
Abra `README.md` → Seção "Guia Rápido de Consulta"

### Passo 2: Identifique o Sintoma
Encontre na tabela:
```
"Este site não pode ser acessado" → Problema de DNS
```

### Passo 3: Acesse Documentação
Clique no link: `01-DNS.md`

### Passo 4: Vá Direto ao Problema
No arquivo DNS, procure seção: "Problemas Comuns de DNS"

### Passo 5: Siga o Checklist
Use o checklist de troubleshooting no final do documento

### Passo 6: Use as Ferramentas
Execute os comandos sugeridos:
```bash
nslookup www.siteproblema.com
```

---

## 🛠️ Ferramentas que Você Vai Precisar

### Instaladas por Padrão
- ✅ **nslookup** (testar DNS)
- ✅ **Navegador com DevTools** (F12 para inspecionar)
- ✅ **curl** (testar HTTP - Linux/Mac)

### Para Instalar (Opcional)
- 🔧 **Postman** - Cliente visual para APIs
- 🔧 **dig** - DNS detalhado (já vem no Mac/Linux)
- 🔧 **httpie** - Cliente HTTP amigável
- 🔧 **websocat** - Testar WebSocket

---

## 📚 Mapinha Mental Rápido

```
Internet não funciona?
    │
    ├─ Site não carrega
    │   └─> DNS (01-DNS.md)
    │
    ├─ Erro 404, 500, etc.
    │   └─> HTTP (02-HTTP.md)
    │
    ├─ "Conexão não é segura"
    │   └─> SSL/TLS (06-SSL-TLS.md)
    │
    ├─ API não responde
    │   └─> RESTful API (04-RESTful-API.md)
    │
    ├─ Chat/notificações não funcionam
    │   └─> WebSocket (03-WebSocket.md)
    │
    └─ Webhook não dispara
        └─> Webhooks (05-Webhooks.md)
```

---

## 💡 Dicas de Ouro

### 1. Use o Glossário
Sempre que encontrar termo desconhecido → `08-Glossario.md`

### 2. Não Decore, Consulte
Esta documentação é para consulta, não para decorar!

### 3. Pratique com Ferramentas
Execute os comandos de exemplo para aprender fazendo

### 4. Siga os Checklists
Durante atendimento, use os checklists para não esquecer nada

### 5. Marque Seus Favoritos
Salve nos favoritos:
- README.md (sempre)
- Glossário (consulta rápida)
- Checklists (troubleshooting)

---

## ⏰ Quanto Tempo Vou Precisar?

### Leitura Completa
- **Iniciante:** 2-3 horas (com pausas)
- **Com experiência:** 1-1.5 horas (revisão)

### Consulta Rápida
- **Por problema:** 5-10 minutos
- **Por checklist:** 2-3 minutos

### Dominar o Conteúdo
- **Prática diária:** 1-2 semanas
- **Uso em atendimentos:** 1 mês para fluência

---

## 🎯 Metas de Aprendizado

### Semana 1
- [ ] Entender DNS básico
- [ ] Conhecer códigos HTTP principais (200, 404, 500)
- [ ] Saber usar nslookup e curl

### Semana 2
- [ ] Entender estrutura de APIs REST
- [ ] Conhecer diferença HTTP vs WebSocket
- [ ] Usar Postman para testar APIs

### Semana 3
- [ ] Diagnosticar problemas de DNS sozinho
- [ ] Interpretar erros de API
- [ ] Seguir checklists de troubleshooting

### Semana 4
- [ ] Resolver 80% dos casos comuns
- [ ] Explicar conceitos para clientes
- [ ] Saber quando escalar para dev

---

## 📞 Quando Escalar para Desenvolvimento?

Você deve escalar quando:

❌ Erro 500 persistente (bug no servidor)  
❌ Problema afeta múltiplos clientes  
❌ Você seguiu o checklist e não resolveu  
❌ Problema requer mudança de código ou configuração  

✅ Antes de escalar, documente:
- Sintoma exato
- Passos para reproduzir
- O que você já testou
- Logs e mensagens de erro

---

## 🔗 Links Importantes

- **[README.md](./README.md)** - Índice principal
- **[ESTRUTURA.md](./ESTRUTURA.md)** - Como a doc está organizada
- **[01-DNS.md](./01-DNS.md)** - Comece por aqui se for iniciante
- **[08-Glossario.md](./08-Glossario.md)** - Consulta de termos

---

## ✅ Checklist: Estou Pronto?

Antes de começar atendimentos, verifique:

- [ ] Li o README.md completo
- [ ] Sei onde encontrar os checklists
- [ ] Tenho nslookup funcionando
- [ ] Sei abrir DevTools (F12) no navegador
- [ ] Li pelo menos DNS e HTTP
- [ ] Testei os comandos de exemplo
- [ ] Salvei links importantes nos favoritos

---

## 🚀 Próximo Passo

**Agora mesmo:**

1. Abra [README.md](./README.md)
2. Leia a seção "Overview da Documentação"
3. Escolha seu caminho (estudo completo ou consulta rápida)

**Boa jornada de aprendizado! 📚**

---

[← Ir para README](./README.md)
