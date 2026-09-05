# Configuração do Supabase — passo a passo

Tempo: ~10 minutos. Custo: R$ 0 (free tier).

## 1. Criar o projeto

1. Entrar em https://supabase.com → **New project**.
2. Nome: `controldoctor`.
3. **Region: South America (São Paulo)** — importante, mantém os dados no Brasil.
4. Definir uma senha de banco (guardar; não é usada pelo app).
5. Criar. Aguardar ~2 min o provisionamento.

## 2. Habilitar login por e‑mail

1. **Authentication → Sign In / Providers → Email**: deixar **Enabled**.
2. Para os testes, desligar a confirmação de e‑mail:
   **Authentication → Sign In / Providers → Email → "Confirm email" = OFF**.
   (Depois, em produção, ligar de novo.)
3. **Authentication → URL Configuration → Site URL**:
   `https://marcosmartins86.github.io/controldoctor/`

## 3. Pegar as credenciais

**Project Settings → API**:

| Campo | Onde usar |
|---|---|
| **Project URL** (`https://xxxx.supabase.co`) | substitui `__SUPABASE_URL__` no `index.html` |
| **anon public** key | substitui `__SUPABASE_ANON_KEY__` no `index.html` |

> A `anon key` pode ficar pública no código — ela só permite o que as políticas
> (RLS) e o Auth deixarem. **Nunca** usar a `service_role` key no frontend.

## 4. Aplicar no código

No `index.html`, no topo do `<script>`:

```js
const SB_URL = 'https://SEU-PROJETO.supabase.co';
const SB_KEY = 'eyJhbGciOi...sua-anon-key...';
```

Depois:

```bash
git add index.html
git commit -m "config: supabase"
git push
```

## 5. Aceitar o DPA

**Project Settings → Legal → Data Processing Addendum** → aceitar.
(É o contrato que coloca o Supabase como *operador* de dados sob a LGPD.)

## 6. Criar a conta do médico de teste

Opção A — ele mesmo: abre a URL e clica em **Criar conta**.

Opção B — você cria: **Authentication → Users → Add user** → e‑mail + senha →
passa a senha para ele.

---

## Próximo passo (não obrigatório agora): sincronizar os dados

Hoje os dados ficam só no navegador. Para sincronizar entre dispositivos e ter
backup automático, criar 1 tabela:

```sql
create table public.user_state (
  user_id    uuid primary key references auth.users(id) on delete cascade,
  data       jsonb not null default '{}'::jsonb,
  updated_at timestamptz not null default now()
);
alter table public.user_state enable row level security;

create policy "own row - select" on public.user_state
  for select using (auth.uid() = user_id);
create policy "own row - upsert" on public.user_state
  for insert with check (auth.uid() = user_id);
create policy "own row - update" on public.user_state
  for update using (auth.uid() = user_id);
```

E ajustar `load()` / `save()` no `index.html` para ler/gravar essa linha.
(Peça ao Claude para fazer essa parte quando quiser.)
