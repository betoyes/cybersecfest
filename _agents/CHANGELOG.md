# CybersecFEST — Changelog dos Agentes

## [2.5.0] — 2026-06-21 — Gerador de Artes
### Adicionado
- PROTOCOLO MULTI-AGENTE: fetch fresco obrigatório nos PASSOs 6 e 7 (GET SHA antes de qualquer PUT)
- Prefixo `[SuperAgent]` em todas as mensagens de commit
- PASSO -1 agora exibe os últimos 5 commits do repo para detectar mudanças de agentes parceiros
- AGENTS.md criado no repo com protocolo completo de colaboração

## [2.4.0] — 2026-06-21 — Gerador de Artes
### Adicionado
- Variação A/B de legenda (FOMO vs aspiracional) com pausa antes do upload
- Campo `legenda_variante` em artes.json
- Botão toggle ◀▶ Legenda no modal da galeria

## [1.5.2] — 2026-06-21 — Pipeline Orquestrador
### Adicionado
- Preview ao vivo (card visual markdown com tabela + painel unificado de decisão)

## [1.5.1] — 2026-06-21 — Pipeline Orquestrador
### Adicionado
- Histórico de aprovações (temas.json v3.1.0 com historico_aprovacoes[])
- PASSO 5.5 no Orquestrador

## [1.5.0] — 2026-06-21 — Pipeline Orquestrador
### Adicionado
- Filtro anti-repetição (Mapa de Bloqueio no PASSO 1, consulta no PASSO 4)

## [2.3.0] — 2026-06-21 — Gerador de Artes
### Adicionado
- Layout N (Acento Diagonal) — gradiente 155deg 5 paradas, sem barra diagonal
- Logos ecossistema padronizados em height 33px

## Stack Atual
| Agente | ID | Versão |
|--------|-----|--------|
| Gerador de Artes | 3ae0829d | v2.5.0 |
| Pipeline Orquestrador | 86597381 | v1.5.2 |
| Campaign Planner | e4b59707 | v1.0.0 |
| Protocolo Multi-Agente | AGENTS.md | v1.0.0 |
