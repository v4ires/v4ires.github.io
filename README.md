# Meu website pessoal [![Build Status](https://travis-ci.org/v4ires/v4ires.github.io.svg?branch=master)](https://travis-ci.org/v4ires/v4ires.github.io)

<p align="center"><img src="assets/images/logo-social.png" width="50%" /></p>

## Como funciona?

Este projeto utiliza [Jekyll](http://jekyllrb.com/), que é um gerador de páginas estáticas em Ruby.

<p align="center"><img src="assets/images/icons/jekyll.png"/></p>

<caption>Página inicial do Website</caption>

## Primeiros passos

1. Instale o [Git](http://git-scm.com/downloads) e [Ruby](http://www.ruby-lang.org/pt/downloads/), se você ainda não tiver instalado.

2. Primeiramente, fork o repositório em [v4ires.github.io](https://github.com/v4ires/v4ires.github.io).

<p align="center"><img src="assets/images/fork.png" /></p>

3. Após isso, abra um terminal e instale o [Jekyll](http://jekyllrb.com/) com o seguinte comando:

```sh
~$ gem install jekyll
```

4. Agora clone o projeto:

```sh
~$ git clone git@github.com:YOUR_USER_NAME/v4ires.github.io.git
```

5. Vá até a pasta do projeto:

```sh
~$ cd v4ires.github.io/
```

6. E finalmente execute:

```sh
~$ sh run.sh
```

Você terá acesso ao site no endereço `localhost:4000` :smile:

## Navegadores suportados

![IE](https://cloud.githubusercontent.com/assets/398893/3528325/20373e76-078e-11e4-8e3a-1cb86cf506f0.png) | ![Chrome](https://cloud.githubusercontent.com/assets/398893/3528328/23bc7bc4-078e-11e4-8752-ba2809bf5cce.png) | ![Firefox](https://cloud.githubusercontent.com/assets/398893/3528329/26283ab0-078e-11e4-84d4-db2cf1009953.png) | ![Opera](https://cloud.githubusercontent.com/assets/398893/3528330/27ec9fa8-078e-11e4-95cb-709fd11dac16.png) | ![Safari](https://cloud.githubusercontent.com/assets/398893/3528331/29df8618-078e-11e4-8e3e-ed8ac738693f.png)
--- | --- | --- | --- | --- |
IE 8+ ✔ | Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ |

## Estrutura de arquivos

A estrutura básica dos arquivos deste projeto é organizada da seguinte maneira:

```
|-- _includes
|-- _layouts
|-- _posts
|-- _site
|-- _images
|-- _public
|-- _config.yml
`-- index.html
```

### [_includes](https://github.com/v4ires/v4ires.github.io/tree/master/_includes)

São blocos de código usados para gerar a página principal do site ([index.html](https://github.com/v4ires/v4ires.github.io/blob/master/index.html)).

### [_posts](https://github.com/v4ires/v4ires.github.io/tree/master/_posts)

Aqui você encontrará uma lista de arquivos para cada post.

### [_layouts](https://github.com/v4ires/v4ires.github.io/tree/master/_layouts)

Aqui você encontrará o template padrão da aplicação

### [_site](https://github.com/v4ires/v4ires.github.io/tree/master/_site)

Aqui você encontrará todos os arquivos estáticos gerados pelo jekyll após a sua execução. No entanto, este diretório é desnecessário neste projeto, portanto ele é ignorado em ([.gitignore](https://github.com/v4ires/v4ires.github.io/blob/master/.gitignore)).

### [_config.yml](https://github.com/v4ires/v4ires.github.io/blob/master/_config.yml)

Contém a maioria das configurações da aplicação.

### [index.html](https://github.com/v4ires/v4ires.github.io/blob/master/index.html)

Contém todas as seções da aplicação.

## Autor

Este projeto foi idealizado por [@v4ires](https://www.github.com/v4ires)

## Créditos

README inspirado nos repositórios do [@zenorocha](https://github.com/zenorocha) :+1:.

## Licença

[MIT License](https://www.viniciusaires.dev/mit) © Vinícius Aires Barros

Visite o site [https://www.viniciusaires.dev](https://www.viniciusaires.dev).
