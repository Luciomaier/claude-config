---
name: ZenPro — Bolsão Moderado (status atual)
description: Estado do desenvolvimento do bolsão moderado no atendichat-hub-30, próximos passos para retomar
type: project
originSessionId: e4dded55-89a5-4303-bccc-9afca3251702
---
## Estado atual (17/04/2026)

Bolsão moderado completo e em produção. ✅

## O que está pronto (código)

1. **Migration `hide_assigned_from_pool`** — coluna na tabela organizations (já rodada no Supabase)
2. **Filtro no chat** — todos os roles (admin/owner/agent) seguem a mesma regra: não veem conversas atribuídas a outros. Monitoramento via /admin/transfers
3. **Espiar conversa** — dialog centralizado estilo chat com MessageBubble (imagens, áudios), somente leitura
4. **Moderador consistente** — removido auto-select que causava inconsistência entre owner e admin
5. **webhook-worker** — lógica completa do bolsão moderado:
   - Lead novo → assigned_to = moderador
   - Conversa fechada reaberta → assigned_to = moderador
   - Conversa no bolsão sem dono (assigned_to=null) → assigned_to = moderador
   - Auto-return devolve ao bolsão LIVRE (não passa pelo moderador de novo) ✓
6. **reset-test-number** — corrigido FK constraint (contact_activity_log, agent_score_log) + feedback "número não encontrado"
7. **FK fix SQL** — precisa rodar no Supabase SQL Editor (ver abaixo)

## O que foi feito (17/04/2026)

- Deploy `webhook-worker` e `reset-test-number` via Supabase CLI (instalado v1.219.0 + Docker)
- SQL FK fix rodado no Supabase SQL Editor
- `auto-return-to-pool` corrigida: modo `moderated` não devolve conversa do moderador ao bolsão livre
- Bug encontrado: valor no banco é `moderated` (não `moderado`) — corrigido no código
- Testado em produção ✅ — conversa fica com moderador após timer zerar
- Commitado `62ba509` e pushado para GitHub

## Próximos itens pendentes (backlog)
- Bolsão Item 3: bolsão moderado sem temporizador de retorno automático (auto-return deve pular conversas do bolsão moderado)
- Bolsão Item 4: horário comercial ✅ FEITO (18/04/2026) — ver detalhes abaixo
- Tela de vendas para atendentes (/sales) com toggle team vs own
- ZenPro security: token GitHub exposto em .git/config (revogar e gerar novo)

## Item 4 — Horário Comercial (concluído 18/04/2026)

**Commits:** aa7f771 (feat) + 86b1dcd (fix)
**Deploy:** webhook-worker deployado em produção (projeto wympympkabncrihdavxn)
**SQL rodado:** 7 colunas adicionadas em organizations (business_hours_*)

**Comportamento:**
- Lead chega fora do horário → auto-resposta enviada + atribuído ao moderador (ele vê o que chegou fora do expediente)
- Lead chega dentro do horário → fluxo normal (sem alteração)
- Conversa reaberta fora do horário → mesma lógica

**UI:** card "Horário Comercial" na aba Avançado de AdminOrganization

**Limitação conhecida / próxima melhoria:**
- Horário único para todos os dias selecionados (ex: Seg–Sex 08h–18h)
- Sábado meio período (ex: 08h–12h) não é possível na estrutura atual
- Lucio quer resolver isso: horário diferente por dia da semana

## Fluxo confirmado pelo usuário
```
Lead novo → Moderador → Transfere para vendedor → Vendedor perde prazo → Bolsão livre (qualquer um pega)
```
Auto-return NÃO volta ao moderador, vai direto ao bolsão livre. ✓

## Referências
- Projeto: /home/usuario/Documentos/atendichat-hub-30
- GitHub: https://github.com/Luciomaier/atendichat-hub-30
- Supabase project ref: wympympkabncrihdavxn
