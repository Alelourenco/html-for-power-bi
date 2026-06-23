# LinkedIn Post — HTML for Power BI

---

## 🇧🇷 Versão PT-BR

---

Novo recurso pra Power BI! 🚀

Em breve vou começar a postar components customizados feitos com DAX, HTML, CSS e SVG. E pensando nisso, antes de sair publicando os visuais, resolvi fazer primeiro um compilador próprio pra renderizar esses components.

A ideia por trás dele é simples: trazer inteligência pro render. Nem todo mundo tem facilidade com programação ou DAX avançado, e eu quero que qualquer pessoa consiga usar esses visuais sem dor de cabeça.

**O que ele faz hoje:**

Ele lê as variáveis (VAR) do código DAX e traz os valores direto no painel de formatação do visual. Então o usuário consegue alterar facilmente por lá:
- Cores (color picker nativo)
- Espaçamento e dimensionamento
- Zoom global proporcional
- Estilo de fonte (dropdown)

Tudo sem precisar mexer em uma linha de código.

Por enquanto o desenvolvimento tem alguns recursos básicos, mas em breve vou trazendo melhorias pra tornar ele cada vez mais low-code pra quem utilizar.

**Pra quem já tem código pronto:**

Esse visual também serve pra códigos de outros desenvolvedores ou seus mesmo. Se você já tem um visual customizado em HTML/DAX, pode adaptar ele pra funcionar com esse compilador usando um prompt que eu criei. É só colar o prompt + seu código em qualquer IA e pronto.

O prompt já pede pra IA ler a documentação completa do visual, que tá no meu site:

📖 https://portfolio-alexandre-lourenco.vercel.app/en/resources/html-para-power-bi

**Prompt pra adaptar seu código:**

```
Adapte a seguinte medida DAX para funcionar com o custom visual "HTML for Power BI".

REGRAS OBRIGATÓRIAS — siga TODAS sem exceção:

1. BLOCO VARS: Adicione <!-- VARS (sem fechar!) no início do RETURN. Cada variável em uma linha como _Nome:" & _Nome & " (cores) ou _FontNome:min|preferred|max (font-sizes). Feche com --> em linha separada. NUNCA escreva <!-- VARS --> na mesma linha.

2. NUNCA USE var(--nome) NO CSS: O visual faz find-and-replace direto no HTML. Os valores devem estar INLINE concatenados com &. Correto: background: " & _CorFundo & "; Errado: background: var(--_CorFundo);

3. CLAMP() COM 3 ARGUMENTOS EXPLÍCITOS: Sempre clamp(0.5rem, 2vmin, 1rem). Nunca clamp(var(--algo)).

4. FORMAT() ENTRE & USA ASPAS NORMAIS: Quando FORMAT está fora da string (entre operadores &), use aspas normais. Correto: " & FORMAT(val, "0.0") & " Errado: " & FORMAT(val, ""0.0"") & "

5. ESTRUTURA DO RETURN: Alterna entre strings (entre aspas) e expressões DAX (entre &). Tudo que é HTML fica dentro de aspas, tudo que é DAX fica fora.

6. CSS RESPONSIVO COM vmin: Use vmin (não vw/vh). Exemplo: font-size: clamp(0.5rem, 2vmin, 1rem); padding: clamp(4px, 2vmin, 16px);

7. O QUE VAI NO BLOCO VARS: Apenas cores e font-sizes de estilo. Cores: _Nome:" & _Nome & " Font-sizes: _Nome:min|preferred|max (correspondendo ao clamp no CSS). NÃO listar variáveis de dados (_Valor, _Meta, etc).

8. CONTAINER RAIZ: width:100%; height:100%; background:transparent. O card interno define seu próprio fundo.

Aqui está minha medida:
[COLE SEU CÓDIGO AQUI]

Retorne a medida DAX completa pronta para colar no Power BI.
```

Download do visual: https://github.com/Alelourenco/html-for-power-bi

#PowerBI #DAX #HTML #CSS #SVG #CustomVisual #DataVisualization #OpenSource #BusinessIntelligence

---
---

## 🇺🇸 English Version

---

New Power BI resource! 🚀

I'll soon start posting custom components built with DAX, HTML, CSS and SVG. But before publishing the visuals themselves, I decided to build a compiler first to render these components.

The idea behind it is simple: bring intelligence to the rendering. Not everyone is comfortable with coding or advanced DAX, and I want anyone to be able to use these visuals without headaches.

**What it does today:**

It reads the DAX variables (VAR) from your code and brings the values directly into the visual's format pane. So the user can easily change:
- Colors (native color picker)
- Spacing and sizing
- Proportional global zoom
- Font style (dropdown)

All without touching a single line of code.

For now the development has some basic features, but I'll keep bringing improvements to make it more and more low-code for everyone.

**For those who already have code:**

This visual also works with other developers' code or your own. If you already have a custom HTML/DAX visual, you can adapt it to work with this compiler using a prompt I created. Just paste the prompt + your code into any AI and you're done.

The prompt tells the AI to read the full documentation first, which lives on my site:

📖 https://portfolio-alexandre-lourenco.vercel.app/en/resources/html-para-power-bi

**Prompt to adapt your code:**

```
Adapt the following DAX measure to work with the custom visual "HTML for Power BI".

MANDATORY RULES — follow ALL without exception:

1. VARS BLOCK: Add <!-- VARS (without closing!) at the beginning of RETURN. Each variable on its own line as _Name:" & _Name & " (colors) or _FontName:min|preferred|max (font-sizes). Close with --> on a separate line. NEVER write <!-- VARS --> on the same line.

2. NEVER USE var(--name) IN CSS: The visual does string find-and-replace on the HTML. Values must be INLINE concatenated with &. Correct: background: " & _BgColor & "; Wrong: background: var(--_BgColor);

3. CLAMP() WITH 3 EXPLICIT ARGUMENTS: Always clamp(0.5rem, 2vmin, 1rem). Never clamp(var(--something)).

4. FORMAT() BETWEEN & USES NORMAL QUOTES: When FORMAT is outside the string (between & operators), use normal quotes. Correct: " & FORMAT(val, "0.0") & " Wrong: " & FORMAT(val, ""0.0"") & "

5. RETURN STRUCTURE: Alternates between strings (in quotes) and DAX expressions (between &). HTML goes inside quotes, DAX goes outside.

6. RESPONSIVE CSS WITH vmin: Use vmin (not vw/vh). Example: font-size: clamp(0.5rem, 2vmin, 1rem); padding: clamp(4px, 2vmin, 16px);

7. WHAT GOES IN THE VARS BLOCK: Only styling colors and font-sizes. Colors: _Name:" & _Name & " Font-sizes: _Name:min|preferred|max (matching the clamp in CSS). Do NOT list data variables (_Value, _Target, etc).

8. ROOT CONTAINER: width:100%; height:100%; background:transparent. The inner card defines its own background.

Here is my measure:
[PASTE YOUR CODE HERE]

Return the complete DAX measure ready to paste in Power BI.
```

Download the visual: https://github.com/Alelourenco/html-for-power-bi

#PowerBI #DAX #HTML #CSS #SVG #CustomVisual #DataVisualization #OpenSource #BusinessIntelligence
