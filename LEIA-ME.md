# Sudoku OLED — Guia completo (sem saber programar)

## O que tem nesta pasta

- **www/index.html** — o jogo completo (já com salvamento no aparelho via
  localStorage). Abra no navegador pra testar.
- **README.md** — a "vitrine" do repositório no GitHub, com o botão de download.
- **.github/workflows/build-apk.yml** — a mágica: faz o GitHub compilar o APK
  sozinho, na nuvem, de graça. Você não precisa de Android Studio.
- **package.json / capacitor.config.json** — configuração do empacotador
  (Capacitor) que transforma o site em app Android.
- **CLAUDE.md** — contexto do projeto para o Claude Code continuar o trabalho.
- **LEIA-ME.md** — este guia.

## 🚀 Publicar no GitHub com download de APK (passo a passo)

### 1. Criar o repositório
1. Crie uma conta em https://github.com (grátis).
2. Clique em **New repository** → nome: `sudoku-oled` → **Public** → Create.

### 2. Subir os arquivos
Jeito mais fácil, sem instalar nada:
1. Na página do repositório, clique em **uploading an existing file**.
2. Arraste TODO o conteúdo desta pasta (incluindo as pastas `www` e `.github` —
   se o site não deixar arrastar pastas, use o GitHub Desktop:
   https://desktop.github.com, que sobe tudo com 2 cliques).
3. Clique em **Commit changes**.

⚠️ A pasta `.github` é essencial — é ela que contém a automação do APK.
Arquivos que começam com ponto podem ficar ocultos no seu computador; ative
"mostrar arquivos ocultos" no explorador de arquivos.

### 3. Gerar o primeiro APK
1. No repositório, aba **Actions** → workflow **Build APK** → botão
   **Run workflow** → Run.
2. Aguarde uns 5-8 minutos (fica um bolinha amarela girando; vira ✅ quando pronto).
3. O APK aparece na própria página da execução, em **Artifacts**.

### 4. Publicar uma versão para as pessoas baixarem
Para o botão de download do README funcionar, crie uma "Release":
1. Na página principal do repositório → **Releases** (menu à direita) →
   **Create a new release**.
2. Em "Choose a tag", digite `v1.0.0` e clique em **Create new tag**.
3. Título: `Sudoku OLED v1.0.0` → **Publish release**.
4. Isso dispara a automação de novo, e desta vez ela ANEXA o APK à release
   automaticamente. Em alguns minutos o link
   `github.com/SEU-USUARIO/sudoku-oled/releases/latest` mostra o APK pra download.

É esse link que você compartilha com as pessoas. 🎉

### 5. Atualizar o jogo no futuro
1. Edite/substitua o `www/index.html` no repositório (ou peça pro Claude Code).
2. Crie uma nova release com tag `v1.0.1`, `v1.1.0`, etc.
3. O APK novo é gerado e anexado sozinho.

## 📌 Avisos

- **Esse APK é "debug"**: perfeito para distribuir pelo GitHub e testar com
  amigos. Para a **Play Store**, será preciso gerar um AAB assinado com keystore
  próprio — isso está descrito no CLAUDE.md como tarefa para o Claude Code.
- **Android vai avisar "fonte desconhecida"** ao instalar. É o comportamento
  normal para qualquer app fora da Play Store; basta permitir.
- **Ícone do app**: por enquanto será o ícone padrão do Capacitor. Pedir ao
  Claude Code para gerar o ícone personalizado (tarefa descrita no CLAUDE.md).

## 🛠️ Continuar o desenvolvimento com o Claude Code

1. Instale o Node.js (https://nodejs.org) e depois rode no terminal:
   `npm install -g @anthropic-ai/claude-code`
2. Baixe o repositório no computador (botão **Code → Download ZIP** no GitHub,
   ou pelo GitHub Desktop).
3. Abra o terminal dentro da pasta e rode: `claude`
4. O Claude Code lê o CLAUDE.md sozinho e sabe exatamente onde paramos.

Frases prontas pra pedir ao Claude Code:
- "Gere um ícone e splash screen personalizados para o app"
- "Prepare o build assinado (AAB) para a Play Store e me guie no processo"
- "Adicione AdMob seguindo as diretrizes do CLAUDE.md"

## 💰 Custos

- GitHub + compilação do APK na nuvem: **grátis**
- Play Store (quando decidir publicar lá): **US$25 uma única vez**
