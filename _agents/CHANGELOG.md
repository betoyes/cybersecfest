# CHANGELOG — CybersecFEST Agentes

Histórico de versões de todos os agentes do ecossistema.
Formato: [Semântico](https://semver.org) — MAJOR.MINOR.PATCH

---

## CybersecFEST — Gerador de Artes

### [v2.2.0] — 2026-06-21
**MINOR** — Adicionadas validações em duas etapas
- `PASSO -1` Pré-Validação: verifica `temas.json` e `artes.json` antes de iniciar
- `PASSO 8` Pós-Validação: confirma SHA de cada upload e integridade dos registros
- 3 estados de relatório: ✅ OK | ⚠️ alertas | ❌ falha com instrução de correção
- Erros reportados com causa provável e ação recomendada

### [v2.1.0] — 2026-06-21
**MINOR** — Rotação inteligente de layouts + score de legenda
- Rotação por tipo_post: blog(C→M→N), evento(E→L→J), palestrante(D→G→K), patrocinador(F→I→B), cidade(A→H→J)
- Histórico de rotação persistido em `temas.json` (campo `historico_recente`)
- Score automático de legenda 0–10: reescreve automaticamente se score < 7
- Campo `layout` adicionado ao `artes.json`

### [v2.0.0] — anterior
**MAJOR** — Biblioteca de 14 layouts aprovados (A–N)
- CSS canônico para cada layout
- Lei de Foco da Imagem por layout
- Upload automático GitHub + deploy Vercel
- Design system fixo (#02050A, #14A8F4, Ubuntu/Montserrat)

---

## CybersecFEST — Pipeline Orquestrador

### [v1.1.0] — 2026-06-21
**MINOR** — Automação ponta a ponta
- `PASSO 6` reescrito: após aprovação do briefing, chama Gerador de Artes via `start_app_run`
- Inputs do briefing aprovado passados diretamente — sem etapa manual de copiar/colar
- Aprovação humana mantida: usuário confirma antes do disparo automático
- Fluxo: notificação → escolha → confirmação → geração → publicação

### [v1.0.0] — 2026-06-21
**MAJOR** — Criação do agente
- Lê `temas.json` do GitHub em cada execução
- Modo automático: dia da semana → tipo_post → tema → layout → briefing
- Modo manual: usuário define tema e tipo
- Gera 1–3 briefings criativos completos
- Calendário editorial: segunda(blog) / quarta(palestrante) / sexta(evento)
- Agendado: seg/qua/sex às 08:00 BRT (schedule ID: 92a1dbf6)

---

## CybersecFEST — Campaign Planner

### [v1.0.0] — 2026-06-21
**MAJOR** — Reposicionamento como Camada Estratégica
- Reposicionado como Camada 1 do ecossistema (acima do Orquestrador)
- `Step 4` novo: Plano de Temas — gera patch estruturado para `temas.json`
- Calendário editorial sincronizado com o padrão seg/qua/sex do Orquestrador
- Dashboard inclui link para execução no Pipeline Orquestrador
- Todas as 12 regras editoriais mantidas integralmente
- Renomeado de "Content & Campaign Planner v0.5" para "Campaign Planner"

### [v0.5.0] — anterior
**Versão inicial**
- Geração de campanhas multicanal (LinkedIn, Instagram, e-mail)
- 12 regras editoriais anti-invenção de dados
- Briefing visual para designer, copies, calendário, checklist

---

## CybersecFEST — Backup de Agentes

### [v1.0.0] — 2026-06-21
**MAJOR** — Criação do agente
- Backup de SKILL.md e config.json para `_agents/` no GitHub
- README de índice atualizado com timestamp a cada execução
- Relatório com status por arquivo (✅/⚠️/❌)

---

## Agentes Descartados

| Agente | Última versão | Data | Motivo |
|--------|--------------|------|--------|
| Briefing Creator | v1.x | 2026-06-21 | 100% substituído pelo Pipeline Orquestrador |
| LinkedIn Content Planner | v1.x | 2026-06-21 | Fora do ecossistema CybersecFEST |

---

## Regra de Versionamento Adotada

| Situação | Bump |
|----------|------|
| Novo agente criado | MAJOR (X.0.0) |
| Nova funcionalidade ou passo adicionado | MINOR (+0.1.0) |
| Correção de bug, ajuste de texto, tweakzinho | PATCH (+0.0.1) |
| Reformulação completa do fluxo ou campos obrigatórios | MAJOR (+1.0.0) |

*Atualizado em: 2026-06-21*
