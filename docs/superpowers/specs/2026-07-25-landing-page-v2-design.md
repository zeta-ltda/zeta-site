# Zeta Landing Page v2 — Design

## Contexto

O site do Zeta (`index.html`, HTML/CSS estático de arquivo único, sem build tools) vai passar por uma
atualização grande porque o projeto tomou um rumo mais ambicioso: a intenção é vender o produto,
então a landing page precisa comunicar visão de plataforma multi-dispositivo (desktop, mobile,
wearable, carro), mostrar cenários futuristas concretos de uso, e apresentar um plano de assinatura.

Referência de layout pros novos destaques: os painéis grandes alternados do site do Discord
(mídia de um lado, headline forte do outro, se repetindo). Referência de nav: o header do OpenClaw
(links diretos pra Produto/Capacidades, instalação, etc.), adaptado — sem os elementos que não se
aplicam ao Zeta (ex: comandos de terminal, já que Zeta não é instalado via CLI).

## Escopo

Landing page única (`index.html`), sem novas páginas, sem build tools novos. Mudança é de conteúdo
e seções dentro do arquivo já existente.

## Fluxo de seções (ordem final)

1. Nav
2. Hero (mantido como está)
3. App preview screenshot (placeholder mantido)
4. Vídeo "Zeta em ação" (placeholder mantido)
5. **NOVO** — Destaques futuristas (5 painéis alternados)
6. Tagline (mantido)
7. Capacidades (mantido)
8. Agentes especialistas (mantido, sem link direto no nav)
9. Livre-se de tudo (mantido)
10. **NOVO** — Preços
11. Download (grid expandido pra 7 plataformas)
12. Footer

## Nav (header)

Links: `Capacidades` · `Como instalar` (âncora pra seção de Download — sem seção nova) · `Preços`.
CTA continua "Baixar grátis" apontando pro Download.
Removido o link direto "Agentes" do nav pra não poluir (seção continua na página).

## Destaques futuristas (5 painéis)

Estilo visual: cards grandes alternados (mídia de um lado, texto do outro, invertendo a cada painel),
mesmo tom escuro/gradiente roxo-teal do resto do site. Cada painel tem headline em destaque
(peso 800-900, estilo dos outros headlines do site), um parágrafo curto de apoio, e um placeholder de
mídia com legenda explícita indicando tipo (vídeo ou imagem), duração aproximada e descrição do que
deveria ser produzido — seguindo o mesmo padrão que já existe no app-preview e no video-frame atuais.

### Painel 1 — Carro chegando em casa
- Headline: "O carro chega. O Zeta já pergunta."
- Corpo: "O Zeta percebe que você está chegando perto de casa e já pergunta se quer abrir a garagem — sem precisar tocar em nada."
- Placeholder de mídia: "🎥 Vídeo (15-20s) — Carro se aproximando de uma garagem residencial ao entardecer, tela do painel/celular mostra notificação 'Zeta: quer que eu abra a garagem?' com confirmação por voz. Iluminação quente, sensação de chegar em casa."

### Painel 2 — Cliente pede pelo relógio
- Headline: "Um cliente pede. Você resolve do pulso."
- Corpo: "Enquanto você está longe do computador, um cliente pede uma funcionalidade nova — e o Zeta já te avisa e começa a codar, direto do seu relógio."
- Placeholder de mídia: "📷 Imagem ou vídeo curto (10s) — Pulso com smartwatch mostrando notificação 'Cliente pediu: criar tela de agendamento' e resposta sendo dada por voz; fundo desfocado de rua ou ambiente externo. Transmitir mobilidade e produtividade em qualquer lugar."

### Painel 3 — Enquanto você dorme
- Headline: "Você dorme. O Zeta trabalha."
- Corpo: "Enquanto você descansa, o Zeta resolve tarefas pendentes, responde mensagens e organiza sua agenda — e deixa um resumo pronto pra quando você acordar."
- Placeholder de mídia: "🎥 Vídeo (10-15s) — Notebook fechado numa mesa à noite, timelapse até o amanhecer, tela acende mostrando resumo 'Zeta concluiu 12 tarefas enquanto você dormia'. Tom tranquilo, produtividade passiva."

### Painel 4 — Luiza na cozinha
- Headline: "Uma casa. Duas pessoas. Um Zeta cuidando de tudo."
- Corpo: "Luiza cozinha e pergunta o próximo passo da receita. No andar de cima, alguém pede pro Zeta avisar ela sobre o sal. O Zeta conecta as duas pontas da casa, em tempo real."
- Placeholder de mídia: "🎥 Vídeo (3 cortes, ~12s no total) — Cena 1: Luiza mexendo um risoto no fogão, pergunta em voz alta 'Zeta, qual o próximo passo dessa receita?'. Transição (whip-pan) pro andar de cima. Cena 2: alguém no quarto/escritório pergunta 'Zeta, onde a Luiza tá?' e pede 'manda avisar ela pra ver se tem sal'. Transição de volta pra cozinha. Cena 3: Luiza recebe o recado, confere o armário, sorri e confirma 'Tem sim!'. Tom caseiro, acolhedor, luz quente."
- Nota de produção (não vai pro site, é só apoio pra gravação/geração): prompt de IA de vídeo e recomendações de ferramenta já compartilhados na conversa — reaproveitar ao produzir o material.

### Painel 5 — Na rua, fone no ouvido
- Headline: "Longe de casa, mas de olho em tudo."
- Corpo: "Andando na rua, com o fone no ouvido, você manda uma mensagem perguntando se deixou o forno desligado — o Zeta confere pela câmera de casa e te manda a foto na hora."
- Placeholder de mídia: "🎥 Vídeo (10-15s) — Pessoa andando na calçada, fone de ouvido, digitando no celular. Corta pra notificação recebida com foto do fogão desligado, câmera da cozinha ao fundo. Tom prático, tranquilizador."

## Preços

Plano único centralizado (sem comparativo Free vs Pro):

- Badge de destaque: "1º mês grátis"
- Preço: "R$10/mês" com nota pequena "≈ US$10"
- Texto: "Cancele quando quiser, sem multa"
- Lista de benefícios (mesmo estilo de check-list do "Livre-se de tudo"): todos os agentes especializados inclusos, uso ilimitado de tarefas, suporte prioritário
- CTA do card aponta pra âncora de Download

## Download (grid expandido — 7 cards)

Ordem: macOS (ativo) · Windows (Em breve) · Linux (Em breve) · Android (ativo) · iOS (Em breve) ·
Smartwatch (Em breve) · Carro (Em breve).

Ícones novos necessários pra Smartwatch e Carro: como não há uma marca única (Apple Watch/Wear OS,
CarPlay/Android Auto), usar ícones genéricos de linha (stroke, mesmo estilo dos ícones de
`cap-icon`/capacidades) ao invés de tentar imitar um logo de marca específica — evita ambiguidade e
problema de trademark. `platform-name`: "Smartwatch" e "Carro". `file-info`: "Em breve" pros dois,
mesmo tratamento visual (opacidade reduzida, `pointer-events: none`) que Windows/Linux/iOS já têm hoje.

## Testes / verificação

Como é HTML/CSS estático, verificação é visual: abrir `index.html` no browser (via preview_start),
checar:
- Os 5 painéis futuristas alternando corretamente em desktop
- Grid de 7 downloads quebrando bem em mobile (breakpoints existentes em `@media (max-width: 768px)`
  e `@media (max-width: 420px)` precisam ser ajustados pro novo total de cards)
- Seção de preços legível e com hierarquia clara (preço grande, badge de destaque visível)
- Nav com os 3 links novos funcionando (scroll suave já configurado via `html { scroll-behavior: smooth }`)
- Sem quebra de layout em telas pequenas pros painéis alternados (em mobile, empilham verticalmente —
  mídia sempre em cima, texto embaixo, independente da ordem alternada em desktop)
