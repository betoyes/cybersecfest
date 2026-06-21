# CybersecFEST — Agents Changelog

## 2026-06-21

### Pipeline Orquestrador v1.5.0
- Adicionados 6 exemplos reais de posts aprovados como referência de calibração de tom e ritmo
- Regra de tamanho: posts medianos (6–12 linhas de corpo, máx 15)
- Padrões de formatação: 🔹 listas, ✅ CTA, ⚠️ urgência, 10–20 hashtags
- Seção "Padrões Extraídos dos Posts Reais" com o que funciona e o que evitar
- Posts nunca devem começar com "O CybersecFEST"

### Gerador de Artes v2.3.0
- Especificações HTML completas para 8 layouts (A, C, D, E, F, L, M, N)
- Regra crítica: cada layout gera HTML/CSS visualmente distinto — nunca o mesmo template
- Logos ecossistema: height 22px → 33px (50% maior), padrão fixo
- Layout M: overlay radial-gradient (vinheta do centro claro para bordas escuras)

### Correção Galeria (index.html)
- Bug: id="filters-layout" ausente causava TypeError silencioso que impedia renderGrid()
- Fix: adicionado div#filters-layout após div#filters-cidade

---

## Versões anteriores

### Pipeline Orquestrador v1.4.0
- Reformulado para ser criativo — temas.json é referência de contexto, não roteiro
- 5 formatos de gancho, checklist interno, justificativa criativa por briefing
- Dispara Gerador de Artes automaticamente após aprovação

### Gerador de Artes v2.2.0
- Pré-validação + pós-validação com rastreamento de SHA
- Score de legenda 0–10 (reescreve se < 7)
- Campo layout adicionado ao artes.json

### Gerador de Artes v2.1.0
- Rotação inteligente de layouts por tipo_post
- Score automático de legenda

### Campaign Planner v1.0.0
- Camada estratégica — geração de campanhas completas
