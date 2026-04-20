# Vértice — Sistema Financeiro Pessoal Premium

> Controle financeiro completo com design premium, sincronização em tempo real e arquitetura escalável.

\---

## ✨ Funcionalidades

|Módulo|Descrição|
|-|-|
|**Auth**|Cadastro, login, recuperação de senha, sessão persistente|
|**Dashboard**|Saldo, receitas, despesas, gráficos, últimos lançamentos|
|**Lançamentos**|CRUD completo, filtros avançados, busca, export CSV|
|**Parcelamento**|Compras parceladas com cálculo automático de faturas|
|**Cartões**|Gestão de cartões, limites, fechamento/vencimento|
|**Faturas**|Fatura por cartão/mês, pagamento em lote|
|**Contas**|Múltiplas contas/carteiras, saldo total|
|**Relatórios**|Comparativo 6 meses, gráficos, export CSV|
|**Metas**|Metas de economia com progress bar|
|**Orçamento**|Orçamento por categoria com alertas|
|**Categorias**|Categorias personalizadas com subcategorias|
|**Agenda**|Vencimentos próximos e alertas|
|**Configurações**|Perfil, tema claro/escuro, senha|

\---

## 🚀 Stack

* **Frontend**: HTML + CSS + JavaScript ES6 Modules (Vanilla, sem frameworks)
* **Backend/Auth/DB**: Supabase (PostgreSQL + Auth + Realtime)
* **Deploy**: Vercel (static)
* **Fontes**: Playfair Display + DM Sans (Google Fonts)

\---

## 📁 Estrutura do Projeto

```
vertice/
├── index.html                    # Entry point
├── vercel.json                   # Config do Vercel
├── assets/
│   ├── css/
│   │   └── main.css              # Design system completo
│   └── js/
│       ├── app.js                # App principal + router
│       ├── supabase.js           # Cliente Supabase (configure aqui)
│       ├── store.js              # Estado global reativo
│       ├── router.js             # SPA router
│       ├── utils.js              # Utilitários (formato, datas, etc)
│       └── icons.js              # Ícones SVG inline
├── src/
│   ├── auth/
│   │   └── auth.js               # Tela de login/cadastro/reset
│   ├── layout.js                 # Sidebar + topbar + layout
│   ├── dashboard/
│   │   └── dashboard.js          # Dashboard principal
│   ├── lancamentos/
│   │   ├── lancamentos.js        # Lista de lançamentos
│   │   └── modal.js              # Modal de lançamento/parcelamento
│   ├── cartoes/
│   │   └── cartoes.js            # Gestão de cartões
│   ├── faturas/
│   │   └── faturas.js            # Faturas por cartão
│   ├── contas/
│   │   └── contas.js             # Contas e carteiras
│   ├── relatorios/
│   │   └── relatorios.js         # Relatórios + categorias
│   └── metas/
│       └── metas.js              # Metas e orçamento
└── sql/
    ├── 01\_tables.sql             # Estrutura do banco
    ├── 02\_rls.sql                # Row Level Security
    └── 03\_seed.sql               # Categorias padrão
```

\---

## ⚙️ Configuração do Supabase

### 1\. Criar projeto no Supabase

1. Acesse [supabase.com](https://supabase.com) e crie um novo projeto
2. Escolha um nome, senha forte e região (preferencialmente São Paulo)
3. Aguarde o projeto inicializar (\~2 min)

### 2\. Executar os SQLs

No **SQL Editor** do Supabase, execute os arquivos **nesta ordem**:

```sql
-- 1. Criar tabelas e funções
-- Cole o conteúdo de sql/01\_tables.sql

-- 2. Configurar RLS
-- Cole o conteúdo de sql/02\_rls.sql

-- 3. Criar categorias padrão (função)
-- Cole o conteúdo de sql/03\_seed.sql
```

### 3\. Configurar credenciais

Edite o arquivo `assets/js/supabase.js`:

```js
const SUPABASE\_URL  = 'https://SEU\_PROJECT\_ID.supabase.co';
const SUPABASE\_ANON = 'SUA\_ANON\_KEY\_AQUI';
```

> Encontre esses valores em: \*\*Supabase Dashboard → Settings → API\*\*

### 4\. Configurar Auth (opcional)

Em **Supabase → Authentication → Settings**:

* Habilite **Email Confirmations** se quiser confirmar e-mail
* Configure **Site URL**: `http://localhost:3000` para desenvolvimento
* Configure **Redirect URLs** para seu domínio de produção

\---

## 💻 Rodar localmente

O projeto é HTML/CSS/JS puro — sem build. Use um servidor local:

```bash
# Opção 1: Python (geralmente já instalado)
cd vertice
python3 -m http.server 3000

# Opção 2: Node.js com npx
npx serve .

# Opção 3: VS Code Live Server
# Instale a extensão "Live Server" e clique em "Go Live"
```

Acesse: `http://localhost:3000`

\---

## 🌐 Deploy na Vercel

### Via CLI

```bash
# Instalar Vercel CLI
npm i -g vercel

# Na pasta do projeto
cd vertice
vercel

# Seguir as instruções
# Em "Framework Preset" escolha: Other
# Em "Build Command": deixe vazio
# Em "Output Directory": deixe vazio (.)
```

### Via GitHub

1. Suba o projeto para um repositório GitHub
2. Acesse [vercel.com](https://vercel.com) → Import Project
3. Conecte o repositório
4. Configure:

   * **Framework Preset**: Other
   * **Build Command**: *(vazio)*
   * **Output Directory**: `.`
5. Clique em **Deploy**

### Variáveis de ambiente (opcional)

Se preferir não colocar as credenciais no código, use variáveis de ambiente da Vercel e ajuste o `supabase.js` para lê-las. Mas para projeto pessoal, as credenciais públicas (anon key) no código são seguras — a proteção é feita pelo RLS no banco.

\---

## 🔒 Segurança

* **RLS ativo** em todas as tabelas — nenhum dado é acessível entre usuários
* Toda tabela tem `user\_id` vinculado ao `auth.uid()`
* A `anon key` do Supabase é segura para uso público (ela não tem permissão de admin)
* Senhas são gerenciadas pelo Supabase Auth (nunca armazenamos senhas)

\---

## 🗂️ Banco de Dados — Tabelas

|Tabela|Descrição|
|-|-|
|`profiles`|Perfil do usuário (nome, tema, moeda)|
|`accounts`|Contas bancárias e carteiras|
|`credit\_cards`|Cartões de crédito|
|`categories`|Categorias e subcategorias|
|`tags`|Tags livres|
|`places`|Locais/estabelecimentos|
|`transactions`|Lançamentos (tabela principal)|
|`transaction\_tags`|Relação N:N transações ↔ tags|
|`installment\_groups`|Grupo de compra parcelada|
|`recurring\_rules`|Regras de recorrência|
|`budgets`|Orçamentos por categoria/mês|
|`goals`|Metas de economia|
|`card\_invoices`|Faturas de cartão|

\---

## 🧮 Lógica de Parcelamento

1. Usuário lança compra de `R$ 150` em `3x` no cartão Nubank (fecha dia 5)
2. Data da compra: **20 de março** (após fechamento)
3. Sistema cria `installment\_group` + 3 `transactions`:

|Parcela|Mês da fatura|Valor|
|-|-|-|
|1/3|Abril (2025-04)|R$ 50,00|
|2/3|Maio (2025-05)|R$ 50,00|
|3/3|Junho (2025-06)|R$ 50,00|

> Se a compra fosse em \*\*03 de março\*\* (antes do fechamento), cairia em Março, Abril, Maio.

**Regra**: `dia\_compra > dia\_fechamento` → fatura do mês seguinte

\---

## 🎨 Design System

* **Estética**: Luxury Minimal — escuro sofisticado com detalhes dourados
* **Fontes**: Playfair Display (títulos) + DM Sans (corpo)
* **Cor de destaque**: `#c9a84c` (dourado elegante)
* **Temas**: Escuro (padrão) e Claro, com transição suave
* **Responsivo**: Mobile-first, sidebar retrátil em telas pequenas

\---

## 📱 PWA

O projeto está configurado como Progressive Web App. No mobile:

* Chrome/Safari → "Adicionar à tela inicial"
* Funciona como app nativo sem barra de URL

\---

## 🔮 Arquitetura Multi-usuário (Futuro)

A arquitetura já está preparada:

* Todo dado tem `user\_id` UUID
* RLS isola completamente os dados por usuário
* Para adicionar permissões/compartilhamento: criar tabela `user\_permissions` e ajustar policies
* Para organizações: adicionar `org\_id` nas tabelas e criar policies por organização

\---

## 🤝 Contribuição / Manutenção

O código é organizado em módulos ES6 independentes:

* Cada página é um módulo separado em `src/`
* Estilos centralizados em `assets/css/main.css` (design tokens via CSS variables)
* Estado global em `assets/js/store.js`
* Para adicionar página: criar módulo + registrar rota em `app.js`

\---

*Vértice — Controle financeiro com precisão absoluta.*

