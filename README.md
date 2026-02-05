# 🛒 QA-Commerce

Loja virtual Geek desenvolvida especialmente para **simulação de testes de QA**. Projeto educacional com bugs intencionais para prática de testes manuais e automatizados.

## 📋 Pré-requisitos

Antes de começar, você precisa ter instalado em sua máquina:

1. **Node.js** (versão 16 ou superior)
   - Download: https://nodejs.org/
   - Para verificar se está instalado: `node --version`

2. **Git** (para clonar o repositório)
   - Download: https://git-scm.com/downloads
   - Para verificar: `git --version`

3. **Editor de código** (recomendado)
   - Visual Studio Code: https://code.visualstudio.com/download
   - Ou qualquer editor de sua preferência

## 🚀 Instalação e Execução

### Passo 1: Clonar o repositório

Abra o terminal e execute:

```bash
git clone https://github.com/QA-Impact/qa-commerce.git
```

### Passo 2: Entrar na pasta do projeto

```bash
cd qa-commerce
```

### Passo 3: Instalar as dependências

```bash
npm install
```

⏱️ *Aguarde alguns minutos enquanto as dependências são instaladas.*

### Passo 4: Iniciar o servidor

```bash
npm start
```

✅ **Pronto!** Se tudo deu certo, você verá no terminal:

```
Servidor rodando em http://localhost:3000
Documentação rodando em http://localhost:3000/api-docs
Conectado ao banco de dados SQLite.
```

O navegador abrirá automaticamente na página inicial do projeto.

## 🌐 Acessando a aplicação

- **Site principal:** http://localhost:3000
- **Documentação da API (Swagger):** http://localhost:3000/api-docs
- **Credenciais de administrador:**
  - Email: `admin@admin.com`
  - Senha: `admin`

## 🗂️ Estrutura do Projeto


## 🧪 Para QA: Como começar a testar

1. **Exploração inicial:** Navegue pelo site e experimente todas as funcionalidades
2. **Criar conta:** Registre-se como novo usuário
3. **Testar fluxos:** Adicione produtos ao carrinho, faça checkout, etc.
4. **Usar a API:** Teste os endpoints via Swagger ou Postman
5. **Reportar bugs:** Documente todos os problemas encontrados

## 📚 Recursos Adicionais

### Reinicializar o banco de dados

Se precisar resetar o banco de dados para o estado inicial:

```bash
npm run db
```

### Testar a API com Postman

1. Importe o arquivo `tests/collection-pm.json` no Postman
2. Execute os testes automatizados da collection

### Parar o servidor

No terminal onde o servidor está rodando, pressione:
- **Windows/Linux:** `Ctrl + C`
- **Mac:** `Cmd + C`

## ❓ Problemas Comuns

### "Porta 3000 já está em uso"

```bash
# Encontre o processo usando a porta
lsof -i :3000

# Encerre o processo (substitua PID pelo número mostrado)
kill -9 PID
```

### "npm: command not found"

Instale o Node.js pelo link acima. O npm vem incluído com o Node.js.

### Banco de dados não inicializa

```bash
# Remova o banco existente e recrie
rm src/qa_commerce.db
npm start
```

## 👥 Créditos

**Parceria:** Fábio Araújo, Bruna Emerich e Tamara Fontanella

## 📝 Licença

Este projeto é de código aberto para fins educacionais.






