---
name: UniMasso — Auditoria de Segurança
description: 18 problemas encontrados (3 críticos, 6 altos) — RLS fraco em subscriptions é o mais grave
type: project
originSessionId: e8938bb6-fe94-4732-af6c-7860b19ec481
---
Auditoria realizada em 15/04/2026. Relatório completo no vault:
`/home/usuario/Documentos/Revolucio/Revolucio/Projetos/UniMasso — Auditoria de Segurança.md`

Projeto local: `/home/usuario/Documentos/unimasso-pass`
GitHub: `Luciomaier/unimasso-pass`

**Why:** SaaS de massoterapia com dados financeiros e de usuários. RLS fraco pode permitir manipulação de assinaturas.

**How to apply:** Antes de alterar qualquer coisa relacionada a pagamentos ou assinaturas no UniMasso, verificar o status do RLS em `subscriptions`.

---

## Top problemas

### URGENTE
- `.env` commitado — adicionar ao `.gitignore`
- `supabase/migrations/20250414000000_create_subscription_tables.sql` (linhas 26-32) — RLS `USING (true)` em `subscriptions`: qualquer usuário pode manipular assinaturas de outros
- Múltiplas tabelas com `DISABLE ROW LEVEL SECURITY`
- Auditar modificações em `subscriptions` entre abril-agosto 2025

### BREVE
- `src/pages/CampaignCheckout.tsx` — query params sem validação Zod
- `src/components/auth/RoleGuard.tsx` — retorna `null` em vez de redirecionar
- `src/contexts/AuthContext.tsx` — signup sem transação atômica
- `src/hooks/useTrocaMassagem.ts` — race condition, usar upsert

### MÉDIO PRAZO
- 206 usos de `any` — ativar `noImplicitAny`
- 96 setTimeout sem cleanup
- 7 componentes > 500 linhas
