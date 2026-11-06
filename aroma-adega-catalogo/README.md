# 🍷 Aroma Adega — Catálogo Digital

<p align="left">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5">
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3">
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript">
  <img src="https://img.shields.io/badge/Bootstrap-5.3.2-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white" alt="Bootstrap 5.3.2">
  <img src="https://img.shields.io/badge/Backend-Nenhum%20(Client--Side)-lightgrey?style=for-the-badge" alt="Sem backend">
  <img src="https://img.shields.io/badge/Status-MVP%20funcional-success?style=for-the-badge" alt="Status: MVP funcional">
</p>

## Visão Geral e Proposta de Valor

Mais do que um exercício técnico, este projeto foi construído como estudo de caso de **raciocínio de negócio de varejo/e-commerce** aplicado a um segmento específico: bebidas alcoólicas premium. Numa adega, o produto costuma ser presente ou consumo social — o cliente decide sem provar, sem ver o rótulo de perto e, com frequência, sem conhecer a loja pessoalmente. Isso torna a **confiança** (prova de que outros clientes compraram e aprovaram, transparência de estoque, clareza de preço e forma de pagamento) tão determinante para a conversão quanto o catálogo em si — é esse racional de negócio, não apenas a implementação em HTML/CSS/JS, que orienta as decisões de interface descritas abaixo (ver "Prova Social e Confiança do Cliente").

**Aroma Adega** é o catálogo digital de uma adega física em Piracicaba-SP (vinhos, espumantes e destilados). O problema de negócio era claro: a lojista precisava de presença digital com aparência e comportamento de e-commerce — busca, filtros, carrinho, favoritos — **sem o custo, a complexidade e o tempo de implantação de uma loja virtual completa** (gateway de pagamento, backend, autenticação real).

A solução foi um modelo de **"catálogo com venda assistida"**: toda a experiência de navegação (filtrar, comparar, montar um carrinho, favoritar produtos) acontece 100% no navegador do cliente, e o único ponto de saída do site é o WhatsApp, onde um atendente humano fecha o pedido. Isso elimina a necessidade de PCI compliance, banco de dados de pedidos e login real — trade-off deliberado para o estágio atual do negócio, documentado como decisão de arquitetura (não como limitação escondida).

**Impacto prático:** um site publicável em qualquer hospedagem estática gratuita (GitHub Pages, Netlify, Vercel), sem custo recorrente de infraestrutura, que já entrega catálogo pesquisável, carrinho persistente, lista de desejos e captação de leads (cadastro/login) — validando presença digital antes de qualquer investimento em checkout real.

## Seção Visual

> 📸 **Espaço reservado para demonstração visual.**
> Sugestão: um GIF curto (10–15s) navegando pelo hero → aplicando um filtro de categoria → adicionando um produto ao carrinho → finalizando pelo WhatsApp, **ou** 2–3 screenshots (desktop + mobile) do catálogo e do modal de detalhes do produto.
>
> ```
> ![Demonstração do Aroma Adega](./docs/demo.gif)
> ```

## Prova Social e Confiança do Cliente

Numa adega sem provador nem degustação no site, a decisão de compra depende de sinais que substituam a confiança que uma loja física transmite. O projeto já implementa vários desses sinais — nem todos ligados a um backend real, mas todos pensados como interface de confiança:

| Elemento | Função de UX/negócio |
|---|---|
| **Avaliação por estrelas + nº de avaliações por produto** (`rating`, `reviewsCount` em `products.js`, renderizadas via `formatStarsHTML()`) | Reduz a incerteza de comprar um rótulo à distância — o cliente vê o consenso de outros compradores antes de decidir |
| **Depoimentos de clientes** (foto, nota 5 estrelas, citação e selo "Cliente Verificado") | Ancoragem de confiança logo nos primeiros segundos de navegação, reforçada por um segundo bloco de avatares + "+500 clientes curtiram e deixaram depoimento" já no hero |
| **Formulário "Conte sua experiência"** | Convite ativo à cliente para gerar novo depoimento — mantém a prova social viva mesmo sem sistema de reviews real por trás |
| **Selos dinâmicos "🔥 Mais Vendido / ⭐ Novidade / 🏷️ Oferta"** | Gatilho de prova social e urgência, gerado a partir das flags do próprio catálogo (`bestseller`, `isNew`, `onSale`) — não é texto fixo, muda junto com o produto |
| **Indicador de estoque ("X itens em estoque") e "Avisar quando chegar"** | Gatilho de escassez honesto — e, quando o produto está esgotado, converte a frustração em captura de lead pelo WhatsApp em vez de abandono da venda |
| **Tags de PIX/cashback e parcelamento em 3x** | Remove a barreira "não sei se cabe no orçamento" num segmento (vinhos, espumantes) com ticket médio de R$ 94,50 a R$ 438,50 |
| **Compartilhar produto/site (Web Share API)** | Transforma cada visitante satisfeito em canal de divulgação, sem depender de tráfego pago |
| **Faixa de redes sociais + carrossel de parceiros (vinícolas/distribuidoras)** | Reforça procedência e legitimidade do produto — relevante num segmento onde a autenticidade do rótulo é parte da confiança |

**A leitura de negócio:** nenhum desses elementos é tecnicamente complexo — estrelas, um selo ou um contador de estoque são poucas linhas de código. O valor está em reconhecer que, num produto de consumo/presente como vinho e espumante, a interface precisa fazer o trabalho que a loja física faria sozinha: mostrar que outras pessoas compraram e gostaram, ser transparente sobre disponibilidade e nunca deixar o cliente sem uma próxima ação (comprar, favoritar, ser avisado do reabastecimento). Por isso os depoimentos, avatares e o formulário de avaliação já foram desenhados como parte definitiva do produto, mesmo com conteúdo hoje placeholder — o [`DOCUMENTACAO.md`](./Loja%20Adega%20Aroma/DOCUMENTACAO.md) documenta explicitamente o que ainda precisa ser substituído por depoimentos e fotos reais antes de publicar em produção, para que a intenção de produto fique clara mesmo antes desse conteúdo existir.

## Possíveis Aplicações para Negócio e Expansões Futuras

O catálogo atual resolve presença digital com venda assistida, mas a mesma base (vitrine de produtos + confiança social + WhatsApp como canal de conversão) abre caminho para expandir tanto o alcance de produto quanto a rede de parceiros da adega, sem exigir reescrita da arquitetura:

- **Marketplace de pequenos produtores/vinícolas locais** — abrir a vitrine para rótulos de produtores da região de Piracicaba além do estoque próprio da loja, com comissão por venda intermediada via WhatsApp. Aproveita o carrossel de parceiros (vinícolas/distribuidoras) já existente no README como prova de procedência e transformaria esse selo de "quem produz" em canal de receita, não só de credibilidade.
- **Clube de assinatura (caixa mensal de vinhos selecionados)** — evolução natural do catálogo: em vez de compra avulsa, curadoria recorrente (ex: 2–3 rótulos por mês, com ficha de degustação) usando as mesmas flags de catálogo (`bestseller`, `isNew`) para montar a seleção e o mesmo checkout assistido por WhatsApp para confirmar a assinatura e o pagamento recorrente.
- **Cupons cruzados com comércio local complementar** — parcerias com restaurantes, queijarias e lojas de decoração/presentes da cidade, oferecendo desconto cruzado (ex: cupom da adega ao comprar num restaurante parceiro, e vice-versa). Faz sentido direto com o próprio racional do README de que o vinho é "produto de presente ou consumo social" — os parceiros ideais são quem atende essa mesma ocasião de compra.
- **Delivery via parceiro logístico local** — como alternativa ou complemento ao fechamento manual por WhatsApp, integrar um motoboy/transportadora parceira da região para pedidos que não exigem atendimento consultivo, reduzindo a carga do atendente humano nos horários de pico sem abrir mão do WhatsApp para os casos que precisam de orientação (ex: harmonização, escolha de rótulo).
- **Eventos de degustação (presenciais ou lives) como motor de prova social real** — hoje o formulário "Conte sua experiência" mantém a prova social viva sem persistir dados de verdade (limitação já documentada no [`DOCUMENTACAO.md`](./Loja%20Adega%20Aroma/DOCUMENTACAO.md)); eventos de degustação, na loja física ou em live, dão um motivo concreto para captar depoimentos, fotos e avaliações genuínas de clientes reais, substituindo gradualmente o conteúdo placeholder por prova social autêntica.

## Arquitetura e Decisões Técnicas

O projeto é **HTML/CSS/JS estático, sem build e sem framework de front-end** — decisão deliberada, não limitação técnica:

- **Sem backend**: não há servidor, banco de dados ou API própria. Todo estado (carrinho, lista de desejos, produtos vistos recentemente, preferência de acessibilidade) vive em `localStorage`, no navegador do próprio cliente.
- **Bootstrap 5.3.2 (via CDN)** para grid, modais, navbar responsiva e carrossel — evita reescrever componentes de UI testados, mantendo o CSS customizado (`style.css`) focado só na identidade visual da marca.
- **JavaScript puro (vanilla)**, organizado em módulos por responsabilidade e compartilhados entre as 5 páginas do site:
  - `products.js` — fonte única do catálogo (`AROMA_PRODUCTS`), consumida pelo carrinho, pela wishlist, pelas vitrines e pelo "você também pode gostar".
  - `cart.js` — API do carrinho (`getCart`, `addToCart`, `setCartQty`, `updateCartBadge`...) reutilizada em `index.html` e em todas as páginas de `pages/`.
  - `accessibility.js` — alternância de alto contraste, com um script inline no `<head>` de cada página aplicando a preferência **antes** do primeiro paint (evita flash de tela).
- **Checkout via WhatsApp (`wa.me`)**: o carrinho monta dinamicamente uma mensagem com todos os itens, quantidades e total, e abre o WhatsApp já preenchido — esse é o único caminho de finalização de pedido do site.
- **Identificação de produto por ID estável, não por nome**: todo o estado (carrinho, wishlist, vistos recentemente) referencia produtos por `data-product-id` (`p1`...`p8`), nunca pelo texto do título — decisão que evitou um bug real (ver desafio técnico abaixo).
- **Zero dependência de build**: os arquivos são servidos como estão; o único cuidado documentado é preferir um servidor local (`npx serve .` / `python -m http.server`) em vez de abrir por `file://`, já que Firefox/Safari particionam `localStorage` de forma mais rígida entre arquivos locais que Chrome/Edge.

## O Desafio Técnico

**Problema:** o site tem 8 cards de produto, mas apenas 2 títulos realmente distintos — 6 dos 8 cards compartilham o texto "Vinhos Tinto, Rosé e Branco". Ao implementar a alternância de filtros (`.category-filter`), a visibilidade dos cards precisava ser controlada dinamicamente em JavaScript, alternando uma classe `.d-none` em vez de manipular `style.display` diretamente. A primeira tentativa, usando `element.style.display = 'none'`, simplesmente **não funcionava** para esconder os cards.

A causa era um problema de especificidade CSS: cada `.product-item` também carrega `.d-flex` do Bootstrap, uma classe `!important`. Um `style` inline nunca vence uma classe `!important` — então o card continuava visível independentemente do JS.

**Solução:** trocar a manipulação para `classList.toggle('d-none', !visible)`. Como `.d-none` e `.d-flex` são duas classes `!important` de mesma especificidade, o empate é resolvido pela **ordem de declaração no CSS** — e no bundle do Bootstrap, `.d-none` é declarado depois de `.d-flex`, então vence. A correção ficou documentada explicitamente no código-fonte do projeto para não ser revertida por engano em uma futura refatoração.

O mesmo cuidado com "referenciar por ID, nunca por texto" apareceu de novo na lista de desejos: uma implementação ingênua baseada no nome do produto favoritaria simultaneamente todos os 6 cards com título repetido. A correção foi padronizar `toggleWishlist()` e `handleAddToCart()` para sempre resolver o produto pelo `data-product-id` do card mais próximo (`.closest()`), independentemente do texto exibido — um padrão replicado em carrinho, wishlist e "vistos recentemente".

## Funcionalidades

- [x] **Catálogo com busca, filtro por categoria/preço e ordenação** — tudo em JavaScript puro, sem dependências extras, com resultado e contador atualizados em tempo real
- [x] **Carrinho persistente (`localStorage`)** com seletor de quantidade, badge dinâmico na navbar e finalização de pedido via WhatsApp (mensagem montada automaticamente com itens, quantidades e total)
- [x] **Lista de desejos (wishlist)** com contador em tempo real e página dedicada, sincronizada entre todas as páginas do site
- [x] **Vitrines dinâmicas** (Mais Vendidos, Novidades, Vistos Recentemente) com carrossel de avanço automático, geradas a partir das flags do catálogo (`bestseller`, `isNew`)
- [x] **Cadastro e login "assistidos"** — formulários validados que abrem o WhatsApp com os dados preenchidos, sem expor senha em nenhum momento na mensagem enviada
- [x] **Alto contraste (acessibilidade)** com preferência persistida entre páginas e aplicada antes da primeira renderização, evitando flash visual

## Como Executar o Projeto

**Pré-requisitos:** nenhum. É um site 100% estático — sem `npm install`, sem build, sem variáveis de ambiente.

O código-fonte está em [`Loja Adega Aroma/`](./Loja%20Adega%20Aroma/).

```bash
# Opção 1 — abrir direto no navegador
# (funciona bem no Chrome/Edge; carrinho/wishlist compartilham localStorage normalmente)
cd "Loja Adega Aroma"
start index.html   # Windows
# ou apenas dê duplo clique em index.html

# Opção 2 — servidor local (recomendado para testar em qualquer navegador)
cd "Loja Adega Aroma"
npx serve .
# ou
python -m http.server 8080
```

Documentação técnica detalhada (seção por seção do código, pontos de customização e limitações conhecidas) está em [`Loja Adega Aroma/DOCUMENTACAO.md`](./Loja%20Adega%20Aroma/DOCUMENTACAO.md).
