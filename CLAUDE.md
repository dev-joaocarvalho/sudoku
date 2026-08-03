# Sudoku OLED — Contexto do Projeto

## O que é
Jogo de Sudoku com design minimalista/moderno, tema OLED (preto puro) como padrão.
Primeiro app de uma "fábrica de aplicativos" que o dono do projeto quer publicar
na Play Store para gerar renda (monetização planejada: AdMob + compras no app).
O dono do projeto NÃO programa — explique as coisas de forma simples e faça o
trabalho técnico por ele.

## Estado atual
Todo o jogo está em UM único arquivo: `www/index.html` (HTML+CSS+JS puro, sem frameworks).
Funcional e aprovado pelo dono. Recursos já implementados:

- **Menu**: carrossel horizontal de TEMAS no topo (arraste livre, snap no mais próximo,
  aplica ao vivo); título; carrossel VERTICAL de dificuldades (Fácil/Médio/Difícil/
  Especialista) com cards grandes, blur de profundidade nos vizinhos, pips de
  intensidade, setas de dica que somem nos extremos; botão Jogar; seletor PT/EN.
- **Temas (7)**: Amoled (padrão, #000 + âmbar #caa24e), Signal Blue, Butter Iris,
  Lime Spark, Dragonfruit, Emerald Champagne, Ultra Violet. Implementados como
  `body[data-theme=...]` com CSS variables (--bg, --accent, --accent-rgb, etc).
- **Idiomas**: PT/EN via dicionário I18N no JS; troca anima com efeito "decode/
  scramble" (letras aleatórias que travam na palavra final).
- **Geração de puzzle**: backtracking com verificação de SOLUÇÃO ÚNICA
  (countSolutions), pistas por dificuldade: easy 40, medium 32, hard 26, expert 23.
  Geração roda no cliente; expert pode levar ~1-2s em celular fraco (otimizável).
- **Jogabilidade**:
  - Dois fluxos: número-primeiro (toca no teclado → número "armado" → toca em
    célula vazia pra colocar; toca de novo no teclado pra desarmar) e
    célula-primeiro (toca célula vazia → toca número).
  - Tocar em número JÁ NO TABULEIRO só destaca os iguais (estado `armed=false`),
    NÃO arma pra colocar — decisão explícita do dono pra evitar toques acidentais.
  - O botão do teclado só fica destacado quando armado de verdade.
  - Notas (modo lápis com grade 3x3 na célula, auto-limpeza ao colocar número),
    Desfazer (histórico), Dica (revela célula), contagem restante de cada número
    no canto da tecla, tecla apaga como modo, dígito esgotado fica apagado.
  - Destaque de linha/coluna/quadrante da célula selecionada.
- **Persistência** (via `window.storage` — API do artifact do Claude, VER TAREFA 1):
  progresso por dificuldade (tabuleiro, notas, tempo, salvo a cada jogada e a cada
  10s), recordes por dificuldade. "Novo jogo" descarta o save; vitória limpa o save.
- **Vitória**: tela dedicada "Parabéns!" com tempo, indicação de novo recorde,
  ranking das 4 dificuldades (destaca a jogada) e botão de voltar ao menu.
- **Animações**: abertura do jogo com "focus-in" (tela nasce desfocada e foca) +
  pílula flutuante com o nome da dificuldade que aparece e dissolve + células em
  cascata diagonal; pop ao colocar número; shake em conflito; scramble na troca
  de idioma; transições entre telas.

## Decisões de design (respeitar)
- Fontes: Space Grotesk (UI) + IBM Plex Mono (números/tempos), via Google Fonts.
- Nada de verde-neon "hacker" nem roxo-sobre-preto genérico; o acento do tema
  Amoled é âmbar/dourado #caa24e.
- Minimalismo: sem excesso de texto, sem bordas grossas, cantos arredondados,
  animações com cubic-bezier(.22,1,.36,1).
- Nomes de temas ficam em inglês nos dois idiomas (nomes próprios).

## TAREFAS PENDENTES (em ordem)

### 0. [CONCLUÍDO] Persistência migrada para localStorage
As funções saveProgress/loadProgress/clearProgress/saveBestTimes/loadBestTimes
já usam localStorage. Chaves: `sudoku-save-<dificuldade>` e `sudoku-best-times`.

### 0.5. [CONCLUÍDO] Distribuição via GitHub
O repositório tem workflow (.github/workflows/build-apk.yml) que compila o APK
debug na nuvem via Capacitor e anexa em Releases quando se cria uma tag v*.
package.json e capacitor.config.json já existem (appId com.sudokuoled.app).
O dono distribui o APK pelo link releases/latest.

### 1. Ícone e splash personalizados
Usar @capacitor/assets: criar resources/icon.png (1024x1024, fundo #000, grade
de sudoku minimalista com acento #caa24e) e resources/splash.png, adicionar
passo `npx capacitor-assets generate` no workflow antes do build.

### 2. Build assinado para a Play Store (AAB)
O workflow atual gera APK debug (só para distribuição direta/GitHub). Para a
Play Store é preciso AAB assinado:
```bash
npm install
npx cap add android
npx cap sync
```
(package.json e capacitor.config.json já existem no repo.)
Depois: abrir `android/` no Android Studio, gerar o **AAB assinado**
(Build > Generate Signed Bundle). Guardar o keystore com MUITO cuidado
(perder = não poder mais atualizar o app). Alternativa avançada: assinar no
próprio GitHub Actions guardando o keystore como secret.

### 3. Play Store
- Criar conta de desenvolvedor em https://play.google.com/console (taxa única US$25).
- Preparar: nome do app, descrição PT/EN, ícone 512x512, feature graphic
  1024x500, mínimo 2 screenshots de celular, política de privacidade (página web
  simples — obrigatória), classificação de conteúdo (questionário), declaração de
  segurança de dados.
- Subir o AAB em produção (ou teste fechado primeiro), aguardar revisão (1-7 dias).
- IMPORTANTE 2024+: contas pessoais novas exigem teste fechado com 12+ testadores
  por 14 dias antes de poder publicar em produção. Planejar isso.

### 4. Monetização (depois de publicado e estável)
- AdMob: criar conta, instalar @capacitor-community/admob, banner discreto no
  rodapé do jogo e/ou intersticial ao concluir um puzzle (nunca no meio do jogo —
  quebra a experiência premium do design).
- Compra "remover anúncios" via @capacitor-community/in-app-purchases ou RevenueCat.

### 5. Melhorias futuras que o dono já sinalizou interesse
- Continuar polindo animações e jogabilidade.
- Este é o app-piloto; o objetivo é replicar o processo pra outros jogos simples
  ("fábrica de apps").

## Como trabalhar com o dono
- Ele valida visualmente: gere o app, deixe ele testar, itere.
- Ele gosta de: animações elegantes, interações intuitivas (arraste com snap,
  live-preview), detalhes de sombra/reflexo/blur de profundidade.
- Ele pediu mudanças pontuais várias vezes — prefira mudanças cirúrgicas a
  reescrever tudo.
