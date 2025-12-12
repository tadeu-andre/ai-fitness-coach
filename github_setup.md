# 📦 Guia de Setup do Repositório GitHub

## Estrutura de Pastas do Projeto

```
ai-fitness-coach/
│
├── workflows/                          # Workflows do N8N
│   ├── ai-fitness-coach.json          # Workflow principal
│   └── ai-fitness-coach-debug.json    # Workflow de debug
│
├── interface/                          # Interface web
│   ├── index.html                     # Chat UI
│   └── assets/
│       ├── screenshot-chat.png        # Screenshot da interface
│       ├── screenshot-workflow.png    # Screenshot do N8N
│       └── logo.png                   # Logo (opcional)
│
├── docs/                              # Documentação adicional
│   ├── ARCHITECTURE.md                # Arquitetura detalhada
│   ├── API.md                        # Documentação da API
│   └── DEPLOYMENT.md                  # Guia de deployment
│
├── examples/                          # Exemplos de uso
│   ├── conversation-examples.md       # Exemplos de conversas
│   ├── api-requests.http             # Exemplos de requests
│   └── postman-collection.json       # Coleção Postman
│
├── video/                            # Vídeo de demonstração
│   ├── demo-video.mp4                # Vídeo principal (1-3 min)
│   └── ROTEIRO-VIDEO.md              # Roteiro usado
│
├── scripts/                          # Scripts utilitários (opcional)
│   ├── test-webhook.sh               # Script para testar webhook
│   └── setup-credentials.sh          # Script de setup
│
├── .gitignore                        # Arquivos ignorados
├── README.md                         # Documentação principal
├── LICENSE                           # Licença MIT
└── CONTRIBUTING.md                   # Guia de contribuição (opcional)
```

---

## 🚀 Passo a Passo para Criar o Repositório

### 1. Criar Repositório no GitHub

1. Acesse [github.com](https://github.com)
2. Clique em **"New repository"**
3. Configure:
   - **Repository name:** `ai-fitness-coach`
   - **Description:** `🏋️ Personal trainer virtual baseado em IA usando N8N e Groq API (LLaMA 3.3)`
   - **Visibility:** Public
   - **Initialize:** ✅ Add a README file
   - **Add .gitignore:** None (vamos adicionar manualmente)
   - **Choose a license:** MIT License
4. Clique em **"Create repository"**

### 2. Clonar Repositório Localmente

```bash
git clone https://github.com/seu-usuario/ai-fitness-coach.git
cd ai-fitness-coach
```

### 3. Criar Estrutura de Pastas

```bash
# Criar todas as pastas
mkdir -p workflows interface/assets docs examples video scripts

# Criar arquivos vazios para commitar estrutura
touch workflows/.gitkeep
touch interface/assets/.gitkeep
touch docs/.gitkeep
touch examples/.gitkeep
touch video/.gitkeep
touch scripts/.gitkeep
```

### 4. Adicionar Arquivos

#### 4.1. Copiar Workflow do N8N

1. No N8N, abra seu workflow
2. Clique nos **"..."** → **"Download"**
3. Salve como `workflows/ai-fitness-coach.json`

#### 4.2. Criar Interface Web

Copie o arquivo HTML que criamos:
```bash
# Cole o conteúdo no arquivo
nano interface/index.html
# ou use seu editor preferido
```

#### 4.3. Adicionar Documentação

Copie os arquivos markdown que criamos:
- `README.md` (raiz)
- `docs/ARCHITECTURE.md`
- `video/ROTEIRO-VIDEO.md`
- `.gitignore`

#### 4.4. Tirar Screenshots

1. **Screenshot da Interface:**
   - Abra a interface do chat
   - Faça uma conversa de exemplo
   - Tire screenshot (full page)
   - Salve como `interface/assets/screenshot-chat.png`

2. **Screenshot do Workflow:**
   - Abra o workflow no N8N
   - Mostre todos os nós conectados
   - Tire screenshot
   - Salve como `interface/assets/screenshot-workflow.png`

### 5. Commit e Push

```bash
# Adicionar todos os arquivos
git add .

# Commit inicial
git commit -m "feat: Initial commit - AI Fitness Coach project"

# Push para GitHub
git push origin main
```

---

## 📸 Como Tirar Screenshots de Qualidade

### Ferramentas Recomendadas

**Windows:**
- **Snipping Tool** (nativo)
- **ShareX** (gratuito, avançado)
- **Lightshot** (simples)

**Mac:**
- **Cmd + Shift + 4** (nativo)
- **CleanShot X** (pago, profissional)
- **Monosnap** (gratuito)

**Linux:**
- **Flameshot** (recomendado)
- **GNOME Screenshot** (nativo)
- **Shutter** (avançado)

### Dicas para Boas Screenshots

1. **Resolução:** Mínimo 1920x1080
2. **Formato:** PNG (melhor qualidade) ou JPG
3. **Conteúdo:**
   - Mostre a aplicação inteira ou a parte relevante
   - Evite informações sensíveis (API keys, emails)
   - Use modo claro ou escuro consistente
4. **Edição:**
   - Adicione setas ou destaques se necessário
   - Recorte partes irrelevantes
   - Mantenha proporção 16:9 quando possível

---

## 🎥 Preparar Vídeo de Demonstração

### Opção 1: Gravar e Fazer Upload

```bash
# Após gravar o vídeo (seguindo o roteiro)
# Salve como: video/demo-video.mp4

# ATENÇÃO: Vídeos grandes podem ser problemáticos no Git
# Recomendações:
# - Mantenha abaixo de 50MB se possível
# - Comprima o vídeo sem perder muita qualidade
# - Considere usar Git LFS ou hospedar externamente
```

### Opção 2: Hospedar Externamente (Recomendado)

```markdown
# No README.md, adicione link externo:

## 🎥 Demonstração

Assista ao vídeo de demonstração completo:

[![Vídeo de Demonstração](https://img.youtube.com/vi/VIDEO_ID/0.jpg)](https://www.youtube.com/watch?v=VIDEO_ID)

**Ou acesse diretamente:**
- YouTube: [youtu.be/VIDEO_ID](https://youtu.be/VIDEO_ID)
- Loom: [loom.com/share/VIDEO_ID](https://loom.com/share/VIDEO_ID)
- Google Drive: [Assistir Vídeo](https://drive.google.com/...)
```

### Plataformas para Hospedar Vídeo

1. **YouTube** (recomendado)
   - Upload ilimitado
   - Boa qualidade
   - Incorporável

2. **Loom** (profissional)
   - Fácil de gravar e compartilhar
   - Boa para screencasts
   - Limite no plano gratuito

3. **Google Drive**
   - Boa qualidade
   - Controle de acesso
   - 15GB grátis

4. **Vimeo**
   - Alta qualidade
   - Sem anúncios
   - Limite semanal no gratuito

---

## 📝 Criar Arquivo de Exemplos

### examples/conversation-examples.md

```markdown
# Exemplos de Conversas

## Exemplo 1: Usuário Iniciante - Hipertrofia

**Usuário:** Olá, quero começar a treinar

**Bot:** Olá! 👋 Ótimo que você quer começar! Qual é seu objetivo principal?
🎯 Ganhar massa muscular
🔥 Perder peso
💪 Ganhar força
🏃 Melhorar condicionamento

**Usuário:** Quero ganhar massa muscular

**Bot:** Perfeito! 💪 Para criar um plano ideal, preciso de algumas informações:
Quantos anos você tem e qual seu peso atual?

[... continuar exemplo completo ...]

---

## Exemplo 2: Usuário Avançado - Emagrecimento

[... adicionar outro exemplo ...]

---

## Exemplo 3: Consulta sobre Nutrição

[... adicionar exemplo ...]
```

### examples/api-requests.http

```http
### Requisição 1: Primeira Mensagem
POST https://seu-n8n.app.n8n.cloud/webhook/fitness-working
Content-Type: application/json

{
  "message": "Olá, quero começar a treinar",
  "session_id": "user_001"
}

###

### Requisição 2: Responder Objetivo
POST https://seu-n8n.app.n8n.cloud/webhook/fitness-working
Content-Type: application/json

{
  "message": "Quero ganhar massa muscular",
  "session_id": "user_001",
  "history": [
    {"role": "user", "content": "Olá, quero começar a treinar"},
    {"role": "assistant", "content": "..."}
  ]
}

###
```

---

## 🏷️ Configurar Tags e Topics no GitHub

No GitHub, adicione **Topics** ao repositório:

```
n8n
ai
chatbot
fitness
llama
groq
personal-trainer
workflow-automation
javascript
conversational-ai
```

**Como adicionar:**
1. Vá na página do repositório
2. Clique em ⚙️ ao lado de "About"
3. Adicione os topics
4. Salve

---

## ⭐ Adicionar Badges ao README

No topo do `README.md`, adicione badges:

```markdown
[![N8N](https://img.shields.io/badge/N8N-Workflow-EA4B71)](https://n8n.io/)
[![Groq](https://img.shields.io/badge/Groq-LLaMA_3.3-6B46C1)](https://groq.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
```

---

## 📋 Checklist Final

Antes de considerar o projeto completo, verifique:

### Estrutura
- [ ] Todas as pastas criadas
- [ ] Workflow JSON exportado e incluído
- [ ] Interface HTML incluída
- [ ] Screenshots adicionadas
- [ ] Vídeo gravado e incluído/linkado

### Documentação
- [ ] README.md completo e claro
- [ ] ARCHITECTURE.md detalhado
- [ ] Roteiro do vídeo incluído
- [ ] Exemplos de uso adicionados
- [ ] .gitignore configurado

### GitHub
- [ ] Repositório público criado
- [ ] README renderizando corretamente
- [ ] Screenshots aparecendo
- [ ] Topics/tags configuradas
- [ ] License incluída (MIT)

### Funcionalidade
- [ ] Workflow funcionando no N8N
- [ ] Interface testada e funcionando
- [ ] Vídeo demonstra todos os fluxos
- [ ] Instruções de instalação claras

### Qualidade
- [ ] Sem credenciais commitadas
- [ ] Código comentado onde necessário
- [ ] Screenshots de boa qualidade
- [ ] Vídeo com boa qualidade (áudio e vídeo)

---

## 🚀 Comandos Git Úteis

```bash
# Ver status
git status

# Adicionar arquivo específico
git add workflows/ai-fitness-coach.json

# Adicionar todos os arquivos
git add .

# Commit com mensagem
git commit -m "docs: add architecture documentation"

# Push para GitHub
git push origin main

# Criar nova branch
git checkout -b feature/melhorias

# Ver histórico
git log --oneline

# Desfazer último commit (mantendo mudanças)
git reset --soft HEAD~1

# Ver diferenças
git diff
```

---

## 🎯 Mensagens de Commit Sugeridas

Use prefixos para organizar commits:

```bash
# Features
git commit -m "feat: add workout plan generation"

# Documentação
git commit -m "docs: update README with installation steps"

# Correções
git commit -m "fix: resolve CORS issue in webhook"

# Melhorias
git commit -m "refactor: optimize message processing"

# Estilo
git commit -m "style: format code and improve readability"

# Testes
git commit -m "test: add API request examples"
```

---

## 📧 Compartilhar Projeto

Depois de tudo pronto:

1. **Compartilhe o link:**
   ```
   https://github.com/seu-usuario/ai-fitness-coach
   ```

2. **Crie um README.md atrativo** (já feito ✅)

3. **Adicione ao seu portfólio**

4. **Compartilhe nas redes sociais:**
   - LinkedIn
   - Twitter/X
   - Dev.to
   - Reddit (r/n8n)

5. **Submeta para showcases:**
   - N8N Community Workflows
   - Product Hunt (se aplicável)
   - Groq Community Showcase

---

Pronto! Seu projeto está completo e profissional! 🎉