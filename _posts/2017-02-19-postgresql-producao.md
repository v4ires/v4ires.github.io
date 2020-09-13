---
layout: post
title: Configuração do PostgreSQL (Production) - (PT-BR)
type: article
description: Neste tutorial você aprenderá a configurar o banco de dados PostgreSQL e colocar-lo em produção.
background: /assets/images/config-postgresql-production.png
thumbnail: /assets/images/config-postgresql-production-thumbnail.png
img_post: /assets/images/config-postgresql-production-post.png
slug: config-postgresql-prodution
---

Configuração do Banco de Dados PostgreSQL (Production) - (PT-BR)
------

### TL;DR

PostgreSQL é um sistema gerenciador de banco de dados objeto relacional (SGBDOR), desenvolvido como projeto de código aberto. Hoje, o PostgreSQL é um dos SGBDs (Sistema Gerenciador de Bancos de Dados) de código aberto mais avançados, contando com diversos recursos. O PostgreSQL é compatível com diversos Sistemas Operacionais, dentre eles, o Linux, Windows e Mac OS. Para mais informações acessem o site [https://www.postgresql.org](https://www.postgresql.org).

### Let’s get started

Neste tutorial você aprenderá a configurar o banco de dados PostgreSQL para uso em produção. Chega de conversa e vamos inciar o tutorial.

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/getstart-underwood.gif" style="width: 50%;"/>
</p>

### Instalação do PostgreSQL 9.x no Ubuntu e derivados

Atualize os repositórios:

{% highlight shell %}
~$ sudo apt-get update
{% endhighlight %}

Instale o PostgreSQL:

{% highlight shell %}
~$ sudo apt-get install postgresql postgresql-contrib
{% endhighlight %}

Teste o acesso ao PostgreSQL:

{% highlight shell %}
~$ sudo -i -u postgres
~$ psql
~# \q
{% endhighlight %}

Criar, Deletar e Acessar, uma base de dados:

{% highlight shell %}
~$ createdb mydb
~$ dropdb mydb
~$ psql mydb
{% endhighlight %}

Alterar a senha do usuário postgres:

{% highlight shell %}
~$ sudo -i -u postgres
~$ psql -d postgres -U postgres psql
~# ALTER USER postgres with PASSWORD 'minha_senha';
~# \q
~$ exit
{% endhighlight %}

Reinicie o PostgreSQL:

{% highlight shell %}
~$ sudo service postgresql restart
{% endhighlight %}

Adicione o postgres ao grupo sudo:

{% highlight shell %}
~$ sudo gpasswd -a postgres sudo
{% endhighlight %}

Alternar para usuário postgres no terminal como superusuário:

{% highlight shell %}
~$ sudo su - postgres
{% endhighlight %}

Crie um novo usuário no postgres:

{% highlight shell %}
~$ sudo -u postgres createuser -d -R -P my_user
{% endhighlight %}

Crie uma base de dados para o novo usuário:

{% highlight shell %}
~$ sudo -u postgres createdb -O my_user db_test
{% endhighlight %}

Entre no postgres com o superusuário:

{% highlight shell %}
~$ psql --username=postgres --password
{% endhighlight %}

Dê permissão para um usuário específico acessar uma determinada base de dados:

{% highlight shell %}
~# GRANT ALL PRIVILEGES ON DATABASE db_test TO my_user;
~# \q
{% endhighlight %}

Entrar no postgres com o novo usuário:

{% highlight shell %}
~$ psql --username=my_user --password
{% endhighlight %}

### Extra (Opcional)

#### Liberar acesso remoto ao PostgreSQL

Libere o acesso remoto pela rede ao PostgreSQL (não recomendado para uso em produção):<br>
*substitua o 9.x pelo número de sua versão atual do PostgreSQL<br>

{% highlight shell %}
~$ sudo nano /etc/postgresql/9.x/main/postgresql.conf
{% endhighlight %}

Alterar: <br>

{% highlight plaintext %}
de # listen_address = 'localhost'
para listen_address = '*'
{% endhighlight %}

{% highlight shell %}
~$ sudo nano /etc/postgresql/9.x/main/pg_hba.conf
{% endhighlight %}

Adicione a seguinte linha ao final do arquivo:<br>

{% highlight plaintext %}
host all all 0.0.0.0/0 md5
{% endhighlight %}

A forma mais correta de configurar o acesso externo ao banco de dados é configurar um proxy para acesso ao banco. Dessa maneira o acesso não ficará disponível abertamente na internet.

Para mais informações recomendo o seguintes links [01](https://www.postgresql.org/docs/8.2/static/ssh-tunnels.html) e [02](https://support.cloud.engineyard.com/hc/en-us/articles/205408088-Access-Your-Database-Remotely-Through-an-SSH-Tunnel). Em breve postarei um tutorial ensinando passo a passo como fazer esta configuração.

#### Instalando o PgAdmin

Instale o programa PgAdmin3 para gerenciar o banco de dados com uma interface gráfica:

{% highlight shell %}
~$ sudo apt-get install pgadmin3 -y
{% endhighlight %}

Para mais informações acessem o [link](https://www.postgresql.org/ftp/pgadmin3/).

### Considerações Finais

Chegamos ao final do tutorial, caso tenha acontecido algum problema favor reportar nos comentários. Caso tudo tenha dado certo chegou a hora de criar suas tabelas e pôr o seu banco de dados no ar. Happy Code :)

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/hasta-la-vista.gif" style="width: 50%;"/>
</p>

### Update

Foi feito uma pequena correção no tutorial sobre como fazer o acesso remoto ao Banco de Dados, sugestão feita nos comentários do post. Obrigado todos pelo feedback ;)

#### Disclaimer

É permitido a reprodução integral desse material desde que sejam feitas as devidas referências ao autor ;)

#### Referências

[1] PostgreSQL. ***PostgreSQL***. Acessado em Fevereiro de 2017. Disponível em: [https://www.postgresql.org](https://www.postgresql.org).

[2] PostgreSQL Wikipedia. ***PostgreSQL Wiki***. Acessado em Fevereiro de 2017. Disponível em: [https://pt.wikipedia.org/wiki/PostgreSQL](https://pt.wikipedia.org/wiki/PostgreSQL).

[3] Comunidade Brasileira de PostgreSQL. ***PostgreSQL Brasil***. Acessado em Fevereiro de 2017. Disponível em: [https://www.postgresql.org.br](https://www.postgresql.org.br).