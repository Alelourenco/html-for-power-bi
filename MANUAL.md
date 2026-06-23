# ⚡ HTML for Power BI - Manual de Uso

## O que é?

**HTML for Power BI** é um custom visual que compila medidas DAX que retornam HTML/CSS, renderizando visuais personalizados diretamente no Power BI. Diferente do "HTML Content" do AppSource, ele oferece:

- 🎨 **Detecção inteligente de variáveis** (cores, fontes, espaçamentos)
- 🔄 **Edição em tempo real** via painel de formatação (substitui valores no HTML)
- 🔍 **Zoom global** para ajustar proporção em qualquer tamanho
- 📐 **Responsividade nativa** — adapta ao container do Power BI
- 🔒 **Sanitização DOMPurify** — seguro contra XSS
- ⚡ **Cache inteligente** — só re-renderiza quando o conteúdo muda
- 🎯 **Recebe filtros** de Slicers e outros visuais

---

## Como Instalar

1. No Power BI Desktop, vá em **... (Mais opções)** → **Importar um visual de um arquivo**
2. Selecione o arquivo `htmlForPowerBI_A1B2C3D4E5F6.1.0.0.0.pbiviz`
3. O visual aparece na barra de visualizações com o ícone ⚡

---

## Como Usar

### 1. Crie uma Medida DAX que retorna HTML

```dax
MeuVisual = 
    VAR _Valor = SUM(Tabela[Receita])
    VAR _Cor = "#6366f1"
RETURN
"<div style='color:" & _Cor & ";font-size:2rem;font-weight:900;'>" & FORMAT(_Valor, "R$ #,##0") & "</div>"
```

### 2. Arraste a Medida para o Visual

- Adicione o visual **HTML for Power BI** na página
- Arraste sua medida para o campo **"HTML Content"**

### 3. (Opcional) Conecte um campo de filtro

- Arraste um campo (ex: `Mes`, `Categoria`) para o slot **"Filter Field"**
- Isso permite que Slicers e outros visuais filtrem o seu visual

---

## Bloco `<!-- VARS -->` — Detecção Inteligente

Para que o painel de formatação reconheça suas variáveis e permita edição visual, adicione um bloco de comentário HTML no início do RETURN:

```dax
RETURN
"
<!-- VARS
_NomeDaVar:valor
_OutraVar:valor
-->
<div>...seu HTML...</div>
"
```

### Tipos suportados:

| Tipo | Formato no VARS | Exemplo | Aparece como |
|------|----------------|---------|-------------|
| Cor (hex) | `_NomeVar:#hex` | `_CorFundo:#1a1a2e` | 🎨 Color Picker |
| Cor (rgba) | `_NomeVar:rgba(...)` | `_CorCard:rgba(15,23,42,0.85)` | 🎨 Color Picker |
| Font Size | `_NomeVar:min\|preferred\|max` | `_FontTitulo:0.5rem\|2.2vmin\|1rem` | 📐 NumUpDown (vmin) |
| Texto | `_NomeVar:texto` | `_FontFamily:Segoe UI` | ✏️ Text Input |

### Exemplo completo:

```dax
MeuKPI = 
    VAR _Valor = SUM(Vendas[Total])
    VAR _Meta = 1000000
    VAR _CorPrincipal = "#6366f1"
    VAR _CorFundo = "#0f172a"
    VAR _CorTexto = "#f8fafc"

RETURN
"
<!-- VARS
_CorPrincipal:" & _CorPrincipal & "
_CorFundo:" & _CorFundo & "
_CorTexto:" & _CorTexto & "
_FontValor:1rem|5vmin|2.5rem
-->
<div style='background:" & _CorFundo & ";color:" & _CorTexto & ";padding:1em;height:100%;display:flex;align-items:center;justify-content:center;border-radius:12px;'>
  <span style='font-size:clamp(1rem, 5vmin, 2.5rem);font-weight:900;color:" & _CorPrincipal & ";'>" & FORMAT(_Valor, "R$ #,##0") & "</span>
</div>
"
```

---

## Painel de Formatação

### 🎨 Colors (detected)
Mostra todas as cores detectadas com seus nomes de variável. Mude a cor → o visual substitui automaticamente no HTML.

### 📐 Font Sizes (detected)
Mostra os tamanhos de fonte declarados no formato `min|preferred|max`. O número exibido é o valor `vmin` (o "preferred" do clamp). Aumente para fontes maiores.

### ⚡ Rendering
| Controle | Função |
|----------|--------|
| Enable Animations | Liga/desliga todas as animações CSS |
| Enable Hover | Liga/desliga efeitos hover |
| Auto Scale | Escala conteúdo proporcionalmente ao container |
| 🔤 Zoom Global (%) | Multiplica o tamanho de tudo (100=normal, 150=50% maior) |
| 🔠 Font Family | Dropdown para trocar a fonte globalmente |

---

## Dicas de Performance

1. **Evite medidas pesadas** — o visual re-renderiza a cada mudança de filtro
2. **Use o bloco VARS** — a detecção explícita é mais rápida que o scan automático
3. **Desabilite animações** se tiver muitos visuais HTML na mesma página
4. **Use Zoom Global** em vez de ajustar cada font-size individualmente

---

## Dicas de Responsividade

- Use `vmin` nos `clamp()` — escala com a menor dimensão do container
- Use `justify-content: space-between` para distribuir conteúdo verticalmente
- Evite `min-height` fixo — impede o visual de encolher
- Use `white-space: nowrap; overflow: hidden; text-overflow: ellipsis` para textos que podem não caber

---

## Recebendo Filtros

O visual reage automaticamente a filtros quando:
1. A medida DAX referencia colunas do modelo (`SUM(Tabela[Campo])`)
2. Um campo está no slot "Filter Field"

Não é necessário nenhum código extra — o Power BI recalcula a medida com o novo contexto de filtro.

---

## Limitações Conhecidas

- O preenchimento (padding) padrão do Power BI precisa ser zerado manualmente em **Geral → Preenchimento → 0**
- O título do visual precisa ser desligado em **Geral → Título → Off**
- Não emite cross-filter para outros visuais (apenas recebe)
- Máximo de 12 cores e 12 tamanhos detectáveis no painel

---

## Prompt para Adaptar seu Código

Use o prompt abaixo com IA (Copilot, ChatGPT, Claude) para adaptar qualquer medida DAX existente ao formato do HTML for Power BI:

---

### 🤖 Prompt de Adaptação

```
Preciso adaptar a seguinte medida DAX para funcionar com o custom visual "HTML for Power BI".

⚠️ REGRAS OBRIGATÓRIAS (NÃO VIOLAR):

REGRA 1 — BLOCO VARS:
- O bloco abre com <!-- VARS (SEM fechar na mesma linha)
- Cada variável em uma linha: _NomeVar:" & _NomeVar & "
- Fecha com --> em linha separada
- CORRETO:
  <!-- VARS
  _CorFundo:" & _CorFundo & "
  _CorTexto:" & _CorTexto & "
  -->
- ERRADO: <!-- VARS --> (fecha imediatamente!)

REGRA 2 — NUNCA USAR CSS CUSTOM PROPERTIES:
- NÃO USE var(--qualquerCoisa) no CSS
- Os valores devem estar INLINE concatenados do DAX com &
- CORRETO: background: " & _CorFundo & ";
- ERRADO: background: var(--_CorFundo);

REGRA 3 — CLAMP() SEMPRE COM 3 ARGUMENTOS EXPLÍCITOS:
- CORRETO: font-size: clamp(0.5rem, 2vmin, 1rem);
- ERRADO: clamp(var(--algo))
- ERRADO: clamp(var(--min), var(--pref), var(--max))

REGRA 4 — FORMAT() ENTRE & USA ASPAS NORMAIS:
- Quando FORMAT está FORA da string (entre operadores &), use aspas normais
- CORRETO: " & FORMAT(valor, "0.0") & "%
- ERRADO: " & FORMAT(valor, ""0.0"") & "%
- Aspas dobradas ("") só são usadas quando FORMAT está DENTRO de uma string DAX

REGRA 5 — ESTRUTURA DO RETURN:
- O RETURN é uma concatenação de strings com &
- Tudo que é DAX (variáveis, funções) fica FORA das aspas
- Tudo que é HTML/CSS fica DENTRO das aspas
- Exemplo: "<div style='color:" & _Cor & ";'>" & FORMAT(_Val, "0") & "</div>"

REGRA 6 — CSS RESPONSIVO:
- Use vmin (não vw/vh) — vmin é relativo ao container
- Container raiz: width:100%; height:100%; background:transparent
- Flexbox com justify-content:space-between para distribuir verticalmente
- Sem min-height fixo
- white-space:nowrap; overflow:hidden; text-overflow:ellipsis em textos

REGRA 7 — VARS NO BLOCO:
- Cores: listar com valor inline: _CorNome:" & _CorNome & "
- Font sizes: formato fixo com pipes: _FontNome:min|preferred|max
  (corresponde ao clamp(min, preferred, max) no CSS)
- Só listar variáveis de COR e FONT no bloco VARS (não dados)

Aqui está minha medida DAX atual:

[COLE SUA MEDIDA AQUI]

Adapte mantendo o visual idêntico mas compatível com o formato do HTML for Power BI.
Retorne a medida DAX completa pronta para colar no Power BI.
```

---

**Tags:** #PowerBI #CustomVisual #DAX #HTML #CSS #DataVisualization
