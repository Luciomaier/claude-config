---
name: GitHub Backup — Claude Commands e Config
description: Repositórios no GitHub para backup dos agentes, skills e memória do Claude Code
type: project
originSessionId: 546f0be6-f9d2-44ed-815f-729353b44ebc
---
Dois repositórios privados no GitHub do Lucio para backup do Claude Code.

**Why:** Arquivos locais seriam perdidos se o computador quebrasse ou fosse formatado.

**How to apply:** Quando o usuário pedir "faz o backup", "atualiza o GitHub" ou similar, executar os dois comandos abaixo.

## Repositórios

- `github.com/Luciomaier/claude-commands` → todos os agentes e skill packs (`~/.claude/commands/`)
- `github.com/Luciomaier/claude-config` → CLAUDE.md, settings e memória (`~/claude-config/`)

## Comandos de atualização

**Agentes e skills:**
```bash
cd ~/.claude/commands && git add . && git commit -m "atualiza agentes" && git push
```

**Config e memória:**
```bash
cp ~/.claude/CLAUDE.md ~/claude-config/ && cp ~/.claude/projects/-home-usuario/memory/*.md ~/claude-config/memory/ && cd ~/claude-config && git add . && git commit -m "atualiza config" && git push
```

## Como restaurar em máquina nova

1. Instalar Claude Code
2. `git clone https://github.com/Luciomaier/claude-commands.git ~/.claude/commands`
3. `git clone https://github.com/Luciomaier/claude-config.git ~/claude-config`
4. Copiar arquivos: `cp ~/claude-config/CLAUDE.md ~/.claude/ && cp ~/claude-config/settings.json ~/.claude/ && cp ~/claude-config/memory/*.md ~/.claude/projects/-home-usuario/memory/`
