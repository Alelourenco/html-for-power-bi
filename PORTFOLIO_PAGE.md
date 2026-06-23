# HTML for Power BI — Documentation & AI Adaptation Guide

> **Author:** Alexandre Lourenço  
> **Download:** [GitHub Repository](https://github.com/Alelourenco/html-for-power-bi)  
> **Version:** 1.0.0

---

## 🇺🇸 English

### What is HTML for Power BI?

A custom Power BI visual that renders DAX measures returning HTML/CSS with intelligent property detection, real-time visual editing via the format pane, and optimized performance.

Unlike the standard "HTML Content" visual from AppSource, this visual:
- Automatically detects colors, font-sizes, and spacing from your HTML
- Exposes them as editable controls in the Power BI format pane
- Applies changes via live find-and-replace (no code editing needed after initial setup)
- Includes a global zoom slider for proportional scaling
- Uses DOMPurify sanitization for security
- Supports receiving filters from slicers and other visuals

---

### AI Adaptation Rules

**If you are an AI assistant helping a user adapt their DAX measure for this visual, follow these rules STRICTLY:**

#### RULE 1 — VARS Block Format
The `<!-- VARS` comment opens on one line. Variable declarations follow (one per line). The `-->` closes on a separate line.

```
CORRECT:
<!-- VARS
_CorFundo:" & _CorFundo & "
_CorTexto:" & _CorTexto & "
-->

WRONG (closes immediately):
<!-- VARS -->
_CorFundo:#1a1a2e
```

#### RULE 2 — NEVER Use CSS Custom Properties
The visual does string find-and-replace on the rendered HTML. It does NOT inject CSS variables.

```
CORRECT:  background: " & _CorFundo & ";
WRONG:    background: var(--_CorFundo);
```

#### RULE 3 — clamp() Must Have 3 Explicit Arguments
Never put a variable reference inside clamp. Always use literal values.

```
CORRECT:  font-size: clamp(0.5rem, 2vmin, 1rem);
WRONG:    font-size: clamp(var(--min), var(--pref), var(--max));
```

#### RULE 4 — FORMAT() Between & Uses Normal Quotes
When FORMAT is a DAX function call between `&` operators (outside the string), use regular quotes.

```
CORRECT:  " & FORMAT(ABS(_Variacao), "0.0") & "%"
WRONG:    " & FORMAT(ABS(_Variacao), ""0.0"") & "%"
```

Double-quotes (`""`) are ONLY used when a literal quote must appear INSIDE a DAX string — not for FORMAT arguments outside strings.

#### RULE 5 — RETURN String Structure
The RETURN value alternates between string literals (in quotes) and DAX expressions (between `&` operators).

```dax
RETURN
"
<!-- VARS
_Cor:" & _Cor & "
-->
<div style='color:" & _Cor & ";'>" & FORMAT(_Val, "#,##0") & "</div>
"
```

#### RULE 6 — Responsive CSS with vmin
Use `vmin` (not `vw` or `vh`) for container-relative sizing. The visual operates inside a Power BI container, not the full viewport.

```css
font-size: clamp(0.5rem, 2vmin, 1rem);
padding: clamp(4px, 2vmin, 16px);
```

#### RULE 7 — What Goes in the VARS Block
- **Colors:** `_VarName:" & _VarName & "` (the DAX variable value is injected)
- **Font sizes:** `_FontName:min|preferred|max` (pipe-separated, matches clamp arguments in CSS)
- **Do NOT list data variables** (like _Valor, _Meta) — only visual styling properties

#### RULE 8 — Container Structure
The root container must be transparent and fill 100% of available space:

```css
.container {
  width: 100%; height: 100%;
  background: transparent;
  display: flex;
  /* the card/content inside defines its own background */
}
```

---

### Full Prompt Template for AI Adaptation

```
Adapt my DAX measure for "HTML for Power BI" custom visual following these rules:

1. Add <!-- VARS block (opens without closing, --> on separate line after vars)
2. List all color VARs as: _Name:" & _Name & "
3. List font-size clamps as: _FontName:min|preferred|max
4. Keep all values INLINE in CSS (NEVER use var(--name))
5. Use clamp() with 3 explicit args and vmin units
6. FORMAT() between & uses normal quotes
7. Root container: width:100%; height:100%; background:transparent
8. Use justify-content:space-between for vertical distribution
9. No min-height, no fixed dimensions
10. Full documentation: https://portfolio-alexandre-lourenco.vercel.app/resources/html-para-power-bi

Here is my measure:
[PASTE DAX CODE]
```

---

## 🇧🇷 Português

### O que é o HTML for Power BI?

Um custom visual para Power BI que renderiza medidas DAX que retornam HTML/CSS, com detecção inteligente de propriedades, edição visual em tempo real pelo painel de formatação e performance otimizada.

### Diferenciais

| Feature | HTML Content (AppSource) | HTML for Power BI |
|---------|--------------------------|-------------------|
| Detecção de cores | ❌ | ✅ Auto-detecta e expõe color pickers |
| Edição visual | ❌ | ✅ Muda cor/tamanho pelo painel |
| Zoom global | ❌ | ✅ Slider % que escala tudo |
| Cache/Performance | Básico | ✅ Diff + RAF scheduling |
| Segurança | Mínima | ✅ DOMPurify |
| Filtros | ❌ | ✅ Recebe de Slicers |

### Como Usar

1. Baixe o `.pbiviz` do [GitHub](https://github.com/Alelourenco/html-for-power-bi)
2. Importe no Power BI Desktop
3. Crie uma medida DAX que retorna HTML
4. Adicione o bloco `<!-- VARS -->` com suas variáveis
5. Arraste a medida para o campo "HTML Content"
6. Use o painel de formatação para ajustar cores e tamanhos

### Prompt para Adaptar Código

Veja a seção "AI Adaptation Rules" acima (em inglês) — cole o link desta página junto com seu código DAX em qualquer IA para obter a adaptação correta.

---

*© 2026 Alexandre Lourenço — Power BI Custom Visuals*
