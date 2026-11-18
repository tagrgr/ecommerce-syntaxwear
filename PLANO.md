# Plano de desenvolvimento: página inicial da Syntaxwear

E-commerce de tênis, feito **só com HTML e CSS** e com abordagem **mobile-first**. O JS (JavaScript) entra numa fase posterior.

Este plano foi montado a partir da descrição das 5 seções. Ajuste as cores, as fontes e as proporções olhando o mockup. Onde algo já está pronto no repositório, está marcado com ✅.

---

## 1. Estrutura de pastas e arquivos

```
ecommerce-syntaxwear/
├── index.html            ✅ página única
├── README.md             ✅
├── PLANO.md              ✅ este plano
├── css/
│   ├── reset.css         ✅ (vazio) zera estilos do navegador
│   ├── tokens.css        ✅ (vazio) variáveis: cores, fontes, espaçamentos
│   ├── style.css         ✅ (vazio) estilos do celular (base)
│   └── responsive.css    ✅ (vazio) adaptações para tablet e desktop
└── assets/
    ├── img/              ✅ fotos de produtos, lifestyle e fundo do hero
    ├── icons/            ✅ logo e ícones em SVG
    ├── video/            ✅ vídeo do card do grid
    └── fonts/            (opcional) só se você for hospedar as fontes
```

**Por que separar assim:** cada arquivo tem uma função só. Quando algo der errado, você sabe onde procurar.

**Convenção importante para o mobile-first:**
- O `style.css` guarda **só o visual do celular**.
- O `responsive.css` guarda **só as media queries com `min-width`** ("a partir de X pixels de largura, faça isto").

Assim, o celular é a base, e telas maiores só acrescentam ou alteram regras.

**Pontos de quebra (breakpoints) sugeridos:**
- **Tablet:** a partir de 768px.
- **Desktop:** a partir de 1024px.
- **Telas largas (opcional):** a partir de 1280px.

**Nomes de imagens:** use minúsculas, sem acento e sem espaço, por exemplo `tenis-casual-branco.webp`. Espaços e acentos quebram links em alguns servidores.

---

## 2. Fase de preparação

### 2.1 Reset de CSS (`reset.css`)
**O que é:** cada navegador aplica margens, tamanhos de título e estilos de lista próprios. O reset tira essas diferenças para você partir do zero.

**O que incluir:**
- **`box-sizing: border-box` em todos os elementos.** Faz a largura de uma caixa incluir o padding e a borda. Sem isso, uma caixa de 300px com 20px de padding ocupa 340px, e é esse tipo de erro que quebra layouts.
- **Zerar as margens padrão** do `body`, dos títulos, dos parágrafos e das listas.
- **Tirar as bolinhas das listas** usadas como menu.
- **Imagens e vídeos com `display: block` e `max-width: 100%`.** Assim eles nunca ficam maiores que a caixa onde estão.
- **Botões e campos de formulário herdando a fonte da página.** Por padrão eles não herdam.

**Pronto quando:** a página sem estilos aparece "crua", sem margens estranhas e com as imagens cabendo na tela.

### 2.2 Variáveis de CSS (`tokens.css`)
**O que é:** variáveis (também chamadas de *custom properties*) são nomes que guardam valores. Se o preto da marca mudar, você troca em um lugar só, e a página inteira se atualiza.

**Defina no seletor `:root`:**
- **Cores:** fundo, texto principal, texto secundário, cor de destaque da marca, cor dos botões e do hover, e cor das bordas. Pegue os valores exatos do mockup.
- **Fontes:** a família dos títulos, a família do texto e os pesos usados (por exemplo, 400 e 700).
- **Tamanhos de texto:** uma escala de 5 ou 6 tamanhos, de pequeno a extra grande, em **rem**. O rem é relativo ao tamanho de fonte que o usuário escolheu no navegador, e por isso respeita quem precisa de letra maior.
- **Espaçamentos:** uma escala fixa, como 4, 8, 16, 24, 32, 48 e 64px, convertidos para rem. Usar sempre os mesmos valores deixa o layout harmônico.
- **Outros:** raio das bordas arredondadas, largura máxima do conteúdo (por exemplo, 1200px) e duração das transições.

**Pronto quando:** nenhuma cor, fonte ou espaçamento aparece "solto" nos outros arquivos CSS. Tudo usa as variáveis.

### 2.3 Escolha e carregamento das fontes
- Identifique no mockup quais fontes ele usa. No Figma, basta clicar no texto.
- **Use no máximo duas famílias** (uma para títulos e outra para texto) e **poucos pesos**. Cada peso é um arquivo a mais para baixar.
- Carregue pelo Google Fonts, colocando o `<link>` no `<head>` **antes** dos seus CSS. Inclua o `display=swap` no endereço: com ele, o texto aparece com uma fonte reserva enquanto a fonte real carrega, em vez de ficar invisível.
- Sempre informe uma fonte reserva genérica no fim da lista, como `sans-serif`.

### 2.4 Base global (início do `style.css`)
- Defina a fonte, a cor e o fundo do `body` usando as variáveis.
- Crie uma classe de **container**: largura máxima, centralizado e com padding lateral. Ela evita que o conteúdo encoste na borda do celular ou se estique demais no monitor.
- Crie uma classe utilitária **"só para leitor de tela"** (normalmente chamada `visually-hidden`). Ela esconde um texto visualmente, mas mantém ele para quem usa leitor de tela. Você vai usar nos ícones.
- Escolha um padrão de nomes para as classes. Sugestão: **BEM (Block Element Modifier)**, por exemplo `card`, `card__title` e `card--destaque`. Assim o nome já diz de onde a classe vem.

**Pronto quando:** o texto da página já aparece com a fonte, a cor e o fundo certos.

---

## 3. Fases por seção

### Fase 3.1: Menu

**Objetivo:** a barra do topo com o logo, os 5 links (Masculino, Feminino, Outlet, Nossas lojas, Sobre) e os ícones de conta, ajuda e carrinho.

**Tags semânticas:**
- `header` para a barra inteira.
- `nav` com um `aria-label`, por exemplo "Menu principal". ARIA significa *Accessible Rich Internet Applications*: são atributos que dão informações extras aos leitores de tela.
- `ul`, `li` e `a` para os links. Um menu é uma lista de links.
- O logo deve ser um `a` apontando para a página inicial, com um `img` dentro. O texto alternativo do `img` deve ser "Syntaxwear – página inicial".
- Os ícones também devem ser `a` (ou `button` na fase de JS). Use o SVG (*Scalable Vector Graphics*, imagem vetorial que não perde qualidade ao ampliar) e um texto escondido com a classe `visually-hidden`, como "Carrinho". Um ícone sozinho não tem nome para o leitor de tela.

> ⚠️ Hoje o `index.html` tem `<h1>SyntaxWear</h1>` dentro do `header`. O `h1` deve ser o título principal da página, que aqui é o título do hero. Troque o logo por um `a` com `img` e deixe o `h1` para a fase 3.2.

**Técnica de layout: Flexbox.**
- **Por quê:** o menu é uma linha só (uma dimensão) com três grupos: logo, links e ícones. O Flexbox é feito para distribuir itens em uma direção.
- Use `justify-content: space-between` para empurrar os grupos para as pontas e `align-items: center` para alinhá-los na vertical.
- Use `gap` para separar os links, em vez de margens.

**Comportamento responsivo:**
- **Celular:** mostre o logo e os ícones. Os 5 links não cabem lado a lado, então ficam dentro de um **menu hambúrguer**. Há uma solução só com HTML: os elementos `details` e `summary`. O `summary` é o botão de "abrir menu" e o resto do conteúdo aparece ao clicar. Ele já funciona com teclado e leitor de tela, sem nenhum JS. O ícone de três linhas fica dentro do `summary`.
  - *Alternativa:* o truque do "checkbox escondido com `label`" também funciona, mas é pior para acessibilidade. Prefira o `details`.
  - *Fase de JS:* fechar o menu ao clicar fora dele e animar a abertura.
- **Desktop (a partir de 1024px):** esconda o `summary` e mostre os links em linha, entre o logo e os ícones.
- **Opcional:** `position: sticky` com `top: 0` deixa o menu grudado no topo ao rolar a página, só com CSS.

**Pronto quando…**
- [ ] o logo leva para a página inicial e tem texto alternativo;
- [ ] no celular, o menu abre e fecha pelo `details`, inclusive usando Tab e Enter;
- [ ] no desktop, os 5 links aparecem em linha;
- [ ] cada ícone tem um nome que o leitor de tela consegue ler;
- [ ] nada estoura a largura da tela em 320px.

---

### Fase 3.2: Painel (hero)

**Objetivo:** uma área grande com foto de fundo, o título "Transforme qualquer passo em presença." e os botões "Ver modelos" e "Comprar".

**Tags semânticas:**
- `section` com um `aria-labelledby` apontando para o título.
- `h1` com o título. É o **único** `h1` da página.
- `a` para os botões, porque eles levam a outras páginas. Use `button` só para ações na própria página, como enviar um formulário.

**Técnica de layout: Flexbox em coluna.**
- **Por quê:** o conteúdo é uma pilha (título em cima, botões embaixo) que precisa ser alinhada dentro de uma caixa alta. O Flexbox com `flex-direction: column` resolve com poucas linhas.
- Os dois botões ficam lado a lado com Flexbox e `gap`.
- **Imagem de fundo:** use `background-image` com `background-size: cover` (a imagem cobre a área toda sem distorcer) e `background-position` para escolher qual parte da foto fica visível.
- **Contraste:** coloque um degradê escuro por cima da foto, na mesma propriedade `background-image`. Sem isso, o texto branco some nas partes claras da foto.
- **Altura:** use uma altura mínima (`min-height`), nunca uma altura fixa. Com altura fixa, o texto vaza quando a tela é estreita e o título quebra em várias linhas.

**Comportamento responsivo:**
- **Celular:** texto menor e botões um embaixo do outro, cada um com 100% da largura. Botões largos são mais fáceis de tocar com o dedo.
- **Tablet e desktop:** título maior e botões lado a lado. O texto fica alinhado onde o mockup indicar.
- **Opcional:** use `clamp()` no tamanho do título para ele crescer aos poucos conforme a tela, sem precisar de media query.
- **Opcional:** uma versão menor da foto de fundo para o celular, trocada na media query. Isso economiza dados móveis.

**Pronto quando…**
- [ ] o título é legível sobre qualquer parte da foto;
- [ ] os botões têm área de toque de pelo menos 44 × 44px;
- [ ] os botões mudam de visual no hover e no foco do teclado;
- [ ] a seção não corta o texto em nenhuma largura.

---

### Fase 3.3: Destaque (4 cards de categoria)

**Objetivo:** 4 cards (Casual, Esporte, Moderno e Futurista), cada um com imagem e nome, levando para a categoria.

**Tags semânticas:**
- `section` com um `h2` de título. Se o mockup não mostrar um título, use um `h2` escondido com `visually-hidden`, porque toda seção precisa de um título para o leitor de tela.
- `ul` e `li` para os cards, já que é uma lista de 4 itens.
- Cada card é um `a` envolvendo um `img` e um `h3` com o nome da categoria. Assim o card inteiro é clicável.

**Técnica de layout: CSS Grid.**
- **Por quê:** os cards formam colunas e linhas que precisam ter o mesmo tamanho. O Grid controla as duas direções ao mesmo tempo.
- Use `aspect-ratio` nas imagens (por exemplo, 3/4) com `object-fit: cover`. Assim todos os cards ficam com a mesma proporção, mesmo que as fotos tenham tamanhos diferentes.

**Comportamento responsivo:**
- **Celular:** 2 colunas (uma grade 2 × 2). Uma alternativa é uma fila que rola para o lado, com `overflow-x: auto` e `scroll-snap-type`. O *scroll snap* faz cada card "encaixar" ao rolar, como um carrossel, só com CSS. Escolha a opção mais parecida com o mockup.
- **Tablet:** 2 colunas maiores, ou 4 se couber.
- **Desktop:** 4 colunas em uma linha.

**Pronto quando…**
- [ ] os 4 cards têm a mesma altura e a mesma proporção de imagem;
- [ ] o card inteiro é clicável e mostra foco visível;
- [ ] cada imagem tem texto alternativo, ou `alt=""` se o nome escrito já descreve a imagem (veja a fase 4).

---

### Fase 3.4: Grid bento

**Objetivo:** uma grade de blocos de tamanhos diferentes (estilo "marmita bento") misturando fotos de produtos, fotos lifestyle e um card de vídeo.

**Tags semânticas:**
- `section` com um `h2`.
- `figure` para cada bloco de imagem, com um `figcaption` se houver legenda visível.
- Os blocos de produto que levam a uma página são `a`.
- `video` para o card de vídeo.

**Técnica de layout: CSS Grid com `grid-template-areas`.**
- **Por quê:** no bento, alguns blocos ocupam 2 colunas ou 2 linhas. O `grid-template-areas` permite "desenhar" o layout com nomes, como num mapa. Para mudar o layout em cada tamanho de tela, basta redesenhar o mapa na media query, sem mexer no HTML.
- Dê um nome de área a cada bloco (por exemplo, `video`, `produto1`, `lifestyle1`) com `grid-area`.
- As imagens preenchem o bloco com `width: 100%`, `height: 100%` e `object-fit: cover`.

**O vídeo sem JS:**
- O `video` funciona só com HTML usando os atributos `controls` (mostra os botões de play e pause do navegador), `muted`, `playsinline` (no iPhone, toca dentro da página em vez de abrir em tela cheia) e `poster` (imagem mostrada antes do play).
- Se o mockup mostrar o vídeo tocando sozinho, existem os atributos `autoplay` e `loop`. Mas vídeo em movimento automático incomoda algumas pessoas e não pode ser pausado sem os controles. Recomendação: use `controls` agora e deixe para a **fase de JS** qualquer botão de play personalizado e a lógica de respeitar a preferência "reduzir movimento".
- Comprima o vídeo (poucos MB, formato MP4). Ele é o arquivo mais pesado da página.

**Comportamento responsivo:**
- **Celular:** 1 coluna, ou 2 colunas com alguns blocos ocupando a largura toda. Os blocos ficam um embaixo do outro.
- **Tablet:** 2 colunas, com o vídeo ocupando as 2.
- **Desktop:** o layout do mockup, por exemplo 4 colunas com blocos ocupando 2 × 2.
- Use `grid-auto-rows` com um tamanho fixo ou com `minmax()`. Assim as linhas têm altura previsível e os blocos não ficam achatados.

**Pronto quando…**
- [ ] o layout do desktop bate com o mockup;
- [ ] no celular, nada fica espremido nem com rolagem lateral;
- [ ] o vídeo toca e pausa pelos controles e tem um `poster`;
- [ ] as imagens fora da primeira tela usam `loading="lazy"`, para só carregarem quando o usuário rolar até elas.

---

### Fase 3.5: Rodapé

**Objetivo:** newsletter, redes sociais, colunas de links e copyright.

**Tags semânticas:**
- `footer` envolvendo tudo.
- **Newsletter:** `form` com um `label` ligado ao `input type="email"` e um `button type="submit"`.
  - O `type="email"` mais o atributo `required` fazem o navegador validar o e-mail sozinho, sem JS. No celular, ele também abre o teclado com "@".
  - Não use só o `placeholder` como rótulo, porque ele some quando a pessoa começa a digitar. Se o mockup não mostrar rótulo, use um `label` com a classe `visually-hidden`.
  - O envio de verdade fica para a **fase de JS ou do back-end**.
- **Redes sociais:** `ul` de links com ícones, cada um com um nome escondido ("Instagram da Syntaxwear").
- **Colunas de links:** cada coluna é um `nav` com um `aria-label`, ou um `div` com um título (`h2` ou `h3`) e um `ul`.
- **Copyright:** um `p` com a tag `small`.

**Técnica de layout:**
- **CSS Grid para as colunas de links,** porque elas formam uma grade que muda o número de colunas conforme a tela.
- **Flexbox para a linha da newsletter** (campo e botão) e para os ícones sociais, porque os dois são uma linha só.

**Comportamento responsivo:**
- **Celular:** tudo empilhado (newsletter, sociais, colunas e copyright), com as colunas de links em 1 ou 2 colunas.
  - *Opcional:* cada coluna pode virar um `details` e `summary` (um "acordeão" que abre e fecha), só com HTML.
- **Tablet:** colunas de links em 2 ou 3 colunas.
- **Desktop:** newsletter de um lado e colunas do outro, como no mockup.

**Pronto quando…**
- [ ] o campo de e-mail tem um `label` e recusa um e-mail inválido ao enviar;
- [ ] todos os ícones sociais têm nome;
- [ ] os links do rodapé têm contraste suficiente, que costuma ser um problema em rodapés escuros com texto cinza.

---

## 4. Fase final: qualidade

### 4.1 Acessibilidade
Siga as WCAG (*Web Content Accessibility Guidelines*), as diretrizes internacionais de acessibilidade na web. Os pontos principais para este projeto:

- **Texto alternativo (`alt`) nas imagens:**
  - Para uma foto de produto, descreva o que importa: "Tênis casual branco com sola bege".
  - Para uma imagem decorativa, ou uma que repete o texto ao lado, use `alt=""` (vazio). Assim o leitor de tela pula a imagem.
  - Nunca use "imagem" ou "foto" como texto alternativo. O leitor de tela já anuncia que é uma imagem.
- **Contraste:** o texto normal precisa de uma proporção de pelo menos **4,5:1** com o fundo, e o texto grande de **3:1**. Teste no WebAIM Contrast Checker ou no DevTools do navegador.
- **Foco visível:** navegue pela página só com a tecla Tab. Você precisa sempre ver onde está. **Nunca remova o `outline`** sem colocar outro estilo de foco no lugar. Use `:focus-visible` para mostrar o foco só para quem usa o teclado.
- **Títulos:**
  - Um `h1`, depois `h2` para as seções e `h3` dentro delas, sem pular níveis.
  - Teste com a extensão HeadingsMap.
- **Idioma:** o `lang="pt-BR"` ✅ já está no HTML.
- **Movimento:** se você usar transições, desligue-as dentro da media query `prefers-reduced-motion: reduce`. Isso respeita quem configurou o sistema para reduzir animações.

### 4.2 Testes em diferentes tamanhos de tela
- Use o **modo responsivo do DevTools** (F12 e depois o ícone de celular). Teste nas larguras **320, 375, 768, 1024, 1280 e 1440px**.
- Em cada largura, confira:
  - se aparece uma **barra de rolagem horizontal** (isso é sempre um erro);
  - se algum texto foi cortado;
  - se alguma imagem ficou distorcida.
- **Zoom de 200%:** a página precisa continuar usável.
- **Celular de verdade:** se puder, abra a página no seu celular. Com a extensão Live Server do VS Code, você acessa pelo IP do computador na mesma rede Wi-Fi.
- Abra a página em pelo menos dois navegadores, como Chrome e Firefox.

### 4.3 Validação no W3C Validator
- O W3C (*World Wide Web Consortium*) é a organização que define os padrões da web.
- Valide o HTML em validator.w3.org, na aba "Validate by File Upload".
- Valide o CSS em jigsaw.w3.org/css-validator.
- Corrija todos os **erros**. Leia os **avisos** e entenda cada um, porque nem todo aviso precisa ser corrigido.

### 4.4 Desempenho (bônus)
- Converta as fotos para **WebP**, um formato mais leve que JPG e PNG com a mesma qualidade.
- Redimensione as fotos para o tamanho em que elas realmente aparecem. Não sirva uma foto de 4000px num card de 300px.
- Coloque `width` e `height` em todo `img`. Assim o navegador reserva o espaço antes da imagem carregar, e o conteúdo não "pula" durante o carregamento. Esse pulo se chama CLS (*Cumulative Layout Shift*).
- Rode o **Lighthouse**, que fica no DevTools, na aba Lighthouse. Ele dá notas de acessibilidade e desempenho.

---

## 5. Erros comuns de iniciante e como evitar

| Erro | Por que é ruim | Como evitar |
|---|---|---|
| Começar pelo desktop e "espremer" depois | Gera dezenas de correções e media queries brigando entre si | Faça cada seção primeiro em 375px, e só depois abra o `responsive.css` |
| Altura fixa (`height`) em caixas com texto | O texto vaza quando a tela estreita | Use `min-height` ou deixe a altura automática |
| Tudo em `px` | Ignora o tamanho de fonte que o usuário escolheu | Use `rem` para fontes e espaçamentos |
| `div` para tudo | O leitor de tela e o Google não entendem a página | Pergunte "o que é isto?" e escolha a tag pelo significado |
| Vários `h1` ou níveis pulados | Bagunça a estrutura da página para quem navega por títulos | Um `h1`, depois `h2` e `h3` em ordem |
| Escolher a tag pelo tamanho visual | O `h3` não é "título pequeno", é "subtítulo de um `h2`" | Escolha a tag pela hierarquia e o tamanho pelo CSS |
| Ícone sem nome | O leitor de tela diz só "link" | Use um texto com `visually-hidden` ou `aria-label` |
| Remover o contorno de foco | Quem usa teclado se perde na página | Estilize o `:focus-visible` em vez de removê-lo |
| Usar `width: 100vw` | Cria uma barra de rolagem horizontal, porque o `100vw` inclui a largura da barra de rolagem | Use `width: 100%` |
| Posicionar com `position: absolute` ou margens "mágicas" | Quebra em qualquer outro tamanho de tela | Use Flexbox, Grid e `gap` para o layout. Deixe o `absolute` só para sobreposições pequenas, como um selo |
| Cores e espaçamentos soltos no CSS | O visual fica inconsistente e difícil de mudar | Use sempre as variáveis do `tokens.css` |
| Usar `!important` para "ganhar" uma briga de estilos | Cria uma bola de neve de `!important` | Entenda a especificidade e prefira classes simples, sem IDs no CSS |
| Imagens enormes | A página fica lenta, principalmente no 4G | Redimensione as fotos, use WebP e `loading="lazy"` |
| Testar só no próprio monitor | Os erros só aparecem no celular do usuário | Teste nas larguras da fase 4.2 a cada seção terminada, não só no fim |
| Commits gigantes no fim | Fica difícil voltar atrás se algo quebrar | Faça um commit por etapa concluída, por exemplo "Add header markup" e depois "Style header for mobile" |

---

## Ordem de execução resumida
1. Preparação: reset, tokens, fontes e base global.
2. Para cada seção, em ordem (Menu, Hero, Destaque, Grid e Rodapé):
   1. Escreva o HTML semântico, sem CSS, e confira se a página faz sentido "crua".
   2. Faça o CSS do celular no `style.css`.
   3. Faça o tablet e o desktop no `responsive.css`.
   4. Passe pelo checklist "pronto quando…" e faça o commit.
3. Fase final: acessibilidade, testes, validação e desempenho.
