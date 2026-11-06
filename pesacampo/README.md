# PesaCampo — Check-in e Pesagem Automatizada

Sistema de autoatendimento e controle de pesagem para uma balança rural (agronegócio): o motorista faz check-in num totem, e o ciclo de pesagem (bruta → descarga → tara → peso líquido) é conduzido como uma máquina de estados validada no servidor.

## Funcionalidades

- **Totem de check-in** (`checkin.html`) — autoatendimento: o motorista escolhe seu crachá, o veículo e o tipo de operação (Rota/Pátio) e recebe um ticket de rastreio numérico.
- **Ciclo de pesagem em 4 passos**, cada um só aceito a partir do estado anterior correto:
  1. Check-in no totem → `AguardandoBruta`
  2. Pesagem bruta → `AguardandoDescarga`
  3. Registro da descarga (ponto de descarga) → `AguardandoTara`
  4. Pesagem tara → `Concluída` (peso líquido calculado no servidor)
- **Sorteio de amostra** — ao check-in, ~25% das movimentações são sorteadas (no servidor, não no cliente) para coleta de amostra laboratorial; o resultado é lançado depois via `/api/movimentacoes/:id/amostra`.
- **Cadastros** — Transportadoras, Motoristas, Veículos e Pontos de Descarga, com validações (CPF/CNPJ/placa únicos) e bloqueio de exclusão quando há vínculo em uso (ex.: não é possível excluir um veículo com movimentações associadas).
- **Dashboard** — resumo do dia (movimentações por status, peso líquido total, amostras pendentes, ranking de pontos de descarga mais usados).

## Stack

- Backend: Node.js + Express, persistência em arquivo JSON (`server/data/db.json`, sem banco externo).
- Frontend: HTML/CSS/JS puro, sem framework — `js/api.js` encapsula as chamadas REST e `js/crud.js`/`js/grafico.js` são utilitários genéricos de tabela e gráfico.

## Como rodar

```
npm install
npm start
```

Acesse `http://localhost:3003`. O totem de check-in fica em `checkin.html`; o painel administrativo é o `index.html`.

## Estrutura

```
server/
  db.js              # leitura/escrita do "banco" em JSON
  regras-negocio.js  # validações e máquina de estados da pesagem
  rotas.js           # rotas REST (/api/*)
  seed.js            # dados de exemplo
js/
  api.js             # cliente REST
  pages/             # uma tela por recurso (motoristas, veículos, etc.)
```
