---
layout: post
title: Configurando Servidor Wildfly 10 para Produção
type: article
description: Tutorial de configuração do servidor Wildfly em um servidor de produção. Neste tutorial você irá aprender as configurações básicas do Wildfly e como fazer o deploy de sua aplicação Java.
background: {{ site.baseurl }}/assets/images/configurando-servidor-wildfly-para-producao.png
thumbnail: http://placehold.it/372x124
img_post: http://placehold.it/1200x630
slug: config-wildfly
---

## TL;DR

#### Configurações Iniciais do Servidor

##### Instalar Java

```
~# add-apt-repository -y ppa:webupd8team/java && apt-get update && echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections && apt-get install oracle-java8-installer -y
```

##### Baixar servidor Wildfly

```
~# cd /opt
~# wget http://download.jboss.org/wildfly/10.0.0.Final/wildfly-10.0.0.Final.zip
~# unzip wildfly-10.0.0.Final.zip
~# rm -r wildfly-10.0.0.Final.zip
~# mv wildfly-10.0.0.Final/ wildfly/
```

#### Configurações Opcionais

#### Configuração Wildfly 10.x

#### Configurando acesso externo

#### Configurando usuário
