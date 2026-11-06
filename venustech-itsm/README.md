# Venustech · Plataforma de Gestão de Demandas e Projetos de TI

Simulação full-stack de uma esteira **ITSM + execução ágil + governança** para áreas de TI que
atendem demandas de negócio — construída para reduzir três atritos clássicos entre TI e
negócio: **falta de comunicação** sobre em que pé está cada solicitação, **falta de governança**
sobre custo/tempo/processo, e **falta de comprovação de valor** (o projeto é entregue no prazo,
mas ninguém confere se o benefício prometido de fato aconteceu).

> Projeto desenvolvido durante estágio em desenvolvimento, evoluído com arquitetura full-stack
> própria (front-end + API + banco de dados) como estudo aplicado de engenharia de software.

## O que a ferramenta resolve

| Dor comum em TI↔negócio | Como a plataforma trata isso |
|---|---|
| Negócio não sabe em que fase sua solicitação está | Esteira de valor visível (Backlog → Prospecção → Concepção → Aprovação → Execução → Aceite), com checklist de tarefas por etapa e responsável nomeado |
| Regras de aprovação inconsistentes | Fluxo **varia por tipo de demanda** (normal, padrão, emergencial, simplificada, incidente) — cada um com seu rigor de aprovação, modelado como dado, não como exceção no código |
| Orçamento de projeto sem controle de execução | Backlog/sprints estilo Jira, com custo estimado (horas × custo/hora por papel) comparado ao CAPEX+OPEX planejado |
| **"Entregamos, mas ninguém sabe se funcionou"** | Cada projeto declara uma métrica de impacto **planejado** no business case e é cobrado a registrar o impacto **real** medido após a entrega — indicador de % de atingimento, o tipo de evidência que justifica investimento em TI para a diretoria |
| Governança dispersa | Painéis únicos de CMDB (sistemas), aprovadores por alçada de valor, níveis de atendimento (N1/N2/N3) e indicadores GRC/ITBM (lead time, distribuição por tipo/prioridade, investimento total) |

## Capturas de tela

| Quadro ITSM | Projetos & sprints |
|---|---|
| ![Quadro ITSM](docs/screenshots/quadro-itsm.png) | ![Projetos e sprints](docs/screenshots/projetos.png) |

| Indicadores (com Impacto planejado x real) | Painel de demandas |
|---|---|
| ![Indicadores](docs/screenshots/indicadores.png) | ![Painel de demandas](docs/screenshots/painel-demandas.png) |

## Destaques técnicos

- **Motor de regras de negócio orientado a dados**: o fluxo de etapas de cada demanda é uma
  tabela (`FLUXO_ETAPAS_POR_TIPO`), não um emaranhado de `if/else` — trocar o comportamento de
  um tipo de demanda é editar dados, não reescrever lógica. As tarefas de cada etapa são
  geradas por um "motor de templates" (`tarefasTemplate`) que varia por tipo e etapa, de forma
  idempotente (não duplica tarefa se a etapa já foi visitada).
- **Backend real migrado sem reescrever a regra de negócio**: a aplicação nasceu 100%
  client-side (estado em `localStorage`). A migração para Express + SQLite foi feita trocando
  **a origem/destino dos dados**, preservando as ~3.600 linhas de lógica de UI e regra de
  negócio já validadas — decisão deliberada para minimizar risco de regressão numa base sem
  testes automatizados. Detalhes e trade-offs documentados em [DOCUMENTACAO.md](DOCUMENTACAO.md#8-decisões-técnicas-e-trade-offs).
- **Zero dependência de compilador nativo**: banco SQLite rodando via WebAssembly (`sql.js`),
  contornando a ausência de toolchain de compilação C++ no ambiente Windows de desenvolvimento
  — troca consciente de uma lib mais comum (`better-sqlite3`) por restrição real de ambiente.
- **Visualizações de dados sem biblioteca externa**: gráficos de barras, barras pareadas
  (planejado x real), donut e burndown renderizados em SVG puro, com paleta de cores acessível
  e consistente entre tema claro/escuro.

📄 Para o detalhamento completo de regras de negócio, arquitetura, modelo de dados e decisões
técnicas: **[DOCUMENTACAO.md](DOCUMENTACAO.md)**.

## Stack

**Front-end:** HTML/CSS/JS puro, sem framework nem build step (SPA "multi-página", uma
sidebar/design system compartilhado via `styles.css`).
**Back-end:** Node.js + Express.
**Banco de dados:** SQLite (via `sql.js`), schema equivalente também documentado em PostgreSQL.

## Como rodar

```bash
cd server
npm install
npm start
```

Acesse **http://localhost:3000** — o banco é criado e populado com dados de exemplo
automaticamente no primeiro start.

## Possíveis evoluções

- Autenticação/perfis reais (hoje o usuário é fixo, "Ivone Antunes")
- Endpoints CRUD granulares com controle de concorrência (hoje cada gravação substitui a
  coleção inteira — ver trade-offs no [DOCUMENTACAO.md](DOCUMENTACAO.md#8-decisões-técnicas-e-trade-offs))
- Testes automatizados (o projeto ainda não tem suíte de testes)
- Notificações/alertas quando uma demanda fica parada numa etapa além do SLA
