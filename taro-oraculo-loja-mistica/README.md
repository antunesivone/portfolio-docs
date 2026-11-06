# Tarô Oráculo

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Web Audio API](https://img.shields.io/badge/Web%20Audio%20API-áudio%20procedural-6f42c1?style=flat-square)
![Web Crypto API](https://img.shields.io/badge/Web%20Crypto%20API-hash%20SHA--256-6f42c1?style=flat-square)
![Build step](https://img.shields.io/badge/build%20step-nenhum-informational?style=flat-square)
![Status](https://img.shields.io/badge/status-conclu%C3%ADdo-success?style=flat-square)

## Visão geral

**Tarô Oráculo** é uma aplicação web de tiragens de Tarô voltada a autoconhecimento e
reflexão — baralho completo de 78 arcanos, três tiragens interpretativas e uma camada de
"engenharia" sobre a leitura (dignidades elementais entre cartas vizinhas, alertas
estatísticos de predominância) que vai além da carta isolada e aproxima a experiência de
uma leitura orientada por padrões, não só de sorte.

O mesmo repositório também hospeda a **Loja Mística Venus**, uma loja esotérica de
demonstração com catálogo, carrinho, lista de desejos e checkout simulado — construída
para replicar decisões reais de UX de e-commerce (navegação livre sem cadastro, login
exigido só no momento de conversão) e não apenas uma vitrine estática de produtos.

As duas partes do repositório usam a mesma stack, mas a decisão de projetá-las juntas é de
**negócio, não de código**: quem chega para uma tiragem de Tarô já demonstrou interesse
pelo universo esotérico, e a loja é a extensão natural de monetização desse mesmo
público — não um catálogo genérico anexado ao projeto para preencher o portfólio. Por
isso a Loja Mística Venus reproduz, de propósito, mecanismos reais de varejo online
(ancoragem de preço, prova social, gatilho de escassez, checkout sem fricção — ver seção
[Prova social e persuasão de compra](#prova-social-e-persuasão-de-compra)) que existem
para converter interesse em compra: o mesmo raciocínio de produto que orienta qualquer
e-commerce de verdade, não apenas a habilidade de escrever o carrinho em JavaScript.

## Prévia

> **[Espaço reservado para GIF/print da aplicação]**
> Sugestão: capturar a tiragem completa (embaralhar → revelar cartas com flip 3D →
> interpretação com dignidades) e, em seguida, o fluxo de carrinho da loja como
> convidado até o checkout.

## Arquitetura e decisões técnicas

Projeto 100% front-end — HTML, CSS e JavaScript puro, sem framework, sem bundler e sem
dependências via npm. Dois sites (jogo e loja) compartilham módulos via `shared/`, e cada
arquivo JS expõe um único objeto global via IIFE (`Auth`, `Cart`, `HeaderUI`, `LojaUI`,
`App`...) — como não há build step, a ordem dos `<script>` no HTML é o próprio mecanismo
de resolução de dependências.

| Decisão | Alternativa considerada | Por quê |
|---|---|---|
| Vanilla JS, sem framework/bundler | React/Vue + Vite/Webpack | Peça de portfólio 100% estática, sem infraestrutura de build, deploy em qualquer servidor de arquivos, evidencia domínio direto de DOM/JS |
| Módulos como globais IIFE + ordem de `<script>` | ES Modules com bundler | Mantém "abrir e rodar" sem etapa de compilação, ao custo de disciplina manual na ordem de carregamento |
| Música ambiente sintetizada via Web Audio API | Arquivo de áudio (mp3/ogg) | Elimina asset binário pesado; o pad místico é gerado em tempo real por osciladores, não é um arquivo tocando em loop |
| Senha com salt aleatório + SHA-256 (Web Crypto API) | Senha em texto puro no `localStorage` | Mesmo sendo uma demo sem backend, evita o erro básico de guardar credencial em claro — decisão deliberada de segurança, com limite documentado |
| Carrinho/desejos liberados para "convidado" | Bloquear qualquer interação sem login | Replica UX de e-commerce real: reduz fricção de navegação e só exige conta no momento de conversão (checkout/WhatsApp) |
| Especificidade CSS resolvida por ordem de regras (`.product-grid--carousel` depois de `.product-grid`) | `!important` ou seletor mais específico | Mantém o CSS legível sem `!important`; ordem documentada em `DOCS.md` para não reintroduzir o bug já visto uma vez |

## O desafio técnico: estado de carrinho entre visitante anônimo e conta logada

**Problema.** A loja precisa permitir navegar, adicionar ao carrinho e favoritar produtos
sem exigir cadastro — login só é cobrado no momento de finalizar a compra ou pedir pelo
WhatsApp, igual à maioria dos e-commerces reais. Isso cria um problema clássico de estado
client-side: como manter o carrinho de um visitante anônimo isolado do carrinho de cada
conta (várias pessoas podem usar o mesmo navegador), e ainda assim **não perder nada**
quando esse visitante decide logar no meio da sessão de compras?

**Solução.** `Cart` e `Wishlist` (`loja/js/cart.js`, `loja/js/wishlist.js`) derivam a
chave de `localStorage` dinamicamente a partir da sessão atual —
`taro_loja_cart_<username>` quando há login, `taro_loja_cart_convidado` caso contrário —
consultando `Auth.getSession()` a cada operação, em vez de fixar o dono do carrinho uma
única vez. No instante exato em que o login é concluído (callback `onAuthenticated`, tanto
em `loja/js/app.js` quanto em `loja/js/contato.js`), `Cart.mergeGuestIntoUser(username)` e
`Wishlist.mergeGuestIntoUser(username)` somam o conteúdo salvo como convidado dentro da
conta recém-autenticada e só então apagam a chave de convidado — eliminando o cenário em
que a cliente monta o carrinho, faz login para fechar a compra, e o carrinho aparece
vazio.

**A leitura de negócio.** Tecnicamente isso é só troca de chave de `localStorage`, mas a
motivação é de retenção: em qualquer loja real, forçar login antes de deixar a visitante
montar o carrinho é a maneira mais comum de perder venda por atrito, e um carrinho que
"esquece" o que a cliente escolheu no momento exato em que ela decide confiar no site
(criando conta) é o tipo de bug que destrói a confiança recém-conquistada. Resolver isso
não é elegância técnica gratuita — é proteger a conversão no ponto mais frágil do funil.

## Prova social e persuasão de compra

A Loja Mística Venus não depende de uma marca física para gerar confiança — ela precisa
convencer com o que está na tela. Os elementos abaixo existem de verdade no código
(`loja/js/ui.js`, `loja/js/testimonials.js`, `loja/js/products-data.js`) e cada um mapeia
a uma decisão de UX/negócio, não é decoração:

| Elemento no card/produto | Função de UX/negócio |
|---|---|
| Selo de desconto (`-X%`) + preço riscado (`precoOriginal`) | Ancoragem de valor — o desconto fica visualmente óbvio, sem cálculo mental da cliente |
| Parcelamento calculado automaticamente (`3x de R$ ...`) | Remove a barreira "não sei se cabe no orçamento" antes mesmo do clique |
| Estrelas de avaliação + número de avaliações (`nota`, `numAvaliacoes`) | Prova social explícita em todo card e no modal de detalhes, mesmo sem sistema de reviews real por trás |
| Depoimentos com estrelas (`Testimonials`, formulário "deixe seu depoimento") | Confiança de terceiros — e conteúdo que a própria visitante pode gerar, sem custo de produção para a loja |
| Indicador de estoque ("Em falta" / "X unidades disponíveis") + botão desabilitado no limite | Gatilho de escassez honesto — nunca deixa a cliente pedir mais do que existe |
| Vitrines segmentadas por contagem de vendas (Ofertas, Mais Vendidos, Novidades, Vistos Recentemente) | "Mais Vendidos" é prova social por popularidade; "Vistos Recentemente" reduz o custo de retomar uma decisão adiada |
| Botão de compartilhar (Web Share API, com fallback WhatsApp/Facebook/X/Pinterest) | Transforma cada visitante em canal de divulgação em potencial, sem depender de tráfego pago |
| Coração de favoritos (wishlist) + "Você também pode gostar" no modal | Caminho de volta para quem não compra agora, em vez de abandono definitivo |

O padrão é o mesmo em toda a tabela: nenhum desses elementos é tecnicamente complicado de
implementar — o valor está em saber *que* eles precisam existir e *onde* posicioná-los no
funil (perto do preço, perto do botão de compra, numa seção própria de comunidade) para
reduzir a incerteza de comprar à distância, que é o problema central de qualquer loja sem
vitrine física.

## Possíveis Aplicações para Negócio e Expansões Futuras

O catálogo atual da Loja Mística Venus já cobre nove categorias (velas, óleos de
aromaterapia, ervas, saboaria, cristais, acessórios/decoração, tarô, livros e vestuário
esotérico) e a arquitetura de sessão/carrinho/wishlist foi construída para escalar além de
"vender produto físico". As direções abaixo partem do que o projeto já resolve
tecnicamente, não de funcionalidades desconexas do tema:

- **Marketplace de consultas com prestadoras de serviço do nicho**: tarólogas,
  terapeutas holísticas e astrólogas parceiras poderiam ser cadastradas como
  "vendedoras" de um novo tipo de item — a consulta —, reaproveitando o mesmo card de
  produto (estrelas, depoimentos, preço, botão de compra) e o mesmo checkout via
  WhatsApp já usado para produtos físicos. A loja deixaria de vender só o baralho e
  passaria a vender também a leitura feita por uma pessoa real, capturando uma fatia de
  receita maior do mesmo público que já demonstrou interesse esotérico ao jogar.
- **Marketplace multi-vendedor de baralhos e itens esotéricos parceiros**: a estrutura
  de `products-data.js` (categoria, preço, estoque, avaliações) já é genérica o
  suficiente para receber um campo de vendedor/fornecedor, permitindo que ateliês de
  baralhos artesanais, cristaleiras ou saboarias parceiras publiquem produtos na mesma
  vitrine em troca de comissão — sem precisar de uma loja própria para começar a vender.
- **Cupons e parcerias de afiliados com criadoras de conteúdo do nicho**: tarólogas,
  astrólogas e criadoras de bem-estar com audiência própria poderiam divulgar um cupom
  de desconto rastreável (reaproveitando o mecanismo de `precoOriginal`/desconto já
  existente) em troca de comissão por venda, transformando o botão de compartilhar
  (Web Share API) hoje pensado para a cliente final em um canal também para
  divulgadoras profissionais do nicho.
- **Tiragens ao vivo e conteúdo recorrente como funil de retenção e conversão**: uma
  sessão de tiragem de cartas ao vivo (própria equipe ou taróloga parceira), anunciada
  para quem já jogou no site, dá um motivo recorrente para a jogadora voltar — e cada
  sessão é uma vitrine natural para os produtos exibidos ("Mais Vendidos",
  "Novidades") sem parecer publicidade forçada, fechando o ciclo jogo → engajamento →
  compra que o README já descreve na visão geral.
- **Programa de fidelidade sobre a base de merge de carrinho/wishlist**: o mecanismo que
  hoje soma carrinho e lista de desejos do visitante anônimo à conta no momento do login
  (`Cart.mergeGuestIntoUser`, `Wishlist.mergeGuestIntoUser`) já resolve o problema mais
  difícil de um programa de pontos — identificar de forma confiável o histórico de uma
  cliente entre sessões. É pouco esforço incremental transformar essa mesma trilha de
  eventos em acúmulo de pontos, descontos progressivos ou acesso antecipado a
  lançamentos, em vez de construir identidade de cliente do zero.
- **Recomendação orientada pelos dados de comportamento já coletados**: as vitrines de
  "Vistos Recentemente" e "Mais Vendidos" já capturam sinal de intenção de compra por
  produto; esse mesmo sinal, cruzado com a carta ou tiragem que a pessoa tirou no jogo
  (ex.: quem tira cartas de terra recebe sugestão de cristais de enraizamento), abre
  caminho para personalizar a loja pelo resultado do próprio jogo — uma ponte entre as
  duas metades do repositório que hoje compartilham stack, mas não compartilham dados.

## Funcionalidades

- [x] Motor de tiragem com 78 cartas, embaralhamento Fisher-Yates, 50% de chance de carta
      invertida e cálculo de dignidades elementais entre cartas vizinhas
- [x] Autenticação local sem backend, com hash de senha (salt aleatório + SHA-256 via Web
      Crypto API) — usada tanto no jogo quanto na loja
- [x] E-commerce de demonstração completo: catálogo com busca/filtro/ordenação, carrinho
      e lista de desejos com suporte a visitante anônimo, checkout simulado e pedido via
      WhatsApp
- [x] Prova social de compra: avaliação por estrelas, depoimentos de clientes, indicador
      de estoque e compartilhamento de produto (Web Share API, com fallback de redes
      sociais)
- [x] Modo de alto contraste (acessibilidade), independente do tema claro/escuro e
      persistido no navegador
- [x] Música ambiente mística gerada em tempo real com Web Audio API, sem arquivos de
      áudio
- [x] Vitrines dinâmicas em carrossel na loja (Ofertas, Mais Vendidos, Novidades, Vistos
      Recentemente)

## Como executar o projeto

**Pré-requisitos**: um navegador moderno. Não há variáveis de ambiente, chaves de API nem
instalação de pacotes — o projeto não tem dependências.

Sirva a pasta com qualquer servidor estático (recomendado, evita restrições de `file://`
em alguns navegadores) ou abra `index.html` diretamente:

```bash
npx serve .
# ou
python -m http.server 8000
```

Depois acesse:

- `http://localhost:8000/` — jogo (Tarô Oráculo)
- `http://localhost:8000/loja/` — loja (Loja Mística Venus)
- `http://localhost:8000/loja/contato.html` — contato

> Documentação técnica de arquitetura e guia de expansão (como adicionar uma tiragem, um
> produto, uma categoria etc.) está em **[DOCS.md](DOCS.md)**.

---

&copy; 2026 Ivone Antunes | VenusTech. Todos os direitos reservados.
