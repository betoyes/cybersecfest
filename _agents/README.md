# CybersecFEST — Backup de Agentes

Backup automático dos skills e configurações dos agentes CREAO.
**Última atualização:** 2026-06-21

---

## Agentes Ativos

| Agente | Versão | Camada | Skill | Config |
|--------|--------|--------|-------|--------|
| Gerador de Artes | v2.2.0 | Produção | [SKILL.md](gerador-de-artes/SKILL.md) | [config.json](gerador-de-artes/config.json) |
| Pipeline Orquestrador | v1.1.0 | Operacional | [SKILL.md](pipeline-orquestrador/SKILL.md) | [config.json](pipeline-orquestrador/config.json) |
| Campaign Planner | v1.0.0 | Estratégica | [SKILL.md](campaign-planner/SKILL.md) | [config.json](campaign-planner/config.json) |
| Backup de Agentes | v1.0.0 | Infraestrutura | — | — |

## Arquitetura

```
Campaign Planner v1.0   →   Orquestrador v1.1   →   Gerador de Artes v2.2
  (Camada Estratégica)       (Camada Operacional)     (Camada de Produção)
  planeja o mês              executa por post          publica a arte
       │                           │                         │
       └──── alimenta ────► temas.json ◄──── atualiza ───────┘
```

## Histórico de versões

Ver [CHANGELOG.md](CHANGELOG.md)

---
*Gerado automaticamente pelo Backup de Agentes*
