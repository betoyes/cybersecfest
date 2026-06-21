# CybersecFEST — Agents Changelog

# CybersecFEST — Agents Changelog

## 2026-06-21 (sessão 3)

### Gerador de Artes v2.4.0
- **#4 Variação A/B de legenda:** PASSO 4 reformulado — gera 2 versões com ângulos distintos (FOMO/urgência vs aspiracional/conquista)
- Painel de escolha A/B exibido ao usuário com separadores visuais antes do upload — agente pausa e aguarda confirmação
- Suporte a "editar": ajuste de uma versão sem perder a outra
- Campo `legenda_variante` ("A" ou "B") adicionado ao registro em artes.json para rastreabilidade de performance
- Score ≥ 7/10 aplicado individualmente a cada variante antes de apresentar

### Galeria (index.html) — melhorias visuais
- **#6 Dashboard de métricas:** 4 painéis com barras clicáveis — Por Tipo (azul), Por Layout (ciano), Por Cidade (lilás), Legenda A/B (verde)
  - Painel Cidade normaliza entradas compostas ("BH e SP" → BH + SP separados)
  - Painel A/B acumula dados de legenda_variante do artes.json
- **Fix scroll iframes:** cards da home não exibem mais scrollbar em artes antigas
- **Toggle modal ◀▶ Legenda:** botão no painel da arte que recolhe/expande o painel de metadados com transição suave; arte reescalada automaticamente

---

## 2026-06-21 (sessão 2)

### Pipeline Orquestrador v1.5.2
- **#2 Preview ao vivo:** briefings entregues como cards visuais markdown (tabela, contexto destacado, legenda completa)
- **#5 Filtro anti-repetição:** Mapa de Bloqueio construído no PASSO 1 — bloqueia layouts recentes (últimos 3) e ângulos rejeitados (últimos 5 do historico_aprovacoes)
- Painel unificado de decisão com comandos: aprovar / rejeitar / refazer

### Pipeline Orquestrador v1.5.1
- **#3 Histórico de aprovações:** PASSO 5.5 criado — registra decisão (aprovado/rejeitado) no historico_aprovacoes do temas.json via GitHub API após cada briefing
- PASSO 1 ampliado: lê historico_aprovacoes e calibra criativamente (evitar rejeitados, reforçar aprovados)
- Checklist inclui: "Ângulo não repete padrão rejeitado recentemente"

### Gerador de Artes v2.3.1
- **#9 Layout N — Acento Diagonal:** barra decorativa removida (era confusa e sobrepunha conteúdo)
- Overlay redefinido: linear-gradient(155deg) com 5 paradas — topo direito 5% → inferior esquerdo 97%
- O gradiente diagonal é o acento visual do layout (sem elemento extra)

### temas.json v3.1.0
- **#3 Histórico de aprovações:** campo historico_aprovacoes[] adicionado (array, máx 50 entradas)
- Estrutura de cada entrada: data, tipo_post, angulo_resumo, headline, status, motivo, slug

---

## 2026-06-21 (sessão 1)

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
