# CybersecFEST — Backup de Agentes

Backup automático dos skills e configurações dos agentes CREAO.

**Última atualização:** 2026-06-21 15:08 UTC

## Agentes Ativos

### [CybersecFEST — Gerador de Artes](_agents/gerador-de-artes/SKILL.md)
- **ID:** `3ae0829d-a05d-471a-aace-7af889c28ccb`
- **Descrição:** Gera posts para feed do CybersecFEST com imagem IA + HTML final + legenda + upload GitHub. v2.2: validação em duas etapas (pré + pós) para rastreamento de erros, rotação inteligente de layouts e score automático de legenda.
- **Skill:** [_agents/gerador-de-artes/SKILL.md](_agents/gerador-de-artes/SKILL.md)
- **Config:** [_agents/gerador-de-artes/config.json](_agents/gerador-de-artes/config.json)

### [CybersecFEST — Campaign Planner](_agents/campaign-planner/SKILL.md)
- **ID:** `e4b59707-ca32-4fea-94ed-53e2c48ae934`
- **Descrição:** Camada estratégica do ecossistema CybersecFEST: planeja campanhas completas, calendários editoriais mensais e gera o plano de temas que alimenta o Pipeline Orquestrador. Opera exclusivamente com dados confirmados — nunca inventa informações.
- **Skill:** [_agents/campaign-planner/SKILL.md](_agents/campaign-planner/SKILL.md)
- **Config:** [_agents/campaign-planner/config.json](_agents/campaign-planner/config.json)

### [CybersecFEST — Pipeline Orquestrador](_agents/pipeline-orquestrador/SKILL.md)
- **ID:** `86597381-a3ca-4970-ac69-7f84368800cb`
- **Descrição:** Orquestra o fluxo completo de ponta a ponta: lê calendário + banco de temas, gera briefings, aguarda aprovação humana e chama o Gerador de Artes automaticamente — sem etapa manual de copiar inputs.
- **Skill:** [_agents/pipeline-orquestrador/SKILL.md](_agents/pipeline-orquestrador/SKILL.md)
- **Config:** [_agents/pipeline-orquestrador/config.json](_agents/pipeline-orquestrador/config.json)

---
*Gerado automaticamente pelo Pipeline Orquestrador*
