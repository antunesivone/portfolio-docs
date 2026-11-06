# Sistema de Logística & SSMA

Aplicação web para gestão de logística e SSMA (Segurança, Saúde e Meio Ambiente), inspirada no
**Manual DENOX Pro Raízen** (sistema real de monitoramento logístico/safety) e no **Relatório de
Estágio SN&TECH** (ITSM / soluções EAB) da Raízen.

## Stack

TypeScript de ponta a ponta: **Next.js (App Router) + React** no front-end, **rotas de API do
próprio Next.js** no back-end, estilo com **Tailwind CSS** e gráficos com **Recharts**. Os dados
ficam em um arquivo JSON local (`data/db.json`, criado automaticamente na primeira execução) — não
requer instalação de banco de dados.

## Módulos

- **Dashboard** — KPIs e gráficos consolidados (frota, violações, estoque, eventos SSMA).
- **Frota & Logística** — Veículos, Motoristas, Transportadoras, Rotas & Áreas, Viagens
  (réplica do Robô de Logística / Cockpit de Monitoramento do DENOX Pro).
- **SSMA & Safety** — Violações de condução com tratativa (Aprovar/Revisar/Recusar) e SLA por
  gravidade, réplica do módulo CCO-Safety; Eventos SSMA (acidentes, quase acidentes, não
  conformidades) com os indicadores OPI/IA/IQA citados no relatório de estágio.
- **Abastecimento** — Estoque por terminal/produto (LOL/AOL/UOL) com cálculo automático do volume
  programado, réplica simplificada da lógica da "Tríade" do DENOX Pro.
- **Contratações** — Parâmetros de frete por transportadora (DFDV / R$ por m³).

## Rodando localmente

```bash
npm install
npm run dev
```

Abra [http://localhost:3000](http://localhost:3000). Os dados de demonstração são gerados
automaticamente em `data/db.json` no primeiro acesso — apague esse arquivo a qualquer momento para
resetar para os dados de exemplo.

Para build de produção:

```bash
npm run build
npm run start
```
