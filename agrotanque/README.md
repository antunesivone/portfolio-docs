# 🚜 AgroTanque — Controle de Abastecimento de Frota Rural

## Visão Geral e Proposta de Valor

Sem informação, fica impossível fazer gestão de combustível: qual veículo recebeu, quem abasteceu, quando e quanto foi abastecido são perguntas que, numa propriedade rural sem sistema, costumam ficar sem resposta — e o resultado é gasto descontrolado, perda de combustível e decisão tomada sem dado nenhum.

**AgroTanque** é um backend real (Node.js + Express) que modela esse processo como um fluxo controlado em 4 passos, cada um validado no servidor, não só na tela:

1. **Identificação do operador** — toda solicitação exige um operador com crachá, e o servidor recusa se ele não estiver Ativo.
2. **Vinculação ao veículo** — idem para o veículo: só recebe abastecimento quem está Ativo.
3. **Registro do abastecimento** — a solicitação entra no sistema com status `Solicitado`.
4. **Liberação controlada** — um encarregado libera o abastecimento; o servidor confere se a bomba tem litros suficientes e debita o estoque automaticamente. Sem estoque, não libera — como uma bomba de verdade.

A inspiração de negócio vem de soluções de telemetria/controle de abastecimento usadas em frotas agrícolas reais (identificação do operador → vínculo ao veículo → registro → liberação); o código, os dados e a interface aqui são uma implementação própria, escrita do zero.

## Arquitetura e Decisões Técnicas

- **Node.js + Express**, sem framework de front-end — HTML, CSS e JavaScript puro, o mesmo padrão usado no projeto irmão [Sistema de Logística & SSMA](../projetoLogistica/sistema-logistica-ssma-html-css-js).
- **"Banco" em arquivo JSON** (`server/data/db.json`), lido e escrito a cada operação — sem cache em memória, para que o estado "oficial" nunca fique em dúvida. Não precisa de SQLite, nem de compilação nativa: `npm install` funciona em qualquer máquina.
- **Motor de CRUD genérico** (`js/crud.js`): uma página descreve só os campos e colunas que tem; listar/criar/editar/excluir é escrito uma única vez e reutilizado em Operadores, Veículos e Bombas.
- **Regras de negócio centralizadas** (`server/regras-negocio.js`): validação de campos obrigatórios, unicidade (crachá, identificador do veículo, código da bomba), bloqueio de exclusão de cadastro em uso, e a máquina de estados do abastecimento (`Solicitado → Liberado → Concluído`, ou `Recusado`) — a liberação é a única transição que mexe em outra tabela (debita `nivelAtual` da bomba), e isso só acontece no servidor.

## Funcionalidades

- [x] Cadastro de operadores, veículos/máquinas e bombas (postos internos de combustível)
- [x] Registro de abastecimento com os 4 passos do processo controlado
- [x] Liberação que valida estoque da bomba e debita automaticamente
- [x] Dashboard com litros abastecidos (7 dias), pendências, bombas com estoque baixo, ranking de consumo por veículo e histórico recente
- [x] Histórico completo de abastecimentos — rastreabilidade por operador, veículo e bomba

## Regras de Negócio Implementadas

- Abastecimento só é registrado com operador **e** veículo com status `Ativo`.
- Litros solicitados não podem exceder a capacidade do tanque do veículo.
- Liberação só é permitida com estoque suficiente na bomba vinculada; ao liberar, o servidor debita os litros do estoque.
- Exclusão de operador, veículo ou bomba é bloqueada se houver abastecimento vinculado a eles.
- Crachá de operador, identificador de veículo e código de bomba são únicos.

## Possíveis Expansões Futuras

- Identificação do operador por leitura de crachá/QR Code em vez de seleção manual.
- Alerta automático (e-mail/WhatsApp) quando uma bomba cruza o limiar de estoque baixo.
- Comparação de consumo por veículo contra uma média esperada por tipo de máquina, para sinalizar desvio fora do padrão.
- Exportação do histórico de abastecimentos para análise externa (planilha/BI).

## Como Executar o Projeto

```bash
npm install
npm start
```

Depois acesse `http://localhost:3002`.
