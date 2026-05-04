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
- Restart policy: `unless-stopped` configurado em 03/05/2026 — sobe automaticamente se cair

## Alterações VPS — 03/05/2026 (Nick)

### O que foi feito
- UFW ativado: `deny incoming` / `allow outgoing`
- Fail2ban instalado protegendo SSH
- Credenciais Telegram/gateway movidas para `~/.openclaw/.env`
- OpenClaw instalado (AI assistant — Docker Compose, porta 18789 só localhost)
- Crons adicionados: backup (seg 6h) e security audit (seg 7h)

### Estado da infraestrutura
- Docker Swarm já estava ativo (pré-existente) — todos os serviços rodam como Swarm services
- `financeiro-bot` é o único container fora do Swarm (bridge network)
- Webhook Evolution API → bot funciona via `172.17.0.1:8502` (rede interna Docker)

### Firewall UFW — estado final seguro
| Porta | Regra | Motivo |
|-------|-------|--------|
| 22 | ALLOW | SSH |
| 80/443 | ALLOW | Traefik |
| 8503 | ALLOW 127.0.0.1 | Dashboard local |
| 8502 | ALLOW 172.0.0.0/8 | Só redes internas Docker (Evolution API usa 172.18.x.x) |
| 2377 | DENY | Docker Swarm management |
| 7946 tcp/udp | DENY | Docker Swarm node comm |
