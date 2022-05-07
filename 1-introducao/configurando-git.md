# Configurando o Git

Antes de começar o projeto segue algumas orientações de configuração, para alterar as configurações do Git podemos utilizar o seguinte comando com a flag `--edit`:

```bash
git config --local --edit
```

Caso deseje abrir as configurações no VSCode:

```bash
git config --global core.editor code
```

Resumo das Flags:

- `--local`: Configurações do projeto
- `--global`: Configurações de qualquer projeto do usuário do sistema operacional
- `--system`: Todos os projetos e usuários do sistema operacional
- `core.editor`: Define em qual editor será realizado as alterações, caso não queira utilizar o VIM ou Nano pode utilizar o próprio VSCode.

A princípio o Git estará configurado dessa forma:

```bash
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = git@github.com:jotunes23/clean-react.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
	remote = origin
	merge = refs/heads/main

```

Na propriedade `core` adicione a linha abaixo, a flag `--wait` ajuda na performance do editor, em casos onde ele não carrega a tempo informações necessárias para operações do Git.

```bash
editor = code --wait
```

Neste arquivo de configurações, podemos criar novos atalhos personalizados de comandos Git através da propriedade alias, os atalhos são adicionados com a seguinte sintaxe:

```bash
[alias]
	c = !git {seu código}
```

## Criandos Alias

Nosso primeiro alias será para facilitar o commit, colocando o comando `git add --all` em sequencia ao `git commit -m`:

```bash
c = !git add --all && git commit -m
```

Depois de criado podemos utilizar o alias de seguinte forma:

```bash
git c "commit de exemplo"
```

> A diferença entre `git add .` e `git add --all`, é que na flag `.`, se criarmos um diretório novo e navegarmos dentro dele com arquivos modificados, o Git não identificará, enquanto a flag `--all` pega qualquer arquivo não interessando em qual nível de diretório esteja.

Nosso segundo alias se chamará `s` e será utilizado para customizar a saída do comando `git status`, ao utilizar este comando recebemos um overview das alterações do repositório, porém com a flag `-s` (short) podemos visualizar uma versão mais simples e direta, então o alias fica assim:

```bash
s = !git status -s
```

> Caso queira ver mais detalhes do repositório ainda pode continuar utilizando o `git status` normalmente.

Nosso terceiro alias se chamará `l` , será utitilizado para customizar a saida do `git log`, este comando por padrão exibe muitas informações sobre os commits realizados, porém ele pode ser simplificado com a flag `--oneline`.

O problema dessa flag é que ela não exibe o autor do commit e nem a data, porém com a flag `--pretty` podemos configurar as opções de informações do commit, cada item seguido do caratere `%` é um item que pode ser exibido do commit, como por exemplo `%h` (hash reduzida co commit).

```bash
l = !git log --pretty=format:'%C(yellow)%h%C(magenta)%d %C(white)%s - %C(cyan)%cn, %C(green)%cr'
```

O quarto alias será o `amend`, o comando `git amend` permite juntar uma alteração atual ao commit anterior, ideal para quando esquecemos de commitar ou erramos em algum arquivo mas não desejamos fazer um novo commit:

```bash
amend = !git add --all && git commit --amend --no-edit
```

Neste último alias, utilizaremos o comando `git count` que aliado ao uso de _conventional commits_ permite realizar um levantamento de quantos commits de determinado tipo foram realizados no projeto, para fins estátisticos por exemplo.

```bash
count = !git shortlog -s --grep
```

## Configuraçẽo do Push

Ao realizar um push para o servidor (Github), por padrão as tags não são enviadas junto ao push, sendo necessário enviá-las depois separadamente.

Existem dois tipos de tags a "normal" e a "anotada", é recomendado utilizar a tag normal para auxiliar no desenvolvimento marcando algum ponto importante do projeto e a tag anotada para de fato anotar uma release e enviá-las para o servidor.

Criando uma Tag Normal:

```bash
git tag "1.0"
```

Criando uma Tag Anotada:

```bash
git tag "1.0.1" -m "Tags anotadas precisam de mensagem"
```

Para corrigir esse comportamento podemos explicitar na propriedade `push` que queremos enviar as tags (neste caso somente as Tags Anotadas) para o servidor, junto ao push.

```bash
[push]
	followTags = true
```

## Referências

- [Git - Pretty Formats](https://git-scm.com/docs/pretty-formats)
- [Conventional Commits](https://www.conventionalcommits.org)
- [Artigo - Conventional Commits em pt-br](https://startecjobs.com/artigos/carreira/conventional-commits/)
