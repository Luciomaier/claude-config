---
name: Holos Connect — Deploy na Vercel
description: Status do deploy do Holos Connect — código já no GitHub, falta conectar na Vercel
type: project
originSessionId: e8938bb6-fe94-4732-af6c-7860b19ec481
---
Projeto Holos Connect localizado em `/home/usuario/Documentos/holos-connect`.

**Why:** App React + Vite + Supabase da Holos. Lucio opera como SaaS gerenciado (~1.000 alunos ativos). Deploy pendente na Vercel.

**How to apply:** Quando retomar, ir direto para o passo de conectar o repositório na Vercel.

---

## Estado atual (15/04/2026)

- Código local: atualizado e funcionando (testado localmente)
- GitHub: `Luciomaier/holos-universit` (privado) — push feito, branch `main` atualizada
- Vercel: **ainda não conectada** — próximo passo

## O que foi feito

- Corrigido bug de RLS (permissões) nos quizzes para staff — migration `20260415000001_fix_quizzes_core_rls_for_staff.sql`
- Ajustes em `AdminSidebar.tsx` e `useAdminBoletim.ts`
- `.env` removido do git e adicionado ao `.gitignore`
- `vercel.json` já configurado (rewrites para SPA)

## Variáveis de ambiente para configurar na Vercel

```
VITE_SUPABASE_URL=https://ijuhdovdbvtgbbggdfuf.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
VITE_SUPABASE_PROJECT_ID=ijuhdovdbvtgbbggdfuf
```

(valores completos no arquivo `.env` local da máquina)

## Próximo passo

Deploy já está no ar — Vercel estava conectada e disparou automaticamente após o push.

## Auditoria de segurança realizada em 15/04/2026

Relatório completo salvo em:
`/home/usuario/Documentos/Revolucio/Revolucio/Projetos/Holos Connect — Auditoria de Segurança.md`

Auditoria de segurança detalhada em: [Holos Connect — Auditoria de Segurança](project_holos_connect_seguranca.md)

**10 problemas encontrados — resumo:**
- CRÍTICO: credenciais hardcoded em `client.ts`, webhook Asaas sem HMAC, senha em email, XSS (dangerouslySetInnerHTML), falta autorização em room_bookings
- ALTO: bypass de auth em DEV nos Guards, CORS aberto com `*` em todas as functions
- MÉDIO: 272 usos de `any`, componentes >700 linhas, dados sensíveis em localStorage

## Pendências importantes

### PENDENTE — IMPORTANTE
- **Verificar e testar sistema de email marketing integrado com a Brevo** — exportação de usuários via Supabase (`profiles` + `user_communications` + `auth.users`) já validada em 26/04/2026. Próximo passo: confirmar importação no Brevo e fazer disparo de teste.

### Segurança
- Token do GitHub está na URL do remote local (`.git/config`) — revogar e gerar novo em github.com → Settings → Developer settings → Personal access tokens
