# ControlDoctor

Ferramenta de controle de pagamentos de cirurgias por convênio (2M Consultoria).

- **Frontend:** um único `index.html` + `cbhpm.js` (tabela CBHPM). Sem build.
- **Login:** Supabase Auth (e‑mail + senha).
- **Dados:** ficam **no navegador do médico** (localStorage). Nenhum dado de paciente
  vai para servidor nesta versão. Backup por exportar/importar (`Backup e configurações`).
- **Hospedagem:** GitHub Pages (repositório público).

## URL de produção

`https://marcosmartins86.github.io/controldoctor/`

## Publicar uma nova versão

```bash
git add index.html cbhpm.js
git commit -m "..."
git push
```
O GitHub Pages atualiza sozinho em ~1 min.

## Configurar o Supabase (uma vez)

Ver **[SUPABASE.md](SUPABASE.md)**. Resumo:

1. Criar projeto no Supabase — região **South America (São Paulo)**.
2. Authentication → Providers → **Email** habilitado. Para testes, desabilite
   "Confirm email" (Authentication → Providers → Email → *Confirm email* OFF).
3. Copiar **Project URL** e **anon public key** (Settings → API).
4. Em `index.html`, substituir:
   - `__SUPABASE_URL__`  → a Project URL
   - `__SUPABASE_ANON_KEY__` → a anon key
   (a `anon key` pode ser pública — é protegida por RLS/Auth.)
5. `git commit` + `git push`.
6. Aceitar o **DPA** do Supabase (Settings → Legal).

Enquanto os dois placeholders não forem trocados, o app roda em **modo local de testes**
(conta `ricardo@controldoctor.app` / `123456`, com dados de exemplo).

## Entregar uma conta a um médico

- Opção A: o médico abre a URL → **Criar conta** (e‑mail + senha).
- Opção B: você cria em Supabase → Authentication → Users → *Add user*, e passa a senha.

Na primeira entrada ele vê a página de **responsabilidade / LGPD** e precisa aceitar.

## LGPD

- Nomes de paciente são reduzidos a iniciais automaticamente antes de gravar.
- O médico é o controlador dos dados; o app não os transmite.
- Ver a página "Responsabilidade / LGPD" dentro do app.

## Desenvolvimento local

Abrir `index.html` direto no navegador (modo local de testes). Trabalhar sempre com
dados fictícios — nunca importar backup real de um médico no ambiente de dev.
