# Claude Config — Backup

Backup dos arquivos essenciais do Claude Code do Lucio Maier.

## Conteúdo

| Arquivo/Pasta | O que é |
|---------------|---------|
| `CLAUDE.md` | Perfil global — quem é o Lucio, projetos, família, contexto |
| `settings.json` | Configurações do Claude Code |
| `memory/` | Memória acumulada entre sessões — projetos, decisões, contexto |

## Como restaurar

Se trocar de máquina ou reinstalar:

```bash
cp CLAUDE.md ~/.claude/
cp settings.json ~/.claude/
cp memory/*.md ~/.claude/projects/-home-usuario/memory/
```

## Como atualizar o backup

Após mudanças importantes, rodar:

```bash
cp ~/.claude/CLAUDE.md ~/claude-config/
cp ~/.claude/settings.json ~/claude-config/
cp ~/.claude/projects/-home-usuario/memory/*.md ~/claude-config/memory/
cd ~/claude-config
git add .
git commit -m "atualiza backup claude config"
git push
```

## Repositório separado

Os agentes e skills ficam em: `github.com/SEU_USUARIO/claude-commands`
