# 🏋️ AI Fitness Coach - Personal Trainer Virtual

> Sistema inteligente de coaching fitness baseado em IA, desenvolvido com N8N e Groq API (LLaMA 3.3)

[![N8N](https://img.shields.io/badge/N8N-Workflow-EA4B71)](https://n8n.io/)
[![Groq](https://img.shields.io/badge/Groq-LLaMA_3.3-6B46C1)](https://groq.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## [VIDEO DE APRESENTAÇÃO](#video-de-apresentacao)

https://www.youtube.com/watch?v=YB7xuAlFBE0

<p align="center">
  <a href="https://www.youtube.com/watch?v=YB7xuAlFBE0">
    <img src="https://img.youtube.com/vi/YB7xuAlFBE0/0.jpg" width="600">
  </a>
</p>

<p align="center">
  ▶️ Clique na imagem para assistir à demonstração
</p>


## 📋 Índice
- [Sobre o Projeto](#sobre-o-projeto)
- [Funcionalidades](#funcionalidades)
- [Arquitetura](#arquitetura)
- [Requisitos](#requisitos)
- [Instalação](#instalação)
- [Configuração](#configuração)
- [Como Usar](#como-usar)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Fluxos do Sistema](#fluxos-do-sistema)
- [Demonstração](#demonstração)
- [Tecnologias](#tecnologias)
- [Contribuição](#contribuição)
- [Licença](#licença)

---

## 🎯 Sobre o Projeto

O **AI Fitness Coach** é um chatbot inteligente que atua como personal trainer virtual, oferecendo:

- 💬 Conversação natural sobre fitness e saúde
- 🎯 Coleta de informações do usuário (idade, peso, objetivos, disponibilidade)
- 📋 Criação de planos de treino personalizados
- 💡 Dicas de nutrição e motivação
- 📊 Acompanhamento de progresso através de histórico de conversas

O sistema utiliza **N8N** para orquestração de workflows e **Groq API** (modelo LLaMA 3.3 70B) para processamento de linguagem natural.

---

## ✨ Funcionalidades

### 🤖 Chatbot Inteligente
- Conversação contextualizada com memória de histórico
- Respostas personalizadas baseadas no perfil do usuário
- Linguagem motivadora e profissional

### 📝 Coleta de Informações
- Idade, peso e altura
- Objetivo fitness (hipertrofia, emagrecimento, força, condicionamento)
- Disponibilidade de treino (dias por semana)
- Nível de experiência

### 🏋️ Geração de Planos de Treino
- Divisão de treino adequada (2 a 6 dias/semana)
- Exercícios específicos por grupo muscular
- Séries, repetições e tempo de descanso
- Progressão de carga personalizada

### 💪 Suporte Contínuo
- Dicas de nutrição
- Orientações sobre recuperação
- Motivação e acompanhamento

---

## 🏗️ Arquitetura

### Diagrama do Workflow N8N

```
┌─────────────┐
│   Webhook   │ ← Recebe requisição POST com mensagem do usuário
└──────┬──────┘
       │
       ▼
┌─────────────────────┐
│ Processar Mensagem  │ ← Extrai dados e constrói histórico de conversa
└──────┬──────────────┘
       │
       ▼
┌─────────────────────┐
│  Preparar Request   │ ← Organiza parâmetros para API (model, temp, tokens)
└──────┬──────────────┘
       │
       ▼
┌─────────────────────┐
│   Chamar Groq API   │ ← Envia mensagens para LLaMA 3.3 via Groq
└──────┬──────────────┘
       │
       ▼
┌─────────────────────┐
│  Formatar Resposta  │ ← Processa resposta e atualiza histórico
└──────┬──────────────┘
       │
       ▼
┌─────────────────────┐
│     Responder       │ ← Retorna JSON com resposta da IA
└─────────────────────┘
```

### Fluxo de Dados

```json
{
  "input": {
    "message": "Quero ganhar massa muscular",
    "session_id": "session_123",
    "history": []
  },
  "output": {
    "success": true,
    "message": "Ótimo objetivo! Para criar um plano ideal...",
    "session_id": "session_123",
    "history": [...],
    "timestamp": "2025-12-12T19:30:00Z",
    "tokens_used": 450
  }
}
```

---

## 📦 Requisitos

### Software Necessário

- **N8N** (versão 1.0+)
  - N8N Cloud OU
  - N8N Self-hosted
- **Conta Groq** com API Key válida
- Navegador web moderno (Chrome, Firefox, Safari, Edge)

### APIs e Credenciais

- **Groq API Key**: Obtenha em [console.groq.com/keys](https://console.groq.com/keys)

---

## 🚀 Instalação

### Opção 1: N8N Cloud (Recomendado)

1. **Crie uma conta no N8N Cloud**
   - Acesse: [n8n.io/cloud](https://n8n.io/cloud)
   - Faça login ou crie uma conta gratuita

2. **Clone este repositório**
   ```bash
   git clone https://github.com/seu-usuario/ai-fitness-coach.git
   cd ai-fitness-coach
   ```

3. **Importe o workflow**
   - No N8N, clique em **"Add workflow"**
   - Vá em **"..." (menu)** → **"Import from File"**
   - Selecione o arquivo `workflows/ai-fitness-coach.json`

### Opção 2: N8N Self-hosted

1. **Instale o N8N**
   ```bash
   npm install -g n8n
   ```

2. **Inicie o N8N**
   ```bash
   n8n start
   ```

3. **Acesse o N8N**
   - Abra o navegador em `http://localhost:5678`

4. **Importe o workflow**
   - Siga os mesmos passos da Opção 1

---

## ⚙️ Configuração

### 1. Configurar Credencial da Groq

1. No N8N, vá em **Settings** → **Credentials**
2. Clique em **"Add Credential"**
3. Selecione **"Groq API"**
4. Cole sua **API Key** da Groq
5. Clique em **"Save"**

### 2. Configurar o Workflow

1. Abra o workflow importado
2. Clique no nó **"Chamar Groq"**
3. Certifique-se que a credencial está selecionada
4. Salve o workflow

### 3. Ativar o Workflow

1. No canto superior direito, **ative o toggle** (deve ficar verde)
2. Copie a **URL do webhook** que será gerada
3. Exemplo: `https://seu-n8n.app.n8n.cloud/webhook/fitness-working`

### 4. Configurar a Interface Web

1. Abra o arquivo `interface/index.html` no navegador
2. Cole a URL do webhook no campo correspondente
3. Clique em **"Testar Conexão"**
4. Se conectar com sucesso, você está pronto! ✅

---

## 💻 Como Usar

### Via Interface Web

1. Abra `interface/index.html` no navegador
2. Configure a URL do webhook
3. Comece a conversar com o FitCoach AI!

**Exemplo de conversa:**
```
Usuário: Olá, quero começar a treinar
Bot: Olá! 👋 Ótimo que você quer começar! Qual é seu objetivo principal?

Usuário: Quero ganhar massa muscular
Bot: Perfeito! 💪 Para criar um plano ideal, preciso de algumas informações...

Usuário: Tenho 25 anos, peso 75kg e posso treinar 4x por semana
Bot: Ótimo! Com essas informações, posso criar um plano de treino...
```

### Via API (cURL)

```bash
curl -X POST https://seu-n8n.app.n8n.cloud/webhook/fitness-working \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Quero ganhar massa muscular",
    "session_id": "user_123",
    "history": []
  }'
```

**Resposta:**
```json
{
  "success": true,
  "message": "Ótimo objetivo! Para criar um plano ideal...",
  "session_id": "user_123",
  "history": [...],
  "timestamp": "2025-12-12T19:30:00Z",
  "tokens_used": 450
}
```

### Via Postman ou Insomnia

1. Método: **POST**
2. URL: `https://seu-n8n.app.n8n.cloud/webhook/fitness-working`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
  "message": "Olá!",
  "session_id": "test_123"
}
```

---

## 📁 Estrutura do Projeto

```
ai-fitness-coach/
├── workflows/
│   ├── ai-fitness-coach.json          # Workflow principal do N8N
│   └── ai-fitness-coach-debug.json    # Workflow de debug/teste
├── interface/
│   ├── index.html                     # Interface web do chatbot
│   └── assets/
│       └── screenshot.png             # Screenshot da interface
├── docs/
│   ├── ARCHITECTURE.md                # Documentação detalhada da arquitetura
│   ├── API.md                         # Documentação da API
│   └── DEPLOYMENT.md                  # Guia de deployment
├── examples/
│   ├── conversation-examples.md       # Exemplos de conversas
│   └── api-requests.http              # Exemplos de requests HTTP
├── video/
│   └── demo-video.mp4                 # Vídeo de demonstração
├── README.md                          # Este arquivo
├── LICENSE                            # Licença do projeto
└── .gitignore                         # Arquivos ignorados pelo Git
```

---

## 🔄 Fluxos do Sistema

### Fluxo 1: Primeira Interação
```
Usuário envia mensagem
  ↓
Sistema processa e identifica que não tem informações
  ↓
Bot pergunta sobre objetivos
  ↓
Usuário responde
  ↓
Bot coleta dados progressivamente
```

### Fluxo 2: Criação de Plano de Treino
```
Usuário fornece todas as informações necessárias
  ↓
Sistema processa: idade, peso, objetivo, disponibilidade
  ↓
Bot gera plano personalizado com:
  - Divisão de treino
  - Exercícios específicos
  - Séries e repetições
  - Tempo de descanso
  - Dicas de progressão
```

### Fluxo 3: Consulta e Dicas
```
Usuário faz pergunta sobre nutrição/treino
  ↓
Sistema busca no contexto da conversa
  ↓
Bot responde com dicas personalizadas baseadas no perfil
```

### Fluxo 4: Acompanhamento
```
Usuário retorna em sessão futura
  ↓
Sistema recupera histórico via session_id
  ↓
Bot continua conversa com contexto mantido
```

---

## 🎥 Demonstração

### Vídeo de Demonstração

**Localização:** `video/demo-video.mp4`

**Conteúdo do vídeo (1-3 minutos):**

1. **Introdução (0:00-0:15)**
   - Apresentação do projeto
   - Objetivo do sistema

2. **Configuração (0:15-0:45)**
   - Mostrando o workflow no N8N
   - Configuração da credencial Groq
   - Ativação do workflow

3. **Demonstração de Uso (0:45-2:30)**
   - **Fluxo 1:** Primeira conversa - coleta de informações
   - **Fluxo 2:** Geração de plano de treino personalizado
   - **Fluxo 3:** Consulta de dicas de nutrição
   - **Fluxo 4:** Continuação da conversa com histórico

4. **Conclusão (2:30-3:00)**
   - Recapitulação das funcionalidades
   - Benefícios do sistema

### Screenshots

![Interface do Chatbot](interface/assets/screenshot.png)

---

## 🛠️ Tecnologias

### Backend (N8N Workflow)
- **N8N** - Plataforma de automação de workflows
- **Node.js** - Runtime JavaScript
- **JavaScript (ES6+)** - Lógica de processamento

### IA e NLP
- **Groq API** - Inferência de IA de alta performance
- **LLaMA 3.3 70B** - Modelo de linguagem (via Groq)
- **Temperature: 0.8** - Balanço entre criatividade e coerência
- **Max Tokens: 1500** - Tamanho máximo de resposta

### Frontend
- **HTML5** - Estrutura da interface
- **CSS3** - Estilização responsiva
- **JavaScript Vanilla** - Interatividade
- **Fetch API** - Comunicação com backend

### Integrações
- **Webhook** - Endpoint HTTP para receber mensagens
- **REST API** - Comunicação via JSON
- **CORS** - Configurado para acesso cross-origin

---

## 🤝 Contribuição

Contribuições são bem-vindas! Para contribuir:

1. Faça um **fork** do projeto
2. Crie uma **branch** para sua feature (`git checkout -b feature/MinhaFeature`)
3. **Commit** suas mudanças (`git commit -m 'Adiciona MinhaFeature'`)
4. **Push** para a branch (`git push origin feature/MinhaFeature`)
5. Abra um **Pull Request**

### Melhorias Futuras Sugeridas

- [ ] Integração com banco de dados para persistência
- [ ] Sistema de autenticação de usuários
- [ ] Dashboard de métricas e progresso
- [ ] Integração com wearables (Fitbit, Apple Watch)
- [ ] Versão mobile (React Native)
- [ ] Geração de PDFs com planos de treino
- [ ] Sistema de notificações (lembretes de treino)
- [ ] Análise de fotos para acompanhamento visual

---

## 📄 Licença

Este projeto está sob a licença **MIT**. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

## 👨‍💻 Autor

**Seu Nome**
- GitHub: [@seu-usuario](https://github.com/seu-usuario)
- LinkedIn: [Seu Perfil](https://linkedin.com/in/seu-perfil)
- Email: seu.email@example.com

---

## 🙏 Agradecimentos

- **N8N** pela plataforma incrível de automação
- **Groq** pela API de inferência de alta performance
- **Meta AI** pelo modelo LLaMA 3.3
- Comunidade open-source

---

## 📞 Suporte

Se você tiver dúvidas ou problemas:

1. Verifique a [documentação completa](docs/)
2. Abra uma [issue no GitHub](https://github.com/seu-usuario/ai-fitness-coach/issues)
3. Entre em contato: seu.email@example.com

---

<div align="center">

**Desenvolvido com 💪 e ☕**

[⬆ Voltar ao topo](#-ai-fitness-coach---personal-trainer-virtual)

</div>
