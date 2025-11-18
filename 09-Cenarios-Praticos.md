# 9️⃣ Cenários Práticos de Suporte

[← Voltar ao Índice](./README.md)

> **Tempo de leitura:** ~15 minutos  
> **Nível:** Prático

---

### 📞 Cenário 1: "Site Fora do Ar"

**Cliente reporta:** "Nosso site não está abrindo!"

**Passo a passo de diagnóstico:**

```
1. TESTAR ACESSO
   - Você consegue acessar de outro computador/rede?
   - Qual erro aparece?

2. VERIFICAR DNS
   $ nslookup www.cliente.com
   - Retorna IP? → DNS OK
   - Erro? → Problema de DNS

3. VERIFICAR SERVIDOR (PING)
   $ ping www.cliente.com
   - Responde? → Servidor online
   - Timeout? → Servidor offline ou bloqueado

4. VERIFICAR PORTA
   $ telnet www.cliente.com 443
   - Conecta? → Porta aberta
   - Recusa? → Serviço não está rodando

5. VERIFICAR CERTIFICADO
   $ curl -I https://www.cliente.com
   - 200 OK? → Site responde
   - Erro SSL? → Problema de certificado
   - 502/503? → Servidor com problema

6. PRÓXIMOS PASSOS
   - DNS: Verificar registros, propagação
   - Servidor: Verificar logs, processos
   - Certificado: Renovar se expirado
```

### 📞 Cenário 2: "API Retornando Erro"

**Cliente reporta:** "Minha requisição está falhando com erro 401"

**Diagnóstico:**

```
1. CONFIRMAR ERRO
   - Qual código exato? 401, 403, 500?
   - Qual endpoint? GET /api/usuarios?
   - Qual mensagem de erro?

2. SE 401 (Unauthorized)
   - Token está sendo enviado?
     $ curl -H "Authorization: Bearer TOKEN" https://api.com/endpoint

   - Token é válido?
     Verificar se não expirou

   - Token está no formato correto?
     Bearer antes do token?

3. SE 403 (Forbidden)
   - Usuário tem permissão?
   - Role/permissão está correta?

4. SE 400 (Bad Request)
   - JSON está válido?
     Testar em https://jsonlint.com

   - Campos obrigatórios foram enviados?
   - Tipos de dados estão corretos?

5. SE 500 (Internal Server Error)
   - Problema no servidor
   - Escalar para dev com:
     * URL completa
     * Payload enviado
     * Horário do erro
     * Resposta completa
```

### 📞 Cenário 3: "Webhook Não Está Chegando"

**Cliente:** "Cadastrei webhook mas não recebo notificações"

**Diagnóstico:**

```
1. VERIFICAR URL CADASTRADA
   - URL está correta?
   - Typo? https vs http?
   - Endpoint existe?

2. TESTAR ENDPOINT MANUALMENTE
   $ curl -X POST https://cliente.com/webhook \
     -H "Content-Type: application/json" \
     -d '{"teste": true}'

   - Retorna 200? → Endpoint funciona
   - Erro? → Problema no endpoint

3. VERIFICAR FIREWALL
   - Firewall está bloqueando IP do serviço?
   - Porta 443 está aberta?

4. VERIFICAR CERTIFICADO SSL
   - Certificado é válido?
   - Não expirou?

5. VERIFICAR LOGS DO SERVIÇO
   - Webhook está sendo disparado?
   - Qual erro está retornando?
```

### 📞 Cenário 4: "Site Lento"

**Cliente:** "Site demora muito para carregar"

**Diagnóstico:**

```
1. MEDIR TEMPO DE RESPOSTA
   $ curl -w "Tempo: %{time_total}s\n" https://cliente.com

2. TESTAR DE DIFERENTES LOCAIS
   - Slow global ou só em certa região?
   - Usar: https://www.webpagetest.org

3. VERIFICAR TAMANHO DE ASSETS
   - Imagens muito grandes?
   - Muitos arquivos JS/CSS?

4. ESCALAR PARA DEV/INFRA
   Se > 3 segundos e não é problema de rede,
   escalar com:
   - URLs lentas específicas
   - Tempo de resposta
   - Região geográfica
   - Horário (sempre lento ou só pico?)
```

### 📞 Cenário 5: "API Retornando SSL: CERTIFICATE_VERIFY_FAILED"

**Cliente reporta:** "Meu sistema não está validando o certificado SSL do seu servidor"

**Diagnóstico:**

```
1. CONFIRMAR ERRO
   - Qual mensagem exata aparece?
     Ex.: "SSL: CERTIFICATE_VERIFY_FAILED", "unable to get local issuer certificate", etc.
   - Em qual linguagem/ambiente?
     Ex.: Python (requests), Node.js, Java, cURL, n8n, etc.
   - O erro ocorre sempre ou só em algum servidor/ambiente específico?

2. VERIFICAR CERTIFICADO DO NOSSO LADO
   - Rodar teste em: https://www.ssllabs.com/ssltest/
   - Conferir:
     * Certificado válido? (não expirado, CN/SAN corretos)
     * Cadeia de certificados completa?
     * Nota A ou similar no SSL Labs?
   - Se estiver OK:
     → Problema provavelmente no ambiente do cliente (pacote de certificados raiz desatualizado).

3. VALIDAR DOMÍNIO E URL
   - Confirmar com o cliente:
     * A URL usada é exatamente a do certificado?
       Ex.: `https://api.suaempresa.com` e não `https://ip` ou outro domínio.
     * Não está usando domínio antigo ou ambiente de teste com certificado diferente?

4. SE O CERTIFICADO ESTIVER CORRETO
   - Orientar o cliente a atualizar o pacote de certificados raiz do sistema onde a aplicação roda:
     * Em Linux (Debian/Ubuntu):
       - Atualizar `ca-certificates`:
         - `sudo apt-get update`
         - `sudo apt-get install --reinstall ca-certificates`
         - `sudo update-ca-certificates`
     * Em Linux (CentOS/RHEL/Alma/Rocky):
         - `sudo yum update ca-certificates -y`
         - ou `sudo dnf update ca-certificates -y`
         - `sudo update-ca-trust enable`
         - `sudo update-ca-trust extract`
     * Em containers Docker:
       - Garantir que a imagem base está atualizada
       - Reinstalar `ca-certificates` dentro do container
       - Rebuild/restart do serviço
     * Em Windows:
       - Garantir que o sistema está atualizado
       - Verificar se o certificado raiz/intermediário da autoridade (ex. Let's Encrypt)
         está presente no "Trusted Root Certification Authorities".

   - Pedir para o cliente testar diretamente com cURL no MESMO servidor onde a aplicação roda:
     * `curl -v https://api.suaempresa.com/saude`
     - Se o cURL também falhar:
       → Confirma que o problema é do sistema (CAs).
     - Se o cURL funcionar:
       → Problema provavelmente é da linguagem/lib de HTTP usada na aplicação
         (ex.: configuração específica de CA bundle ou proxy).

5. CASOS ESPECIAIS
   - Cliente usando linguagem com CA bundle próprio:
     * Python (requests):
       - Verificar se está usando `certifi` muito antigo.
       - Atualizar:
         - `pip install --upgrade certifi`
     * Node.js:
       - Verificar se não há variável `NODE_EXTRA_CA_CERTS`
         apontando para bundle antigo.
       - Atualizar Node ou bundle de CAs usado.
     * Java:
       - Verificar `cacerts` da JVM:
         - Atualizar JDK/JRE ou importar certificados raiz recentes.

6. ALTERNATIVA TEMPORÁRIA (SEGUINDO BOAS PRÁTICAS)
   - Explicar que desativar verificação SSL (ex.: `verify=False`, `rejectUnauthorized: false`)
     é INSEGURO e só deve ser usado:
     * Em ambiente de desenvolvimento
     * Por tempo limitado
   - Reforçar: solução definitiva é sempre atualizar o pacote de certificados confiáveis.

7. QUANDO ESCALAR
   - Se o SSL Labs indicar algum problema do nosso lado:
     * Certificado expirado
     * Cadeia incompleta
     * Domínio errado no certificado
   → Escalar para a equipe de infraestrutura com:
     - Domínio afetado
     - Resultado do SSL Labs
     - Horário que o cliente percebeu o problema
```

---

## 🎓 Próximos Passos de Aprendizado

### Para Aprofundar em Cada Tópico:

**DNS:**

- Praticar com `nslookup`, `dig`
- Configurar DNS em Cloudflare/Route53
- Estudar propagação e TTL

**HTTP/REST:**

- Instalar Postman, fazer requisições
- Criar conta em APIs públicas (GitHub, OpenWeather)
- Estudar códigos de status (decorar principais)

**WebSocket:**

- Testar chat em tempo real
- Usar browser DevTools para ver mensagens
- Criar projeto simples de notificações

**APIs REST:**

- Ler documentação de APIs (Stripe, Twilio)
- Praticar com curl/httpie
- Entender versionamento e paginação

**Webhooks:**

- Usar webhook.site para inspecionar
- Configurar ngrok para testes locais
- Implementar endpoint simples

**SSL/TLS:**

- Obter certificado Let's Encrypt
- Usar SSL Labs para testar
- Forçar HTTPS no servidor

**Microserviços:**

- Estudar arquitetura de empresas (Netflix, Uber)
- Aprender Docker básico
- Entender comunicação síncrona vs assíncrona

### Recursos Online Recomendados:

**Cursos Gratuitos:**

- freeCodeCamp (APIs e HTTP)
- Khan Academy (Fundamentos de Internet)
- YouTube (Traversy Media, Fireship)

**Documentações Oficiais:**

- MDN Web Docs (HTTP, WebSocket)
- Postman Learning Center
- Docker Docs
- Kubernetes Docs

**Ferramentas Para Praticar:**

- HTTPBin.org (teste de requisições HTTP)
- JSONPlaceholder (API fake para testes)
- Webhook.site (inspecionar webhooks)
- RequestBin (capturar requisições)

---

## ✅ Checklist Final para Equipe de Suporte

**Conhecimentos Essenciais:**

- [ ] Entendo o que é DNS e como troubleshoot
- [ ] Sei os métodos HTTP (GET, POST, PUT, DELETE)
- [ ] Conheço os códigos de status HTTP principais
- [ ] Sei diferença entre HTTP e HTTPS
- [ ] Entendo o que é WebSocket e quando usar
- [ ] Sei o que é API REST e como testar
- [ ] Conheço webhooks e como debugar
- [ ] Entendo SSL/TLS e certificados
- [ ] Sei conceitos básicos de microserviços

**Ferramentas Que Domino:**

- [ ] nslookup / dig (DNS)
- [ ] curl / Postman (APIs)
- [ ] Browser DevTools (Network, Console)
- [ ] ngrok (webhooks locais)
- [ ] openssl (certificados)

**Troubleshooting:**

- [ ] Sei diagnosticar "site fora do ar"
- [ ] Sei investigar erros de API
- [ ] Sei verificar problemas de DNS
- [ ] Sei identificar problemas de certificado
- [ ] Sei debugar webhooks que não chegam

---

[← Voltar ao Índice](./README.md)
