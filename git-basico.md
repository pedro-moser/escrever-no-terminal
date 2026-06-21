# Git para Escrita Acadêmica — Versão para quem escreve a tese

> Esta é uma adaptação do guia *Git Básico* pensada para pesquisadores e pesquisadoras — gente de psicologia, humanas, gente que escreve tese e artigo, e que nunca programou na vida. Não tem pré-requisito nenhum. Se a palavra "Git" já te assustou alguma vez, respira: aqui ele é só uma ferramenta de paz e de coragem para a sua escrita.

O Git é um programa que guarda versões do seu trabalho ao longo do tempo. Pensa assim: **toda vez que você termina uma etapa, o Git tira uma foto completa do seu documento naquele instante e guarda pra sempre, com um bilhetinho seu dizendo o que mudou.** Essas fotos ficam todas ali, intactas, sempre que você precisar. É isso que ele faz, no fundo — e é isso que devolve duas coisas pra quem escreve: a **paz** de saber que nenhuma versão se perde, e a **liberdade** de mexer, apagar e reescrever sem medo, porque dá pra voltar.

---

## 1. Instalação e configuração inicial

Primeiro, instala o Git. Escolhe a sua plataforma:

- **Windows** — baixa o instalador em [git-scm.com/download/win](https://git-scm.com/download/win) (ou, se você usa o gerenciador de pacotes do Windows, `winget install Git.Git`). Ele já vem com um terminal próprio, o **Git Bash**, onde você roda os comandos deste guia.
- **macOS** — no Terminal, `brew install git` (se você tem o [Homebrew](https://brew.sh)). Sem Homebrew, rodar `xcode-select --install` também instala o Git.
- **Linux** — pelo gerenciador da sua distro: `sudo pacman -S git` (Arch/CachyOS), `sudo apt install git` (Debian/Ubuntu) ou `sudo dnf install git` (Fedora).

Antes de qualquer coisa, diz pro Git quem é você. Esse nome e e-mail vão assinar cada foto que você tirar — é o seu jeito de dizer "fui eu que escrevi isso":

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"
git config --global init.defaultBranch main
```

Pra conferir o que ficou configurado:

```bash
git config --list
```

Dica: se você escreve num editor específico (Word não conta aqui — o Git trabalha com arquivos de texto), dá pra dizer ao Git qual programa abrir quando ele precisar te pedir um bilhete. Por exemplo, com o Neovim:

```bash
git config --global core.editor nvim
```

---

## 2. As três etapas de tirar uma foto

Essa é a ideia que confunde no começo, mas é simples. Antes de uma foto ficar guardada de verdade, o seu documento passa por três lugares:

1. **A pasta de trabalho** — o seu documento como você o vê e edita agora, com as mudanças do dia.
2. **A área de preparação** — onde você separa o que vai entrar na próxima foto. É como escolher quais páginas vão pro álbum.
3. **O histórico guardado** — onde as fotos ficam para sempre, cada uma com o seu bilhete.

O caminho é sempre o mesmo: escrever → escolher o que entra na foto → tirar a foto (com o bilhete).

```
escrever na pasta  --git add-->  preparar a foto  --git commit-->  guardar para sempre
```

---

## 3. Começando a versionar a tese

Pra dizer ao Git "passa a cuidar desta pasta aqui, onde mora a minha tese":

```bash
cd minha-tese
git init
```

Se a sua tese já está guardada num servidor (GitHub, GitLab, o da universidade) e você quer trazer uma cópia pro seu computador:

```bash
git clone https://github.com/usuario/minha-tese.git
```

---

## 4. O dia a dia: tirar fotos da sua escrita

### Ver como está a pasta agora

```bash
git status
```

Mostra o que você mexeu desde a última foto, o que já está separado pra entrar na próxima e o que o Git ainda nem está acompanhando.

### Escolher o que entra na foto

```bash
git add introducao.md        # separa um documento específico
git add .                    # separa tudo que mudou na pasta
git add -p                   # vai te mostrando pedaço por pedaço, você decide
```

O `git add -p` é ótimo quando você mexeu em várias coisas ao mesmo tempo — uma correção na introdução, uma ideia nova no método — e quer guardar cada coisa na sua própria foto, com seu próprio bilhete.

### Tirar a foto (com o bilhete)

```bash
git commit -m "primeiro rascunho da introdução"
```

O texto entre aspas é o **bilhete**: uma frase sua dizendo o que aconteceu naquela etapa. Escreve do jeito que faria sentido pra você daqui a seis meses. No espírito de:

```bash
git commit -m "hoje reescrevi a introdução"
git commit -m "apliquei as correções do orientador"
git commit -m "quase desisti deste capítulo e recomecei"
```

Se você não puser o `-m` com a frase, o Git abre o seu editor pra você escrever o bilhete com calma. Pra ver tudo que mudou enquanto escreve o bilhete:

```bash
git commit -v
```

### Ver o histórico de fotos

```bash
git log                      # o histórico completo, com data e bilhete
git log --oneline            # uma linha por foto: enxuto, fácil de percorrer
git log --oneline --graph    # mostra também os caminhos paralelos (ver seção 6)
```

Isso é a história da sua tese contada pelos seus próprios bilhetes — o dia em que você reescreveu a introdução, o dia das correções, o dia em que quase desistiu e recomeçou.

---

## 5. Voltar no tempo, sem medo

Essa é a parte que mais assusta no começo, e é justamente onde o Git te dá a maior liberdade. Lembra: **nenhuma versão se perde.** Tudo que você já guardou continua a um comando de distância.

### Desistir das mudanças de hoje (que ainda não viraram foto)

```bash
git restore introducao.md          # joga fora o que você mexeu hoje neste arquivo e volta à última foto
git restore --staged introducao.md # tira o arquivo da preparação, mas mantém o que você escreveu
```

### Corrigir a última foto

Se você esqueceu de incluir um arquivo ou escreveu o bilhete errado:

```bash
git commit --amend
```

Use isso **só enquanto a foto ainda não foi enviada** pro servidor (ver seção 7).

### Olhar uma versão antiga da tese

Quer ver como estava a introdução três semanas atrás, antes de você reescrever tudo? Dá pra abrir aquela versão antiga e olhar à vontade:

```bash
git log --oneline                # acha o código (uns números e letras) da foto que você quer
git checkout <código-da-foto>    # abre a tese como ela estava naquele dia
git switch -                     # volta pro presente quando terminar de olhar
```

Você não estraga nada fazendo isso — é só uma viagem no tempo pra olhar. Quando voltar pro presente, a sua tese de hoje está lá inteira, do jeito que você deixou.

---

## 6. Experimentar numa ramificação, sem estragar o tronco

Às vezes você quer testar uma ideia ousada — reorganizar um capítulo inteiro, mudar a direção do argumento — mas não quer arriscar o texto principal, que já está de pé. Pra isso existem as **ramificações** (branches): linhas paralelas onde você experimenta à vontade. O texto principal, o "tronco", normalmente se chama `main`. **O que prestar, você junta de volta ao tronco; o que não prestar, você abandona sem prejuízo nenhum.**

```bash
git branch                       # lista as ramificações que existem
git branch reescrita-capitulo3   # cria uma ramificação pra experimentar
git switch reescrita-capitulo3   # entra nela pra trabalhar
git switch -c outra-ideia        # cria e já entra, num comando só
```

### Juntar o que prestou de volta ao tronco

Estando de volta no tronco que vai *receber* o experimento:

```bash
git switch main
git merge reescrita-capitulo3
```

### Abandonar uma ramificação

```bash
git branch -d reescrita-capitulo3   # apaga uma ramificação que já foi juntada ao tronco
git branch -D reescrita-capitulo3   # descarta um experimento que você decidiu não usar
```

---

## 7. Guardar uma cópia fora do seu computador

Um "remoto" é uma cópia da sua tese em outro lugar — um servidor como o GitHub, o GitLab, ou um da universidade. Serve de cópia de segurança e, mais adiante, de janela pra outras pessoas (ver a seção sobre ciência aberta).

```bash
git remote -v                    # mostra onde tem cópia guardada lá fora
git remote add origin <endereço> # diz onde fica essa cópia (apelidada de "origin")
```

### Enviar e trazer fotos

```bash
git push origin main             # envia as suas fotos pro servidor
git push -u origin main          # mesma coisa, mas memoriza pra próxima vez
git pull                         # traz pro seu computador o que mudou no servidor
git fetch                        # só dá uma olhada no que mudou, sem mexer no seu
```

Depois daquele `-u` na primeira vez, você pode usar só `git push` e `git pull`.

---

## 8. Dizer ao Git o que ignorar

Na pasta da tese vão aparecer arquivos que não fazem parte da escrita — os PDFs gerados, arquivos temporários do sistema, rascunhos que você não quer versionar. Você cria um arquivo chamado `.gitignore` na raiz da pasta e lista o que o Git deve fingir que não existe. Por exemplo:

```
# PDFs e saídas geradas (o documento se gera de novo a partir do texto)
*.pdf
_book/
*.log

# Arquivos temporários de editor
*.swp
*~

# Coisas do sistema
.DS_Store

# Material que não vai pra cópia pública
dados-sensiveis/
```

Importante: o `.gitignore` só funciona para arquivos que o Git **ainda não está acompanhando**. Se um arquivo já entrou numa foto antes, primeiro você tira ele de cena com `git rm --cached arquivo` (isso não apaga o arquivo do seu computador, só pede pro Git parar de acompanhá-lo).

---

## 9. Ver o que mudou

```bash
git diff                         # o que você mexeu hoje e ainda não preparou
git diff --staged               # o que já está preparado pra próxima foto
git diff main..reescrita-capitulo3   # a diferença entre o tronco e um experimento
```

É a forma de comparar, linha a linha, duas versões do seu texto — o que entrou, o que saiu.

---

## 10. Socorros para os apertos do dia a dia

Algumas situações que sempre acontecem quando se está escrevendo:

| Situação | Solução |
|----------|---------|
| "Tirei a foto com o bilhete errado" | `git commit --amend` (se ainda não enviou pro servidor) |
| "Quero jogar fora tudo de hoje e voltar à última foto" | `git reset --hard HEAD` |
| "Preciso parar no meio de uma ideia sem tirar foto ainda" | `git stash` pra guardar de lado, depois `git stash pop` pra retomar |
| "Apaguei um parágrafo sem querer" | `git restore arquivo.md` |
| "Quero saber em que foto este trecho foi escrito" | `git blame arquivo.md` |
| "Fiz uma bagunça, quero voltar uma hora atrás" | `git reflog` mostra todos os seus passos recentes |

O `git reflog` é o seu salva-vidas: o Git guarda registro de quase tudo por uns 30 dias, mesmo coisas que pareceram perdidas. Na prática, é muito difícil perder trabalho de verdade.

---

## 11. Um dia inteiro de escrita, do começo ao fim

```bash
# Começar a versionar a tese
mkdir minha-tese && cd minha-tese
git init
echo "# Minha Tese" > tese.md

# Primeira foto
git add tese.md
git commit -m "primeiro rascunho da introdução"

# Experimentar uma reescrita ousada, sem mexer no tronco
git switch -c reescrita-introducao
# ...reescreve a introdução à vontade...
git add .
git commit -m "hoje reescrevi a introdução"

# Gostou? Junta de volta ao tronco
git switch main
git merge reescrita-introducao

# Aplicar o que o orientador pediu e guardar
git add .
git commit -m "apliquei as correções do orientador"

# Mandar uma cópia pro servidor da universidade
git remote add origin git@servidor.universidade:seu-usuario/minha-tese.git
git push -u origin main
```

Repara que a pasta continua **limpa, com um arquivo só** — a versão de agora. Acabou o cemitério de `tese_final_v2_AGORA.docx`. Todo o passado fica guardado, completo, mas fora do seu campo de visão. Você trabalha sempre no presente, com a mesa arrumada, e mesmo assim a história inteira está lá, contada pelos seus bilhetes.

---

## Por que isso também é ciência aberta

Tem um sentido a mais nessa sequência de fotos com bilhetes, que vale a pena perceber. Ela não serve só pra você. É um **registro de como o seu trabalho foi feito, passo a passo** — quando a introdução nasceu, quando entraram as correções, o que mudou e por quê.

Numa época em que a ciência (a psicologia em especial) tenta ser mais transparente e mais fácil de conferir, esse histórico vira uma janela. O que era uma rede de segurança só sua passa a ser algo que outra pessoa pode reabrir, percorrer e verificar. No fim das contas, isso não é só conforto — é **confiança**. Um pedacinho de como se faz uma ciência em que dá pra acreditar.

---

## Próximos passos

Quando o básico já estiver fluindo, tem mais coisa pra explorar com calma — mas nada disso é necessário pra começar a escrever com paz e liberdade:

- **Marcar versões importantes** (`git tag`) — pra fixar pontos como "versão entregue à banca" ou "artigo submetido".
- **Trazer uma foto específica** de uma ramificação pra outra (`git cherry-pick`).
- **Histórico mais linear** (`git rebase`) — uma forma alternativa de organizar as fotos.
- **Encontrar quando algo mudou** (`git bisect`) — útil pra rastrear quando um trecho entrou ou sumiu.

E, claro, configurar o acesso ao servidor por chave (SSH) — bem mais cômodo que digitar senha a cada envio. Mas isso é assunto pra depois. Por agora, basta tirar a primeira foto.
