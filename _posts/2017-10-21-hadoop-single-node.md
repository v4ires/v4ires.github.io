---
layout: post
title: Configuração do Apache Hadoop em Single Node - (PT-BR)
type: article
description: Neste tutorial você aprenderá a configurar o Apache Hadoop em Single Node
background: /assets/images/configurando-hadoop-single-node.png
thumbnail: /assets/images/configurando-hadoop-single-node-thumbnail.png
img_post: /assets/images/configurando-hadoop-single-node-post.png
slug: config-hadoop-single-node
---

Instalação do Apache Hadoop 2.8.x em modo Single Node - (PT-BR)
------

#### Requisitos iniciais:

- 1) Tenha o Java 7+ instalado
- 2) Tenha o servidor SSH devidamente instalado e configurado
- 3) Tenha a variável JAVA_HOME configurada
- 4) Configure o SSH para o acesso sem senha com o endereço *localhost*

*ps: Este tutorial foi testado com o sistema operacional Ubuntu 16.04 e 14.04. Caso deseje configurar em uma versão mais antiga, recente ou em outro SO, pequenas mudanças podem ocorrer nos procedimentos apresentados.

### TL;DR

O projeto Apache Hadoop é um *software* de código aberto mantido pela Apache Foundation que tem como propósito fornecer uma implementação de código aberto do modelo de programação *MapReduce* de forma confiável e escalável. O Hadoop é projetado para ampliar o processamento de um único servidor em milhares de máquinas, onde cada uma das máquinas oferecem poder de processamento e armazenamento local. Esta ferramenta é utilizada para processamento em *batch* de grandes volumes de dados (*Big Data*). Atualmente, o Apache Hadoop é uma das ferramentas mais conhecidas para processamento distribuído, mas existem outras ferramentas similares que se integram ao Hadoop, como o **Apache Spark**, **Apache Storm** e dentre outros.

*para mais informações a respeito do Apache Hadoop acessem o site [http://hadoop.apache.org/](http://hadoop.apache.org/)

### Let's get started

Neste tutorial você irá aprender a configurar o Apache Hadoop no modo *Single Node*. Montando um ambiente capaz de executar seus primeiros *scripts* em *MapReduce*.

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/coding-girl-fast.gif" style="width: 60%;"/>
</p>

### Instale o Java JDK e configure o JAVA_HOME

Instale o Java 8 com o seguinte comando:

{% highlight shell %}
~# apt-get install openjdk-8-jdk
{% endhighlight %}

Verifique a instalação do Java:

{% highlight shell %}
~# java -version
{% endhighlight %}

Adicionar o JAVA_HOME na variável PATH:

{% highlight shell %}
~# vim /etc/environments
{% endhighlight %}

Adicionar ao final do arquivo a variável JAVA_HOME:

{% highlight shell %}
~# JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
{% endhighlight %}

Atualize os índices da variável de ambiente:

{% highlight shell %}
~# source /etc/environment
{% endhighlight %}

Verifique se o Path está correto:

{% highlight shell %}
~# echo $JAVA_HOME
{% endhighlight %}

### Instale e Configure o Servidor SSH (Sem Senha)

Caso não tenha instalado o ssh, instale o servidor ssh com o seguinte comando:

{% highlight shell %}
~# apt-get install ssh
{% endhighlight %}

Verifique se a máquina tem acesso a ssh sem senha no localhost:

{% highlight shell %}
~# ssh localhost
{% endhighlight %}

Caso seja solicitado uma senha, faça o seguinte procedimento:

Gere uma chave pública de acesso ao ssh

{% highlight shell %}
~# ssh-keygen -t rsa -P ""
{% endhighlight %}

Agora com a chave gerada e adicione ela ao diretório de chaves autorizadas:

{% highlight shell %}
~# cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
{% endhighlight %}

### Configure o Apache Hadoop (Single Node)

Faça o download do Apache Hadoop, neste caso a versão 2.8.1:

{% highlight shell %}
~# wget http://mirror.nbtelecom.com.br/apache/hadoop/common/hadoop-2.8.1/hadoop-2.8.1.tar.gz
{% endhighlight %}

Descompacte o Apache Hadoop no diretório /opt:

{% highlight shell %}
~# tar -zxvf hadoop-2.8.1.tar.gz
{% endhighlight %}

Agora configure as variáveis de ambiente:

{% highlight shell %}
#caso você prefira que todos usuários do sistema tenham as variáveis de ambiente
~# vim /etc/profile 
{% endhighlight %}

ou

{% highlight shell %}
#caso você prefira que apenas o seu usuário tenham as variáveis de ambiente
~# vim ~/.bashrc 
{% endhighlight %}

Adicione ao final do arquivo os seguintes variáveis de ambiente:

{% highlight shell %}
#HADOOP VARIABLES START
export JAVA_HOME=caminho_para_o_java_home
export HADOOP_INSTALL=caminho_para_o_haddop
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib"
#HADOOP VARIABLES END
{% endhighlight %}

OBS: Substitua ***caminho_para_o_java_home*** e ***caminho_para_o_hadoop*** pelos seus respectivos paths ;)

Atualize os índices das variáveis de ambiente:

{% highlight shell %}
~# source ~/.bashrc
{% endhighlight %}

ou

{% highlight shell %}
~# source /etc/profile
{% endhighlight %}

Agora edite o arquivo de variáveis de ambiente do Hadoop:

{% highlight shell %}
~# vim hadoop-2.8.1/etc/hadoop/hadoop-env.sh
{% endhighlight %}

Edite a variável de ambiente JAVA_HOME:

{% highlight shell %}
~# export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
{% endhighlight %}

Crie o diretório onde será o ponto de montagem do sistemas de arquivos do Hadoop o HDFS:

{% highlight shell %}
~# mkdir -p ~/hadoop_store/hdfs/namenode
~# mkdir -p ~/hadoop_store/hdfs/datanode
{% endhighlight %}

Configure o arquivo hdfs-site.xml com o caminho das pastas criadas:

{% highlight shell %}
~# vim hadoop-2.8.1/etc/hadoop/hdfs-site.xml
{% endhighlight %}

Adicione as seguintes propriedades:

{% highlight xml %}
<property>
  <name>dfs.replication</name>
  <value>1</value>
  <description>Default block replication.
  The actual number of replications can be specified when the file is created.
  The default is used if replication is not specified in create time.
  </description>
</property>

<property>
  <name>dfs.namenode.name.dir</name>
  <value>file:######NAMENODE_FOLDER_PATH######</value>
</property>

<property>
  <name>dfs.datanode.data.dir</name>
  <value>file:######DATANODE_FOLDER_PATH######</value>
</property>
{% endhighlight %}

*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

Crie um diretório temporário na pasta de instalação do Hadoop:

{% highlight shell %}
~# mkdir -p ~/hadoop_store/hdfs/tmp
{% endhighlight %}

Configure o arquivo core-site.xml

{% highlight shell %}
~# vim hadoop-2.8.1/etc/hadoop/core-site.xml
{% endhighlight %}

Adicione as seguintes propriedades

{% highlight xml %}
<property>
  <name>hadoop.tmp.dir</name>
  <value>######TMP_FOLDER_PATH######</value>
  <description>A base for other temporary directories.</description>
</property>

<property>
  <name>fs.default.name</name>
  <value>hdfs://localhost:54310</value>
  <description>The name of the default file system.  A URI whose
  scheme and authority determine the FileSystem implementation.  The
  uri's scheme determines the config property (fs.SCHEME.impl) naming
  the FileSystem implementation class.  The uri's authority is used to
  determine the host, port, etc. for a filesystem.</description>
</property>
{% endhighlight %}
 
*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

Configure o arquivo mapred-site.xml:

{% highlight shell %}
~# cp hadoop-2.8.1/etc/hadoop/mapred-site.xml.template hadoop-2.8.1/etc/hadoop/mapred-site.xml
{% endhighlight %}

Adicione a seguinte propriedade:

{% highlight xml %}
<property>
  <name>mapred.job.tracker</name>
  <value>localhost:54311</value>
  <description>The host and port that the MapReduce job tracker runs
  at.  If "local", then jobs are run in-process as a single map
  and reduce task.
  </description>
</property>
{% endhighlight %}

*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

Configure o arquivo o arquivo yarn-site.xml:

{% highlight shell %}
~# vim yarn-site.xml
{% endhighlight %}

Adicione as seguintes propriedades:

{% highlight xml %}
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>frontend</value>
  <description>The hostname of the RM.</description>
</property>

<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>

<property>
  <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
{% endhighlight %}

*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

Formate o sistema de arquivos HDFS:

{% highlight shell %}
~# hadoop namenode -format
{% endhighlight %}

Inicie os serviços do Hadoop:

{% highlight shell %}
~# start-dfs.sh && start-yarn.sh
{% endhighlight %}

Verifique os processos do Hadoop:

{% highlight shell %}
~# jps
{% endhighlight %}

O console deve aparecer uma saída semelhante a essa:

{% highlight shell %}
9757 Jps
8950 DataNode
9145 SecondaryNameNode
9438 NodeManager
8799 NameNode
9311 ResourceManager
{% endhighlight %}

*Importante que apareça os processos ***DataNode***, ***SecondaryNameNode***, ***NodeManager***, ***NameNode*** e ***ResourceManager***.

## Configurar o Memory Heap da JVM

Caso enfrente algum problema com relação ao tamanho do *heap* de memória da JVM

1ª Etapa:

Adicione esta linha no arquivo /etc/profile:

{% highlight shell %}
export JVM_ARGS="-Xms1024m -Xmx1024m"
{% endhighlight %}

Esta mudança muda o heap de memória do Java para 1024 mb. Por padrão é 128 mb.

Atualize os índices do arquivo /etc/profile:

{% highlight shell %}
~# source /etc/profile
{% endhighlight %}

2ª Etapa:

Adicione ou edite a seguinte variável de ambiente:

{% highlight shell %}
~# export HADOOP_CLIENT_OPTS="-Xmx1024m $HADOOP_CLIENT_OPTS"
{% endhighlight %}

3ª Etapa:

Adicione a seguinte propriedade ao arquivo mapred-site.xml:

{% highlight xml %}
<property>
  <name>mapred.child.java.opts</name>
  <value>-Xmx1024m</value>
</property>
{% endhighlight %}

*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

Se tudo estiver ok, pronto temos o Apache Hadoop configurado em modo *Single Node* ;)<br>

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/elephant-crazy.gif" style="width: 50%;"/>
</p>

O artigo Configuração do Apache Hadoop em Multi Node está disponível no seguinte <a href="{{ site.baseurl }}/config-hadoop-multi-node/" target="_blank">link</a>.

## Disclaimer

Material desenvolvido durante o meu Mestrado no Instituto de Ciências Matemáticas e de Computação da Universidade de São Paulo (<a href="http://icmc.usp.br/" target="_blank">ICMC-USP</a>). Além disso, faço um agradecimento em especial ao Laboratório de Sistemas Distribuídos e Programação Concorrente (<a href="http://lasdpc.icmc.usp.br/" target="_blank">LaSDPC</a>), o qual faço parte que me permitiu a criação deste material. Por fim, informo que é permitido livremente a reprodução integral deste material desde que sejam feitas as devidas referências ao autor ;)

## Referências

[1] Apache Foundation. Hadoop: ***Setting up a Single Node Cluster***. Acessado em Agosto de 2016. Disponível em: [http://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html](http://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html)

[2] Rajesh N. ***“Java Heap space Out Of Memory Error” while running a mapreduce program***. Acessado em Agosto de 2016. Disponível em:  [http://stackoverflow.com/questions/30295606/java-heap-space-out-of-memory-error-while-running-a-mapreduce-program](http://stackoverflow.com/questions/30295606/java-heap-space-out-of-memory-error-while-running-a-mapreduce-program)