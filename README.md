# SEDUC-AM — Simulação de Inscrição Concurso 2026 

PWA de simulação de inscrições para o Concurso Público SEDUC-AM 2026.  
Deploy automático via **GitHub Actions → Firebase Hosting**.

---

## 📁 Estrutura do projeto

```
concurso-seduc/
├── public/
│   ├── index.html          ← App principal
│   ├── manifest.json       ← PWA manifest
│   ├── sw.js               ← Service Worker (offline)
│   ├── favicon.png
│   ├── apple-touch-icon.png
│   └── icon-*.png          ← Ícones 72–512px
├── .github/
│   └── workflows/
│       └── firebase-deploy.yml   ← CI/CD automático
├── firebase.json           ← Config Firebase Hosting
├── .gitignore
└── README.md
```

---

## 🚀 Configuração — passo a passo

### 1. Criar projeto no Firebase

1. Acesse [console.firebase.google.com](https://console.firebase.google.com)
2. Clique em **Adicionar projeto**
3. Dê um nome (ex.: `concurso-seduc-2026`)
4. Pode desativar o Google Analytics
5. Anote o **Project ID** (ex.: `concurso-seduc-2026`)

### 2. Ativar Firebase Hosting

No Console Firebase:
1. Menu lateral → **Hosting** → **Começar**
2. Siga o assistente (não precisa instalar nada localmente)
3. Confirme o domínio padrão: `SEU-PROJECT-ID.web.app`

### 3. Criar repositório no GitHub

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/concurso-seduc.git
git push -u origin main
```

### 4. Gerar credencial do Firebase para o GitHub

No terminal (com Node/npm instalado):

```bash
npm install -g firebase-tools
firebase login
firebase init hosting
# Quando perguntar "public directory": public
# Quando perguntar "single-page app": Yes
# NÃO sobrescreva o index.html existente

# Gerar token de serviço para o GitHub Actions
firebase init hosting:github
# Ele vai:
# - Pedir o repositório GitHub (usuario/repo)
# - Criar automaticamente os secrets no GitHub
# - Criar o workflow (já está criado aqui)
```

> ⚠️ O `firebase init hosting:github` adiciona automaticamente os secrets
> `FIREBASE_SERVICE_ACCOUNT` e configura tudo. Se preferir fazer manual, veja abaixo.

#### Alternativa manual (sem CLI)

1. No Console Firebase → ⚙️ Configurações do projeto → **Contas de serviço**
2. Clique em **Gerar nova chave privada** → baixe o JSON
3. No GitHub → Settings → Secrets → Actions → **New repository secret**:
   - `FIREBASE_SERVICE_ACCOUNT` → cole o conteúdo inteiro do JSON
   - `FIREBASE_PROJECT_ID` → ex.: `concurso-seduc-2026`

### 5. Deploy automático

A partir de agora, qualquer `git push` na branch `main` dispara o deploy:

```bash
# Editar qualquer arquivo e fazer push
git add .
git commit -m "atualização"
git push
# → GitHub Actions roda → Firebase faz deploy em ~30s
```

Acompanhe em: **GitHub → Actions** (aba do repositório)

---

## 📱 Instalar como aplicativo (PWA)

### Android (Chrome)
1. Abra o site no Chrome
2. Menu (⋮) → **Adicionar à tela inicial**
3. Confirme → ícone aparece na tela inicial

### iOS (Safari)
1. Abra o site no Safari
2. Botão de compartilhar (⬆️) → **Adicionar à Tela de Início**
3. Confirme → ícone aparece na tela inicial

### Desktop (Chrome/Edge)
1. Ícone de instalação (🖥️) na barra de endereços
2. Ou Menu → **Instalar aplicativo**

---

## 🔐 Acesso ao painel administrativo

- Dê **dois cliques** no ícone ⚙️ no canto superior direito do header
- Digite a palavra-chave (a senha é verificada via **SHA-256** — nunca fica em texto puro no código)

---

## 🌐 URL do app após deploy

```
https://SEU-PROJECT-ID.web.app
```
