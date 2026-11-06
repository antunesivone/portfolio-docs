# 🛍️ Lady Ella — Catálogo de Moda com UX/UI Orientado à Conversão

![Status](https://img.shields.io/badge/status-em%20produção-brightgreen)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E?logo=javascript&logoColor=black)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?logo=bootstrap&logoColor=white)
![No Backend](https://img.shields.io/badge/backend-nenhum%20(100%25%20client--side)-lightgrey)

> Desenvolvido por **Ivone Antunes** | VenusTech — desenvolvedora web com +3 anos de experiência em ambientes corporativos (Raízen, Diretoria Regional de Saúde).

---

## 🎨 Objetivo do Projeto

Este projeto foi construído como peça de portfólio para mostrar que a habilidade técnica é meio, não fim: cada linha de código aqui serve a uma decisão de **negócio de varejo/e-commerce**. São três frentes demonstradas num único case:

- **Desenvolvimento responsivo:** layout mobile-first funcional em qualquer dispositivo, sem framework JS, usando o grid e os componentes do Bootstrap como base.
- **UI/UX e criatividade visual:** a página é tratada como uma *landing page* de moda (hero, vitrines, seção de clientes reais, depoimentos) e não como uma lista de produtos genérica — um exercício de design tanto quanto de código.
- **Visão de negócio de varejo/e-commerce:** cada decisão de interface parte de perguntas de negócio, não só de estética — "o que faz o cliente **não se sentir perdido** e seguir até a compra?", "o que substitui a confiança que uma loja física transmite?" — respondidas com hierarquia de informação no card, prova social (estrelas, depoimentos, looks de clientes reais) e um caminho de checkout sem fricção via WhatsApp.

---

## 📌 Visão Geral e Proposta de Valor

**Lady Ella** é um catálogo online (moda, calçados, acessórios e cosméticos) para uma loja que vende por WhatsApp, não por checkout de cartão.

**O problema de negócio:** a loja precisava de uma vitrine digital profissional — com busca, filtros, favoritos e carrinho — sem herdar a complexidade (e o custo recorrente) de um e-commerce completo com gateway de pagamento, backend e gestão de pedidos. A venda em si já acontece manualmente, no WhatsApp.

**A solução:** um site 100% estático que replica a experiência de navegação de um e-commerce moderno, mas onde "finalizar compra" monta uma mensagem de WhatsApp pronta (itens, cores, tamanhos e total) em vez de abrir um checkout. Resultado: zero custo de hospedagem/manutenção de servidor, deploy em qualquer CDN estática, e uma base de código que a própria loja consegue editar sem saber programar.

---

## 🧠 UX/UI e Estratégia de Conversão nos Cards de Produto

O card de produto é o elemento mais repetido do site — por isso concentra a maior parte do trabalho de UX/UI e de negócio: cada informação nele foi escolhida para **reduzir a fadiga de decisão e induzir a compra**, sem esconder nada que o cliente precise saber antes de decidir.

| Elemento no card | Função de UX/negócio |
|---|---|
| **Selos "Novidade / Oferta / Mais Vendido"** | Gatilho de prova social e urgência — sinaliza em 1 olhar por que aquele produto merece atenção |
| **Preço riscado (`precoAntigo`) + preço atual** | Ancoragem de valor — o desconto fica visualmente óbvio, sem cálculo mental do cliente |
| **Parcelamento calculado automaticamente** | Remove a barreira "não sei se cabe no orçamento" antes mesmo do clique |
| **Estrelas de avaliação + nº de avaliações** | Prova social explícita, mesmo sem sistema de reviews real por trás |
| **Indicador de estoque ("últimas unidades" / esgotado)** | Gatilho de escassez honesto — e desativa o botão de compra quando não há estoque, evitando frustração |
| **Seletor de cor/tamanho direto no card** | Decisão de compra sem sair da grade — menos cliques até a intenção de compra |
| **Botão "Comprar" e "Ver detalhes" sempre visíveis** | Ação principal nunca escondida atrás de scroll ou de outro clique |
| **Quick View com carrossel, variações e produtos similares** | Mantém o cliente "dentro do fluxo" — resolve dúvida sem sair da página e recomenda alternativa na hora |
| **Coração de favoritos + "Vistos recentemente"** | Reduz abandono definitivo: o cliente que não compra agora tem um caminho fácil de voltar |

Esse mesmo raciocínio orienta a página como um todo: vitrines segmentadas (Novidades, Ofertas, Mais Vendidos) para que o cliente encontre o que busca em poucos cliques, navbar sticky com busca sempre acessível, e um único CTA de checkout — o WhatsApp — repetido em todos os pontos de decisão (card, Quick View, carrinho) para nunca deixar a pergunta "como eu compro isso?" sem resposta óbvia.

---

## 💬 Prova Social e Comunidade: Depoimentos, Estrelas e Looks das Clientes

Num varejo sem loja física nem provador, a confiança do cliente não vem da marca sozinha — vem de **ver outras pessoas reais comprando e gostando**. Essa é a lógica de negócio por trás de toda essa camada do site:

- **Avaliação por estrelas em cada produto:** reduz a incerteza de comprar à distância — o cliente vê rapidamente o que outras pessoas acharam antes de decidir, sem precisar perguntar.
- **Depoimentos e FAQ:** respondem objeções comuns antes que o cliente precise digitar no WhatsApp, economizando tempo de atendimento da loja e evitando que a venda esfrie por dúvida não resolvida.
- **Seção "Looks da Loja em Nossas Clientes":** vitrine de reels reais de clientes usando os produtos, com link "Siga-nos" para Instagram/TikTok — transforma a cliente satisfeita em vitrine viva da marca, prova social muito mais convincente que foto de catálogo.
- **Formulário "Apareça aqui!":** convite direto para a cliente enviar seu próprio depoimento + link do reels, incentivando geração contínua de conteúdo (UGC) sem custo de produção para a loja.
- **Compartilhamento de produtos (Web Share API):** qualquer produto pode ser compartilhado com um toque para WhatsApp/redes sociais — transforma cada visitante em um canal de divulgação em potencial, reduzindo a dependência de tráfego pago.

**A leitura de negócio:** nenhuma dessas peças é tecnicamente complexa — o valor está em decidir *onde* colocar prova social no funil (perto do preço, perto do botão de compra, e numa seção própria dedicada à comunidade) e em desenhar cada uma para reduzir trabalho manual da loja (a cliente gera o conteúdo, o FAQ tira dúvidas em escala). É a interface pensada para vender no varejo, não apenas para exibir produtos.

---

## 📈 Possíveis Aplicações para Negócio e Expansões Futuras

A base atual (catálogo em `products.js` + checkout via WhatsApp) já foi pensada para crescer por **conteúdo e parcerias**, não só por código:

- **Marketplace de múltiplas marcas parceiras:** hoje o catálogo é de uma única loja; o mesmo modelo de `products.js` + filtros por categoria/subcategoria suportaria produtos de **lojas parceiras** (outras marcas locais de moda/acessórios), com comissão por venda intermediada via WhatsApp.
- **Cupons e programa de afiliados:** o campo `precoAntigo` já sinaliza desconto — generalizar isso para cupons vinculados a **influenciadoras/clientes parceiras** cria um canal de indicação rastreável, sem precisar de backend (um cupom pode virar apenas um parâmetro na mensagem de WhatsApp).
- **Ampliar "Looks da Loja em Nossas Clientes" para parceiros locais:** a seção de UGC já recebe reels de clientes; abrir o mesmo formulário para **prestadores de serviço parceiros** (maquiadoras, fotógrafas, personal stylists que usam as peças) transforma a vitrine de clientes numa vitrine de comunidade/rede de parceiros, ampliando o alcance orgânico sem custo de produção.
- **Cross-sell com o segmento de bebidas/varejo local:** o mesmo padrão de checkout via WhatsApp usado aqui é idêntico ao usado no projeto irmão [Loja Adega Aroma](../lojaAromaCatalogo/README.md) — uma parceria cruzada (ex: cupom de uma loja aparecendo no checkout da outra) é viável sem mudar a arquitetura de nenhum dos dois sites.

Nenhum desses pontos exige mudar a arquitetura de fundo — o modelo "catálogo em array + render manual + checkout via WhatsApp" já foi desenhado para absorver novos tipos de conteúdo (parceiro, cupom, UGC de terceiros) sem virar gargalo técnico.

---

## 🖼️ Demonstração

<div align="center">

*(Inserir aqui um GIF ou screenshot do catálogo, do Quick View com carrossel, e do fluxo de checkout via WhatsApp)*

```
[ COLOQUE AQUI UM GIF/PRINT DA APLICAÇÃO RODANDO ]
```

</div>

---

## 🏗️ Arquitetura e Decisões Técnicas

Projeto construído como **SPA de página única, 100% estática, sem build step e sem framework** — uma escolha deliberada, não uma limitação técnica:

| Decisão | Alternativa "mais correta" | Por que não foi usada aqui |
|---|---|---|
| Vanilla JS (sem React/Vue) | Framework component-based | Adicionaria build step e uma curva de aprendizado incompatível com o requisito de a própria loja editar o catálogo sozinha |
| Estado em variáveis globais + funções `render*()` | Store centralizada (Redux-like) | Complexidade de estado insuficiente para justificar; cada `render*()` já sabe de onde ler seu pedaço da tela |
| `localStorage` para carrinho/favoritos/recentes | Backend + sessão de usuário | Sem servidor, é a única forma de persistir dados entre visitas — aceitável porque o checkout real acontece fora do site, no WhatsApp |
| `products.js` como array JS (não JSON) | Banco de dados / CMS | Permite comentários e edição manual mais amigável, ao custo de exigir sintaxe JS válida |

**Padrão de renderização:** sem Virtual DOM — cada mudança de estado dispara manualmente a função `render*()` responsável, que reconstrói aquele bloco da tela do zero via template string. Simples, previsível e suficiente para o volume de dados do catálogo.

**Reaproveitamento de componentes:** uma única função (`criarCardProduto`) gera o card de produto usado em **4 contextos diferentes** — grid do catálogo, vitrines da home, "vistos recentemente" e "produtos similares" no Quick View — garantindo que qualquer mudança visual se propague para todo o site sem duplicação de código.

📄 Documentação técnica completa em [ARQUITETURA.md](ARQUITETURA.md).

---

## 🎯 O Desafio Técnico

**Problema:** o Quick View (modal de detalhes do produto) precisa reabrir um carrossel de fotos diferente a cada produto clicado. A primeira implementação reescrevia o `innerHTML` do carrossel a cada abertura — mas o componente `Carousel` do Bootstrap mantém internamente uma referência ao índice do slide ativo e aos nós DOM dos slides. Ao reescrever o `innerHTML`, esses nós deixavam de existir, e a instância antiga do carrossel ficava "presa" apontando para elementos fantasmas: as setas paravam de funcionar ou pulavam para o slide errado a partir do segundo produto aberto.

**Solução:** antes de reconstruir o markup dos slides, a instância anterior do Bootstrap Carousel é explicitamente descartada (`bootstrap.Carousel.getInstance(el)?.dispose()`) e uma nova é instanciada depois do `innerHTML` atualizado. O listener que sincroniza a miniatura ativa com o slide atual (`slid.bs.carousel`) foi registrado **uma única vez**, na inicialização da página — não dentro da função que roda a cada abertura de modal — evitando acúmulo de listeners duplicados a cada Quick View aberto. Produtos sem fotos extras caem num fallback de imagem única, que esconde as setas de navegação via CSS.

Esse mesmo raciocínio (evitar re-render completo em interações de alta frequência) também guiou o **sistema de filtros avançados**: em vez de re-renderizar o catálogo a cada checkbox marcado, os filtros ficam num estado "pendente" (com contador de resultados ao vivo) e só substituem o estado "aplicado" quando o usuário confirma — reduzindo drasticamente o número de re-renderizações do grid.

---

## ✅ Funcionalidades

- [x] Catálogo com chips rápidos, filtros avançados (subcategoria, cor, linha, material, tamanho, faixa de preço) e busca por texto
- [x] Cards de produto com selos, avaliação por estrelas, parcelamento e indicador de estoque — informação de compra completa sem precisar abrir o produto
- [x] Quick View com carrossel de fotos, variações e produtos similares
- [x] Carrinho persistente (`localStorage`) com checkout via WhatsApp (mensagem pré-formatada com itens, cores, tamanhos e total)
- [x] Lista de desejos (favoritos) com painel lateral dedicado
- [x] Compartilhamento de produtos via Web Share API (com fallback de cópia de link)
- [x] Prova social: depoimentos, FAQ e seção "Looks da Loja em Nossas Clientes" com formulário de UGC ("Apareça aqui!")
- [x] Acessibilidade: modo de alto contraste com preferência salva no navegador

---

## 🚀 Como Executar o Projeto

### Pré-requisitos

Nenhuma dependência de build — apenas um servidor HTTP simples (o site é 100% estático, sem `.env` ou variáveis de ambiente).

### Instalação e execução

```bash
# Clone o repositório
git clone <url-do-repositorio>
cd ladyEllaCatalago

# Sirva a pasta com qualquer servidor HTTP estático
python -m http.server 8000
# ou
npx serve .
```

Acesse `http://localhost:8000`.

> Abrir o `index.html` diretamente via `file://` costuma funcionar, mas servir por HTTP evita bloqueios de recursos em alguns navegadores.

### Configuração

O número de WhatsApp da loja é definido em uma única constante no topo de `script.js`:

```js
const WHATSAPP_NUMBER = "5519971383626"; // DDI + DDD + número, só dígitos
```

O catálogo de produtos fica em `products.js` — veja [MANUAL.md](MANUAL.md) para o passo a passo de edição sem programar.

### Deploy

Publicável gratuitamente, sem build, em **GitHub Pages**, **Netlify**, **Vercel** ou qualquer hospedagem estática tradicional via FTP.
