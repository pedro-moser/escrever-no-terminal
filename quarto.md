# Quarto para Escrita Acadêmica — Escrevendo a Tese em Texto Puro

Este guia é para quem escreve prosa de verdade — tese, dissertação, artigo — e nunca programou na vida. A ideia é simples: você escreve o texto inteiro num arquivo de **texto puro**, sem caixinhas nem menus, e um comando transforma isso num PDF formatado de forma profissional. A ferramenta se chama **Quarto**, e o arquivo tem extensão `.qmd`.

A tese por trás de tudo: **a ferramenta em que você escreve não é neutra**. Um editor cheio de botões, barras e formatação no meio do caminho disputa a sua atenção o tempo todo. Quando o texto vira só texto, a cabeça fica mais livre para pensar — e, de quebra, você ganha coragem, porque para de ter medo de "estragar a formatação" e passa a mexer no que importa: as ideias.

---

## 1. O que é Quarto, e por que texto puro?

Pensa no Word. Quando você deixa uma palavra em itálico, o programa guarda isso num emaranhado invisível de instruções. Você não vê o que está acontecendo por baixo — confia que está. Quando dá ruim (a numeração desanda, a fonte muda sozinha, a referência some), você fica refém de um sistema que não consegue inspecionar.

Texto puro é o contrário. O arquivo `.qmd` é só caracteres — as mesmas letras que você lê. Para deixar uma palavra em itálico, você escreve `*assim*`. Tudo é visível, tudo é seu. Você abre o arquivo daqui a dez anos, em qualquer computador, e ele continua lá, legível.

O fluxo é este:

1. Você escreve no `.qmd` usando **Markdown** (uma forma leve de marcar texto — vamos ver já já) mais um **cabeçalho** no topo.
2. Roda um comando no terminal:

```bash
quarto render tese.qmd --to pdf
```

3. O Quarto gera `tese.pdf`, formatado, paginado, com referências montadas, sumário, tudo.

Por baixo, o Quarto usa o **LaTeX** (o sistema que a academia usa há décadas para produzir PDFs lindos). Mas você **nunca encosta no LaTeX**. Ele fica escondido, fazendo o trabalho pesado, enquanto você só escreve. Essa é a graça: a beleza tipográfica do LaTeX sem a dor de cabeça do LaTeX.

> Quarto não é "coisa de programador". É um processador de texto que decidiu ser honesto sobre o que está fazendo.

---

## 2. O cabeçalho: dizendo ao Quarto o que você quer

Todo `.qmd` começa com um pequeno bloco entre `---`. Ele se chama **cabeçalho YAML** (não se assuste com o nome — é só uma lista de configurações no formato `chave: valor`). É aqui que você define título, autor, idioma e o formato de saída.

```yaml
---
title: "A escuta clínica na era da atenção fragmentada"
author: "Seu Nome"
lang: pt
format:
  pdf:
    geometry: margin=2.5cm
bibliography: referencias.bib
csl: apa.csl
---
```

Lendo linha por linha:

- `title` e `author` — o que vai na capa.
- `lang: pt` — diz ao Quarto que o texto é em português (afeta hifenização, nomes de seções como "Sumário", etc.).
- `format: pdf:` — você quer um PDF. (Daria também `docx` para Word ou `html` para web, com o mesmo arquivo.)
- `geometry: margin=2.5cm` — as margens da página.
- `bibliography: referencias.bib` — o arquivo onde ficam suas referências (veremos na seção 4).
- `csl: apa.csl` — o estilo de citação (APA, ABNT, etc.).

O recuo (os espaços antes de `pdf:` e `geometry:`) **importa** no YAML — é assim que ele entende que `pdf` está "dentro" de `format`. Use dois espaços por nível e mantenha a consistência.

---

## 3. Markdown: escrevendo o texto

Markdown é a forma de marcar o texto. A filosofia é que a marcação pareça natural mesmo antes de virar PDF — um asterisco já "sente" como ênfase.

### Títulos e seções

```markdown
# Introdução
## Revisão de literatura
### Um subtópico
```

Um `#` é capítulo/seção de primeiro nível, `##` é subseção, `###` desce mais um nível. O Quarto numera tudo e monta o sumário sozinho a partir disso.

### Ênfase e listas

```markdown
Texto em *itálico* e em **negrito**.

Lista com marcadores:

- primeiro item
- segundo item

Lista numerada:

1. primeiro
2. segundo
```

### Notas de rodapé

```markdown
Isso merece um comentário à parte.[^1]

[^1]: O comentário vem aqui, e vira nota de rodapé no PDF.
```

E é basicamente isso para o texto corrido. Você escreve como quem escreve um e-mail caprichado — sem nunca tirar a mão do teclado para clicar em botão de formatação. A formatação fina (fonte, espaçamento, numeração) é o Quarto que resolve, igual para o documento inteiro.

---

## 4. Citações: a parte que encanta

Aqui está o motivo número um pelo qual pesquisador nenhum volta para o Word depois de provar isto.

Você mantém suas referências num arquivo à parte — tradicionalmente `referencias.bib` (formato **BibTeX**). Cada referência tem uma **chave** que você inventa, tipo `silva2020`. Gerenciadores como o Zotero exportam esse `.bib` para você com um clique, então na prática você nem digita isso à mão.

No texto, você cita pela chave:

| Você escreve | Vira no PDF |
|---|---|
| `[@silva2020]` | (Silva, 2020) |
| `@silva2020` | Silva (2020) |
| `[@almeida2019; @braga2020]` | (Almeida, 2019; Braga, 2020) |

A diferença entre `[@silva2020]` e `@silva2020` é exatamente a diferença entre "...como já se demonstrou (Silva, 2020)" e "Silva (2020) demonstrou que...". O colchete decide se o nome entra no parêntese ou na frase.

E o melhor: **a lista de referências se monta sozinha no fim**, só com as obras que você de fato citou, no estilo certo, em ordem alfabética. Para colocá-la, você adiciona ao final do documento:

```markdown
## Referências

::: {#refs}
:::
```

Aquele `::: {#refs} :::` é o lugar onde o Quarto vai despejar a bibliografia formatada. Você escreve isso uma vez e nunca mais pensa nisso.

**Nunca mais formatar referência à mão.** Acrescentou uma citação no meio do texto? A lista no fim já a inclui, na posição alfabética certa, sozinha.

E aqui está o ponto que muda tudo: **mudar de estilo é trocar uma linha.** Cada orientador, cada revista, cada programa exige um formato diferente — e isso deixou de ser um problema. No cabeçalho, o `csl:` decide o estilo de citação do documento inteiro:

```yaml
csl: apa.csl                   # APA — comum em psicologia
# csl: abnt.csl                # ABNT — o padrão brasileiro
# csl: vancouver.csl           # Vancouver — saúde e medicina
# csl: chicago-author-date.csl # Chicago — humanas
# csl: ieee.csl                # IEEE — engenharia
```

Troca a linha, roda de novo, e as citações **e** a lista inteira se reformatam — não importa se são 10 ou 300 referências. Existem arquivos `.csl` prontos para praticamente toda revista do mundo (o repositório oficial do CSL tem milhares). Para APA com todos os detalhes que os periódicos exigem, há ainda a extensão **apaquarto**, que cuida do estilo inteiro. Formatar bibliografia, que já foi um inferno manual, virou escolher um item de menu.

---

## 5. Figuras e tabelas que se numeram sozinhas

Outra dor que some. Você nunca mais escreve "como mostra a Figura 3" e descobre, depois de reorganizar o texto, que agora ela é a Figura 5.

Você dá um **rótulo** à figura (começando com `fig-`) e referencia por esse rótulo:

```markdown
![Distribuição das respostas por faixa etária](grafico.png){#fig-idades}

Como se observa na @fig-idades, a tendência é clara.
```

O `@fig-idades` vira "Figura 1" (ou o número que for, na hora) automaticamente. Tabelas funcionam igual, com o prefixo `tbl-` e referência via `@tbl-x`. Reorganizou os capítulos? Os números se reajustam todos no próximo render. Você nunca mais conta figura na mão.

---

## 6. Código que roda: o pulo do gato da reprodutibilidade

Até aqui falamos de escrever. Agora o passo que muda o jogo — e que conecta com a ideia de **ciência aberta**.

Você pode colocar, dentro do mesmo `.qmd`, um pedacinho de análise que **roda de verdade na hora do render** e cospe o gráfico ou a tabela direto no PDF. O texto, os dados e a análise passam a viver no mesmo arquivo.

Um bloco de código fica assim (este é Python; também funciona com R):

````markdown
```{python}
#| label: fig-disp
#| fig-cap: "Relação entre horas de sono e escore de ansiedade"

import matplotlib.pyplot as plt
plt.scatter(dados["sono"], dados["ansiedade"])
plt.xlabel("Horas de sono")
plt.ylabel("Escore de ansiedade")
plt.show()
```
````

As linhas que começam com `#|` são **opções** do bloco:

- `#| label: fig-disp` — dá um rótulo, para você poder escrever `@fig-disp` no texto e referenciar este gráfico como faria com qualquer figura.
- `#| fig-cap: "..."` — a legenda que aparece embaixo.

Quando você roda `quarto render`, esse código executa, gera o gráfico, e ele entra no PDF já numerado e legendado. **Se o dado mudar, o gráfico muda junto** — você não exporta imagem, não cola, não atualiza nada à mão. Roda de novo, está atualizado.

Para tabelas, a lógica é a mesma: um bloco que **devolve uma tabela em Markdown** vira tabela no PDF. Em Python, um jeito comum é converter um quadro de dados e mandar exibir como Markdown:

````markdown
```{python}
#| label: tbl-resumo
#| tbl-cap: "Estatísticas descritivas por grupo"

from IPython.display import Markdown
Markdown(resumo.to_markdown(index=False))
```
````

Aqui `resumo` é um `DataFrame` do pandas; `.to_markdown()` o transforma em tabela de texto, e `Markdown(...)` pede ao Quarto que a renderize como tabela de verdade.

Para rodar Python desse jeito, você precisa do **jupyter** instalado (é o motor que executa o código). Se preferir R, o caminho é análogo com o ecossistema do R. O ponto não é a linguagem — é que **a análise deixa de ser uma caixa-preta**. Qualquer pessoa abre o seu `.qmd` e vê, lado a lado, a afirmação e a conta que a sustenta.

---

## 7. Diagramas como código

Para esquemas, fluxogramas e modelos teóricos, dá para desenhar com texto também — sem abrir programa de desenho. Um bloco `dot` (linguagem do Graphviz) vira uma figura:

````markdown
```{dot}
digraph {
  "Estímulo" -> "Percepção"
  "Percepção" -> "Resposta"
  "Resposta" -> "Estímulo"
}
```
````

Isso desenha três caixas ligadas por setas, automaticamente. A vantagem é a mesma de tudo aqui: o diagrama é texto, versionável, editável, e renderiza igual em qualquer máquina. Para isso funcionar, é preciso ter o **graphviz** instalado.

---

## 8. O que instalar (sem susto)

Você não instala "um ambiente de desenvolvimento". São quatro coisinhas, e só as duas primeiras são obrigatórias:

1. **Quarto** — o programa principal. Baixe o instalador em [quarto.org](https://quarto.org/docs/get-started/): tem versão para **Windows** (`.msi`), **macOS** (`.pkg`) e **Linux** (`.deb`/`.rpm`), e instala como qualquer aplicativo. (Quem usa gerenciador de pacotes: `winget install Posit.Quarto` no Windows, `brew install --cask quarto` no macOS.)
2. **Um motor LaTeX** — para gerar PDF. O Quarto oferece um caminho indolor: rode uma vez

   ```bash
   quarto install tinytex
   ```

   e ele instala o **TinyTeX**, uma versão enxuta do LaTeX, sozinho. Você não configura nada.
3. **jupyter** — só se você for rodar código Python (seção 6). Com o Python instalado, em qualquer sistema: `pip install jupyter`. (Python: [python.org](https://python.org) no Windows, `brew install python` no macOS, já costuma vir no Linux.)
4. **graphviz** — só se você for desenhar diagramas `dot` (seção 7). **Windows:** `winget install Graphviz.Graphviz`; **macOS:** `brew install graphviz`; **Linux:** `sudo apt install graphviz` ou `sudo pacman -S graphviz`.

Para checar se está tudo de pé, há um comando que faz o diagnóstico:

```bash
quarto check
```

Ele diz o que encontrou e o que falta. Se reclamar de algo, geralmente é uma das quatro coisas acima.

---

## 9. Gerando o PDF

Com o arquivo escrito e as ferramentas instaladas, o documento nasce com um comando:

```bash
quarto render tese.qmd --to pdf
```

Na primeira vez pode demorar um pouco (o LaTeX se aquecendo, o código rodando). Da segunda em diante é rápido. O resultado é `tese.pdf` na mesma pasta.

Enquanto escreve, vale usar o modo de pré-visualização, que reconstrói o documento sozinho cada vez que você salva:

```bash
quarto preview tese.qmd
```

Você escreve no editor de um lado, vê o PDF se atualizar do outro. Salvou, mudou.

---

## 10. O fechamento: por que isso importa

Repara no que você acabou de montar. Num único arquivo de texto puro convivem:

- a **prosa** da sua tese,
- as **referências**, que se formatam e se ordenam sozinhas,
- os **dados e as análises**, que rodam e produzem os gráficos e tabelas na hora,
- os **diagramas**, escritos como texto.

Isso é um trabalho **aberto**. Não é uma caixa-preta que pede para o leitor confiar — é algo **conferível** e **reproduzível**. Outra pessoa (ou você, daqui a três anos) abre o arquivo, roda `quarto render`, e obtém exatamente o mesmo PDF, com exatamente os mesmos números, porque a conta está ali, não num arquivo de Excel perdido numa pasta qualquer. Quando texto, dados e análises vivem juntos num formato aberto, a ciência para de ser performance e volta a ser método.

E há o ganho mais silencioso, que é também o mais importante: a **paz**. A paz de não costurar formatação à mão, de não ter medo de mexer no documento, de saber que o arquivo vai abrir daqui a dez anos. A ferramenta saiu da frente, e sobrou cabeça para a única coisa que era para estar ali: pensar e escrever.

Foi disso que falamos no começo. A ferramenta não é neutra. Escolha uma que te deixe livre.
