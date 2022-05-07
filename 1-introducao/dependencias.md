# Configurando as dependências

Para iniciar o projeto execute o comando:

```bash
npm init -y
```

## git-commit-msg-linter

Proibe a uitilização de commits fora do padrão de _Conventional Commits_.

```bash
npm install -D git-commit-msg-linter
```

## TypeScript

Execute o comando abaixo para instalar o TypeScript e o @types do Node.js. No arquivo de configuração do TypeScript, vamos utilizar as mesmas configurações que vem no boilerplate _Create React App_.

```bash
npm install -D typescript @types/node
```

## ESLint

Para o linter do TypeScript utilizaremos o ESLint junto ao padrão 'Standard'.

> Como a dependências `eslint-config-standard-with-typescript` está apresentando alguns problemas após a versão 12, neste caso vamos forçar a instalação da versão 11

```bash
npm install -D eslint eslint-config-standard-with-typescript@11 eslint-plugin-import eslint-plugin-promise eslint-plugin-standard @typescript-eslint/eslint-plugin eslint-plugin-node
```

Segue configuração inicial do ESLint, com extensão do `standard-with-typescript` e duas rules:

- `@typescript-eslint/consistent-type-definitions`: Permite utilizar tanto _Interfaces_ quanto _Type Aliases_, já que o TypeScript não permite usar os dois por padrão.
- `@typescript-eslint/strict-boolean-expressions`: Por padrão o TypeScript não permite comparações que não sejam do tipo Boolean, então podemos desabilitar esse comportamento.

> Os arquivos e diretórios que não queremos rodar no lint como por exemplo o `/node_modules`, estão listados no arquivo de configuração `.eslintignore`.

## Lint Staged e Husky

Essas bibliotecas serão utilizadas em conjunto para impedir que façamos commits defeituosos:

```bash
npm install -D lint-staged husky
```

A biblioteca `Lint Staged` será utilizada para pegar todos os arquivos que estão na _stage area_ do git, ou seja, os arquivos modificados, e aplicar scripts em cima deles. No exemplo abaixo do arquivo de configuração `.lintstagedrc.json`, ao tentar realizar um commit, será acionado um script que rodará o `eslint` dentro da pasta `/src` procurando arquivos `.ts` ou `.tsx`, se houver algum erro o script rodará o fix para corrigir e se não for possível corrigir automaticamente, o commit será bloqueado para corrigirmos manualmente.

```json
{
  "*.{ts,tsx}": ["eslint 'src/**' --fix"]
}
```

A biblioteca `Husky` possui hooks que podem ser utilizados para realizar alguma ação nas operações de Git, neste caso, utilizaremos o hook `pre-commit` para rodar o script do `Lint Staged` antes de realizar qualquer commit:

```json
{
  "hooks": {
    "pre-commit": "lint-staged"
  }
}
```

## Jest

A biblioteca Jest será usada realizar testes, a princípio reconhece apenas arquivos JavaScript, então para trabalhar com TypeScript devemos instalar as seguintes dependências:

```bash
npm install -D jest @types/jest ts-jest
```

## Referências

- [git-commit-msg-linter](https://github.com/legend80s/commit-msg-linter#readme)
- [TypeScript](https://www.typescriptlang.org/)
- [Lint Staged](https://github.com/okonet/lint-staged)
- [Husky](https://typicode.github.io/husky/#/)
- [Jest](https://jestjs.io/)
- [ESLint](https://eslint.org/)
- [Eslint + Typescript + Standard](https://github.com/standard/eslint-config-standard-with-typescript#readme)
- [ts-jest](https://github.com/kulshekhar/ts-jest)
- [git-commit-msg-linter](https://github.com/legend80s/commit-msg-linter#readme)
