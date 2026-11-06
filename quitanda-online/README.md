# 🥬 Quitanda Online

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap%205-7952B3?style=flat&logo=bootstrap&logoColor=white)
![Font Awesome](https://img.shields.io/badge/Font%20Awesome%206-528DD7?style=flat&logo=fontawesome&logoColor=white)
![Status](https://img.shields.io/badge/status-protótipo%20front--end-yellow)

## Visão Geral e Proposta de Valor

**Quitanda Online** é o wireframe navegável de um e-commerce de hortifruti: página inicial com catálogo de produtos, busca, ordenação, paginação, indicação de estoque e carrinho de compras. O objetivo do artefato é validar a experiência de navegação e a organização de layout **antes** de investir em back-end — uma prática comum em projetos reais onde o front-end estático serve como especificação viva para stakeholders e para a etapa seguinte de integração com API.

O recorte de negócio é claro: produtor rural vendendo direto ao consumidor, com destaque para promoções, controle visual de disponibilidade (`em estoque` / `esgotado` / `reabastecendo`) e canais de contato institucional no rodapé. Essas escolhas não são só estéticas — refletem raciocínio de varejo (gestão de expectativa de estoque, redução de fricção na descoberta de produto, sinalização de confiança institucional), detalhado na seção **Visão de Negócio de Varejo** abaixo.

## Demonstração Visual

> 📸 **Placeholder** — inserir aqui um GIF ou screenshot da página renderizada (recomenda-se capturar o carrossel, o grid de produtos responsivo e o comportamento do rodapé fixo em mobile/desktop).
>
> ```
> assets/demo/preview.gif
> ```

## Arquitetura e Decisões Técnicas

O projeto é **HTML estático puro**, sem build step, sem framework de JavaScript e sem dependências instaladas via gerenciador de pacotes — decisão adequada para um protótipo de validação de UI que precisa ser aberto e revisado por qualquer pessoa sem setup de ambiente.

- **Bootstrap 5** (via CDN) resolve o grid responsivo (`col-xl-2 col-lg-3 col-md-4 col-sm-6`) do catálogo, o carrossel do cabeçalho e os componentes de formulário (busca, ordenação, paginação), evitando CSS customizado repetitivo.
- **Bootstrap Icons + Font Awesome 6** cobrem os ícones de interface (carrinho, favoritos) sem depender de sprites próprios.
- **CSS inline mínimo**: apenas uma regra customizada (`p.truncate-3l`, via `-webkit-line-clamp`) para truncar a descrição dos produtos em 3 linhas — tudo o mais é resolvido com utilitários do Bootstrap, mantendo a superfície de manutenção pequena.
- **Estrutura de assets** organizada por domínio (`assets/produtos`, `assets/slides`, `assets/favicon`), preparada para crescer conforme novas páginas (`pages/contato.html`, `pages/cadastro.html`, `pages/login.html`, `pages/carrinho.html`) forem implementadas.

**Nota de manutenção:** o HTML referencia imagens em `assets/img/produtos/...`, `assets/img/slides/...` e `assets/img/favicon/...`, mas a estrutura real do projeto não tem o subdiretório `img/` (os arquivos estão direto em `assets/produtos/`, `assets/slides/` e `assets/favicon/`). É um ajuste de path pendente antes de considerar o protótipo "pronto para revisão visual". Da mesma forma, há duas versões do Bootstrap CSS carregadas simultaneamente (`5.0.0-alpha1` e `5.3.2`); a alpha deve ser removida para evitar conflito de especificidade.

## O Desafio Técnico

**Problema:** o rodapé usa `fixed-bottom`, o que o remove do fluxo normal do documento — sem tratamento, o conteúdo final da página (paginação e último produto do grid) fica coberto pelo rodapé, e a altura ocupada pelo rodapé muda de acordo com o breakpoint (o texto quebra em mais linhas em telas estreitas, menos em telas largas).

**Solução:** em vez de um `padding-bottom` fixo (que sobraria ou faltaria dependendo da tela), o layout usa três `<div>` espaçadores com alturas calculadas manualmente e associadas aos breakpoints do Bootstrap:

```html
<div style="height: 273px;" class="d-block d-md-none"></div>      <!-- mobile -->
<div style="height: 153px;" class="d-none d-md-block d-lg-none"></div> <!-- tablet -->
<div style="height: 129px;" class="d-none d-lg-block"></div>      <!-- desktop -->
```

Cada spacer é visível apenas na faixa de tela correspondente à altura real que o rodapé ocupa naquele breakpoint, garantindo que nenhum conteúdo fique escondido atrás do `fixed-bottom` — uma solução pragmática para uma limitação conhecida de footers fixos em CSS, sem recorrer a JavaScript para medir alturas em runtime.

## Funcionalidades

- [x] Catálogo de produtos em grid responsivo (12 itens, com imagem, preço, título e descrição truncada)
- [x] Indicação visual de estoque por produto (disponível com quantidade, esgotado, reabastecendo)
- [x] Carrossel de banners promocionais com imagens adaptadas para mobile e desktop
- [x] Busca, ordenação (nome/preço) e paginação da listagem
- [x] Navegação para carrinho, login, cadastro e contato (estrutura de páginas prevista em `pages/`)
- [x] Rodapé institucional fixo com dados da empresa e links legais

## Visão de Negócio de Varejo

Mesmo sendo um protótipo estático sem back-end, o HTML já embute decisões típicas de varejo alimentar — não apenas escolhas de layout:

- **Gestão de expectativa sobre estoque:** os três estados exibidos por produto (`X kg em estoque`, `Produto Esgotado!`, `Reabastecendo Estoque`) existem porque, em hortifruti, a disponibilidade muda com frequência conforme safra e fornecimento. Comunicar isso na vitrine — em vez de deixar todo produto "comprável" — evita frustração no momento do pedido e antecipa uma dúvida recorrente do cliente antes que ela vire contato de suporte.
- **Redução de fricção na descoberta do produto:** busca, ordenação por nome/preço e paginação (hoje apenas visuais, sem lógica de filtragem real) já refletem a decisão de estruturar o catálogo em torno de como o cliente decide — por nome ou por preço — antes de qualquer investimento em back-end de busca.
- **Sinalização de intenção de compra:** o ícone de favoritos (`bi-bookmark-heart`) em cada card antecipa um padrão comum de e-commerce — deixar o cliente marcar um produto para decidir depois — mesmo sem persistência implementada.
- **Confiança institucional:** o rodapé expõe CNPJ, endereço físico, política de privacidade, termos de uso e trocas/devoluções, elementos que reduzem a percepção de risco de quem está comprando alimento fresco de um fornecedor que ainda não conhece.

**Sobre prova social:** este protótipo, por ser mais simples, ainda **não** tem avaliações por estrelas, depoimentos de clientes ou compartilhamento social — recursos presentes no catálogo irmão **Lady Ella**. A evolução natural deste projeto seria incorporá-los aqui também: em hortifruti, onde o cliente decide sem ver ou tocar o produto, prova social de frescor e qualidade tende a pesar tanto quanto preço na decisão de compra.

## Possíveis Aplicações para Negócio e Expansões Futuras

O protótipo hoje é apenas front-end estático, sem lógica de backend ou integrações — os itens abaixo são **sugestões de evolução** para quando o projeto avançar para uma etapa com back-end real, e não funcionalidades já implementadas:

- **Parcerias com produtores e agricultura familiar local:** a mesma vitrine de catálogo já usada para os produtos atuais poderia dar espaço a pequenos produtores da região, com identificação de origem/produtor por item — agregando valor percebido (procedência) e abrindo um canal de venda para quem normalmente não tem loja própria online.
- **Assinatura de caixa de hortifruti (recorrência semanal/quinzenal):** evolução natural do catálogo de produtos frescos já existente — em vez de compra avulsa, o cliente assina uma caixa recorrente com curadoria de sazonalidade, reduzindo esforço de decisão e criando receita previsível para o negócio.
- **Avaliações e depoimentos reais:** como já apontado na seção de Visão de Negócio de Varejo, a ausência de prova social é uma lacuna conhecida deste protótipo; vale reforçar aqui que ela também viabiliza destacar produtores/parceiros específicos nas avaliações, e não só os produtos.
- **Delivery via parceiros logísticos locais:** hortifruti tem janela curta de validade, o que torna entrega própria ou centralizada pouco escalável; integrar transportadoras/entregadores locais (motoboy, cooperativas de entrega) permitiria expandir a área de cobertura sem o negócio assumir frota própria.

## Como Executar o Projeto

Não há dependências, build ou servidor obrigatório — é um site estático.

**Pré-requisitos:**
- Qualquer navegador moderno
- (Opcional) Um servidor HTTP local, recomendado para evitar restrições de `file://` em alguns navegadores

**Opção 1 — abrir direto:**

```bash
# Windows
start index.html
```

**Opção 2 — servir localmente (recomendado):**

```bash
# Python 3
python -m http.server 8000

# ou Node.js
npx serve .
```

Depois acesse `http://localhost:8000` (ou a porta indicada) e abra `index.html`.
