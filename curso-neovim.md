# Curso de Neovim — versão para escrita acadêmica

> **Sobre esta versão.** Este é o material de apoio do vídeo-essay. É uma adaptação de um guia originalmente escrito para programadores, reescrito aqui para quem **escreve prosa**: tese, capítulos, artigos. Não pressupõe nenhuma base de programação. Onde o original falava de código, aqui falamos de parágrafos, frases, citações e revisão de manuscrito.
>
> A tese é simples: **a ferramenta em que você escreve não é neutra.** Ela molda a sua atenção e a sua coragem. Um editor que te deixa navegar e mexer no texto sem fricção é um editor que te deixa escrever com a cabeça mais livre — e, principalmente, mexer no que já está escrito *sem medo*. É disso que este curso trata.

Um guia progressivo do iniciante ao usuário fluente, escrito para quem quer entender *por que* o Vim pode mudar a sua relação com a escrita — não só *como* usá-lo.

**Antes de começar — instalar o Neovim.** Em qualquer sistema:

- **Windows** — `winget install Neovim.Neovim` (ou baixe o instalador em [neovim.io](https://neovim.io)).
- **macOS** — `brew install neovim`.
- **Linux** — pelo gerenciador da distro: `sudo pacman -S neovim` (Arch/CachyOS), `sudo apt install neovim` (Debian/Ubuntu) ou `sudo dnf install neovim` (Fedora).

O Neovim "puro" é minimalista de propósito. Pra escrever sem ter que configurar tudo do zero, vale começar de um kit pronto como o [LazyVim](https://www.lazyvim.org) ou o [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) — assim você já abre num ambiente confortável e ajusta o resto com calma.

---

## Módulo 0 — Por que Vim/Neovim para escrever?

Vim não é "outro editor de texto com atalhos estranhos". É um **modelo de edição** baseado numa ideia simples e profunda: **mexer em texto é uma linguagem**, e você deveria conseguir "conversar" com o seu editor sobre o seu próprio texto.

### A tese central: editar é conversar com o texto

No Word, no Google Docs, ou em qualquer editor convencional, você:

1. Move o cursor com as setas ou o mouse.
2. Seleciona com shift ou arrastando.
3. Aperta Delete, ou Ctrl+X, ou Ctrl+V.

Cada ação é isolada. Você não *compõe* as operações — toda vez recomeça do zero, mira com o mouse, seleciona de novo. Isso parece pouco, mas é exatamente aí que mora boa parte do medo de revisar: mexer dá trabalho, então você deixa o parágrafo ruim como está.

No Vim, cada tecla em modo normal é parte de uma gramática. Os comandos se formam assim:

```
[quantidade] verbo alvo
```

O *verbo* é o que você quer fazer (apagar, mudar, copiar). O *alvo* é sobre o que (uma palavra, uma frase, um parágrafo). Exemplos:

- `dw` → **d**elete **w**ord (apaga até o fim da palavra)
- `cis` → **c**hange **i**nner **s**entence (reescreve a frase inteira em que o cursor está)
- `3dd` → apaga 3 linhas
- `yap` → **y**ank (copia) **a**round **p**aragraph (o parágrafo inteiro)
- `dap` → apaga o parágrafo inteiro, sobra e tudo

Quando você aprende um alvo novo (digamos, "a frase", `is`/`as`), ele **combina automaticamente** com tudo que já sabe: se você já sabia apagar (`d`), agora sabe `das` (apagar a frase). Se já sabia copiar (`y`), sabe `yas`. Não precisa aprender nada novo.

É essa **composição** que falta nos outros editores. Você não está decorando 200 atalhos — está aprendendo uns ~15 verbos, ~15 alvos, e a combinação dá os milhares de comandos. É a mesma economia da própria língua: poucas palavras, infinitas frases.

### Por que isso importa pra quem escreve tese

O ponto não é digitar mais rápido. É que **a distância entre pensar "esse parágrafo devia vir antes" e fazer isso acontecer encolhe quase a zero.** Quando mexer no texto custa caro, você evita mexer — e a tese congela com defeitos que você já viu. Quando mexer é barato, você revisa de verdade. A ferramenta, de novo, não é neutra: ela decide se você vai ter coragem de mexer no que já escreveu.

### Por que Neovim (e não o Vim "clássico")

Neovim é uma versão modernizada do Vim. Para os nossos fins, o que importa é:

1. **Configuração mais simples e um ecossistema de extensões muito melhor** (úteis, por exemplo, para correção ortográfica e completar citações).
2. **Verificação de texto integrada** — dá pra plugar dicionário, corretor e até checagem de gramática, com a mesma tecnologia que editores modernos usam.
3. **Entendimento da estrutura do documento** — ele consegue reconhecer títulos, seções e blocos do seu Markdown/Quarto, o que ajuda a navegar e dobrar seções longas.

Com um pacote pronto como o **LazyVim** (uma configuração de fábrica, pra você não ter que montar tudo na mão), Neovim já vem utilizável desde o primeiro dia.

### Quando Vim *não* é a melhor escolha

Seja honesto com você mesmo:

- Edição colaborativa em tempo real, vários autores no mesmo documento ao mesmo tempo — Google Docs ganha.
- Comentários e controle de alterações no fluxo "orientador comenta, você aceita/rejeita" da forma tradicional — Word ainda é o que a maioria das bancas espera (mas dá pra exportar pra lá no fim).
- Tabelas visuais complexas e diagramação fina — ferramentas gráficas ganham.

A força do Vim é para **escrever e revisar texto corrido como atividade intelectual**. Se o seu trabalho é 80% ler, escrever e remexer prosa estruturada — que é o caso de quem faz tese —, o ganho é enorme e permanente.

---

## Módulo 1 — Os modos

Este é *o* conceito que confunde quem começa. O Vim tem modos porque o teclado não serve só para digitar letras — ele também é um painel de comando.

| Modo | Como entrar | Para quê |
|------|-------------|----------|
| **Normal** | `Esc` (ou `Ctrl-[`) | Navegar pelo texto e operar sobre ele |
| **Insert** | `i`, `a`, `o`, `I`, `A`, `O` | Digitar texto novo |
| **Visual** | `v`, `V`, `Ctrl-v` | Selecionar texto |
| **Command** | `:` | Executar comandos (`:w` salvar, `:q` sair, `:%s/...` substituir) |
| **Replace** | `R` | Sobrescrever letra por letra |

**A regra de ouro:** você passa a maior parte do tempo em **Normal**, lendo e navegando. O modo Insert é só para *digitar texto novo*. Quem começa fica em Insert o tempo inteiro — como se fosse o Word — e perde toda a vantagem. A escrita acadêmica, na prática, é mais revisão do que digitação: você lê, navega, mexe, lê de novo. Logo, você vive em Normal.

Mnemônico para entrar em Insert:
- `i` — **i**nsert antes do cursor
- `a` — **a**ppend depois do cursor
- `I` — insert no começo da linha (no primeiro caractere de verdade)
- `A` — append no fim da linha (ótimo para emendar uma frase)
- `o` — abre uma linha nova abaixo (começar um parágrafo novo)
- `O` — abre uma linha nova acima

---

## Módulo 2 — Movimentos (a base de tudo)

Esquece as setas. Pode usá-las se quiser, mas o objetivo é navegar pelo texto sem tirar as mãos do meio do teclado.

### Movimentos básicos

```
h j k l       — esquerda, baixo, cima, direita
w W           — próxima palavra (W ignora pontuação)
b B           — palavra anterior
e E           — fim da palavra
0             — começo absoluto da linha
^             — primeiro caractere de verdade (pula recuo)
$             — fim da linha
)  (          — próxima / frase anterior
}  {          — próximo / parágrafo anterior
gg            — começo do documento
G             — fim do documento
42G  ou  :42  — vai para a linha 42
Ctrl-d/Ctrl-u — meia tela para baixo/cima
Ctrl-f/Ctrl-b — uma tela inteira
H M L         — topo / meio / fim da parte visível na tela
zz zt zb      — centraliza / leva pro topo / pro fim a linha atual
```

Repara nas estrelas pra quem escreve prosa: `}` e `{` saltam **parágrafo por parágrafo**, e `)` e `(` saltam **frase por frase**. É assim que você percorre um capítulo: não letra por letra, mas pela estrutura do que escreveu.

### Movimentos de busca dentro da linha

```
f{letra}  — pula para a próxima ocorrência de {letra} na linha
F{letra}  — igual, mas para trás
t{letra}  — pula para ANTES de {letra} (útil pra parar em pontuação)
T{letra}  — idem, para trás
;  ,      — repete o último f/t para frente / para trás

/texto    — busca para frente no documento
?texto    — busca para trás
n  N      — próxima / anterior ocorrência
*  #      — busca a palavra que está sob o cursor, pra frente / pra trás
```

**Exercício mental:** "ir até o próximo ponto-final" é `f.`. "Apagar até a próxima vírgula" é `dt,` (apaga até *antes* da vírgula). Repara como o `f` e o `t` **combinam** com os verbos. Quer cortar tudo de onde você está até o parêntese de fecho de uma citação? `dt)`.

### Saltos ("voltar pra onde eu estava")

```
Ctrl-o   — volta para a posição anterior
Ctrl-i   — avança de novo
``       — volta para onde você estava antes do último salto grande
%        — pula entre pares: (), [], {} — útil pra conferir se um parêntese fechou
```

`Ctrl-o` é **mágico** para quem escreve. Você está no meio do capítulo 3, lembra de uma coisa lá na introdução, busca, lê, conserta — e `Ctrl-o` te leva de volta exatamente para o ponto onde você estava escrevendo. Você nunca "se perde" no documento.

---

## Módulo 3 — Verbos e alvos (text objects)

Aqui a gramática do Vim fica realmente poderosa — e é o módulo que mais muda a vida de quem escreve.

### Os verbos principais

```
d  — delete (apaga)
c  — change (apaga e já entra no modo de digitar, pra reescrever)
y  — yank (copia)
p  — paste (cola depois; P cola antes)
>  — recua (indenta)
<  — tira o recuo
~  — inverte maiúscula/minúscula
gu — passa para minúsculas
gU — passa para MAIÚSCULAS
gc — comenta/descomenta (vem no LazyVim; útil pra "desligar" um trecho temporariamente)
```

Cada verbo aceita um movimento ou um alvo como complemento.

### Alvos (a função que vai te ganhar)

Os alvos descrevem **regiões com sentido** dentro do texto. Cada um tem duas variantes:

- `i` = **inner** (só o conteúdo de dentro)
- `a` = **around** (o conteúdo mais os delimitadores em volta)

```
iw / aw  — palavra
is / as  — frase  ← o alvo mais importante pra prosa
ip / ap  — parágrafo
i" / a"  — entre aspas duplas
i' / a'  — entre aspas simples
i( i)    — entre parênteses (i b também funciona)
i[ i]    — entre colchetes  ← onde moram as citações [@autor2020]
i{ i}    — entre chaves
```

Com o entendimento de estrutura que o Neovim tem do seu Markdown/Quarto, alguns alvos extras ficam disponíveis em documentos estruturados — por exemplo, selecionar uma seção inteira ou o conteúdo de uma célula de código. Mas, pra prosa, os de cima já são 95% do que você usa.

### Juntando tudo — exemplos reais de revisão

```
cis       — reescreve a frase inteira em que o cursor está
das       — apaga a frase inteira (incluindo o espaço depois dela)
dap       — apaga o parágrafo inteiro
yap       — copia o parágrafo inteiro (pra mover pra outro lugar)
ci"       — troca o conteúdo entre aspas (corrigir uma citação direta)
ci[       — troca o que está dentro dos colchetes — ex.: a chave da citação
ci(       — reescreve o que está dentro de um parêntese (um aposto, uma glosa)
gUiw      — bota a palavra sob o cursor em MAIÚSCULAS
guis      — joga a frase inteira pra minúsculas
ct,       — reescreve da posição atual até a próxima vírgula
```

Pensa no que `cis` faz: o cursor pode estar em qualquer lugar no meio de uma frase de quatro linhas; você aperta `cis` e a frase inteira some, pronta pra ser redigitada do jeito certo. Não precisou mirar o começo nem o fim com o mouse. Depois que isso vira músculo, voltar pra um editor sem alvos dói fisicamente — e, de novo, é isso que te dá coragem de reescrever em vez de empurrar a frase torta com a barriga.

### O verbo dobrado = a linha inteira

Qualquer verbo dobrado opera na linha: `dd` apaga a linha, `yy` copia a linha, `cc` reescreve a linha, `gcc` comenta a linha, `>>` recua a linha.

### Quantidade

Qualquer comando aceita um número na frente:

```
5j       — desce 5 linhas
3dw      — apaga 3 palavras
dap      — apaga um parágrafo
d}       — apaga daqui até o fim do parágrafo
2dd      — apaga 2 linhas
```

---

## Módulo 4 — Edição eficiente

### Desfazer e refazer (o que te dá segurança pra ousar)

```
u        — desfaz (undo)
Ctrl-r   — refaz (redo)
U        — desfaz todas as mudanças feitas na linha atual
```

Este é, talvez, o módulo mais importante pra coragem de escrever. O Neovim tem um **histórico de desfazer persistente**: ele sobrevive a fechar e reabrir o arquivo. Ou seja, você pode reescrever um parágrafo inteiro de outro jeito *sabendo* que, se não gostar, `u` traz tudo de volta — mesmo amanhã. Com o plugin `undotree` dá até pra ver a árvore de versões e voltar pra qualquer ponto. Reescrever deixa de ser apostar; vira experimentar.

### O ponto `.` — o comando mais subestimado

O `.` repete a última alteração que você fez. Fluxo clássico de revisão:

1. `/teleologico` — busca uma palavra que você escreveu errado a tese inteira
2. `cwteleológico<Esc>` — corrige a primeira (coloca o acento)
3. `n` — vai pra próxima ocorrência
4. `.` — repete a mesma correção
5. `n.n.n.` — vai corrigindo uma a uma, conferindo cada caso

Isso é o "buscar e substituir manual", e muitas vezes é mais seguro que o `:%s/` automático, porque você olha cada ocorrência antes de aplicar — importante quando a substituição depende do contexto.

### Registradores (vários "ctrl+c" ao mesmo tempo)

O Vim tem várias áreas de transferência, chamadas registradores. Usa-se com `"{letra}`:

```
"ayy   — copia a linha para o registrador 'a'
"ap    — cola do registrador 'a'
"+y    — copia para a área de transferência do sistema (pra colar no navegador, no email)
"0p    — cola do último texto copiado (útil quando um apagar sobrescreveu o clipboard)
```

Na prática: você pode "guardar" um parágrafo no registrador `a`, uma citação no `b`, e ir colando cada um onde precisa, sem perder um quando copia o outro.

**Dica prática:** no LazyVim, copiar já costuma estar ligado à área de transferência do sistema (`clipboard=unnamedplus`), então `yy` copia e você cola normalmente em qualquer outro programa. Confere com `:set clipboard?`.

### Macros — repetir uma sequência sem decorar nada

Macros gravam uma sequência de teclas pra você repetir.

```
qa            — começa a gravar no registrador 'a'
...você faz a edição...
q             — para de gravar
@a            — executa a macro
@@            — repete a última macro
10@a          — executa 10 vezes
```

Exemplo acadêmico de verdade: você tem uma lista de referências coladas cruas, uma por linha, e quer transformar cada uma num item de lista do Markdown. Você grava a macro fazendo a edição **numa linha só** (ex.: ir pro começo, inserir `- `, descer pra próxima) e depois `@a` repete pra cada linha seguinte. Numa bibliografia de 80 entradas, `80@a` resolve. É automação sem escrever uma linha de programação.

### Substituir com `:s` (revisão em escala)

```
:s/velho/novo/           — 1ª ocorrência na linha atual
:s/velho/novo/g          — todas na linha atual
:%s/velho/novo/g         — todas no documento inteiro
:%s/velho/novo/gc        — com confirmação a cada uma (o 'c' pergunta sim/não)
:'<,'>s/velho/novo/g     — só no trecho que você selecionou
```

Usos típicos numa tese:

```
:%s/sujeito/participante/gc   — troca um termo no manuscrito inteiro, conferindo caso a caso
:%s/ \./\./g                  — remove espaços antes de ponto-final
:%s/  / /g                    — troca espaços duplos por um só
```

O `c` no fim (confirmação) é o seu amigo: ele para em cada ocorrência e pergunta antes de trocar. Numa tese, raramente você quer trocar *tudo* às cegas.

---

## Módulo 5 — Arquivos, buffers, janelas

Tese boa quase nunca é um arquivo só: é um capítulo por arquivo, mais a bibliografia, mais as notas. Essa hierarquia confunde no começo, mas é simples:

- **Buffer** = um arquivo aberto na memória (um capítulo, por exemplo).
- **Janela (window)** = uma área da tela mostrando um buffer.
- **Aba (tab)** = um conjunto de janelas (não é "arquivo aberto" como no Word!).

Você pode ter 50 buffers abertos (a tese inteira, capítulo por capítulo) e uma janela só. Isso é o normal.

### Buffers

```
:e capitulo3.qmd   — abre o arquivo num novo buffer
:ls                — lista os arquivos abertos
:b 3               — vai para o buffer 3
:b intro           — vai para o buffer cujo nome contém "intro"
:bd                — fecha o buffer atual
Ctrl-^             — alterna entre o buffer atual e o anterior
```

No LazyVim, `<leader>,` (a tecla "leader" é o Espaço, por padrão) abre um seletor visual dos arquivos abertos. `Ctrl-^` é ótimo pra pular entre o capítulo que você escreve e a bibliografia, ida e volta.

### Janelas (dividir a tela)

```
:sp arquivo      — divide a tela na horizontal
:vsp arquivo     — divide na vertical
Ctrl-w h/j/k/l   — pula de uma divisão pra outra
Ctrl-w w         — próxima divisão
Ctrl-w q         — fecha a divisão atual
Ctrl-w =         — iguala os tamanhos
Ctrl-w o         — fecha todas menos a atual
```

Cenário clássico: capítulo de um lado, notas de leitura do outro, lado a lado (`:vsp notas.md`). Ou o texto em cima e a lista de referências embaixo.

### Abas

```
:tabnew       — nova aba
gt / gT       — próxima / anterior
:tabclose     — fecha a aba
```

**Filosofia:** usa **buffers** como os seus arquivos abertos e **janelas** como o layout da tela. Reserva as **abas** para contextos totalmente separados — por exemplo, "a tese" numa aba e "o artigo que você submeteu pra revista" em outra. Quem vem do Word tende a usar aba = arquivo, e isso desperdiça a ferramenta: trocar de buffer com `Ctrl-^` é muito mais rápido.

---

## Módulo 6 — Buscar e navegar pelo material

### Dentro do arquivo

```
/padrão           — busca para frente
?padrão           — para trás
*  #              — a palavra que está sob o cursor
:noh              — apaga o destaque amarelo da última busca
```

A busca é o jeito de "pular" para qualquer ponto: em vez de rolar a tese atrás de onde você mencionou Vigotski, `/Vigotski` te leva direto.

### No projeto inteiro (vários arquivos de uma vez)

No LazyVim, `<leader>` é o `Espaço`. Os principais:

```
<leader>ff   — encontrar arquivos (procura um arquivo do seu projeto pelo nome)
<leader>fg   — buscar texto em TODOS os arquivos de uma vez
<leader>fb   — lista os arquivos abertos
<leader>fr   — arquivos recentes
```

`<leader>fg` é provavelmente o comando mais útil de todos: você digita um termo e ele acha *em qual capítulo* aquilo aparece, mesmo que o arquivo esteja fechado. "Onde foi mesmo que eu defini 'subjetividade'?" — `<leader>fg subjetividade` e pronto, ele lista todos os capítulos e linhas.

### Verificação e auxílio à escrita

O original deste guia falava aqui em "saltos semânticos de código" (ir para a definição de uma função, renomear variáveis em todo o projeto). Para quem escreve, a parte equivalente e útil é a **verificação de texto** — corretor ortográfico, dicionário e completar citações. Isso fica no Módulo 8, reenquadrado para escrita. O resto (análise de código) não se aplica e foi deixado de fora.

---

## Módulo 7 — Comandos essenciais (os que começam com `:`)

Tudo que começa com `:` é um comando. Os que valem decorar:

```
:w                — salva
:w arquivo        — salva com outro nome ("salvar como")
:wa               — salva todos os arquivos abertos
:q                — sai
:qa               — sai de tudo
:q!               — sai SEM salvar (descarta o que você mexeu)
:wq  ou  ZZ       — salva e sai
:x                — salva (se houve mudança) e sai
:e!               — recarrega o arquivo do disco (joga fora o que você mexeu)
:r arquivo        — insere o conteúdo de outro arquivo aqui (ex.: colar um trecho de notas)
```

### Ferramentas externas dentro do texto

Dá pra passar o texto por programas externos sem sair do editor. Isso é mais nichado pra prosa, mas tem usos úteis:

```
:%!fmt              — reflui o texto pra largura confortável de leitura
:'<,'>!sort         — ordena em ordem alfabética as linhas selecionadas (ex.: uma lista de referências)
:'<,'>!sort -u      — ordena e ainda remove duplicatas
```

### Intervalos (faixas de linhas)

```
:10,20d          — apaga as linhas 10 a 20
:.,$d            — apaga daqui até o fim
:%y              — copia o documento inteiro (% = todas as linhas)
:g/TODO/d        — apaga todas as linhas que contêm "TODO"
:g/^$/d          — remove as linhas em branco
```

O `:g/.../` é um superpoder: "em todas as linhas que batem com X, faça Y". Por exemplo, `:g/^>/d` apaga todas as linhas de citação em bloco (que no Markdown começam com `>`), caso você queira limpar rascunhos de citações de uma vez. Use com cuidado — é poderoso e não pergunta.

---

## Módulo 8 — Recursos do Neovim para escrita (ortografia, dicionário, citações)

No guia original, este módulo era sobre ferramentas de programação. Aqui ele vira o módulo de **apoio à escrita** — que é onde o Neovim deixa de ser "só um editor rápido" e vira um ambiente de redação.

### Correção ortográfica (essencial para tese)

Isto é, possivelmente, o recurso que você mais vai usar. Liga o corretor com:

```
:set spell                       — liga a verificação ortográfica
:set spelllang=pt_br             — define o dicionário para português do Brasil
:set spelllang=pt_br,en          — dois idiomas ao mesmo tempo (texto em PT com termos em EN)
```

Com isso ligado, as palavras erradas ficam sublinhadas. E aí:

```
]s        — pula para a próxima palavra com erro
[s        — pula para a anterior
z=        — mostra as sugestões de correção (escolhe pelo número)
zg        — adiciona a palavra ao seu dicionário (ex.: "Foucault", "epistêmico") — para de marcar como erro
zw        — marca uma palavra como errada de propósito
```

Fluxo típico de revisão ortográfica: `]s` pula pro próximo erro, `z=` abre as sugestões, você escolhe; ou, se for um termo técnico/nome próprio correto, `zg` ensina o dicionário a aceitá-lo. Em poucos minutos o seu dicionário pessoal já conhece o vocabulário da sua área.

Vale deixar isso ligado por padrão para arquivos de texto. No LazyVim, dá pra configurar com poucas linhas (ver a parte de Lua abaixo) para que `spell` ligue sozinho ao abrir um `.md` ou `.qmd`.

### Completar palavras e citações

```
Ctrl-n / Ctrl-p   — no modo de digitar, completa a palavra com base no que já existe no texto
```

Isso ajuda com termos longos e repetidos (você digita "fenomenoló" e ele completa "fenomenológico"). Para **citações no estilo Quarto/Pandoc** — aquelas no formato `[@autor2020]` —, existem extensões que leem o seu arquivo de bibliografia (`.bib`) e completam a chave da citação enquanto você digita `@`. É o equivalente, para quem escreve, do "autocompletar" que programadores usam: você digita `@vig` e ele oferece `@vigotski1991`, sem você precisar lembrar a chave exata nem ir conferir no arquivo de referências.

### Gramática e estilo (opcional)

Há extensões que plugam verificadores de gramática e estilo (concordância, repetição, voz passiva em excesso) direto no editor. São opcionais e variam de qualidade por idioma, mas existem para quem quiser. Não são necessárias pra começar.

### Entendimento da estrutura do documento

O Neovim consegue reconhecer a estrutura do seu Markdown/Quarto — títulos, subtítulos, blocos de citação, código. Na prática isso te dá:

1. **Destaque visual correto** dos títulos e ênfases.
2. **Dobrar (folding) seções inteiras** — colapsar o capítulo 2 inteiro numa linha pra enxergar a estrutura geral da tese e reorganizar capítulos. (Comandos de dobra: `za` alterna, `zR` abre tudo, `zM` fecha tudo.)
3. **Navegar por seções** com mais precisão.

### Uma pitada de configuração (Lua)

A configuração do LazyVim fica em `~/.config/nvim/lua/`. Você **não precisa** programar para usar o editor, mas vale saber que ajustes pequenos são fáceis. Por exemplo, ligar o corretor e a quebra de linha suave automaticamente para arquivos de texto:

```lua
-- ~/.config/nvim/lua/config/autocmds.lua
vim.api.nvim_create_autocmd("FileType", {
  pattern = { "markdown", "quarto", "text" },
  callback = function()
    vim.opt_local.spell = true          -- liga o corretor
    vim.opt_local.spelllang = "pt_br"   -- em português
    vim.opt_local.wrap = true           -- quebra de linha na tela
    vim.opt_local.linebreak = true      -- quebra em palavras, não no meio delas
  end,
})
```

Você cola isso uma vez e esquece: toda vez que abrir um capítulo, o editor já vem no modo escrita. É só um exemplo — copiar e colar, sem precisar entender cada linha.

---

## Módulo 9 — Padrões de fluxo para quem escreve

Estes hábitos separam quem "usa Vim devagar" de quem flui.

### 1. Edite com o `.`

Se você se pega fazendo a mesma correção duas vezes com teclas diferentes, perdeu. Refaz até que a segunda correção seja só `n.` (próxima ocorrência, repete).

### 2. `cis` e `cip`, não selecionar com o mouse

Reescrever a frase é `cis`. Reescrever o parágrafo é `cip`. Esquece de mirar começo e fim — deixa o alvo fazer isso. Esses dois comandos sozinhos já transformam a sua revisão.

### 3. Use `*` mais `cgn` para trocar um termo conferindo

`*` busca a palavra sob o cursor. `cgn` reescreve a próxima ocorrência. Então:

1. Põe o cursor sobre o termo (digamos, "indivíduo", que você decidiu trocar por "sujeito").
2. `*` — marca todas as ocorrências.
3. `cgn sujeito <Esc>` — troca a primeira.
4. `.` `.` `.` — nas que você quiser trocar, pulando as que não.

É a substituição com julgamento humano em cada caso — perfeito pra terminologia de tese, onde nem toda ocorrência deve mesmo mudar.

### 4. Gravou-se fazendo algo 3+ vezes? Vira macro.

Se você está fazendo uma edição manual repetitiva 3 ou mais vezes (formatar uma lista de referências, por exemplo), para, grava uma macro, e deixa o `@a` repetir.

### 5. Quebra de linha suave (wrap) e navegar por linha visual

Prosa não tem "linhas" curtas como código; um parágrafo é uma linha longa que a tela quebra. Para isso:

```
:set wrap        — quebra a linha na borda da tela (não rola pro lado)
:set linebreak   — quebra entre palavras, não no meio de uma palavra
gj  gk           — desce/sobe por linha VISUAL (a que você enxerga), não pela linha "real"
g0  g$           — começo/fim da linha visual
```

Sem isso, um parágrafo longo vira uma única linha que some pra fora da tela. Com `wrap`+`linebreak`, o texto se comporta como você espera, e `gj`/`gk` navegam parágrafo longo abaixo/acima de forma natural. Muita gente remapeia `j`/`k` para `gj`/`gk` justamente por isso.

### 6. Marcações (marks) — voltar a um ponto importante

```
ma          — marca 'a' na posição atual
'a          — volta para a linha da marca 'a'
`a          — volta para a posição exata da marca 'a'
:marks      — lista as marcas
```

Útil pra deixar um "aqui eu paro" no capítulo enquanto vai conferir uma referência lá em cima. Marcas maiúsculas (`A`–`Z`) funcionam *entre arquivos* — `'I` poderia ser sempre "o começo da introdução", em qualquer lugar da tese.

### 7. Escrita sem distração

Para focar só no texto, sem barras e enfeites, há extensões de "modo zen" (como o `zen-mode`) que centralizam o parágrafo e escondem o resto da interface. Combina com `wrap`/`linebreak` e cria um ambiente de redação calmo — exatamente o oposto da tela cheia de botões que dispersa a atenção. (Voltamos à tese do vídeo: a ferramenta molda a atenção. Aqui você escolhe ativamente uma tela quieta.)

---

## Módulo 10 — Currículo de prática (como realmente aprender)

Você não aprende isso lendo. Aprende por hábito, sobrepondo uma camada de cada vez. Não tenta aprender tudo numa semana.

### Semana 1–2: sobreviver

Objetivo: escrever no Neovim de verdade, mesmo mais devagar no começo.

- Faça o `vimtutor` no terminal (ou `:Tutor` dentro do Neovim) — 30 minutos, uma vez.
- Ligue o corretor (`:set spell` / `:set spelllang=pt_br`) já no primeiro dia.
- Pratique: `hjkl`, `w b e`, `i a o`, `x`, `dd`, `yy`, `p`, `u` (undo!), `Ctrl-r`, `:w`, `:q`.
- Use `/` para buscar em vez de rolar a tela.

### Semana 3–4: verbos e alvos

- Pratique `cis`, `cip`, `das`, `dap`, `ciw`, `daw`.
- Pratique `ci"`, `ci(`, `ci[` (este último é pra mexer dentro de `[@citações]`).
- Force o uso de `f` e `t` pra andar dentro de uma frase.
- Aprenda `gg`, `G`, `:42`, `Ctrl-d/u` pra se mover pelo documento.

### Semana 5–6: fluxo de projeto

- O seletor de arquivos: `<leader>ff` e `<leader>fg` viram reflexo.
- Trabalhe com buffers, não abas. `Ctrl-^` pra alternar entre capítulo e bibliografia.
- Divisões de tela: `Ctrl-w v` (texto e notas lado a lado).
- Domine a revisão ortográfica: `]s`, `z=`, `zg`.

### Mês 2: automação e revisão em escala

- Macros em tarefas repetitivas reais (formatar referências, por exemplo).
- `:%s/.../gc` (com confirmação) e `:g/.../` para limpar e padronizar o manuscrito.
- O fluxo `*` → `cgn` → `.` para trocar terminologia com julgamento.

### Mês 3: deixar do seu jeito

- Adicione o autocmd que liga `spell` + `wrap` sozinho nos arquivos de texto (Módulo 8).
- Experimente uma extensão de completar citações a partir do seu `.bib`.
- Experimente o "modo zen" para escrever sem distração.

### Recursos que valem o tempo

- **Vimtutor** — 30 minutos, o básico absoluto (`:Tutor` dentro do Neovim).
- **LazyVim docs** — <https://www.lazyvim.org/> — sobre a configuração de fábrica que você usa.
- **Neovim :help** — `:help` + tópico. Surpreendentemente bom. Para o corretor: `:help spell`.
- **Documentação do Quarto** — <https://quarto.org/> — sobre escrever em texto puro com citações, referências e exportar pra PDF/Word.
- **Practical Vim** (Drew Neil) — o livro de referência. Pensado pra código, mas a parte de movimentos e edição vale para qualquer texto.

---

## Módulo 11 — O que você ganha no longo prazo

Depois de uns meses de uso real, três coisas mudam:

1. **Velocidade deixa de ser o ponto.** O ganho de digitação é modesto. O ganho grande é que **a fricção entre pensar e mexer no texto some**. Você pensa "esse parágrafo devia abrir o capítulo" e os seus dedos já fizeram. E aí — voltando à tese do vídeo — você *revisa de verdade*, porque mexer parou de doer. A coragem de reescrever vem da facilidade de reescrever.

2. **A escrita fica menos assustadora.** O `undo` persistente e os alvos te dão uma rede de segurança: nenhuma reescrita é definitiva, nenhum corte é irreversível. Você para de defender parágrafos ruins só porque mexer dá trabalho. O texto fica vivo, manipulável — um rascunho de verdade, não um monumento.

3. **A tela fica quieta.** Texto puro, sem a interface barulhenta de um processador de texto, com o corretor discreto e o modo zen quando você quer. A ferramenta para de competir pela sua atenção e passa a sustentá-la. É isso que queremos dizer com "escrever com a cabeça mais livre": menos coisa entre você e a frase.

Nenhuma dessas três é sobre ser programador. São sobre a relação entre quem escreve e o instrumento. A ferramenta não é neutra — então vale escolher uma que te deixe corajoso.

---

## Apêndice — "Cola" rápida

### Os comandos para o primeiro mês (foco em prosa)

```
i I a A o O          — entrar no modo de digitar
Esc                  — sair do modo de digitar
h j k l              — mover
w b e                — palavras
) (   } {            — frases / parágrafos
0 ^ $                — começo/fim da linha
gg G                 — começo/fim do documento
gj gk                — descer/subir por linha visual (prosa com wrap)
f{c} t{c} ; ,        — busca dentro da linha
/ ? n N              — busca no documento
* #                  — palavra sob o cursor
dd yy cc             — operações de linha
cis das              — frase: reescrever / apagar
cip dap yap          — parágrafo: reescrever / apagar / copiar
ciw diw              — palavra
ci" ci( ci[          — dentro de aspas / parênteses / colchetes (citações)
p P                  — colar
u Ctrl-r             — desfazer / refazer
.                    — repetir a última edição
v V                  — seleção visual
:w :q :wq :q!        — salvar / sair
:%s/a/b/gc           — substituir no documento (com confirmação)
:set spell           — ligar o corretor ortográfico
]s z= zg             — próximo erro / sugestões / aprender palavra
```

Cola isso num papel ao lado da tela até virar reflexo. Depois esquece o papel e adiciona mais.
