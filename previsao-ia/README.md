# 🧳 Lookbook — Previsão do Tempo + Guia de Looks para Viagens

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![OpenWeatherMap](https://img.shields.io/badge/OpenWeatherMap-EB6E4B?style=for-the-badge&logo=openweathermap&logoColor=white)
![Groq AI](https://img.shields.io/badge/Groq_AI-F55036?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0iI2ZmZiIgZD0iTTEyIDJMMiAyMmgyMHoiLz48L3N2Zz4=&logoColor=white)
![Leaflet](https://img.shields.io/badge/Leaflet-199900?style=for-the-badge&logo=leaflet&logoColor=white)
![No Build Step](https://img.shields.io/badge/Build_Step-nenhum-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/status-ativo-success?style=for-the-badge)

> Projeto 100% front-end (HTML, CSS e JavaScript puro, **sem framework e sem build step**) que combina previsão do tempo com IA e um mini-site de guia de looks para viagens.

---

## Visão Geral e Proposta de Valor

Este projeto responde a duas perguntas práticas do dia a dia de quem viaja ou só quer se vestir bem para o clima do dia:

- **"O que eu visto hoje?"** — o módulo de **Previsão do Tempo** busca o clima real de qualquer cidade (via [OpenWeatherMap](https://openweathermap.org/)) e usa um modelo de IA (via [Groq](https://groq.com/)) para transformar temperatura e umidade em uma recomendação de roupa em linguagem natural, sem que a usuária precise interpretar números.
- **"O que eu levo na mala?"** — o **Lookbook** é um guia de estilo por destino (Itália, Espanha, Turquia, Brasil), com looks reais organizados por estação e ocasião, cada peça com link de compra e cupom — um catálogo de conteúdo com potencial claro de monetização via parcerias de afiliados (B2B) e assinatura (B2C).

O diferencial de arquitetura: **tudo isso roda sem backend próprio**. Autenticação, favoritos, curtidas e rascunhos do painel administrativo vivem inteiramente no `localStorage` do navegador, e o catálogo de conteúdo é editável por um painel admin que exporta um arquivo JS pronto para commit — um modelo enxuto para publicar e manter conteúdo sem infraestrutura de servidor.

---

## Visão de Produto: Personalização, Retenção e Escala de Conteúdo

Aqui não há carrinho nem checkout — o produto não é vender um item, é **resolver uma decisão** ("o que eu visto?", "o que eu levo pra viagem?"). Cada funcionalidade abaixo existe para reduzir essa fricção de decisão e dar à usuária um motivo concreto para voltar, não só para demonstrar uma técnica de programação:

- **Sugestão de roupa por IA = recomendação personalizada.** Em vez de a usuária interpretar "23°C, 78% de umidade" por conta própria, o modelo (`openai/gpt-oss-120b` via Groq) traduz esses números numa resposta pronta em linguagem natural. É a mesma lógica de negócio por trás de "recomendado para você" em e-commerce e travel-tech: quanto menos esforço cognitivo até a resposta, maior a chance de a pessoa completar a ação em vez de abandonar a busca.
- **Favoritos e curtidas = motivo de retorno.** Sem persistência, o Lookbook seria consumido uma vez e esquecido. Com login e favoritos salvos por usuária (`interacoes.js`, ver [favoritos.html](lookbook/favoritos.html)), existe uma razão concreta para voltar ao site antes de uma viagem — o mesmo papel que uma lista de desejos cumpre num catálogo de produtos, aqui aplicado a conteúdo editorial.
- **Painel admin = curadoria que escala sem depender de programador.** O catálogo inteiro vive num único arquivo (`dados-lookbook.js`); sem um painel, qualquer destino, look, peça ou cupom novo exigiria editar código à mão. O [painel admin](lookbook/admin.html) resolve isso com formulário e exportação do arquivo pronto para commit — pré-requisito para o modelo de monetização por afiliados e assinatura (citado acima) crescer por conteúdo, sem virar gargalo técnico a cada atualização.
- **Compartilhamento nativo = aquisição orgânica.** O menu de compartilhar (WhatsApp, redes sociais, Stories) em cada look e destino transforma quem gostou do conteúdo num canal de indicação, sem depender de mídia paga para trazer a próxima visitante.

Em conjunto, essas quatro peças respondem à mesma pergunta de negócio que orientou as decisões técnicas: como transformar duas ferramentas de utilidade prática num produto que a usuária tem motivo para reabrir.

---

## Possíveis Aplicações para Negócio e Expansões Futuras

A arquitetura atual (catálogo em `dados-lookbook.js` + painel admin) já foi pensada para crescer por **conteúdo e parcerias**, não só por código. Caminhos naturais de evolução:

- **Guia turístico completo, não só de looks.** O Lookbook já organiza conteúdo por destino — o próximo passo natural é indicar **pontos turísticos locais** e roteiros junto com cada look, transformando o produto de "o que vestir" para "guia de viagem" completo por cidade.
- **Parcerias com o varejo local de cada destino.** Cada peça de roupa no Lookbook já linka para compra; isso pode se estender para **lojas, restaurantes e prestadores de serviço locais** de cada destino (parceiros do comércio da região, não só marcas de moda), com o painel admin cadastrando cada parceiro do mesmo jeito que cadastra uma peça hoje.
- **Cupons de parceiros.** A estrutura de cupom por peça já existe no catálogo — generalizar esse campo para qualquer parceiro (loja local, passeio, restaurante) cria um canal de desconto exclusivo por destino, com potencial de comissão por conversão para quem mantém o site.
- **Reels e looks compartilhados por parceiros e clientes (UGC).** O menu de compartilhamento já existe (WhatsApp, redes sociais, Stories) — abrir um caminho **de entrada**, não só de saída, para que parceiros locais e clientes enviem seus próprios reels/looks (nos mesmos moldes da seção "Looks da Loja em Nossas Clientes" do projeto [Lady Ella](../../ladyEllaCatalago/README.md)) transformaria o Lookbook de catálogo curado em **guia vivo**, atualizado por quem realmente visita cada destino — e reduz o trabalho de produção de conteúdo do painel admin.
- **Monetização por afiliação e assinatura**, já citada na seção anterior, ganha força direta com esses parceiros: comissão por clique/cupom convertido no varejo local, e uma assinatura para acesso a roteiros/parcerias exclusivas por destino.

Nenhum desses pontos exige mudar a arquitetura de fundo — o modelo de "catálogo único + painel admin sem backend" já foi desenhado para absorver novos tipos de conteúdo (parceiro, cupom, reels) sem virar gargalo técnico.

---

## Seção Visual

> 🖼️ **Espaço reservado para demonstração visual.**
> Insira aqui um GIF ou print mostrando o fluxo completo: busca de clima → sugestão de roupa por IA → navegação pelo carrossel de destinos do Lookbook.
>
> `![Demo do projeto](img/demo.gif)`

---

## Arquitetura e Decisões Técnicas

| Decisão | Motivação |
|---|---|
| **Vanilla JS, sem framework, sem build step** | Projeto pensado para ser aberto direto no navegador (`file://`) e servir como exercício deliberado de fundamentos: `fetch`, `async/await`, manipulação de DOM e APIs nativas do navegador, sem a abstração de um framework. |
| **Padrão MVVM manual no Lookbook** | Mesmo sem Vue/React, o código em `lookbook/` separa responsabilidades: **Model** (`dados-lookbook.js`, `auth.js`, `interacoes.js`) não toca DOM; **View** (funções `render*`) só transforma dados em HTML; **ViewModel** (funções `init*`/`atualizar*`) liga os dois e escuta eventos. Documentado em detalhe em [`docs/ARQUITETURA.md`](docs/ARQUITETURA.md). |
| **Delegação de eventos via `document.addEventListener`** | Como a maior parte do conteúdo (cards, looks, menus) é renderizada dinamicamente após o carregamento da página, os listeners ficam no `document` (não em cada botão), usando `evento.target.closest(seletor)` para identificar a ação clicada. |
| **`localStorage` como única persistência** | Sem backend próprio: sessão de usuária, favoritos, curtidas e rascunho do painel admin ficam no navegador. Senhas passam por hash SHA-256 (Web Crypto API) — suficiente para uma demonstração, mas documentado explicitamente como **não sendo autenticação de produção** (sem salt, sem custo computacional, sem servidor). |
| **`dados-lookbook.js` como fonte única da verdade** | O catálogo inteiro (destinos, looks, peças parceiras, cupons) é um único objeto JS comentado — sem banco de dados. O painel admin edita uma cópia em memória e exporta o arquivo atualizado para substituir e commitar, mantendo o "sem backend" mesmo com conteúdo editável. |
| **`escapeHtml` em toda interpolação de dados no HTML** | Convenção de segurança aplicada de forma consistente sempre que texto vindo do catálogo, de uma API externa ou de input da usuária é inserido em uma template string — defesa contra XSS sem depender de um framework que sanitize por padrão. |
| **APIs externas gratuitas, na maioria sem chave** | OpenWeatherMap e Groq exigem chave própria (via `config.js`, fora do Git); Leaflet/OpenStreetMap, Overpass API, Web Speech API, Geolocation API e Web Share API são nativas ou gratuitas sem cadastro — mantendo o projeto 100% sem custo de infraestrutura. |

---

## O Desafio Técnico

**Problema:** o menu de compartilhamento (`.btn-compartilhar`) precisava fechar ao clicar fora dele, mas boa parte da tela contém o mapa **Leaflet** — um widget de terceiros que captura e interrompe a propagação de cliques (`stopPropagation()`) para controlar seus próprios pinos e popups. Como o listener de "fechar menu ao clicar fora" escutava a fase padrão de **bubbling** (do alvo clicado até o `document`), qualquer clique que passasse por cima do mapa era "engolido" pelo Leaflet antes de chegar ao `document` — o menu simplesmente não fechava.

**Solução:** o listener global de compartilhamento em [`lookbook.js`](lookbook/lookbook.js) foi registrado na **fase de captura** do evento (`document.addEventListener("click", handler, { capture: true })` conceitualmente — do `document` em direção ao alvo, e não o contrário). Isso garante que a lógica de fechar o menu roda **antes** de qualquer `stopPropagation()` de widgets internos como o Leaflet ter a chance de interromper o clique, resolvendo o bug sem precisar tocar no código do mapa ou duplicar listeners por componente.

```js
// Ouve na fase de captura (document -> alvo) e não na de "bubbling": assim o fechamento
// do menu acontece antes de qualquer widget interno (ex: o mapa Leaflet) poder chamar
// stopPropagation() e "engolir" o clique, o que impedia o menu de fechar.
document.addEventListener("click", evento => { /* ... */ });
```

Esse tipo de conflito entre delegação de eventos própria e bibliotecas de terceiros com sua própria captura de cliques é um problema clássico de integração front-end — a solução aqui evitou acoplar o código do Lookbook aos internals do Leaflet.

---

## Funcionalidades

- [x] Busca de clima por cidade (texto ou voz, via Web Speech API) com resultado em tempo real da OpenWeatherMap
- [x] Sugestão de roupa gerada por IA (Groq, modelo `openai/gpt-oss-120b`) a partir da temperatura e umidade exibidas
- [x] Guia de looks por destino, com filtros por região, época e ocasião, e busca de lugares próximos via Overpass API/Geolocation
- [x] Login, favoritos e curtidas persistidos no navegador (sem backend), com eventos customizados (`CustomEvent`) sincronizando Model e View
- [x] Painel administrativo protegido por senha para cadastrar destinos, looks, peças e cupons sem editar código
- [x] Compartilhamento nativo (WhatsApp, redes sociais, copiar link e "Stories") com fallback automático para navegadores sem suporte

---

## Como Executar o Projeto

### Pré-requisitos

- Um navegador moderno (recomendado: **Chrome** ou **Edge** — o reconhecimento de voz via `webkitSpeechRecognition` só funciona em navegadores baseados em Chromium).
- Chaves gratuitas de API para o módulo de Previsão do Tempo:
  - [OpenWeatherMap](https://home.openweathermap.org/) (clima)
  - [Groq](https://console.groq.com/keys) (sugestão de roupa por IA)

> O módulo **Lookbook** não depende de nenhuma chave — os dados ficam em [`lookbook/dados-lookbook.js`](lookbook/dados-lookbook.js) e o mapa usa OpenStreetMap gratuito.

### Configuração

1. Clone o repositório.
2. Copie o arquivo de exemplo de configuração:

```bash
cp config.example.js config.js
```

3. Preencha `config.js` com suas próprias chaves (esse arquivo **não vai para o Git**, veja `.gitignore`):

```js
const CONFIG = {
    chaveClima: "SUA_CHAVE_OPENWEATHERMAP_AQUI",
    chaveIa: "SUA_CHAVE_GROQ_AQUI"
}
```

### Executando

Não há `npm install` nem servidor obrigatório — basta abrir o arquivo no navegador:

```bash
# Previsão do Tempo
abra index.html no navegador

# Guia de Looks para Viagens
abra lookbook/lookbook.html no navegador
```

Para testar a página **`lookbook/explorar.html`** (usa Geolocation API, que exige HTTPS ou `localhost`), sirva a pasta com um servidor simples em vez de abrir o arquivo direto:

```bash
python -m http.server 8000
# depois acesse http://localhost:8000/lookbook/explorar.html
```

📚 Documentação complementar: [Guia de Uso](docs/GUIA-DE-USO.md) · [Arquitetura](docs/ARQUITETURA.md) · [Contributing](CONTRIBUTING.md)
