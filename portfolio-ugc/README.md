# Portfolio UGC — Site para Criadora de Conteúdo

Portfólio comercial para uma criadora de conteúdo UGC (User Generated Content): hero, sobre, marcas atendidas, frentes de atuação, destaques, portfólio filtrável por categoria, processo criativo, métricas de audiência animadas, depoimentos de marcas parceiras e formulário de proposta — com painel administrativo para gerenciar tudo isso sem editar código.

## Funcionalidades

- **Portfólio filtrável** — grade de conteúdos por categoria (Moda, Skincare, etc.), com contagem de views e likes.
- **Formulário de proposta** — grava o lead no banco com status (novo, em contato, fechado), visível só no painel admin.
- **Depoimentos** — carrossel de feedbacks reais de marcas parceiras.
- **Painel administrativo** (`admin.html`) — autenticado por chave, com CRUD completo de projetos, depoimentos e propostas recebidas; toda alteração ali reflete direto no site público.
- **Métricas de audiência** — painel com contagem animada, alimentado pelos mesmos dados do banco.

## Stack

- Backend: Node.js + Express, persistência em SQLite via `sql.js` (WebAssembly puro — sem dependência de compilação nativa, roda em qualquer máquina sem Visual Studio Build Tools).
- Frontend: HTML/CSS/JS puro, sem framework.

## Como rodar

```
npm install
npm start
```

Acesse `http://localhost:3000` (ou a porta definida em `server/server.js`). O site público fica em `index.html`; o painel administrativo em `admin.html`.
