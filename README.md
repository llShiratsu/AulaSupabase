# AulaSupabase

Mini app de lista de tarefas conectado a um banco **Supabase** na nuvem.
Um único arquivo HTML, sem instalação e sem build — roda direto no navegador.

> Projeto da aula prática de **Cloud**: mostra na prática como um front-end
> conversa direto com um backend na nuvem (banco + API automática).

---

## ✅ Pré-requisitos

- Uma conta gratuita no [Supabase](https://supabase.com) (não pede cartão).
- Um navegador (Chrome, Edge, Firefox...) **com internet** — o app carrega a
  biblioteca do Supabase por CDN.

---

## 🚀 Passo a passo

### 1. Crie a conta e o projeto
1. Acesse [supabase.com](https://supabase.com) e clique em **Start your project**.
2. Entre com GitHub ou e-mail e clique em **New project**.
3. Dê um nome (ex.: `aula-cloud`), defina uma senha para o banco e escolha a
   região **South America**. Aguarde ~1–2 min.

### 2. Crie a tabela `tarefas`
No menu lateral, abra **Table Editor** → **New table** e crie a tabela com o
nome exato **`tarefas`** e estas colunas:

| Coluna       | Tipo          | Observação                  |
|--------------|---------------|-----------------------------|
| `id`         | `int8`        | já vem pronta (chave)       |
| `created_at` | `timestamptz` | já vem pronta (`now()`)     |
| `titulo`     | `text`        | adicionar                   |
| `concluida`  | `bool`        | adicionar, padrão `false`   |

### 3. Libere o acesso (RLS)
Tabelas novas vêm **bloqueadas** por padrão. Abra o **SQL Editor** → **New query**,
cole o bloco abaixo e clique em **Run**:

```sql
-- Demo: libera leitura e escrita públicas na tabela tarefas
create policy "demo_leitura" on tarefas for select using (true);
create policy "demo_insert"  on tarefas for insert with check (true);
create policy "demo_update"  on tarefas for update using (true);
create policy "demo_delete"  on tarefas for delete using (true);
```

> ⚠️ Isso deixa a tabela aberta a qualquer um com a URL. Serve **só para teste/demo**,
> não para produção.

### 4. Configure o arquivo
Baixe o **`tarefas.html`** deste repositório, abra num editor e preencha as
**duas linhas** no topo do `<script>` com os dados **do seu projeto**:

```js
const SUPABASE_URL      = "";   // ex: https://xxxxxxxxxxxx.supabase.co
const SUPABASE_ANON_KEY = "";   // ex: sb_publishable_xxxxxxxxxxxx
```

Você pega os dois valores no painel do Supabase, no botão verde **Connect**
(no topo da página). Cole cada um **entre as aspas** e salve.

> 🔑 Use a chave **pública** (`sb_publishable_...`). **Nunca** use a `service_role`.

### 5. Abra o app
Dê um **duplo clique** no `tarefas.html`. Ele abre no navegador e já mostra,
adiciona, conclui e apaga tarefas — tudo gravando no seu banco. 🎉

---

## ⚠️ Problemas comuns

| Sintoma | Causa provável | Solução |
|---|---|---|
| Abre no Bloco de Notas, não no navegador | Arquivo salvo como `.txt` | Salvar como **Todos os arquivos** com a terminação **`.html`** |
| Aparece o banner amarelo "Falta configurar" | URL/chave em branco | Preencher as duas linhas do passo 4 |
| Lista abre vazia / erro de permissão | Faltam as policies (RLS) | Rodar o SQL do passo 3 |
| Lista vazia mesmo com policies | Nome da tabela diferente | A tabela precisa se chamar **`tarefas`** |

---

## 🛠️ Tecnologias

- **Supabase** — banco PostgreSQL + API REST automática na nuvem
- **HTML + JavaScript** puro (sem framework, sem build)
- Biblioteca `@supabase/supabase-js` via CDN
