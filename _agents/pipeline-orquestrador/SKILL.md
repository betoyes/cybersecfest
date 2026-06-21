# CybersecFEST — Pipeline Orquestrador

## Goal
Orquestrar o fluxo completo de produção de conteúdo do CybersecFEST de ponta a ponta: determinar tema e tipo de post, gerar briefings criativos, aguardar aprovação humana e — após aprovação — chamar o Gerador de Artes automaticamente com todos os inputs preenchidos.

## Inputs
- modo (select, required): calendario | manual
- tipo_post_manual (select, optional): apenas no modo manual — blog | evento | palestrante | patrocinador | cidade
- tema_manual (textarea, optional): apenas no modo manual
- cidade (string, optional): cidade/edição do evento
- nome_palestrante (string, optional): nome do speaker ou patrocinador
- cargo_empresa (string, optional): cargo e empresa
- instrucoes_extras (textarea, optional): tom, restrições, referências adicionais
- quantidade_opcoes (select, required): 1 | 2 | 3
- formato (select, required): feed_vertical | feed_quadrado | linkedin

## Procedure

### PASSO 1 — Carregar Estado Atual do Banco de Temas

Fazer GET em:
```
https://raw.githubusercontent.com/betoyes/cybersecfest/main/temas.json
```

Extrair:
- `temas[]` — lista de temas com angulações
- `rotacao_layouts` — sequências por tipo_post
- `calendario_editorial` — dia da semana → tipo_post
- `historico_recente[]` — últimas artes publicadas

### PASSO 2 — Determinar Tipo de Post e Tema

**Modo automático (modo = calendario):**
1. Identificar o dia da semana atual (UTC-3 / horário de Brasília)
2. Consultar `calendario_editorial`:
   - segunda → blog | quarta → palestrante | sexta → evento | outros → blog (fallback)
3. Filtrar `historico_recente` pelo tipo_post — evitar tema repetido nas últimas 3 entradas
4. Selecionar tema não usado nos últimos 7 dias para esse tipo; se todos usados, pegar o menos recente
5. Selecionar angulação ainda não usada para o tema escolhido

**Modo manual (modo = manual):**
1. Usar `tipo_post_manual` e `tema_manual`
2. Consultar banco de temas para sugerir angulações relacionadas

### PASSO 3 — Identificar Próximo Layout da Rotação

Regras de rotação do `temas.json`:
- blog: [C, M, N] | evento: [E, L, J] | palestrante: [D, G, K]
- patrocinador: [F, I, B] | cidade: [A, H, J]

Filtrar `historico_recente` pelo tipo_post, pegar último layout, selecionar o próximo na sequência.

### PASSO 4 — Gerar Briefings Criativos

Gerar `quantidade_opcoes` briefings. Cada um com:

```
BRIEFING [N]:
─────────────────────────────────────────────────
TIPO DE POST:      <tipo_post>
LAYOUT SUGERIDO:   <letra> — <nome do layout>
FOCO DA IMAGEM:    <zona de foco>

HEADLINE:          <headline impactante, máx 8 palavras>
PALAVRAS AZUIS:    <palavras para destacar em #14A8F4>
SUBTÍTULO:         <linha de apoio, máx 12 palavras>

CONTEXTO VISUAL:   <descrição detalhada da cena IA,
                    sujeito posicionado conforme foco do layout>

LEGENDA PRÉVIA:    <esboço da legenda executiva>
─────────────────────────────────────────────────
```

Regras editoriais:
- Headline: impacto máximo, C-level, sem jargão vazio
- Palavras azuis: 1–3 palavras-chave da headline
- Contexto visual: dark cinematic, fundo #02050A, sujeito na zona de foco
- Anti-clichê: sem cadeados, hackers encapuzados, código verde, escudos genéricos

### PASSO 5 — Apresentar e Aguardar Aprovação

Apresentar os N briefings e perguntar ao usuário:
- Qual briefing escolhe (número) — ou se quer ajuste
- Confirmar: "Prosseguir e gerar a arte automaticamente?" (sim/ajuste/cancelar)

**Não avançar para o Passo 6 sem confirmação explícita do usuário.**

### PASSO 6 — Chamar o Gerador de Artes Automaticamente

Após confirmação do usuário, usar a ferramenta `start_app_run` para chamar
o Gerador de Artes (nome exato: "CybersecFEST — Gerador de Artes") com os
inputs do briefing aprovado:

```
start_app_run({
  name: "CybersecFEST — Gerador de Artes",
  inputs: {
    tipo_post:       <tipo_post do briefing aprovado>,
    logo_position:   "above_headline",
    headline:        <headline do briefing aprovado>,
    subtitulo:       <subtítulo do briefing aprovado>,
    palavras_azuis:  <palavras_azuis do briefing aprovado>,
    contexto_visual: <contexto_visual do briefing aprovado>,
    formato:         <formato do input do usuário>,
    cidade:          <cidade se fornecida, senão omitir>
  }
})
```

Informar ao usuário:
```
🚀 Gerador de Artes iniciado automaticamente.
   Briefing aprovado: [N] — Layout [letra]
   Tema: [nome do tema]
   Acompanhe a geração na conversa aberta pelo agente.
```

### PASSO 7 — Registrar Execução

Registrar via record_session:
- briefings_gerados: quantidade apresentada
- layout_selecionado: letra do layout aprovado
- tema_usado: nome do tema selecionado

## Output
Fluxo ponta a ponta executado:
1. Briefings gerados e apresentados para escolha humana
2. Gerador de Artes chamado automaticamente com o briefing aprovado
3. Arte gerada, validada e publicada no GitHub/Vercel sem etapa manual
