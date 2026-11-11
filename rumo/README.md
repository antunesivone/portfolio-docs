# Rumo — Matching de Cursos por Swipe

Protótipo inspirado num workshop de prompts de IA para Figma (EBAC): em vez de um catálogo de cursos que a pessoa precisa filtrar sozinha, a descoberta acontece por swipe — like/pass em cards de curso — e a afinidade de cada card é recalculada no servidor a cada decisão, não fixa no banco.

📘 **[Veja o passo a passo completo, do prompt ao protótipo](tutorial.md)** — escrito para quem está aprendendo a programar.

## O problema que resolve

Alguém migrando de carreira (ex.: Publicidade → tecnologia) trava diante de um catálogo gigante de opções (UX, Produto, Dados, Motion, Marketing) sem saber qual trilha seguir primeiro. Em vez de um formulário longo de "quais são seus objetivos", a pessoa só reage a cards — e o sistema aprende com isso.

## Como a afinidade funciona (a regra de negócio real)

Cada curso tem uma **afinidade-base** fixa no catálogo. Toda vez que o deck é buscado, o servidor recalcula a afinidade de cada curso ainda não visto, somando um bônus por curtida já dada em cursos da mesma trilha:

```
afinidade = min(afinidade_base + (curtidas_na_trilha × 9), 97)
```

Isso significa que curtir um curso de Design faz o PRÓXIMO curso de Design aparecer com afinidade mais alta — o "perfil adaptativo" é uma consulta SQL de verdade, não um texto decorativo na tela.

Ao final do deck, `/api/match` escolhe o curso de maior afinidade entre os curtidos (ou, se nada foi curtido, o de maior afinidade-base do catálogo) e monta uma justificativa citando a trilha dominante.

## Stack

- Backend: Node.js + Express, SQLite via `sql.js` (WebAssembly — sem compilação nativa).
- Frontend: HTML/CSS/JS puro, sem framework. Swipe por arraste (Pointer Events) com fallback de botões.

## Como rodar

```
npm install
npm start
```

Acesse `http://localhost:3005`.

## Estrutura

```
server/
  db.js              # leitura/escrita do banco (sql.js)
  regras-negocio.js  # cálculo de afinidade e do match final
  rotas.js           # GET /api/deck, POST /api/swipes, GET /api/match, POST /api/reset
  seed.js            # catálogo de 8 cursos em 5 trilhas
js/
  api.js             # cliente REST
  app.js             # UI de swipe (drag + botões) e tela de match
```
