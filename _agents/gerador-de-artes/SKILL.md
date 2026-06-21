# CybersecFEST — Gerador de Artes

## Goal
Gerar posts completos para o CybersecFEST: imagem IA + HTML final + legenda executiva + upload automático no GitHub (betoyes/cybersecfest), com rotação inteligente de layouts, score de legenda e validação em duas etapas para rastreamento de erros.

## Inputs
- tipo_post (select, required): blog | evento | palestrante | patrocinador | cidade
- logo_position (select, required): above_headline | top_left
- headline (textarea, required): headline principal do post
- subtitulo (textarea, optional): subtítulo ou linha de apoio
- palavras_azuis (string, optional): palavras da headline para destacar em azul #14A8F4
- nome_palestrante (string, optional): nome completo (palestrante/patrocinador)
- cargo_empresa (string, optional): cargo e empresa
- contexto_visual (textarea, required): descrição da cena/imagem a gerar
- referencias (files, optional): imagens de referência
- formato (select, required): feed_vertical (1080x1350) | feed_quadrado (1080x1080) | linkedin (1200x628)
- cidade (string, optional): cidade do evento em caixa alta
- categoria_patrocinador (string, optional): categoria/cota do patrocinador

## Procedure

### PASSO -1 — Pré-Validação (EXECUTAR ANTES DE QUALQUER COISA)

Antes de iniciar a geração, validar o estado do ecossistema. Se qualquer item falhar, **parar e reportar o erro** — não prosseguir.

**1. Validar temas.json:**
```
GET https://raw.githubusercontent.com/betoyes/cybersecfest/main/temas.json
```
Verificar:
- [ ] HTTP 200 — arquivo acessível
- [ ] JSON válido e parseável
- [ ] Campo `rotacao_layouts` presente e com chave para `tipo_post` atual
- [ ] Campo `historico_recente` presente (pode ser array vazio)
- [ ] Campo `calendario_editorial` presente

**2. Validar artes.json:**
```
GET https://raw.githubusercontent.com/betoyes/cybersecfest/main/artes.json
```
Verificar:
- [ ] HTTP 200 ou 404 aceitável (primeiro uso)
- [ ] Se existir: JSON válido e array parseável
- [ ] Se existir: nenhum registro duplicado de slug nas últimas 3 entradas

**3. Reportar resultado da pré-validação:**

Se tudo OK:
```
✅ PRÉ-VALIDAÇÃO OK
   temas.json: acessível (N temas, M entradas no histórico)
   artes.json: acessível (N artes registradas)
   Prosseguindo para geração...
```

Se houver erro, parar e exibir:
```
❌ PRÉ-VALIDAÇÃO FALHOU
   Erro: [descrição exata do erro]
   Arquivo: [temas.json | artes.json]
   Detalhe: [HTTP status | campo ausente | JSON inválido]
   Ação necessária: [instrução clara para o usuário corrigir]
```

---

### PASSO 0 — Rotação Inteligente de Layout

Usando os dados já carregados na pré-validação:

**Regras de rotação por tipo_post:**
- blog:        [C, M, N] → ciclo de 3
- evento:      [E, L, J] → ciclo de 3
- palestrante: [D, G, K] → ciclo de 3
- patrocinador:[F, I, B] → ciclo de 3
- cidade:      [A, H, J] → ciclo de 3

**Algoritmo:**
1. Filtrar `historico_recente` pelo `tipo_post` atual
2. Pegar o último layout usado para esse tipo
3. Selecionar o próximo na sequência de rotação
4. Se não houver histórico, usar o primeiro da sequência
5. Se o usuário especificar layout via override no contexto_visual, respeitar o override

**Registrar uso:** após publicar, atualizar `historico_recente` em `temas.json`:
```json
{
  "tipo_post": "<tipo>",
  "layout": "<letra>",
  "slug": "<slug-gerado>",
  "data": "<ISO-8601>"
}
```
Manter no máximo os últimos 20 registros.

---

### PASSO 1 — Seleção e Montagem do Layout

**14 layouts disponíveis (A–N):**

| Layout | Nome | Auto-seleção | Foco da Imagem |
|--------|------|-------------|----------------|
| A | Banda Superior | cidade (rotação) | DIREITA |
| B | Mirror Split | patrocinador (rotação) | ESQUERDA |
| C | Subtítulo ao Lado | blog (rotação) | DIREITA |
| D | Diagonal | palestrante (rotação) | CENTRO/DIREITA |
| E | CTA Pill | evento (rotação) | DIREITA |
| F | Coluna Lateral Sólida | patrocinador (rotação) | DIREITA |
| G | Magazine Cover | override | CENTRO |
| H | Rodapé Luminoso | cidade (rotação) | CENTRAL-SUPERIOR |
| I | Coluna Sólida Direita | patrocinador (rotação) | ESQUERDA |
| J | 3 Blocos | evento/cidade (rotação) | CENTRO entre horizontais |
| K | Tríptico | palestrante (rotação) | CENTRO entre verticais |
| L | L Invertido + Traços | evento (rotação) | CENTRO entre zonas |
| M | Pull Quote | blog (rotação) | DIREITA |
| N | Acento Diagonal | blog (rotação) | DIREITA |

**Lei de Foco da Imagem:** A imagem IA deve ser gerada com o sujeito principal na zona de foco do layout, garantindo que o texto não sobreponha o sujeito.

---

### PASSO 2 — Geração da Imagem IA

Construir prompt seguindo o foco do layout:
- Posição do sujeito conforme foco (ex: "subject on the right third")
- Estilo: dark cinematic, fundo #02050A, high contrast
- Resolução: 1080×1350 (feed_vertical) | 1080×1080 (feed_quadrado) | 1200×628 (linkedin)
- Incorporar `contexto_visual`
- Se `referencias` fornecidas, usar como referência de estilo

---

### PASSO 3 — Geração do HTML (arte.html)

**Design system fixo:**
- Fundo: `#02050A` | Azul: `#14A8F4` | Branco: `#F6F8FF` | Lavanda: `#D5D8ED` | Muted: `#94A0B8`
- Headlines: Ubuntu 700 | Subtítulos: Montserrat 400 (nunca itálico)
- Logo CybersecFEST: colorido, **NUNCA** filter CSS
- Logos ecossistema: `filter: brightness(0) invert(1)`

Estrutura do `arte.html`:
```
.art-canvas (540×675px feed_vertical)
  ├── .art-bg (imagem base64)
  ├── .art-overlay (gradiente)
  ├── .art-content (logo + headline + subtítulo)
  ├── .ecosystem (logos ecossistema)
  └── .toolbar (botão print)
```

**Palavras azuis:** `<span style="color:#14A8F4">palavra</span>`

---

### PASSO 4 — Geração da Legenda (Score ≥ 7/10)

Gerar legenda executiva com:
- Gancho na 1ª frase, parágrafos curtos, CTA claro
- 5–8 hashtags (#CybersecFEST #cibersegurança + contextuais)
- Máximo 3 emojis estratégicos

**Score de Legenda:**
1. Gancho (0–2) | 2. Clareza executiva (0–2) | 3. CTA (0–2) | 4. Hashtags (0–2) | 5. Fluidez (0–2)

Se score < 7 → reescrever automaticamente antes de prosseguir.

---

### PASSO 5 — Slug e Index Individual

Slug: `{tipo_post}-{timestamp}` (ex: `blog-1718901234567`)

Criar `artes/{slug}/index.html` com embed do arte.html + metadados + link para galeria.

---

### PASSO 6 — Upload GitHub

Upload via GitHub API (GITHUB_TOKEN) de:
1. `artes/{slug}/arte.html`
2. `artes/{slug}/thumb.png` (base64)
3. `artes/{slug}/index.html`

Registrar o SHA retornado pela API para cada arquivo (usado na pós-validação).

Atualizar `artes.json` adicionando:
```json
{
  "slug": "...",
  "tipo": "...",
  "headline": "...",
  "palavras_azuis": "...",
  "subtitulo": "...",
  "cidade": "...",
  "formato": "...",
  "layout": "...",
  "legenda": "...",
  "image_path": "artes/{slug}/thumb.png",
  "html_path": "artes/{slug}/arte.html",
  "created_at": "ISO-8601"
}
```

---

### PASSO 7 — Atualizar temas.json (Rotação)

Atualizar `historico_recente` em `temas.json`:
- Adicionar entrada: tipo_post, layout, slug, data
- Truncar para máximo 20 entradas

Registrar SHA retornado pela API.

---

### PASSO 8 — Pós-Validação (EXECUTAR APÓS TODOS OS UPLOADS)

Verificar integridade de cada operação realizada.

**1. Confirmar uploads dos arquivos da arte:**
Para cada arquivo enviado (arte.html, thumb.png, index.html), verificar:
- [ ] API retornou status 201 (created) ou 200 (updated)
- [ ] SHA retornado é não-nulo e tem 40 caracteres
- [ ] Path retornado corresponde ao path enviado

**2. Confirmar atualização de artes.json:**
```
GET https://raw.githubusercontent.com/betoyes/cybersecfest/main/artes.json
```
Verificar:
- [ ] Novo slug presente no array
- [ ] Campo `layout` preenchido no novo registro (não nulo, não vazio)
- [ ] Campo `legenda` preenchido
- [ ] `created_at` com data de hoje

**3. Confirmar atualização de temas.json:**
```
GET https://raw.githubusercontent.com/betoyes/cybersecfest/main/temas.json
```
Verificar:
- [ ] Nova entrada em `historico_recente` com slug, layout e data corretos
- [ ] Total de entradas ≤ 20

**4. Reportar resultado da pós-validação:**

Se tudo OK:
```
✅ PÓS-VALIDAÇÃO OK
   arte.html:   SHA confirmado ✓
   thumb.png:   SHA confirmado ✓
   index.html:  SHA confirmado ✓
   artes.json:  novo registro com layout="X" ✓
   temas.json:  historico_recente atualizado (N entradas) ✓
```

Se houver falha parcial:
```
⚠️ PÓS-VALIDAÇÃO COM ALERTAS
   [✓] arte.html: OK
   [✗] artes.json: campo `layout` ausente no novo registro
       → Ação: atualizar manualmente o registro "{slug}" em artes.json adicionando "layout": "X"
   [✓] temas.json: OK
```

Se upload falhou completamente:
```
❌ PÓS-VALIDAÇÃO FALHOU — PUBLICAÇÃO NÃO CONFIRMADA
   Erro: [descrição]
   SHA esperado: não retornado
   Possíveis causas: rate limit da GitHub API | GITHUB_TOKEN expirado | conflito de SHA
   Ação: verificar em github.com/betoyes/cybersecfest se os arquivos foram criados
```

---

### PASSO 9 — Output Final

Apresentar ao usuário:
1. **Status de validação** (pré + pós em bloco compacto)
2. **Preview da arte** (link Vercel)
3. **Legenda completa** + score breakdown
4. **Layout utilizado** + próximo layout na fila
5. **Links:** arte individual, galeria, thumb

---

## Output
Post completo publicado com rastreabilidade total:
- `artes/{slug}/arte.html` + `thumb.png` + `index.html`
- `artes.json` com campo `layout` e `legenda`
- `temas.json` com histórico de rotação atualizado
- Relatório de validação (pré + pós) em cada execução
