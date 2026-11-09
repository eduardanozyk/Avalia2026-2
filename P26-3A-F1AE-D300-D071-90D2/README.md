# Projeto #35: Simulador de E-Commerce com Carteira de Créditos Fictícios

- **Modalidade:** Projeto Oficial Sorteado (Valendo 100 pontos)
- **Estudante:** Eduarda Nozykowski
- **Turma:** 3A
- **Chave Única:** `P26-3A-F1AE-D300-D071-90D2`
- **URL_CLOUDFLARE:** `https://p26-3a-f1ae-d300-d071-90d2.pages.dev` *(Atualize com a URL pública real do seu deploy na Cloudflare antes de abrir o Pull Request)*

---

## Especificação do Projeto

Implemente uma loja virtual completa que simula a compra e venda de itens digitais ou periféricos fictícios. Ao realizar o primeiro login via Discord OAuth2, o backend na Cloudflare cria automaticamente uma conta no D1 com um saldo inicial padrão de créditos virtuais para o estudante utilizar.

O frontend em JavaScript vanilla deve gerenciar o catálogo de produtos, filtros de preço e o estado do carrinho de compras. No momento do checkout, uma Cloudflare Function valida no servidor se o saldo do usuário no banco D1 é suficiente para cobrir o somatório dos itens, deduzindo o valor, abatendo a quantidade em estoque dos produtos e gerando um recibo histórico de pedidos.

---

## Como Publicar esta Subpasta no Cloudflare Pages + D1 + Discord OAuth2

1. No painel da Cloudflare (**Workers & Pages** → **Create** → **Pages** → **Connect to Git**), selecione o seu fork do repositório `Avalia2026-2`.
2. Configure os parâmetros de build apontando para esta subpasta:
   - **Root directory (Diretório raiz):** `P26-3A-F1AE-D300-D071-90D2`
   - **Build command:** *(deixe em branco — HTML/CSS/JS puro)*
   - **Build output directory:** `public`
3. Crie o banco de dados D1 e aplique a migração inicial (no terminal do laboratório usando Scoop):
   ```powershell
   cd P26-3A-F1AE-D300-D071-90D2
   npx.cmd wrangler d1 create db-p26-3a-f1ae-d300-d071-90d2
   # Copie o database_id gerado para o arquivo wrangler.jsonc e execute:
   npx.cmd wrangler d1 migrations apply DB --remote
   ```
4. **Vincule o Banco D1 ao seu Cloudflare Pages (Obrigatório para ativar env.DB):**
   - No painel da Cloudflare: acesse seu projeto Pages → **Settings** → **Functions** → **D1 database bindings**.
   - Clique em **Add binding**:
     - **Variable name:** `DB`
     - **D1 database:** selecione o banco que você criou (`db-p26-3a-f1ae-d300-d071-90d2`).
5. **Configuração do Discord OAuth2 (Obrigatório para autenticação real):**
   - Acesse o **Discord Developer Portal** (https://discord.com/developers/applications) e crie uma nova Application.
   - Na aba **OAuth2**:
     - Copie o **Client ID** e gere o **Client Secret**.
     - Em **Redirects**, adicione a URL exata do deploy:
       `https://p26-3a-f1ae-d300-d071-90d2.pages.dev/api/auth/discord/callback`
   - No painel do seu projeto no Cloudflare Pages:
     - Acesse **Settings** → **Environment variables**.
     - Adicione as variáveis de ambiente:
       - `DISCORD_CLIENT_ID`
       - `DISCORD_CLIENT_SECRET`
6. Atualize o campo **URL_CLOUDFLARE** no topo deste `README.md` com o endereço público gerado pela Cloudflare.
7. Na raiz do repositório, execute `node validar-local.mjs P26-3A-F1AE-D300-D071-90D2` para validar todos os critérios antes de fazer o commit e abrir o Pull Request.
