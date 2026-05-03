---
name: Holos Connect — Auditoria de Segurança
description: Resultado da varredura de segurança e qualidade do código do Holos Connect — 10 problemas encontrados, plano de ação priorizado
type: project
originSessionId: e8938bb6-fe94-4732-af6c-7860b19ec481
---
Auditoria realizada em 15/04/2026. Relatório completo salvo no vault:
`/home/usuario/Documentos/Revolucio/Revolucio/Projetos/Holos Connect — Auditoria de Segurança.md`

Contexto de deploy em: [Holos Connect — Deploy na Vercel](project_holos_connect_deploy.md)

**Why:** App com ~1.000 alunos ativos, sem auditoria prévia. Vários riscos reais de segurança identificados.

**How to apply:** Antes de qualquer alteração no Holos Connect, consultar os itens CRÍTICOS abaixo para não piorar a situação.

---

## Problemas por prioridade

### URGENTE — Fazer agora
- [ ] `src/integrations/supabase/client.ts` (linhas 6-7) — credenciais hardcoded, mover para `import.meta.env.VITE_*`
- [ ] `supabase/functions/asaas-webhook/index.ts` — webhook de pagamentos sem validação HMAC (pagamentos falsificáveis)
- [ ] `supabase/functions/send-activation-email/index.ts` (linha 70) — senha temporária exposta no email e na resposta HTTP
- [ ] `dangerouslySetInnerHTML` sem DOMPurify em 3 arquivos: `CampaignDetailDialog.tsx`, `NewCampaignDialog.tsx`, `FormacaoMassoterapiaLitoralSul.tsx`

### BREVE — Próximas 2 semanas
- [ ] `AuthGuard.tsx`, `AdminGuard.tsx`, `TeacherGuard.tsx`, `StudentGuard.tsx` — bypass de auth em DEV mode (linhas 54-57)
- [ ] CORS `*` em todas as Edge Functions — restringir para domínios da Holos
- [ ] `src/components/rooms/BookingRequestForm.tsx` — INSERT sem validação de permissão/créditos

### MÉDIO PRAZO — Próximo mês
- [ ] 272 usos de `any` no TypeScript — ativar `noImplicitAny` no tsconfig
- [ ] Refatorar componentes >700 linhas: `SessionManagement.tsx`, `ManualPaymentForm.tsx`, `RoomBookingDialog.tsx`
- [ ] `src/components/funnel/hooks/useLeadStorage.ts` — dados de leads em localStorage
