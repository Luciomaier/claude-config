---
name: Centro de Comando Financeiro — Setup Completo
description: Sistema financeiro completo com Google Sheets, Streamlit e bot WhatsApp via Evolution API
type: project
originSessionId: e0be02a6-0ee7-4815-a494-ff0a3558793e
---
Sistema construído em 20/04/2026. Totalmente funcional.

**Why:** Lucio precisava de visibilidade total do fluxo de caixa para tomar decisões cirúrgicas baseadas em dados.

**How to apply:** Referenciar este setup ao ajudar com decisões financeiras ou evoluções do sistema.

## Componentes

### Google Sheets
- URL: https://docs.google.com/spreadsheets/d/1SF69rfXykIERxBlCzwpjYKibVrMAUUnduisJPaCsHWQ/edit
- Abas: Lançamentos, Resumo Mensal, Categorias, Cartões, Faturas, Metas, Despesas Fixas, 📊 Dashboard
- Credencial: ~/.config/google/sheets-key.json (service account: claude-sheets@lucio-financeiro.iam.gserviceaccount.com)

### Scripts locais (~/.config/google/)
- `analise_financeira.py` — diagnóstico completo via terminal
- `dashboard.py` — Streamlit em http://localhost:8501

### Bot WhatsApp
- Servidor: Hetzner 168.119.185.57 (Docker container `financeiro-bot`)
- Porta: 8502 (interna, não exposta)
- Evolution API: https://evolution.m6k.com.br — instância `lucio-financeiro`
- Webhook: http://172.17.0.1:8502/webhook
- Senha de acesso: zencash (sessão 24h)
- Aceita texto e áudio (Whisper)
- Número: 5513981552646

### Manutenção
- Reiniciar bot: `ssh root@168.119.185.57 "docker restart financeiro-bot"`
- Ver logs: `ssh root@168.119.185.57 "docker logs financeiro-bot 2>&1 | tail -20"`
- Atualizar bot: scp bot.py → restart
