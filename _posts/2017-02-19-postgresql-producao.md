---
layout: post
title: Configuração do PostgreSQL (Production)
type: article
description: Neste tutorial você aprenderá a configurar o banco de dados PostgreSQL e colocar-lo em produção.
background: /assets/images/config-postgresql-production.png
thumbnail: /assets/images/config-postgresql-production-thumbnail.png
img_post: /assets/images/config-postgresql-production-post.png
slug: config-postgresql-prodution
---

## Configuração do Banco de Dados PostgreSQL (Production)

### TL;DR

PostgreSQL é um sistema gerenciador de banco de dados objeto relacional (SGBDOR), desenvolvido como projeto de código aberto. Hoje, o PostgreSQL é um dos SGBDs (Sistema Gerenciador de Bancos de Dados) de código aberto mais avançados, contando com diversos recursos. O PostgreSQL é compatível com diversos Sistemas Operacionais, dentre eles, o Linux, Windows e Mac OS. Para mais informações acessem o site [https://www.postgresql.org](https://www.postgresql.org).

### Let’s get started

Neste tutorial você irá aprender a configurar o banco de dados PostgreSQL para uso em produção. Chega de conversa e vamos inciar o tutorial.

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/getstart-underwood.gif" style="width: 50%;"/>
</p>

### Instalação do PostgreSQL 9.x no Ubuntu e derivados

Atualize os repositórios:

```bash
$ sudo apt-get update
```

Instale o PostgreSQL:

```bash
$ sudo apt-get install postgresql postgresql-contrib
```

Teste o acesso ao PostgreSQL:

```bash
$ sudo -i -u postgres
$ psql
# \q
```

Criar, Deletar e Acessar, uma base de dados:

```bash
$ createdb mydb
$ dropdb mydb
$ psql mydb
```

Alterar a senha do usuário postgres:

```bash
$ sudo -i -u postgres
$ psql -d postgres -U postgres psql
# ALTER USER postgres with PASSWORD 'minha_senha';
# \q
$ exit
```

Libere o acesso remoto pela rede ao PostgreSQL (Opcional):<br>
*substitua o 9.x pelo número de sua versão atual do PostgreSQL

```bash
$ sudo nano /etc/postgresql/9.x/main/postgresql.conf
```
Alterar: <br>
	de ```# listen_address = 'localhost'``` <br>
	para ```listen_address = '*'```

```bash
$ sudo nano /etc/postgresql/9.x/main/pg_hba.conf
```
Adicione a seguinte linha ao final do arquivo:<br>
```host    all             all             0.0.0.0/0               md5```

Reinicie o PostgreSQL:

```bash
$ sudo service postgresql restart
```

Adicione o postgres ao grupo sudo:

```bash
$ sudo gpasswd -a postgres sudo
```

Alternar para usuário postgres no terminal como superusuário:

```bash
$ sudo su - postgres
```

Crie um novo usuário no postgres:

```bash
$ sudo -u postgres createuser -d -R -P my_user
```

Crie uma base de dados para o novo usuário:

```bash
$ sudo -u postgres createdb -O my_user db_test
```

Entre no postgres com o superusuário:

```bash
$ psql --username=postgres --password
```

Dê permissão para um usuário específico acessar uma determinada base de dados:

```bash
# GRANT ALL PRIVILEGES ON DATABASE db_test TO my_user;
# \q
```

Entrar no postgres com o novo usuário:

```bash
$ psql --username=my_user --password
```

### Extra

Instale o programa PgAdmin3 para gerenciar o banco de dados com uma interface gráfica:

```bash
$ sudo apt-get install pgadmin3 -y
```

Para mais informações acessem o [link](https://www.postgresql.org/ftp/pgadmin3/).

### Considerações Finais

Chegamos ao final do tutorial, caso tenha acontecido algum problema favor reportar nos comentários. Caso tudo tenha dado certo chegou a hora de criar suas tabelas e pôr o seu banco de dados no ar. Happy Code :)

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/hasta-la-vista.gif" style="width: 50%;"/>
</p>

#### Disclaimer

É permitido a reprodução integral desse material desde que sejam feitas as devidas referências ao autor ;)

#### Referências

[1] PostgreSQL. ***PostgreSQL***. Acessado em Fevereiro de 2017. Disponível em: [https://www.postgresql.org](https://www.postgresql.org).

[2] PostgreSQL Wikipedia. ***PostgreSQL Wiki***. Acessado em Fevereiro de 2017. Disponível em: [https://pt.wikipedia.org/wiki/PostgreSQL](https://pt.wikipedia.org/wiki/PostgreSQL).

[3] Comunidade Brasileira de PostgreSQL. ***PostgreSQL Brasil***. Acessado em Fevereiro de 2017. Disponível em: [https://www.postgresql.org.br](https://www.postgresql.org.br).