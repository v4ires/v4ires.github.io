# My Personal Website [![Build Status](https://travis-ci.org/v4ires/v4ires.github.io.svg?branch=master)](https://travis-ci.org/v4ires/v4ires.github.io)

<p align="center"><img src="assets/images/logo-social.png" width="50%" /></p>

## How it works?

This project is based on [Jekyll](http://jekyllrb.com/), that is a static webpage generator written in Ruby.

<p align="center"><img src="assets/images/jekyll_logo.jpg"/></p>

## First Steps

1. Install [Git](http://git-scm.com/downloads) and [Ruby](http://www.ruby-lang.org/pt/downloads/).

2. Fork the repository in [v4ires.github.io](https://github.com/v4ires/v4ires.github.io).

<p align="center"><img src="assets/images/fork.png" /></p>

3. Open the terminal and install [Jekyll](http://jekyllrb.com/):

```sh
~$ gem install jekyll
```

4. Now clone the project:

```sh
~$ git clone git@github.com:YOUR_USER_NAME/v4ires.github.io.git
```

5. Go to root project folder:

```sh
~$ cd v4ires.github.io/
```

6. Run the command:

```sh
~$ jekyll serve
```

Open the address in your favorite web browser `localhost:4000` :smile:

## Supported Web Browsers

![IE](https://cloud.githubusercontent.com/assets/398893/3528325/20373e76-078e-11e4-8e3a-1cb86cf506f0.png) | ![Chrome](https://cloud.githubusercontent.com/assets/398893/3528328/23bc7bc4-078e-11e4-8752-ba2809bf5cce.png) | ![Firefox](https://cloud.githubusercontent.com/assets/398893/3528329/26283ab0-078e-11e4-84d4-db2cf1009953.png) | ![Opera](https://cloud.githubusercontent.com/assets/398893/3528330/27ec9fa8-078e-11e4-95cb-709fd11dac16.png) | ![Safari](https://cloud.githubusercontent.com/assets/398893/3528331/29df8618-078e-11e4-8e3e-ed8ac738693f.png)
--- | --- | --- | --- | --- |
IE 8+ ✔ | Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ |

## Project Structure

The basis project structures is organized as shown in below:

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

These are blocks of code used to generate the main page of the website ([index.html](https://github.com/v4ires/v4ires.github.io/blob/master/index.html)).

### [_posts](https://github.com/v4ires/v4ires.github.io/tree/master/_posts)

Here you will find a list of files for each blog post.

### [_layouts](https://github.com/v4ires/v4ires.github.io/tree/master/_layouts)

Here you will find the standard website templates.

### [_site](https://github.com/v4ires/v4ires.github.io/tree/master/_site)

Here you will find all generated static files by jekyll. However, this directory is unnecessary in this project, so it is ignored in ([.gitignore](https://github.com/v4ires/v4ires.github.io/blob/master/.gitignore)).

### [_config.yml](https://github.com/v4ires/v4ires.github.io/blob/master/_config.yml)

It contains most of the application's settings.

### [index.html](https://github.com/v4ires/v4ires.github.io/blob/master/index.html)

Contains all sections of the website.

## Author

This project was created by [@v4ires](https://www.github.com/v4ires)

## Credits

README inspired by the [@zenorocha](https://github.com/zenorocha) repositories :+1:.

## License

[MIT License](https://www.viniciusaires.dev/mit) © Vinícius Aires Barros

Visit website [https://www.viniciusaires.dev](https://www.viniciusaires.dev).
