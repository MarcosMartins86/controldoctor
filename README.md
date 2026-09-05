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

## Supabase

Projeto: `dgkdpczzhvyldpvwkutk` · URL `https://dgkdpczzhvyldpvwkutk.supabase.co`.
Credenciais já aplicadas em `index.html` (`SB_URL` / `SB_KEY` — a anon key é pública, protegida por Auth/RLS).

Falta configurar no painel do Supabase (ver **[SUPABASE.md](SUPABASE.md)**):
- Authentication → Providers → Email → **"Confirm email" = OFF** (para testes)
- Authentication → URL Configuration → Site URL = `https://marcosmartins86.github.io/controldoctor/`
- Project Settings → Legal → aceitar o **DPA**

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
