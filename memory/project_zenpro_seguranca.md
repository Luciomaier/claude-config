---
name: ZenPro — Auditoria de Segurança
description: Auditoria concluída — todos os 4 críticos resolvidos em 21/04/2026. Score atual estimado: 7/10.
type: project
originSessionId: e8938bb6-fe94-4732-af6c-7860b19ec481
---
Auditoria realizada em 15/04/2026. Relatório completo no vault:
`/home/usuario/Documentos/Revolucio/Revolucio/Projetos/ZenPro — Auditoria de Segurança.md`

Projeto local: `/home/usuario/Documentos/atendichat-hub-30`
GitHub: `Luciomaier/atendichat-hub-30`

---

## Críticos — TODOS RESOLVIDOS (21/04/2026)

- [x] `src/integrations/supabase/client.ts` — credenciais já estavam em `import.meta.env` (já estava correto)
- [x] `.env` commitado — `.env` está no `.gitignore`, só `.env.example` é trackado (já estava correto)
- [x] `src/lib/dispatchStorage.ts` linha 23 — webhook URL já usa `import.meta.env.VITE_DISPATCH_WEBHOOK_URL` (já estava correto)
- [x] `src/components/admin/cmo/CMOMessage.tsx` — DOMPurify já estava importado e em uso (já estava correto)
- [x] Token GitHub exposto em `.git/config` — era o `token-vps` (expirado em set/2025, sem risco). URL do remote limpa em 21/04/2026. Token `claude-local` está seguro (nunca usado, válido até jul/2026).

**Why:** Problemas já tinham sido corrigidos em sessões anteriores sem atualização da memória. Verificação feita em 21/04/2026 confirmou estado atual do código.

---

## Pendências remanescentes

### BREVE
- [ ] `src/components/SuperAdminRoute.tsx` — super_admin verificado só no frontend
- [ ] `supabase/functions/whatsapp-evolution-send/index.ts` — input sem validação Zod
- [ ] 30+ queries com `.select('*')` — especificar campos
- [ ] `src/pages/Auth.tsx` — sem validação antes de chamar signIn

### MÉDIO PRAZO
- [ ] 159 usos de `any` no TypeScript
- [ ] SESSION_CACHE sem cleanup nas Edge Functions
- [ ] `src/pages/admin/AdminCampaigns.tsx` — 1205 linhas (refatorar)
- [ ] `src/hooks/useOrganization.tsx` — `.single()` quebra com múltiplas orgs
