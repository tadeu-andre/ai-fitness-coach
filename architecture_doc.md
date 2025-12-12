# 🏗️ Arquitetura do Sistema - AI Fitness Coach

## Visão Geral

O AI Fitness Coach utiliza uma arquitetura baseada em **microserviços orquestrados via N8N**, com integração de IA através da **Groq API** (modelo LLaMA 3.3 70B).

---

## Componentes Principais

### 1. **Camada de Interface (Frontend)**

```
┌─────────────────────────────────┐
│     Interface Web (HTML/JS)     │
│  - Chat UI                      │
│  - Gestão de histórico local    │
│  - Formatação de mensagens      │
└─────────────┬───────────────────┘
              │ HTTP POST
              ▼
```

**Responsabilidades:**
- Captura de input do usuário
- Renderização de mensagens formatadas (markdown, emojis)
- Gerenciamento de sessões locais
- Tratamento de erros de conexão

**Tecnologias:**
- HTML5 Semântico
- CSS3 (Flexbox, Animations)
- JavaScript ES6+ (Fetch API, Promises)

---

### 2. **Camada de Orquestração (N8N)**

```
┌─────────────────────────────────┐
│       N8N Workflow Engine       │
│                                 │
│  ┌───────────────────────────┐ │
│  │   1. Webhook Receiver     │ │
│  └───────────┬───────────────┘ │
│              │                  │
│  ┌───────────▼───────────────┐ │
│  │  2. Message Processor     │ │
│  │  - Parse request          │ │
│  │  - Build conversation     │ │
│  │  - Manage history         │ │
│  └───────────┬───────────────┘ │
│              │                  │
│  ┌───────────▼───────────────┐ │
│  │  3. Request Preparer      │ │
│  │  - Format parameters      │ │
│  │  - Validate data          │ │
│  └───────────┬───────────────┘ │
│              │                  │
│  ┌───────────▼───────────────┐ │
│  │  4. Groq API Client       │ │
│  │  - HTTP Request           │ │
│  │  - Auth management        │ │
│  └───────────┬───────────────┘ │
│              │                  │
│  ┌───────────▼───────────────┐ │
│  │  5. Response Formatter    │ │
│  │  - Parse AI response      │ │
│  │  - Update history         │ │
│  │  - Add metadata           │ │
│  └───────────┬───────────────┘ │
│              │                  │
│  ┌───────────▼───────────────┐ │
│  │  6. Webhook Responder     │ │
│  │  - Send JSON response     │ │
│  │  - Set CORS headers       │ │
│  └───────────────────────────┘ │
└─────────────────────────────────┘
```

#### Detalhamento dos Nós

**Nó 1: Webhook Receiver**
- **Tipo:** `n8n-nodes-base.webhook`
- **Função:** Receber requisições HTTP POST
- **Configuração:**
  ```json
  {
    "httpMethod": "POST",
    "path": "fitness-working",
    "responseMode": "responseNode"
  }
  ```

**Nó 2: Message Processor (Code Node)**
- **Tipo:** `n8n-nodes-base.code`
- **Função:** Processar mensagem e construir contexto
- **Lógica:**
  ```javascript
  // Extrai dados da requisição
  const body = $input.first().json.body;
  const userMessage = body.message;
  const chatHistory = body.history || [];
  const sessionId = body.session_id || 'session_' + Date.now();
  
  // Constrói array de mensagens com contexto
  const messages = [
    systemPrompt,
    ...chatHistory.slice(-8),  // Últimas 8 mensagens
    { role: 'user', content: userMessage }
  ];
  ```

**Nó 3: Request Preparer (Set Node)**
- **Tipo:** `n8n-nodes-base.set`
- **Função:** Organizar parâmetros para API
- **Campos:**
  - `model`: String
  - `temperature`: Number (0.8)
  - `max_tokens`: Number (1500)
  - `messages`: Array

**Nó 4: Groq API Client (HTTP Request)**
- **Tipo:** `n8n-nodes-base.httpRequest`
- **Função:** Chamada para Groq API
- **Configuração:**
  ```json
  {
    "method": "POST",
    "url": "https://api.groq.com/openai/v1/chat/completions",
    "authentication": "groqApi",
    "body": {
      "model": "llama-3.3-70b-versatile",
      "temperature": 0.8,
      "max_tokens": 1500,
      "messages": "{{ $json.messages }}"
    }
  }
  ```

**Nó 5: Response Formatter (Code Node)**
- **Tipo:** `n8n-nodes-base.code`
- **Função:** Processar resposta e atualizar histórico
- **Output:**
  ```json
  {
    "success": true,
    "message": "Resposta da IA",
    "session_id": "session_123",
    "history": [...],
    "timestamp": "ISO 8601",
    "tokens_used": 450
  }
  ```

**Nó 6: Webhook Responder**
- **Tipo:** `n8n-nodes-base.respondToWebhook`
- **Função:** Retornar resposta formatada
- **Headers CORS:**
  ```
  Access-Control-Allow-Origin: *
  Access-Control-Allow-Methods: POST, OPTIONS
  Access-Control-Allow-Headers: Content-Type
  Content-Type: application/json
  ```

---

### 3. **Camada de IA (Groq API)**

```
┌─────────────────────────────────┐
│         Groq Cloud API          │
│                                 │
│  ┌───────────────────────────┐ │
│  │   LLaMA 3.3 70B Model     │ │
│  │   - Inferência rápida     │ │
│  │   - Suporte a 128k tokens │ │
│  │   - Multi-turn chat       │ │
│  └───────────────────────────┘ │
└─────────────────────────────────┘
```

**Características do Modelo:**
- **Modelo:** LLaMA 3.3 70B Versatile
- **Contexto:** 128k tokens
- **Velocidade:** ~300 tokens/segundo
- **Temperatura:** 0.8 (balanceada)
- **Max Tokens:** 1500 por resposta

---

## Fluxo de Dados Detalhado

### Request Flow

```
1. USER INPUT
   ↓
   {
     "message": "Quero ganhar massa",
     "session_id": "abc123",
     "history": []
   }

2. WEBHOOK RECEIVE
   ↓
   - Valida método POST
   - Extrai body da requisição

3. MESSAGE PROCESSING
   ↓
   - Extrai: message, session_id, history
   - Adiciona system prompt
   - Constrói array de mensagens
   - Limita histórico a 8 mensagens

4. REQUEST PREPARATION
   ↓
   {
     "model": "llama-3.3-70b-versatile",
     "temperature": 0.8,
     "max_tokens": 1500,
     "messages": [
       { role: "system", content: "..." },
       { role: "user", content: "Quero ganhar massa" }
     ]
   }

5. GROQ API CALL
   ↓
   - Autenticação via Bearer token
   - POST para /v1/messages
   - Timeout: 30s

6. AI PROCESSING (Groq)
   ↓
   - Inferência do modelo
   - Geração de resposta
   - Contagem de tokens

7. RESPONSE FORMATTING
   ↓
   {
     "success": true,
     "message": "Ótimo objetivo! Para ganhar massa...",
     "session_id": "abc123",
     "history": [
       { role: "user", content: "Quero ganhar massa" },
       { role: "assistant", content: "Ótimo objetivo!..." }
     ],
     "timestamp": "2025-12-12T19:45:00Z",
     "tokens_used": 342
   }

8. WEBHOOK RESPONSE
   ↓
   - Adiciona headers CORS
   - Retorna JSON para cliente
```

---

## Gerenciamento de Estado

### Session Management

```javascript
// Cada usuário tem um session_id único
const sessionId = body.session_id || 'session_' + Date.now();

// Histórico é mantido no lado do cliente
const chatHistory = body.history || [];

// A cada interação, o histórico é enviado completo
// e retornado atualizado para o cliente
```

### Conversation History

```javascript
// Estrutura do histórico
[
  { role: "user", content: "Mensagem 1" },
  { role: "assistant", content: "Resposta 1" },
  { role: "user", content: "Mensagem 2" },
  { role: "assistant", content: "Resposta 2" }
]

// Limitação: últimas 8 mensagens (4 turnos)
const recentHistory = chatHistory.slice(-8);
```

### Context Window Management

- **Limite por requisição:** 1500 tokens
- **Histórico mantido:** 8 mensagens (≈ 4 turnos)
- **System prompt:** ≈ 200 tokens
- **Espaço disponível para resposta:** ≈ 1000 tokens

---

## Padrões de Design Aplicados

### 1. **Pipeline Pattern**
Cada nó do N8N representa um estágio do pipeline de processamento.

### 2. **Strategy Pattern**
System prompt define estratégia de resposta da IA.

### 3. **Stateless Design**
Cada requisição carrega todo o contexto necessário.

### 4. **Error Handling**
```javascript
try {
  // Processamento
} catch (error) {
  return {
    success: false,
    error: error.message
  };
}
```

---

## Segurança

### Autenticação
- **Groq API:** Bearer token via credencial N8N
- **Webhook:** Público (pode ser protegido com auth)

### CORS
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, OPTIONS
Access-Control-Allow-Headers: Content-Type
```

### Rate Limiting
- **Groq API:** 30 requests/minuto (free tier)
- **N8N:** Configurável por plano

### Validação de Input
```javascript
if (!userMessage) {
  throw new Error('Mensagem não pode estar vazia');
}
```

---

## Performance

### Métricas Típicas

- **Latência total:** 1-3 segundos
  - Webhook processing: 10-50ms
  - Groq API call: 800-2500ms
  - Response formatting: 10-30ms

- **Throughput:** 30 req/min (limitado pela API)

### Otimizações Aplicadas

1. **História limitada:** Reduz payload e processamento
2. **Temperature balanceada (0.8):** Equilíbrio entre criatividade e latência
3. **Max tokens (1500):** Respostas concisas e rápidas
4. **Async processing:** Não bloqueia outros workflows

---

## Escalabilidade

### Limitações Atuais

- **Groq Free Tier:** 30 req/min
- **N8N Cloud:** Limitado por plano
- **State management:** Cliente-side (não persiste entre sessões)

### Melhorias Futuras

1. **Banco de dados:** PostgreSQL para persistência
2. **Cache:** Redis para históricos frequentes
3. **Load balancing:** Múltiplas instâncias N8N
4. **Queue system:** RabbitMQ para processar em background
5. **CDN:** Servir interface estática

---

## Monitoramento

### Logs do N8N

```json
{
  "executionId": "abc123",
  "workflowName": "AI Fitness Coach",
  "status": "success",
  "duration": 2340,
  "timestamp": "2025-12-12T19:45:00Z",
  "nodesExecuted": 6
}
```

### Métricas Importantes

- Taxa de sucesso das requisições
- Latência média de resposta
- Tokens consumidos
- Erros por tipo

---

## Diagramas Complementares

### Sequence Diagram

```
Usuário          Interface       N8N Workflow      Groq API
   |                |                  |               |
   |--"Mensagem"--->|                  |               |
   |                |--POST /webhook-->|               |
   |                |                  |--Process----> |
   |                |                  |<--Response----|
   |                |<--JSON Response--|               |
   |<--Renderiza----|                  |               |
```

### State Diagram

```
[Início]
   |
   v
[Aguardando Input] --> [Processando] --> [Aguardando IA]
                          ^    |              |
                          |    |              v
                          |    |         [Recebendo]
                          |    |              |
                          |    v              v
                          [Formatando] <-- [Erro]
                                |
                                v
                          [Respondendo]
                                |
                                v
                            [Fim]
```

---

## Conclusão

Esta arquitetura foi projetada para ser:

- ✅ **Simples:** Fácil de entender e manter
- ✅ **Escalável:** Pode crescer conforme necessidade
- ✅ **Modular:** Cada componente é independente
- ✅ **Testável:** Nós podem ser testados isoladamente
- ✅ **Resiliente:** Tratamento robusto de erros

A escolha do N8N como orquestrador permite rápida prototipagem e fácil manutenção, enquanto a Groq API oferece inferência de IA de alta performance a baixo custo.