---
search:
  exclude: true
---
# Como Contribuir

Este site (artigos, design, ...) é hospedado no [Github](https://github.com/filipemsilv4/cp-algorithms).  Contribuições para a tradução são bem-vindas. Tudo o que você precisa é uma conta Github.

As páginas geradas são compiladas e publicadas em [https://paulofilipe.com/cp-algorithms/](https://paulofilipe.com/cp-algorithms/).

Para contribuir com a tradução, siga estes passos:

1. Vá para o artigo que você deseja traduzir e clique no ícone de lápis :material-pencil: ao lado do título do artigo.
2. Faça um fork do repositório, se solicitado.
3. Traduza o artigo para o português.
4. Use a [página de visualização](preview.md) para verificar se você está satisfeito com o resultado da tradução.
5. **Após a tradução, modifique o arquivo `navigation.md` alterando a bandeira do artigo de 🇺🇸 para 🇧🇷 e traduzindo o título do artigo.**  Por exemplo:
    ```
    - [🇺🇸 String Hashing](string/string-hashing.md) -> - [🇧🇷 Hashing de Strings](string/string-hashing.md)
    ```
6. Faça um commit clicando no botão _Propose changes_.
7. Crie um pull-request clicando no botão _Compare & pull request_.
8. Alguém da equipe principal analisará as mudanças. Isso pode levar algumas horas/dias.


## Sintaxe

Usamos [Markdown](https://daringfireball.net/projects/markdown) para os artigos e usamos o [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) para renderizar os artigos Markdown em HTML.  Mantenha a formatação original do artigo em inglês durante a tradução.

Recursos de Markdown do Material for MkDocs com os quais você deve ser particularmente cuidadoso durante a tradução:

- [Fórmulas matemáticas com MathJax](https://squidfunk.github.io/mkdocs-material/reference/mathjax/#usage)
  Observe que você precisa ter uma linha vazia antes e depois de um bloco matemático `$$`.
- [Blocos de código](https://squidfunk.github.io/mkdocs-material/reference/code-blocks/#usage) para trechos de código.
- [Admoestações](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#usage) (por exemplo, para decorar teoremas, provas, exemplos de problemas).
- [Abas de conteúdo](https://squidfunk.github.io/mkdocs-material/reference/content-tabs/#usage) (por exemplo, para exemplos de código em várias linguagens).
- [Tabelas de dados](https://squidfunk.github.io/mkdocs-material/reference/data-tables/#usage).

Traduza todo o conteúdo, incluindo o texto dentro desses elementos de formatação, mas mantenha a formatação original.

Por padrão, o primeiro cabeçalho (`# cabeçalho`) também será o título HTML do artigo. Caso o cabeçalho contenha uma fórmula matemática, você pode definir um título HTML diferente com:

```markdown
---
tags:
    - ...
title: Título HTML alternativo
---
# Prova de $a^2 + b^2 = c^2$

restante do artigo
```


### Tags

Os artigos possuem tags "Original" e "Translated", isso diz respeito à tradução para o inglês, não à tradução para o português. Não modifique essas tags ao traduzir para o português.

```md
---
tags:
  - Translated
e_maxx_link: bfs
---
```


## Desenvolvimento Local

Você pode renderizar as páginas localmente. Tudo que você precisa é do Python, com o pacote `mkdocs-material` instalado.

```console
$ git clone --recursive https://github.com/filipemsilv4/cp-algorithms && cd cp-algorithms
$ scripts/install-mkdocs.sh # requer instalação do pip
$ mkdocs serve
```

Observe que alguns recursos são desabilitados por padrão para builds locais.

### Plugin de data de revisão do Git

Desabilitado porque pode produzir erros quando há alterações não confirmadas na árvore de trabalho.

Para habilitá-lo, defina a variável de ambiente `MKDOCS_ENABLE_GIT_REVISION_DATE` como `True`:

```console
$ export MKDOCS_ENABLE_GIT_REVISION_DATE=True
```

### Plugin de contribuidores do Git

Desabilitado porque leva um tempo para preparar e também requer um token de acesso pessoal do Github para trabalhar com as APIs do Github.

Para habilitá-lo, defina a variável de ambiente `MKDOCS_ENABLE_GIT_COMMITTERS` como `True` e armazene seu token de acesso pessoal na variável de ambiente `MKDOCS_GIT_COMMITTERS_APIKEY`. Você pode gerar o token [aqui](https://github.com/settings/tokens). Observe que você só precisa do acesso público, portanto, não deve conceder nenhuma permissão ao token.

```console
$ export MKDOCS_ENABLE_GIT_COMMITTERS=True
$ export MKDOCS_GIT_COMMITTERS_APIKEY= # coloque seu PAT aqui 
```

