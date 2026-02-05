# QA-Commerce - Instruções para Agentes IA

## Visão Geral da Arquitetura

Aplicação e-commerce educacional para simulação de testes de QA. Stack: Node.js + Express (backend), Vanilla JS (frontend), SQLite (database).

**Estrutura de camadas:**
- `src/server.js` - Servidor Express com todas as rotas API REST
- `public/` - Frontend estático (HTML/CSS/JS, sem framework)
- `config/` - Configuração DB e Swagger
- `middleware/` - Autenticação JWT

**Fluxo de dados:** Frontend faz fetch direto para `/api/*` → Express routing → SQLite DB → Response JSON

## Setup & Comandos Essenciais

```bash
npm install          # Instalar dependências
npm start            # Inicializa DB + inicia servidor (porta 3000)
npm run db           # Apenas reinicializa o banco de dados
```

**IMPORTANTE:** `npm start` executa automaticamente `init_db.js` que recria todas as tabelas e insere dados de exemplo. O DB SQLite é criado em `src/qa_commerce.db`.

**Credenciais padrão admin:**
- Email: `admin@admin.com`
- Senha: `admin`
- Token secret: `admin@admin` (definido em `process.env.JWT_SECRET` ou hardcoded)

**Documentação:** http://localhost:3000/api-docs (Swagger UI)

## Padrões e Convenções Críticas

### 1. Autenticação JWT
- **Header format:** `Authorization: Bearer <token>`
- **Middleware:** `authenticateToken()` para rotas protegidas, `isAdmin()` para admin-only
- **Token payload:** `{ id, isAdmin, exp }`
- Exemplo em [src/server.js](src/server.js#L21-L31)

### 2. Validação com Joi
Checkout usa schema Joi detalhado com validação condicional (ex: campos de cartão obrigatórios apenas se `paymentMethod === "credit_card"`). Ver `checkoutSchema` em [src/server.js](src/server.js#L45-L88).

**Padrão de resposta de erro:**
```javascript
const { error } = schema.validate(req.body);
if (error) return res.status(400).send(error.details[0].message);
```

### 3. Estrutura de Banco de Dados
Tabelas: `Users`, `Products`, `Cart`, `Orders`

**Carrinho:** Relação M:N entre Users e Products via tabela Cart
- `Cart.user_id` + `Cart.product_id` + `Cart.quantity`
- Carrinho é limpo após checkout bem-sucedido

**Senhas:** Sempre hasheadas com bcrypt (saltRounds=10) antes de INSERT/UPDATE

### 4. Frontend: Comunicação API
- **Sem framework:** Vanilla JS com fetch API
- **Estado:** localStorage para `user` (id, name, token)
- **Padrão de chamada:**
```javascript
const user = JSON.parse(localStorage.getItem('user'));
fetch('/api/endpoint', {
    headers: { 'Authorization': user.token }  // Token já no formato "Bearer xxx"
})
```
Ver [public/js/header.js](public/js/header.js) e [public/js/product.js](public/js/product.js)

### 5. Respostas de Erro Consistentes
```javascript
// Erro genérico
res.status(500).send({ error: "Mensagem de erro técnico" });

// Erro de negócio
res.status(400).send({ message: "Mensagem amigável para o usuário" });
```

## Integrações e Dependências Externas

- **swagger-ui-express:** Documentação auto-hospedada em `/api-docs`
- **open:** Abre navegador automaticamente no startup
- **bcrypt:** Hashing de senhas (10 rounds)
- **jsonwebtoken:** Tokens expiram em 1h

## Fluxos Críticos para Entender

### Checkout Flow
1. Frontend envia dados + items do carrinho (que já estão no DB via `POST /api/carrinho`)
2. Se `createAccount === true`: valida email único → cria User com bcrypt
3. Calcula total do carrinho via JOIN entre Cart e Products
4. Cria Order com `total_price` incluindo frete fixo de R$19,90
5. Gera `order_number` no formato `{id}-{random}` (ex: "42-7834")
6. Limpa carrinho (`DELETE FROM Cart WHERE user_id = ?`)

Ver implementação completa em [src/server.js](src/server.js#L140-L257)

### Paginação de Produtos
- Query params: `?page=1&limit=9` (defaults)
- Response inclui: `{ products: [...], totalPages, currentPage }`
- Endpoint: `GET /api/produtos`

## Testes e QA

**Postman Collection:** `tests/collection-pm.json` contém requests de exemplo para todos os endpoints.

**Dados de teste:** `scripts/init_db.js` popula 15 produtos de exemplo (moletons, xícaras, ecobags, etc).

## Notas de Desenvolvimento

- **Sem hot-reload:** Reinicie manualmente o servidor após mudanças em `src/server.js`
- **DB em memória:** Dados resetados a cada `npm start` (por design, para simulação de testes)
- **Single-file backend:** Todas as rotas estão em `src/server.js` (~674 linhas). Considere modularizar se adicionar novos recursos
- **Middleware auth duplicado:** `authenticateToken` está tanto em `server.js` quanto `middleware/auth.js` - use a versão inline do server
