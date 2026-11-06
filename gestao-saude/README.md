# PIn---2026 — Portal Saúde Pira

> ⚠️ **Atenção:** este projeto migrou do backend PHP/XAMPP para um servidor
> **Node.js + SQLite** (ver `package.json`). As instruções de XAMPP abaixo
> ficaram desatualizadas e **não devem mais ser usadas** — os arquivos PHP
> (`processar_login.php`, `api/*.php`, `get_unidades.php`) continuam no
> repositório só por compatibilidade histórica, mas quem roda o projeto via
> XAMPP/Apache sem o banco MySQL `pin2026` configurado vai ver erro de
> conexão no login e no mapa. **Use sempre o fluxo Node abaixo.**

## Como rodar o projeto (atual)

1. Instale as dependências (uma vez só):
   ```
   npm install
   ```
2. Suba o servidor:
   ```
   npm start
   ```
3. Acesse **http://localhost:3000/index3.html** no navegador — login, cadastro
   e mapa funcionam através desse servidor (ele serve o site inteiro e expõe
   as rotas de API, incluindo as de compatibilidade `/processar_login.php` e
   `/get_unidades.php` usadas pelo front-end).

**Importante:** abrir os arquivos `.html` direto no navegador (duplo clique,
`file://...`) ou servir o projeto por outro servidor estático (Live Server,
XAMPP, etc.) **não funciona** — login e mapa dependem das rotas do servidor
Node acima.

Opcional — e-mail de verdade no "Esqueci minha senha": copie `.env.example`
para `.env` e preencha `SMTP_USER`/`SMTP_PASS`. Sem isso, o link de
redefinição é só impresso no terminal (modo simulação, bom para
desenvolvimento).

## Banco de dados

O banco (SQLite) fica em `db/pin2026.sqlite` e é inicializado automaticamente
pelo `server/db.js` na primeira vez que o servidor sobe. Scripts e migrações
de referência ficam em `sql/` — só quem está mexendo direto no schema deve
rodar algo ali.

## Fluxo de contribuição (front-end)

1. Alguém edita o código;
2. Sobe no git imediatamente após terminar;
3. Confere se deu certo o "push";
4. Avisa o pessoal;

