# ☕ AROMA — Loja Virtual & Site Institucional

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Font Awesome](https://img.shields.io/badge/Font%20Awesome-528DD7?style=flat&logo=fontawesome&logoColor=white)
![Backend](https://img.shields.io/badge/backend-nenhum%20(100%25%20client--side)-lightgrey)
![Status](https://img.shields.io/badge/status-concluído-brightgreen)

E-commerce completo para negócios locais (cafeteria/adega/tea house), rodando **100% no navegador** — sem servidor, sem banco de dados, sem build step.

---

## Visão geral

Pequenos negócios que hoje vendem por WhatsApp/Instagram raramente têm orçamento para uma plataforma de e-commerce com servidor, banco de dados e gateway de pagamento próprio. O **AROMA** resolve isso entregando uma vitrine online completa — catálogo, carrinho, checkout, histórico de pedidos, lista de desejos e até uma agente de vendas por IA — usando apenas **HTML, CSS e JavaScript puros**, com o estado do cliente persistido inteiramente em `localStorage` e o fechamento do pedido feito via **WhatsApp**, canal onde o negócio já atende.

O resultado é um modelo replicável: qualquer varejo local pode colocar seu catálogo no ar sem custo de hospedagem além de um site estático, trocando apenas produtos, cores, número de WhatsApp e o treinamento da agente de vendas.

Mais do que um exercício técnico isolado, o projeto foi pensado como **jornada de varejo ponta a ponta**: são 13 páginas coordenadas — home, loja, produto, carrinho, checkout, pedidos, lista de desejos, cadastro/login e institucionais (trocas e devolução, política de privacidade, termos de uso, fale conosco) — e não uma landing page única. A mesma atenção dedicada ao carrinho foi dedicada à página de trocas/devolução e ao "comprar novamente" no histórico de pedidos, porque um cliente real não para de comprar depois do checkout. É esse raciocínio de **negócio de varejo**, tanto quanto a implementação técnica, que o projeto busca demonstrar.

## Demonstração

> 📸 **[Inserir aqui um GIF ou screenshot da aplicação em uso: navegação pela loja, seletor de quantidade, checkout em 3 etapas e/ou o chat da Amanda]**

## Arquitetura e decisões técnicas

Sem framework, sem SPA, sem backend — por escolha, não por limitação. O objetivo era um site 100% estático que qualquer navegador entrega instantaneamente e qualquer host gratuito serve sem configuração.

| Decisão | Alternativa considerada | Por quê |
|---|---|---|
| JS puro, sem framework | React/Vue + build step | Site institucional + catálogo não justificam o custo de bundler/dependências; carregamento mais rápido e zero manutenção de tooling |
| `localStorage` como "banco de dados" | Backend com API + DB | Elimina custo de servidor/hospedagem — essencial para o orçamento do público-alvo (pequenos negócios) |
| Checkout finalizado via WhatsApp | Gateway de pagamento real (Stripe/Mercado Pago) | O negócio já atende e fecha vendas por WhatsApp; integrar ali reduz fricção em vez de forçar um canal novo |
| Um único `script.js`/`style.css` compartilhado entre 13 páginas | Cada página com seu próprio script | Mantém consistência de UI e de dados (carrinho, favoritos) em todo o site sem duplicar lógica |
| Event delegation (listener único em `document`) | `addEventListener` por elemento | Cards são recriados via `innerHTML` (filtros, paginação, wishlist); delegação evita ter que religar listeners a cada re-render |

Documentação técnica completa (organização do `script.js`, chaves de `localStorage`, padrões de módulo) em **[site/DOCUMENTACAO.md](site/DOCUMENTACAO.md)**.

## O desafio técnico

**Problema:** o site é multi-página tradicional (13 arquivos `.html` independentes, sem roteador SPA), mas precisa se comportar como uma aplicação única — carrinho, favoritos, pedidos e histórico de navegação têm que permanecer consistentes conforme o cliente navega entre `loja.html`, `produto.html`, `carrinho.html` e `checkout.html`. Ao mesmo tempo, boa parte da interface (grid da loja, modal de produto, lista de desejos) é **recriada dinamicamente via `innerHTML`** a cada filtro, busca ou paginação — o que quebraria listeners de evento presos a elementos específicos.

**Solução:** duas decisões de arquitetura resolvem isso juntas:

1. **`localStorage` como fonte única de verdade**, lida/gravada por funções puras (`obterCarrinho()`/`salvarCarrinho()`, `obterFavoritosLoja()`, `obterPedidos()`), fazendo o estado sobreviver à troca de página sem precisar de um estado em memória compartilhado.
2. **Event delegation com atributos `data-*`**: em vez de vincular um listener a cada botão de card, um único listener no `document` (ou no container pai) intercepta o clique e resolve o alvo via `evento.target.closest('[data-id]')`. Isso significa que a loja pode re-renderizar 6, 60 ou 0 produtos a qualquer momento — filtro, ordenação, paginação — sem nunca perder a capacidade de resposta aos cliques.

Cada função de inicialização (`inicializarCarrinho`, `inicializarWishlist`, `inicializarCheckout` etc.) também verifica no início se os elementos daquela funcionalidade existem na página atual e se auto-desliga quando não — o que permite que um **único `script.js`** sirva as 13 páginas sem gerar erros de elemento inexistente, dispensando qualquer bundler ou sistema de módulos.

## Prova social e decisões de negócio

Num negócio sem loja física ou sem contato prévio (cafeteria/adega/tea house vendendo online), a confiança do cliente não vem só da marca — vem de ver outras pessoas satisfeitas. O site já tem essa camada implementada, não só planejada:

| Elemento | Função de UX/negócio |
|---|---|
| Depoimentos reais em carrossel na home, com nome, avatar e nota em estrelas (`.estrelas`, `aria-label="5 de 5 estrelas"`) | Substitui a confiança do balcão físico — quem chega pela primeira vez vê clientes satisfeitos antes de decidir comprar |
| Formulário "Deixe seu depoimento" — grava em `localStorage` e o depoimento entra no carrossel na hora, com nota em estrelas escolhida pelo próprio visitante | Conteúdo gerado pelo cliente (UGC), sem custo de produção para o negócio |
| Curtidas em cada depoimento (contador persistido por dispositivo) | Reforça socialmente os relatos mais relevantes, como um "like" de rede social |
| Compartilhamento de produto por WhatsApp/Facebook/copiar link, na loja e na página de detalhe | Cada visitante vira um canal de divulgação em potencial, reduzindo a dependência de tráfego pago |
| Indicador de estoque por variante ("18 itens em estoque", com marcador visual) | Transparência que acelera a decisão de compra sem recorrer a escassez artificial |
| "+500 Clientes Satisfeitos" em destaque logo na home | Prova social de volume — primeira informação que o visitante vê ao entrar no site |

**A leitura de negócio:** nenhuma dessas peças depende de um sistema de reviews ou CRM por trás — notas e curtidas são persistidas localmente, não agregadas de um banco real. O valor está em *onde* essa prova social foi posicionada (na home, antes de qualquer intenção de compra, e no card de produto, no momento da decisão) e em desenhar o formulário de depoimento para gerar conteúdo novo sem esforço manual da loja. O site não tem uma galeria de fotos/reels de clientes — diferente de um catálogo de moda, o produto aqui (bebida, experiência de degustação) se vende mais pelo relato do que pela imagem do cliente usando o produto.

## Possíveis Aplicações para Negócio e Expansões Futuras

O site já resolve o essencial de um e-commerce local; a arquitetura atual (catálogo único, `localStorage`, checkout via WhatsApp) também deixa espaço claro para crescer sem reescrever a base. Algumas direções concretas para o segmento (cafeteria/adega/tea house):

- **Marketplace de marcas parceiras**: a loja já exibe uma faixa de logos de fornecedores (Casa do Chá Imperial, Fazenda Café Dourado, Chá & Cia, Vinhos do Vale) como prova de curadoria — hoje puramente decorativa. Formalizar essa relação como catálogo multi-fornecedor (produto com `parceiroId`, comissão registrada por pedido) transformaria a vitrine de "catálogo próprio" em "vitrine de curadoria", ampliando o sortimento sem exigir estoque próprio do negócio.
- **Cupons e programa de afiliados**: o sistema de curtidas em depoimentos já identifica, sem esforço extra, os clientes mais engajados. Cruzar isso com o histórico de pedidos permite gerar cupons de indicação nominais para esses clientes — reforça a prova social existente com um incentivo mensurável, sem depender de CRM externo.
- **Assinatura recorrente (clube de degustação)**: para vinhos, chás e cafés, a curadoria mensal por plano fixo é um modelo de receita natural do segmento — reduz a dependência do checkout único via WhatsApp e cria previsibilidade de caixa.
- **Parcerias com prestadores de serviço do nicho**: sommeliers, baristas e especialistas em chá oferecendo sessões de degustação guiada (presencial ou por vídeo), vendidas como "produto de serviço" ao lado dos itens físicos — ligando a loja a experiências, não só a mercadoria, e abrindo receita sem exigir mais estoque.
- **Conteúdo gerado por clientes e parceiros**: o compartilhamento de produto (WhatsApp/Facebook/link) já existe; uma extensão natural é uma seção dedicada de fotos/relatos de degustação enviados por clientes, nos moldes da seção "Looks das Clientes" do projeto irmão Lady Ella — dando ao relato sensorial (já apontado como diferencial na seção de prova social) um espaço visual próprio, e funcionando também como vitrine para os parceiros do marketplace.

## Funcionalidades

- [x] Catálogo com busca, filtros (categoria, mais vendidos, novidades), ordenação e paginação
- [x] Carrinho, lista de desejos e histórico de pedidos com "comprar novamente", persistidos em `localStorage`
- [x] Checkout guiado em 3 etapas (pagamento → confirmação → acompanhamento de entrega simulado) com fechamento automático via WhatsApp
- [x] Descoberta de produtos: vistos recentemente, similares e compartilhamento direto de produto por link
- [x] Prova social: depoimentos com avaliação por estrelas, curtidas e formulário de envio (UGC) persistido em `localStorage`
- [x] Modo de alto contraste persistente, aplicado antes do render inicial para evitar flash de tema
- [x] Agente de vendas por IA (**Amanda**) com roteiro de sondagem, cross-sell e checkout estruturado — estudo prático de atendimento automatizado

## Como executar

Site 100% estático — sem instalação de dependências, sem build e **sem variáveis de ambiente**.

**Pré-requisitos:** apenas um navegador (e, opcionalmente, Python ou Node para servir localmente).

```bash
# Opção 1 — abrir direto
# duplo clique em site/index.html

# Opção 2 — servir localmente (recomendado)
cd site
python -m http.server 8000
# ou
npx serve .
```

Acesse `http://localhost:8000`.

---

Desenvolvido por **Ivone Antunes**, 2026 · **Venustech**
