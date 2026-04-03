# Futbol Runner - Documentacao Completa

## Sobre o Projeto

Jogo mobile estilo endless runner com tematica de futebol/Copa do Mundo.
O jogador corre automaticamente e precisa desviar de obstaculos (pular, deslizar).
Conforme avanca de fase, o personagem evolui ate chegar no Neymar,
onde um minigame de penalti finaliza o jogo.

---

## Estrutura do Projeto

```
joguinho/
├── index.html              # Jogo completo (HTML + CSS + JS)
├── manifest.json           # Manifesto PWA (nome, icone, cores)
├── sw.js                   # Service Worker (cache offline)
├── capacitor.config.json   # Configuracao do Capacitor (app nativo)
├── package.json            # Dependencias Node.js
├── package-lock.json       # Lock das dependencias
├── icons/
│   ├── icon-192.png        # Icone do app 192x192
│   └── icon-512.png        # Icone do app 512x512
├── www/                    # Copia dos arquivos web (usado pelo Capacitor)
│   ├── index.html
│   ├── manifest.json
│   ├── sw.js
│   └── icons/
├── ios/                    # Projeto Xcode gerado pelo Capacitor
│   └── App/
│       ├── App/            # Codigo fonte iOS
│       ├── App.xcodeproj   # Projeto Xcode
│       └── ...
├── DOCUMENTACAO.md         # Este arquivo
└── .gitignore
```

---

## Tecnologias Utilizadas

| Tecnologia | Uso |
|---|---|
| HTML5 Canvas | Renderizacao do jogo (toda a parte visual) |
| JavaScript (vanilla) | Logica do jogo, fisica, colisao, input |
| Web Audio API | Efeitos sonoros sintetizados (sem arquivos de audio) |
| PWA (Service Worker) | Funcionamento offline no navegador |
| Capacitor | Empacotamento como app nativo (iOS/Android) |
| Xcode | Compilacao e deploy para iPhone/iPad |

---

## Gameplay

### Controles
- **Toque na metade superior da tela** = Pular
- **Toque na metade inferior da tela** = Deslizar/Carrinho
- **Nao tocar** = Correr normalmente
- **Teclado (PC)**: Seta cima = pular, Seta baixo = deslizar

### Progressao de Fases

| Fase | Jogador | Camisa | Numero | Velocidade | Score Necessario | Obstaculos |
|------|---------|--------|--------|------------|-----------------|------------|
| 1 | Jogador Desconhecido | Branca | ? | 200px/s | 500 | Zagueiro |
| 2 | Kaka | Branca (Milan) | 22 | 250px/s | 1200 | + Carrinho |
| 3 | Ronaldinho | Azul/Grena (Barca) | 10 | 300px/s | 2200 | + Bola alta |
| 4 | Ronaldo Fenomeno | Amarela (Brasil) | 9 | 360px/s | 3500 | + Obstaculo duplo |
| 5 | Neymar | Amarela (Brasil) | 10 | 420px/s | 5000 | Todos os tipos |

### Tipos de Obstaculo
1. **Zagueiro** - Jogador em pe, precisa PULAR
2. **Carrinho** - Jogador deslizando no chao, precisa PULAR
3. **Bola alta** - Bola flutuando no alto, precisa DESLIZAR (ou ficar correndo)
4. **Obstaculo duplo** - Cone embaixo + banner em cima, precisa FICAR CORRENDO (nao pular nem deslizar)

### Minigame de Penalti (Final)
Ao completar a fase 5 com o Neymar:
1. Toque para travar a **MIRA** (indicador oscila esquerda-direita)
2. Toque para travar a **FORCA** (barra sobe e desce)
3. O goleiro mergulha aleatoriamente
4. **GOL** = Voce venceu o jogo!
5. **Defesa/Fora** = Tenta de novo o penalti

### Caracteristicas Visuais dos Jogadores
- **Jogador Desconhecido**: Camisa branca simples, cabelo curto
- **Kaka**: Camisa branca com detalhes vermelhos (Milan), cabelo medio
- **Ronaldinho**: Camisa azul (Barca), cabelo cacheado longo, sorriso com dentes, faixa na cabeca
- **Ronaldo Fenomeno**: Camisa amarela (Brasil), corte de cabelo triangular icônico de 2002
- **Neymar**: Camisa amarela (Brasil), moicano loiro, brinco, faixa vermelha

---

## Efeitos Sonoros

Todos os sons sao sintetizados em tempo real usando Web Audio API (nao precisa de arquivos):

| Som | Quando toca |
|-----|------------|
| jump | Jogador pula |
| slide | Jogador desliza |
| hit | Colisao com obstaculo |
| levelup | Mudanca de fase (arpejo ascendente) |
| whistle | Inicio do jogo e penalti (apito de arbitro) |
| kick | Chute no penalti |
| goal | Gol no penalti (simulacao de torcida) |
| miss | Penalti perdido |

---

## Como Compilar e Instalar

### iOS (iPhone/iPad)

**Pre-requisitos:**
- macOS com Xcode instalado
- Conta Apple ID (gratis para desenvolvimento local)
- iPhone conectado via cabo USB

**Passos:**

1. Sincronizar os arquivos web com o projeto iOS:
```bash
cd /Users/diogocardosomarcelino/Documents/joguinho
npx cap sync ios
```

2. Abrir o Xcode:
```bash
npx cap open ios
```

3. No Xcode:
   - Selecionar o iPhone conectado como destino (topo da janela)
   - Clicar em "App" no painel esquerdo (icone azul)
   - Aba "Signing & Capabilities":
     - Marcar "Automatically manage signing"
     - Selecionar seu Team (Apple ID pessoal)
   - Clicar no botao Play (triangulo) para compilar e instalar

4. No iPhone (primeira vez):
   - Ajustes > Geral > Gerenciamento de Dispositivo (ou "VPN e Gerenciamento de Dispositivo")
   - Tocar no perfil do desenvolvedor > Confiar

**Importante:** Com Apple ID gratis, o app expira em 7 dias e precisa ser reinstalado.
Com conta Apple Developer ($99/ano), nao tem essa limitacao.

### Android (FUTURO)

**Pre-requisitos (a instalar):**
- Android Studio (download em https://developer.android.com/studio)
- Java JDK 17+
- Android SDK (instalado automaticamente pelo Android Studio)

**Passos (quando for implementar):**

1. Instalar Android Studio e configurar SDK

2. Instalar plataforma Android no Capacitor:
```bash
npm install @capacitor/android
npx cap add android
npx cap sync android
```

3. Abrir no Android Studio:
```bash
npx cap open android
```

4. Compilar e gerar APK/AAB para distribuicao

---

## Versao Web (PWA)

O jogo tambem funciona como site e pode ser instalado pelo navegador:

**Link publico:** https://diogocardosomarcelino.github.io/futbol-runner/

**Instalar no celular via navegador:**
- **Android (Chrome)**: Abrir link > 3 pontinhos > "Instalar app"
- **iPhone (Safari)**: Abrir link > Compartilhar > "Adicionar a Tela de Inicio"

---

## Publicacao nas Lojas (FUTURO)

### App Store (iOS)
1. Criar conta Apple Developer ($99/ano) em https://developer.apple.com
2. No Xcode: Product > Archive
3. Distribuir via App Store Connect
4. Preencher informacoes (descricao, screenshots, categoria: Jogos > Casual)
5. Enviar para revisao da Apple (demora 1-3 dias)

### Google Play (Android)
1. Criar conta Google Play Developer ($25 taxa unica) em https://play.google.com/console
2. No Android Studio: Build > Generate Signed Bundle/APK > AAB
3. Criar listagem no Google Play Console
4. Preencher informacoes (descricao, screenshots, classificacao etaria)
5. Enviar para revisao (demora algumas horas)

### Monetizacao (opcoes futuras)
- **Gratis com anuncios**: Integrar AdMob (Google) para banner/intersticiais
- **Pago**: Cobrar um valor unico na loja (ex: R$2,99 - R$9,99)
- **Freemium**: Gratis com compras in-app (skins, jogadores extras)

---

## Comandos Uteis

```bash
# Sincronizar alteracoes do jogo com iOS
npx cap sync ios

# Abrir projeto no Xcode
npx cap open ios

# Rodar servidor local para teste no navegador
npx serve -l 3000

# Verificar sintaxe do JavaScript
node -e "new Function(require('fs').readFileSync('index.html','utf8').match(/<script>([\s\S]*)<\/script>/)[1])"
```

---

## Alterando o Jogo

Todo o codigo do jogo esta em **index.html** dentro da tag `<script>`.
Apos qualquer alteracao no jogo:

1. Copiar os arquivos atualizados para a pasta www:
```bash
cp index.html manifest.json sw.js www/
cp -r icons www/
```

2. Sincronizar com o projeto nativo:
```bash
npx cap sync ios
```

3. Compilar novamente no Xcode (botao Play)

### Secoes do Codigo (dentro do index.html)

| Secao | O que faz |
|-------|-----------|
| SECTION 1 | Constantes, configuracao das fases e obstaculos |
| SECTION 2 | Canvas setup e escala responsiva |
| SECTION 3 | Motor de audio (Web Audio API) |
| SECTION 4 | Estado do jogo e variaveis globais |
| SECTION 5 | Controles (touch e teclado) |
| SECTION 6 | Logica do jogador (pulo, slide, fisica) |
| SECTION 7 | Logica dos obstaculos (spawn, colisao) |
| SECTION 8 | Sistema de particulas |
| SECTION 9 | Renderizacao do cenario (ceu, grama, estadio) |
| SECTION 10 | Desenho dos personagens (5 jogadores + obstaculos) |
| SECTION 11 | HUD (placar, barra de progresso) |
| SECTION 12 | Minigame de penalti |
| SECTION 13 | Telas (menu, game over, transicao, vitoria) |
| SECTION 14 | Atualizacao de estados do jogo |
| SECTION 15 | Game loop e inicializacao |

---

## Proximos Passos

- [x] Jogo completo e funcional (HTML5 Canvas)
- [x] PWA com funcionamento offline
- [x] Publicado no GitHub Pages
- [x] Projeto iOS gerado com Capacitor
- [ ] Testes no iPhone (compilar via Xcode)
- [ ] Ajustes de gameplay baseados nos testes
- [ ] Versao Android (instalar Android Studio, gerar APK)
- [ ] Publicar na App Store (conta Apple Developer necessaria)
- [ ] Publicar no Google Play (conta Google Play necessaria)
- [ ] Melhorias futuras (novas fases, jogadores, modos de jogo)
