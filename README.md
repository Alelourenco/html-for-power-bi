# ⚡ HTML for Power BI

Custom Visual para Power BI que compila medidas DAX em HTML/CSS com inteligência de detecção automática de variáveis, edição visual em tempo real e performance otimizada.

## 📥 Download

**[⬇️ Baixar o .pbiviz](dist/htmlForPowerBI_A1B2C3D4E5F6.1.0.0.0.pbiviz)**

> Clique no link acima, depois em "Download raw file" para baixar o arquivo de instalação.

---

## 🚀 Instalação

1. Baixe o arquivo `.pbiviz` acima
2. No Power BI Desktop: **... (Mais opções)** → **Importar um visual de um arquivo**
3. Selecione o `.pbiviz` baixado
4. O visual aparece na barra de visualizações

---

## 💡 O que faz?

Renderiza qualquer HTML/CSS gerado por uma medida DAX diretamente no Power BI, com:

| Feature | Descrição |
|---------|-----------|
| 🎨 **Detecção de cores** | Reconhece cores no HTML e expõe color pickers no painel |
| 📐 **Detecção de font-sizes** | Reconhece clamp() e expõe controles de tamanho |
| 🔄 **Edição em tempo real** | Muda cor/tamanho no painel → substitui no HTML automaticamente |
| 🔍 **Zoom Global** | Slider % que escala tudo proporcionalmente (fontes, barras, padding) |
| 🔠 **Font Family** | Dropdown para trocar a fonte globalmente |
| ⚡ **Cache inteligente** | Só re-renderiza quando o conteúdo muda |
| 🔒 **Seguro (DOMPurify)** | Sanitização contra XSS |
| 🎯 **Recebe filtros** | Reage a Slicers e cross-filter de outros visuais |
| 📱 **Responsivo** | Adapta a qualquer tamanho de container |

---

## 📝 Como Usar

### 1. Crie uma medida DAX que retorna HTML

```dax
MeuCard = 
    VAR _Valor = SUM(Tabela[Receita])
    VAR _Cor = "#6366f1"
RETURN
"
<!-- VARS
_Cor:" & _Cor & "
-->
<div style='background:#1a1a2e;color:#fff;padding:1em;height:100%;
            display:flex;align-items:center;justify-content:center;
            border-radius:12px;'>
  <span style='font-size:clamp(1rem,5vmin,2.5rem);font-weight:900;
               color:" & _Cor & ";'>" & FORMAT(_Valor, "R$ #,##0") & "</span>
</div>
"
```

### 2. Arraste a medida para o campo "HTML Content" do visual

### 3. (Opcional) Arraste um campo para "Filter Field" para conectar a Slicers

---

## 🎯 Bloco `<!-- VARS -->` — Detecção Inteligente

Para que o painel reconheça suas variáveis, adicione um bloco de metadados no início do HTML:

```
<!-- VARS
_CorFundo:" & _CorFundo & "
_CorTexto:" & _CorTexto & "
_FontValor:0.9rem|5vmin|2.5rem
-->
```

**Regras:**
- Abre com `<!-- VARS` (NÃO fechar na mesma linha!)
- Uma variável por linha: `_Nome:valor`
- Fecha com `-->` em linha separada
- Cores: `_Nome:" & _VarDAX & "` (valor inline do DAX)
- Font sizes: `_Nome:min|preferred|max` (formato pipe para clamp)

---

## ⚠️ Regras Importantes para o DAX

1. **NUNCA use `var(--nome)` no CSS** — o visual faz find-and-replace direto, não injeta CSS variables
2. **Valores sempre inline** no CSS: `background: " & _CorFundo & ";`
3. **`clamp()` com 3 argumentos explícitos**: `clamp(0.5rem, 2vmin, 1rem)`
4. **FORMAT() entre `&` usa aspas normais**: `" & FORMAT(val, "0.0") & "`
5. **Container raiz**: `width:100%; height:100%; background:transparent`

---

## 🔧 Painel de Formatação

| Card | Controles |
|------|-----------|
| 🎨 Colors | Color pickers para cada cor detectada (com nome da VAR) |
| 📐 Font Sizes | NumUpDown para cada font-size (valor vmin do clamp) |
| ⚡ Rendering | Animations on/off, Hover on/off, Auto Scale, Zoom %, Font Family |

---

## 🤖 Prompt para Adaptar seu Código

Veja o arquivo [MANUAL.md](MANUAL.md) para o prompt completo que você pode usar com IA (Copilot, ChatGPT, Claude) para adaptar qualquer medida DAX ao formato deste visual.

---

## 📄 Licença

MIT

---

## 📚 Full Documentation & AI Integration Guide

**🔗 [https://portfolio-alexandre-lourenco.vercel.app/resources/html-para-power-bi](https://portfolio-alexandre-lourenco.vercel.app/resources/html-para-power-bi)**

For AI assistants (Copilot, ChatGPT, Claude): visit the link above to read the complete technical documentation with adaptation rules, prompt templates, and code examples.

---

*Created by [Alexandre Lourenço](https://portfolio-alexandre-lourenco.vercel.app)*
