# HTML for Power BI — Documentation & AI Adaptation Guide

> **Author:** Alexandre Lourenço  
> **Download:** [GitHub Repository](https://github.com/Alelourenco/html-for-power-bi)  
> **Version:** 1.0.0

---

## 🇺🇸 English

### What is HTML for Power BI?

A custom visual that compiles DAX measures returning HTML, CSS and SVG. It reads your DAX variables and brings them into the format pane so anyone can adjust colors, sizes and spacing without editing code.

### Download & Install

1. Download the `.pbiviz` from [GitHub](https://github.com/Alelourenco/html-for-power-bi)
2. Power BI Desktop → **...** → **Import a visual from a file**
3. Select the `.pbiviz`

### How It Works

You write a DAX measure that returns an HTML string. The visual renders it and detects colors, font sizes and other properties automatically from the output.

To get the best results, add a `<!-- VARS` block at the beginning of your HTML. This tells the visual exactly which properties to expose in the format pane with their names.

---

### AI Adaptation Rules

**If you are an AI assistant adapting a DAX measure for this visual, follow ALL rules below. Do not skip any.**

#### RULE 1 — VARS Block Format
```
CORRECT:
<!-- VARS
_ColorName:" & _ColorName & "
_FontTitle:0.5rem|2vmin|1rem
-->

WRONG (closes immediately — variables won't be detected):
<!-- VARS -->
_ColorName:#1a1a2e
```

The block opens with `<!-- VARS` on its own line. Each variable goes on a separate line as `_Name:value`. The block closes with `-->` on a separate line.

For colors, use inline DAX concatenation: `_Name:" & _DaxVariable & "`  
For font sizes, use pipe format: `_Name:min|preferred|max` matching a `clamp(min, preferred, max)` in the CSS.

#### RULE 2 — NEVER Use CSS Custom Properties
The visual does string find-and-replace on the rendered HTML. It does NOT inject CSS variables.

```
CORRECT:  background: " & _BgColor & ";
WRONG:    background: var(--_BgColor);
```

#### RULE 3 — clamp() Needs 3 Explicit Arguments
```
CORRECT:  font-size: clamp(0.5rem, 2vmin, 1rem);
WRONG:    font-size: clamp(var(--min), var(--pref), var(--max));
```

#### RULE 4 — FORMAT() Between & Uses Normal Quotes
When FORMAT is a DAX function call between `&` operators (outside the string), use regular quotes.

```
CORRECT:  " & FORMAT(ABS(_Value), "0.0") & "%"
WRONG:    " & FORMAT(ABS(_Value), ""0.0"") & "%"
```

Double-quotes (`""`) are ONLY for literal quotes INSIDE a DAX string — never for FORMAT arguments outside strings.

#### RULE 5 — RETURN String Structure
The RETURN alternates between string literals (in quotes) and DAX expressions (between `&`).

```dax
RETURN
"
<!-- VARS
_Color:" & _Color & "
-->
<div style='color:" & _Color & ";'>" & FORMAT(_Val, "#,##0") & "</div>
"
```

#### RULE 6 — Responsive CSS
Use `vmin` (not `vw` or `vh`) for container-relative sizing. The visual lives inside a Power BI container, not the full browser viewport.

```css
font-size: clamp(0.5rem, 2vmin, 1rem);
padding: clamp(4px, 2vmin, 16px);
```

#### RULE 7 — What Goes in the VARS Block
- **Colors:** `_VarName:" & _VarName & "`
- **Font sizes:** `_FontName:min|preferred|max`
- **Do NOT list data variables** (like _Total, _Target) — only styling properties

#### RULE 8 — Container Structure
Root must be transparent and fill 100%:
```css
.container { width: 100%; height: 100%; background: transparent; }
```
The card/content inside defines its own background.

---

### Prompt Template

```
Read the full documentation for "HTML for Power BI" at this link before making any changes:
https://portfolio-alexandre-lourenco.vercel.app/en/resources/html-para-power-bi

Now adapt the following DAX measure to work with this visual, following all the rules from the documentation.

Here is my measure:
[PASTE YOUR CODE HERE]
```

---

## 🇧🇷 Português

### O que é o HTML for Power BI?

Um custom visual que compila medidas DAX que retornam HTML, CSS e SVG. Ele lê suas variáveis DAX e traz os valores no painel de formatação, permitindo que qualquer pessoa ajuste cores, tamanhos e espaçamentos sem editar código.

### Download e Instalação

1. Baixe o `.pbiviz` do [GitHub](https://github.com/Alelourenco/html-for-power-bi)
2. Power BI Desktop → **...** → **Importar um visual de um arquivo**
3. Selecione o `.pbiviz`

### Como Funciona

Você escreve uma medida DAX que retorna uma string HTML. O visual renderiza e detecta cores, font-sizes e outras propriedades automaticamente.

Para melhores resultados, adicione um bloco `<!-- VARS` no início do HTML. Isso diz ao visual exatamente quais propriedades expor no painel com seus nomes.

---

### Regras para Adaptar Código (IAs devem seguir)

**Se você é uma IA adaptando uma medida DAX para este visual, siga TODAS as regras abaixo.**

#### REGRA 1 — Bloco VARS
```
CORRETO:
<!-- VARS
_CorFundo:" & _CorFundo & "
_FontTitulo:0.5rem|2vmin|1rem
-->

ERRADO (fecha na mesma linha — variáveis não serão detectadas):
<!-- VARS -->
_CorFundo:#1a1a2e
```

#### REGRA 2 — Nunca Usar CSS Custom Properties
```
CORRETO:  background: " & _CorFundo & ";
ERRADO:   background: var(--_CorFundo);
```

#### REGRA 3 — clamp() com 3 Argumentos Explícitos
```
CORRETO:  font-size: clamp(0.5rem, 2vmin, 1rem);
ERRADO:   font-size: clamp(var(--algo));
```

#### REGRA 4 — FORMAT() entre & Usa Aspas Normais
```
CORRETO:  " & FORMAT(ABS(_Valor), "0.0") & "%"
ERRADO:   " & FORMAT(ABS(_Valor), ""0.0"") & "%"
```

#### REGRA 5 — Estrutura do RETURN
Alterna entre strings (entre aspas) e expressões DAX (entre `&`).

#### REGRA 6 — CSS Responsivo com vmin
Use `vmin` em vez de `vw`/`vh` para que as dimensões respondam ao container do visual.

#### REGRA 7 — O que vai no Bloco VARS
- Cores: `_Nome:" & _Nome & "`
- Font sizes: `_Nome:min|preferred|max`
- NÃO listar variáveis de dados

#### REGRA 8 — Container Transparente
Container raiz: `width:100%; height:100%; background:transparent`

---

### Prompt para Adaptar

```
Leia a documentação completa do visual "HTML for Power BI" neste link antes de fazer qualquer alteração:
https://portfolio-alexandre-lourenco.vercel.app/en/resources/html-para-power-bi

Agora adapte a seguinte medida DAX para funcionar com esse visual, seguindo todas as regras da documentação.

Aqui está minha medida:
[COLE SEU CÓDIGO AQUI]
```

---

*© 2026 Alexandre Lourenço — Power BI Custom Visuals*
