# Identidade visual

> Como a Sabec° aparece em tudo que o Sabec/Os gera.
> As skills de conteúdo, carrossel e post leem esse arquivo antes de criar qualquer visual.

---

## Fonte canônica (leitura obrigatória)

Os dois HTMLs abaixo **são o design system oficial**. Em qualquer task visual (carrossel, post, landing, relatório, slide, banner), ler os dois **antes** de gerar qualquer peça. O `design-guide.md` é resumo — em caso de divergência, **os HTMLs vencem**.

- `identidade/design-system-v2.html` — sistema da marca: cores, tipografia, gradientes, texturas, componentes, logo, do/don't
- `identidade/posts-design-system-v2.html` — sistema de posts: formatos (1:1, 4:5, 9:16), grid + margens, templates de feed/carrossel/story/ad, estrutura de copy

---

## Cores

> Os 4 rótulos do **Núcleo** abaixo são lidos pela UI ao vivo (o servidor parseia esse arquivo). Não alterar o formato `- **<rótulo>:** \`#HEX\` — <nota>` sob pena de a UI ignorar a configuração.

**Núcleo (lido pela UI):**
- **Fundo principal:** `#0A0A0A` — preto base cinematográfico, fundo do sistema
- **Fundo alternativo / cards:** `#141414` — grafite, hierarquia no escuro
- **Texto principal:** `#EDE9DF` — off-white cinema, alto contraste sem agredir
- **Cor de destaque / CTA:** `#1F4FFF` — azul elétrico, único acento cromático

**Apoio (referência humana, não lida pela UI):**
- `#1B1B1B` — grafite elevado, card sobre card
- `#2A2A2A` — line (borda discreta no escuro), `#202020` line-soft (separador)
- `#9A968D` — ink-dim (texto secundário), `#5E5B54` ink-faint (labels técnicas)
- `#0E2BBF` cobalto · `#4D74FF` azul bright (hover) · `#BCD3FF` azul gelo (ice, halftone)

**Modo claro (raro):**
- `#F5F2EC` — papel off-white (nunca `#FFFFFF` puro)
- `#E5E0D5` — paper-line
- Texto sobre papel: `#0A0A0A`

**Espectro expressivo** — **uso primário em fundos de post** (mesh radial / conic) e em **kickers que herdam a atmosfera do slide**. Nunca vira botão, CTA ou cor de UI funcional:
- `#E0662F` âmbar · `#34BFD2` ciano · `#8A4FE0` violeta · `#C44BC9` magenta

Regra do kicker espectral: quando o slide tem mesh atmosférico, o **kicker mono adota a cor dominante daquela atmosfera** (ex: slide com mesh âmbar → kicker `#E0662F`; slide com mesh ciano → kicker `#34BFD2`; slide com mesh azul-frio → kicker `#BCD3FF` ice). Isso **substitui** o kicker azul `#1F4FFF` no miolo — azul fica reservado pra capa neutra, CTA final e UI.

**Acento raro:**
- `#D8CBB0` — areia, monograma stamp pontual

**Proibido:** pastel chapado, neon verde, arco-íris saturado, vermelho de alarme, `#FFFFFF` puro.

---

## Tipografia

> Os 3 rótulos abaixo são lidos pela UI ao vivo. Formato: `- **<rótulo>:** \`<família>\` — <nota>`.

- **Headlines e títulos principais:** `Space Grotesk` — display geométrico, peso **medium (500)**, tracking fechado (`-.04em` a `-.045em`) em uppercase
- **Corpo, subtítulos e botões:** `Inter` — neutra, leitura limpa, peso 400/500
- **Kickers, labels técnicos e dados:** `JetBrains Mono` — peso 500, tracking aberto (`.18em` a `.32em`) em uppercase, prefixo padrão `↳`

Regra do contraste: Space Grotesk domina os títulos com peso visual massivo e tracking fechado; JetBrains Mono enquadra a composição nos kickers, topbar e labels com tracking aberto. Esse contraste é o coração do estilo.

---

## Estilo geral

Observatório técnico. Sistema operacional cinematográfico. **Luz iridescente emergindo do preto** — frestas de cor que nascem das bordas e dispersam como aberração cromática de lente premium. Premium-escuro sem ser sombrio: frio, calmo, confiante, mas com atmosfera. Nada de fanfarra, nada de brilho gratuito.

**Regra de cor por peça:** o preto `#0A0A0A` é a base, o azul `#1F4FFF` é o acento funcional (CTA, régua, kicker neutro), e o **espectro expressivo é a atmosfera** — cada slide tem uma cor de mesh dominante (âmbar, ciano, violeta, magenta, ice, azul) que muda de slide pra slide pra dar respiração cromática. Carrossel inteiro só azul é monotonia — **evite**.

Inspiração viva: Linear, Vercel, Apple late-night, agências de design russas premium (@design.atum), wordmarks com ligadura (Fuse, Connect), capas de revista premium com vidro iridescente e dispersão prismática.

---

## Elementos-chave

- Bordas: 1px em `#2A2A2A` (line) no escuro, `#E5E0D5` no claro
- Border-radius: **12px** cards, **8px** botões, 999px pills
- Botões primários: fundo `#1F4FFF`, texto `#EDE9DF`, radius 8px, glow sutil `0 0 40px rgba(31,79,255,.18)`
- Botões ghost: transparente, border `#2A2A2A`, hover muda border pra `#1F4FFF`
- Pills: padding 9px 17px, border 1px `#2A2A2A`, `.dot` 6px azul, variante `.solid` com fundo azul
- HUD frame: cantos 14px em `#1F4FFF` (marcas de corte) — usar pra destacar dados/painel de observação
- Card de vidro: `linear-gradient(180deg, rgba(31,79,255,.08), rgba(20,20,20,.6))` + `backdrop-filter: blur(6px)`
- Sombras: **evitar**. Só glow azul sutil em CTAs primários
- Halftone: pontos ice `#BCD3FF` 1.2-1.4px em grid de 8-9px, com mask radial — overlay sutil
- Dot-matrix: pontos azul translúcido em grid 7px, com mask diagonal
- Grão: SVG fractalNoise 3-4% opacity, mix-blend `overlay`, sempre presente em fundos grandes
- Grade técnica: linhas 1px em `rgba(237,233,223,.04)`, pitch 80px, com máscara radial posicionada na fonte de luz — presente em todos os slides
- Kicker prefix: `↳` como âncora visual antes do texto; cor varia por atmosfera do slide (ice, azul, ember, ciano)
- Color rule: 72px × 4px, fundo `#1F4FFF`, `box-shadow: 0 0 20px rgba(31,79,255,.4)` — divide kicker de headline na capa
- SVG decorativos: **Radar** (círculos concêntricos + mira, stroke branco 0.3px, opacidade .10–.25, ponto central `#1F4FFF`) e **Anéis** (círculos concêntricos com cor do espectro da peça) — posicionados fora do padding, parcialmente cortados pela borda
- **Gradientes espectrais (default pra capa e atmosfera de slide):**
  - `g-holo` — `radial-gradient(circle at 50% -20%, rgba(52, 191, 210, 0.26), transparent 50%), radial-gradient(circle at 15% 50%, rgba(138, 79, 224, 0.24), transparent 50%), radial-gradient(circle at 85% 50%, rgba(224, 102, 47, 0.22), transparent 50%), radial-gradient(circle at 50% 120%, rgba(196, 75, 201, 0.28), transparent 55%), radial-gradient(circle at 50% 50%, rgba(31, 79, 255, 0.15), transparent 40%)` sobre `#0A0A0A` — Holographic Fluid (capa multicolor mestre ultra suave, **default canônico**)
  - `g-aurora` — onda diagonal suave de ciano, violeta, magenta e azul-elétrico sobre `#0A0A0A`
  - `g-solaris` — malha solar quente combinando âmbar, magenta, cobalto e ciano sobre `#0A0A0A`
  - `g-nebula` — nebulosa cósmica misturando cobalto, magenta, ciano e âmbar sobre `#0A0A0A`
  - `g-ember` — `radial-gradient(ellipse 70% 120% at 78% 18%, #E0662F 0%, #C44BC9 26%, #1F4FFF 52%, #0A0A0A 82%)` — âmbar→magenta→cobalto, quente-frio (Forte)
  - `g-prism` — `linear-gradient(115deg, #0A0A0A 4%, #1F4FFF 22%, #BCD3FF 34%, #E0662F 48%, #C44BC9 60%, #0A0A0A 96%)` — barras prismáticas, wallpaper/capa horizontal (Forte)
  - `g-nova` — `radial-gradient(circle at 85% 15%, #E0662F 0%, rgba(224, 102, 47, 0.8) 18%, rgba(196, 75, 201, 0.7) 42%, rgba(31, 79, 255, 0.5) 70%, #0A0A0A 95%)` — Supernova Flame, brilho de alta intensidade âmbar→magenta→cobalto no topo direito (Forte)
  - `g-flux` — `linear-gradient(135deg, #0A0A0A 8%, #8A4FE0 28%, #C44BC9 48%, #E0662F 68%, #34BFD2 88%, #0A0A0A 98%)` — Prismatic Flux, feixes de luz espectral vibrante em corte diagonal (Forte)
  - `g-eclipse` — `radial-gradient(circle at 15% 85%, #34BFD2 0%, rgba(138, 79, 224, 0.85) 25%, rgba(31, 79, 255, 0.6) 60%, #0A0A0A 95%)` — Neon Eclipse, corona cibernética com flare ciano e violeta emergindo do canto inferior esquerdo (Forte)
  - `g-iris` — **DEPRECADO / PROIBIDO** devido à "ponta do cone" dura no centro. Não usar para capas novas. Substituir sempre por `g-holo`.
  - **Mesh quente-frio inferior-esquerdo + superior-direito:** combo `radial-gradient(circle at 25% 75%, rgba(224,102,47,.26), transparent 50%), radial-gradient(circle at 75% 25%, rgba(138,79,224,.18), transparent 50%)` sobre `#0A0A0A` — boa pra slide de dado
  - **Mesh aberração cromática:** `radial-gradient(ellipse 55% 45% at 90% 12%, rgba(52,191,210,.28), transparent 60%), radial-gradient(ellipse 50% 50% at 10% 88%, rgba(196,75,201,.24), transparent 55%), radial-gradient(circle at 80% 80%, rgba(138,79,224,.16), transparent 50%)` sobre `#0A0A0A` — ciano+magenta+violeta nas bordas
- **Gradientes-assinatura (azul) — uso restrito:** radial hero (`g-radial`), steel diagonal (`g-steel`), light emergindo (`g-light`). Reservados pra **CTA final**, hero de landing, e capa quando o tema pede peso institucional. **Não** usar como fundo de slides do miolo.
- **Fundo azul chapado** (`#1F4FFF` flat, sem gradiente nem textura) — **proibido em post**. Único lugar tolerado: botão.

---

## Uso de fotografia

- Preferência por **preto e branco com grão** ou cor desbotada (low saturation, contraste alto)
- Tema: ambiente real (mesa de trabalho, tela de computador, mão em teclado) — nada de stock corporate de gente sorrindo
- Quando usar cor: paleta restrita ao azul elétrico + neutros
- Evitar: fotografia genérica de banco de imagem, gente posada, "people business"

---

## O que NUNCA fazer

- Escrever "Sabec°" como texto — **sempre** usar o logotipo SVG colorido no lugar. Fundo escuro: `Logotipo Branco e Azul.svg`. Fundo claro: `Logotipo Preto Azul.svg`
- Emoji em qualquer peça
- Branco puro `#FFFFFF` (usar `#F5F2EC` se precisar de modo claro)
- Cores do espectro expressivo (âmbar/ciano/violeta/magenta) virando **botão, CTA, link** ou cor funcional de UI — espectro é fundo/atmosfera e (no máximo) cor do kicker mono. Botão/CTA continua azul `#1F4FFF` ou off-white
- **Fundo azul chapado** (`background: #1F4FFF;` flat sem mesh/textura) em slide de carrossel — só CTA admite radial rico, e mesmo assim com halftone
- **Capa com fundo `#0A0A0A` chapado** (preto sem mesh espectral) — capa **precisa** de gradiente espectral (`g-holo`, `g-ember`, `g-aurora`) ou pelo menos mesh quente-frio. Preto chapado é pra slide interno de transição
- **Carrossel monocromático** — todos os slides com a mesma atmosfera (tudo azul, ou tudo preto sem mesh). Variar atmosfera por slide é regra, não decoração
- Sombras pesadas — no máximo glow azul sutil
- Foto stock corporativo de gente sorrindo posada
- Pastel chapado, neon verde, arco-íris saturado
- Texto em CAPS LOCK longo (mais de 4 palavras)
- Quadro lotado de texto (uma ideia + um CTA por peça)
- Mais de um CTA competindo
- "Vamos juntos!", "Decola!", "Bora!" estampado em peça
- Mockup de site/app dentro de moldura de iPhone genérico

## Regras de post (Instagram)

- **Margem segura:** 8% em toda peça. Texto, logo e CTA nunca cruzam essa calha
- **Ancoragem:** kicker mono no topo, headline display embaixo-esquerda, CTA logo abaixo
- **Uma ideia por slide.** Densidade é pro carrossel, não pro slide
- **Kicker:** mono 9-11px (proporção; em 4:5 1080×1350 vira ~24px), tracking `.22em`, uppercase. **Cor herda a atmosfera do slide** (espectro dominante do mesh). Default neutro: ice `#BCD3FF`
- **Page counter:** mono 9-10px no canto superior direito (`01/05`)
- **CTA pill:** padding 7px 14px, radius 999px, fundo `#1F4FFF` ou ghost com border off-white. Em CTA final com fundo azul rico, usar pill **off-white** (`#EDE9DF`) com texto preto pra contraste máximo
- **Formatos canônicos:** 1:1 (1080×1080 feed) · 4:5 (1080×1350 carrossel — mestre) · 9:16 (1080×1920 story/reels)
- **Arco do carrossel:** capa (gancho+promessa) → miolo (um ponto/slide, mono enumerado `01/02/03`) → dado (KPI mono gigante) → fecho (CTA único)

### Atmosfera por slide — regra crítica de variedade cromática

Cada slide do carrossel tem uma cor de atmosfera diferente do vizinho. Carrossel inteiro com fundo `#0A0A0A` chapado é monótono; carrossel inteiro com mesh azul é o mesmo erro com outro tom. A atmosfera vem do mesh de fundo + cor do kicker + (opcional) cor do anel/radar decorativo.

**Receita padrão (6 slides):**

| # | Tipo de slide | Atmosfera default | Mesh / fundo |
|---|---|---|---|
| 01 | CAPA | **espectral multicolor** | `g-holo` (suave, default) ou as opções fortes: `g-nova` (Supernova), `g-flux` (Fluxo), `g-eclipse` (Eclipse) ou `g-ember` |
| 02 | PONTO 01 | azul-frio | radial azul `rgba(31,79,255,.22)` no canto inferior-esquerdo sobre `#0A0A0A`; kicker `#1F4FFF` |
| 03 | DADO / KPI | quente (âmbar+violeta) | mesh quente-frio (ember 25%/75% + violet 75%/25%) ou a força do `g-nova`; kicker `#E0662F`; ring decorativo âmbar |
| 04 | PONTO 02 | aberração cromática | ciano + magenta + violeta nas bordas (ver "gradientes espectrais"); kicker `#34BFD2` |
| 05 | HUD / ESTRUTURA | azul-frio com moldura HUD | radial azul sutil central + cantos HUD em `#1F4FFF`; kicker ice `#BCD3FF` |
| 06 | CTA FINAL | azul rico | radial azul `#1F4FFF` → `#0E2BBF` → `#0A0A0A` com halftone `#BCD3FF`; pill off-white |

**Por que assim:** capa colorida puxa o olho no feed → miolo varia atmosfera pra dar respiração e mostrar que cada ponto é distincto → CTA final volta pro azul institucional pra fechar com peso.

**Variações aceitas:**
- Carrossel com alta voltagem visual/impactante: usar as opções fortes `g-nova`, `g-flux` ou `g-eclipse` na capa para criar um impacto visual imediato com cores fortes e saturadas.
- Carrossel de tema técnico/sóbrio: trocar a capa por `g-ember` ou `g-holo` (mais contidas).
- Carrossel curto (4 slides): capa espectral → 1 ponto azul-frio → 1 dado quente → CTA azul rico
- Carrossel longo (8+ slides): inserir um segundo slide quente (magenta+âmbar), slide forte (`g-eclipse` / `g-flux`) ou um slide ice (`#BCD3FF` radial) pra não repetir atmosfera vizinha

**Antipadrão proibido:** dois slides seguidos com a mesma cor de atmosfera (ex: 02 e 03 ambos azuis). Se acontecer, trocar a atmosfera de um deles.

---

## Logo

Arquivos em `identidade/SVG/`. Dois tipos: **Logo** (símbolo isolado) e **Logotipo** (wordmark — nome escrito). Quatro variantes de cor cada:

| Arquivo | Tipo | Fundo | Quando usar |
|---|---|---|---|
| `Logotipo Branco e Azul.svg` | Wordmark | Escuro ✦ default | Topbar de slides, header de propostas, slide CTA final |
| `Logotipo Branco.svg` | Wordmark | Escuro | Versão discreta quando azul já domina a peça |
| `Logotipo Preto Azul.svg` | Wordmark | Claro | Landing page modo claro, docs impressos |
| `Logotipo Preto.svg` | Wordmark | Claro | Modo claro sem destaque cromático |
| `Logo Branco Azul.svg` | Símbolo | Escuro | Favicon, avatar, marca d'água pequena |
| `Logo Branco.svg` | Símbolo | Escuro | Versão monocromática do símbolo |
| `Logo Preto azul.svg` | Símbolo | Claro | Símbolo sobre papel |
| `Logo Preto.svg` | Símbolo | Claro | Símbolo monocromático no claro |

- **Default posts/carrossel (fundo `#0A0A0A`):** `Logotipo Branco e Azul.svg`
- **Embed nos slides:** `<img src="../SVG/Logotipo Branco e Azul.svg" height="26" style="display:block">` no topbar
- **Em propostas/HTML:** `width: 140–200px; height: auto`

---

## Templates visuais

Slides-base prontos pra copiar em `identidade/Logos/`:

- `slide-capa.html` — CAPA do carrossel (kicker + título + subtítulo, mesh + grade + grão)
- `slide-dado.html` — NÚMERO/DADO (numeral gigante + unidade + label técnico)

Ambos em 1080×1350 (4:5 mestre). Ver `Logos/README.md` pra detalhes.

---

## Observações adicionais

- O Sabec/Os tem UI própria (servidor local) que **lê esse arquivo** pra aplicar cores e fontes na interface ao vivo. Não alterar o formato dos rótulos sob pena de a UI ignorar a configuração.