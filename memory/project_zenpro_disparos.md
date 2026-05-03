---
name: ZenPro — Módulo Disparos em Massa (BulkDispatch)
description: Fases 1 e 2 concluídas em 20/04/2026 — 10 bugs corrigidos, testes funcionais pendentes, Fases 3 e 4 no backlog
type: project
originSessionId: auto
---

## Estado atual (21/04/2026)

**Fase 1 — Persistência de mensagem:** ✅ Concluída  
**Fase 2 — Seleção por campanha (combobox + tracking_history):** ✅ Concluída  
**Fase 3 — Remarketing por filtros:** 🔲 Pendente  
**Fase 4 — Remarketing automático (cron):** 🔲 Pendente  

Documentação completa: `/home/usuario/Documentos/atendichat-hub-30/ZENPRO.md`

**Why:** 10 bugs corrigidos na sessão 20/04 mas testes funcionais não foram realizados. Importante validar antes de avançar para Fase 3.

---

## Testes pendentes (sessão 21/04/2026)

Testar no ambiente real (dev server + webhook configurado):

1. **Block pause** — confirmar que não entra em loop infinito após pausa de bloco (Bug 1 crítico)
2. **Skip block pause** — botão "Pular Pausa" funciona e retoma corretamente (Bug 2)
3. **Importação CSV** — após importar, banco é atualizado com UUIDs reais (Bug 3)
4. **Continuar lista pausada** — texto e pool restauram automaticamente (Fase 1)
5. **Seleção por campanha** — combobox busca via `tracking_history.tracking_code` (Fase 2)
6. **Histórico** — aparece após disparo concluir sem precisar refresh (Bug 8)
7. **Validação webhook** — disparo não inicia sem URL configurada (Bug 10)
8. **Sliders** — min nunca ultrapassa max (Bug 9)

---

## 🔴 Bug urgente — Formatação de mensagens (23/04/2026)

**Problema:** Ao colar um texto com formatação (quebras de linha, negrito, etc.) no campo de mensagem do chat **e** nas Respostas Rápidas, a formatação é perdida — o texto chega como bloco corrido sem parágrafos ou estilo.

**Impacto:** Vendedor prepara um texto rico (ex: informações de curso) e ao colar perde toda a estrutura — prejudica a comunicação profissional.

**Onde ocorre:**
- Campo de mensagem do Chat
- Ferramenta de Respostas Rápidas

**O que precisa:** O campo deve preservar quebras de linha e formatação ao colar (paste), e enviar para o WhatsApp respeitando essa formatação (quebras de linha viram `\n` no payload da Evolution API).

---

## Backlog — próximas features

### Lista de supressão de compradores (próxima após testes)
Toggle "Suprimir compradores" ao selecionar campanha — cruza contatos com tabela `sales` antes de disparar.
3 modos: global / por campanha / por produto. Ver spec completo em ZENPRO.md.

### Outras melhorias
- Suporte a variáveis na mensagem (`{nome}`, `{numero}`)
- Relatório exportável em CSV
- Reset de lista para reteste via UI
- Fase 3: filtros de remarketing (status conversa, sem resposta há X dias, tag, atendente)
- Fase 4: regras automáticas via cron (Edge Function diária)

---

## Arquitetura resumida

| Camada | Arquivo |
|--------|---------|
| Página | `src/pages/BulkDispatch.tsx` |
| Motor | `src/hooks/useBulkDispatch.ts` |
| Listas | `src/hooks/useBulkDispatchLists.ts` |
| Storage | `src/lib/dispatchStorage.ts` |
| Tabelas | `bulk_dispatch_lists`, `bulk_dispatch_contacts`, `bulk_dispatch_runs` |

Supabase project ref: `wympympkabncrihdavxn`
