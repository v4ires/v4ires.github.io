---
layout: post
title: Configuração do Servidor JBoss Wildfly 10 (Production) - (PT-BR)
type: article
description: Neste tutorial você aprenderá a configurar o Wildfly 10.x e colocar o servidor em produção.
background: /assets/images/config-wildfly10.png
thumbnail: /assets/images/config-wildfly10_thumbnail.png
img_post: /assets/images/config-wildfly10-post.png
slug: config-wildfly-prodution
---

Configuração do Servidor JBoss Wildfly 10 (Production) - (PT-BR)
------

### TL;DR

O Jboss Wildfly é um web-container para aplicações em java mantida pela Red Hat. O Wildfly é um dos mais famosos web-containers java do mercado, sendo uma boa alternativa ao Oracle Glassfish. O WildFly, anteriormente conhecido como JBoss AS, ou simplesmente JBoss, é um servidor de aplicativos criado pelo JBoss, agora desenvolvido pela Red Hat. O WildFly é escrito em Java e implementa a especificação Java Platform Enterprise Edition (Java EE). Ele é executado em várias plataformas. WildFly é um software livre e de código aberto, sujeito aos requisitos da licença LGPL, versão 2.1. Em 20 de novembro de 2014, o JBoss Application Server foi renomeado como WildFly. A comunidade JBoss e outros produtos JBoss da Red Hat, como o JBoss Enterprise Application Platform, não foram renomeados. Para mais informações acessem o site [http://wildfly.org/](http://wildfly.org/).

#### Let's get started

Neste tutorial você aprenderá a configurar o servidor Jboss Wildfly 10.x para produção.
Chega de conversa vamos iniciar o tutorial.

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/typing-fast.gif" style="width: 50%;"/>
</p>

#### Configurações Iniciais do Servidor

Instale o Java (Caso já tenha, desconsidere esta etapa):

{% highlight shell %}
~$ sudo add-apt-repository -y ppa:webupd8team/java \
&& sudo apt-get update \
&& sudo echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections \
&& sudo apt-get install oracle-java8-installer -y
{% endhighlight %}

Faça o download do servidor Wildfly:

{% highlight shell %}
~$ cd /opt
~$ sudo wget http://download.jboss.org/wildfly/10.0.0.Final/wildfly-10.0.0.Final.zip
~$ sudo unzip wildfly-10.0.0.Final.zip
~$ sudo rm -r wildfly-10.0.0.Final.zip
~$ sudo mv wildfly-10.0.0.Final/ wildfly/
{% endhighlight %}

#### Configuração do Servidor Wildfly 10.x

{% highlight shell %}
~$ sudo addgroup wildfly
~$ sudo useradd -g wildfly wildfly
{% endhighlight %}

Mude o dono da pasta do Wildfly recursivamente:

{% highlight shell %}
~$ sudo chown -R wildfly:wildfly /opt/wildfly/
{% endhighlight %}

Crie um link simbólico para o Wildfly:

{% highlight shell %}
~$ sudo ln -s wildfly /opt/wildfly
{% endhighlight %}

Configure o init.d/ para o Wildfly:

{% highlight shell %}
~$ sudo cp /opt/wildfly/docs/contrib/scripts/init.d/wildfly-init-debian.sh /etc/init.d/wildfly
~$ sudo chown root:root /etc/init.d/wildfly
~$ sudo chmod ug+x /etc/init.d/wildfly
{% endhighlight %}

Para dar start e stop no Wildfly pela primeira vez:

{% highlight shell %}
~$ sudo /etc/init.d/wildfly start
~$ sudo /etc/init.d/wildfly stop
{% endhighlight %}

Atualize o Wildfly como serviço:

{% highlight shell %}
~$ sudo update-rc.d wildfly defaults
{% endhighlight %}

#### Configurando acesso externo ao servidor

Acesse o diretório:

{% highlight shell %}
~$ cd /opt/wildfly/standalone/configuration/
{% endhighlight %}

Edite o arquivo de configuração:

{% highlight shell %}
~$ sudo nano standalone.xml
{% endhighlight %}

Substitua as seguintes configurações:

{% highlight xml %}
<interface name="public">
    <inet-address value="${jboss.bind.address:127.0.0.1}"/>
</interface>
{% endhighlight %}

por:

{% highlight xml %}
<interface name="public">
    <any-address/>
</interface>
{% endhighlight %}

E a configuração de acesso ao painel de configurações do Wildfly:

{% highlight xml %}
<interface name="management">
    <inet-address value="${jboss.bind.address:127.0.0.1}"/>
</interface>
{% endhighlight %}

por

{% highlight xml %}
<interface name="management">
    <any-address/>
</interface>
{% endhighlight %}

#### Configurando usuário administrador

Primeiramente finalize o processo do Wildfly:

{% highlight shell %}
~$ sudo service wildfly stop
{% endhighlight %}

Acesse o diretório:

{% highlight shell %}
~$ cd /opt/wildfly/bin
{% endhighlight %}

Execute o script para adicionar um novo usuário:

{% highlight shell %}
~$ sudo ./add-user.sh
{% endhighlight %}

Faça as seguintes instruções:

1. Digite (a) para Management User
Digite (admin) para Username
1. Are you sure you want to add user 'admin' yes/no? (yes)
1. Digite uma senha para password
1. Confirme a senha
1. What groups do you want this user to belong to? (enter)
1. Is this correct yes/no? (2x yes)
1. Digita yes e enter para todas as perguntas

Inicie o Wildfly novamente:

{% highlight shell %}
~$ sudo service wildfly start
{% endhighlight %}

Acesse o IP da máquina :8080 para conferir o acesso ao painel de configuração do Wildfly:

{% highlight shell %}
http://localhost:9990 ou http://IP_EXTERNO:9990
{% endhighlight %}

#### Considerações Finais

Chegamos ao final do tutorial, caso tenha acontecido algum problema favor reportar nos comentários. Caso tudo tenha dado certo chegou a hora de “codar” e colocar suas aplicações em produção no Wildfly. Happy Code :)

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/coding.gif" style="width: 50%;"/>
</p>

#### Disclaimer

É permitido livremente a reprodução integral desse material desde que sejam feitas as devidas referências ao autor ;)

#### Referências

[1] Jboss Wildfly. ***Wildfly***. Acessado em Junho de 2016. Disponível em: [http://wildfly.org/](http://wildfly.org/)

[2] Jboss Wildfly Wikipedia. ***JBoss Wildlfy Wiki***. Acessado em Fevereiro de 2017. Disponível em:  [https://en.wikipedia.org/wiki/WildFly](https://en.wikipedia.org/wiki/WildFly)

[3] ForgeZilla Let's Forge it!. ***Installing Wildfly 10 on Debian/Ubuntu***. Acessado em Junho de 2016. Disponível em: [http://forgezilla.com/installing-wildfly-10-on-debianubuntu/](http://forgezilla.com/installing-wildfly-10-on-debianubuntu/)
