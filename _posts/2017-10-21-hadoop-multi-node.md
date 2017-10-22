---
layout: post
title: Configuração do Apache Hadoop em Multi Node
type: article
description: Neste tutorial você aprenderá a configurar o Apache Hadoop em Single Node
background: /assets/images/configurando-hadoop-multi-node.png
thumbnail: /assets/images/configurando-hadoop-multi-node-thumbnail.png
img_post: /assets/images/configurando-hadoop-multi-node-post.png
slug: config-hadoop-multi-node
---

## Instalação do Apache Hadoop 2.8.x em modo Multi Node

Requisitos iniciais:

- 1) Tenha o Java 7+ instalado
- 2) Realize a configuração em modo <a href="{{ site.baseurl }}/config-hadoop-single-node/" target="_blank">*single node*</a> para todas a máquinas desejadas
- 3) Tenha o servidor SSH devidamente instalado e configurado
- 4) Configure o SSH para o acesso sem senha com todos os endereços de IP das máquinas desejadas incluindo o *localhost*

*ps: Este tutorial foi testado com o sistema operacional Ubuntu 16.04 e 14.04. Caso deseje configurar em uma versão mais antiga, recente ou em outro SO, pequenas mudanças podem ocorrer nos procedimentos apresentados.

### TL;DR

O projeto Apache Hadoop é um *software* de código aberto mantido pela Apache Foundation que tem como propósito fornecer uma implementação de código aberto do modelo de programação *MapReduce* de forma confiável e escalável. O Hadoop é projetado para ampliar o processamento de um único servidor em milhares de máquinas, onde cada uma das máquinas oferecem poder de processamento e armazenamento local. Esta ferramenta é utilizada para processamento em *batch* de grandes volumes de dados (*Big Data*). Atualmente, o Apache Hadoop é uma das ferramentas mais conhecidas para processamento distribuído, mas existem outras ferramentas similares que se integram ao Hadoop, como o **Apache Spark**, **Apache Storm** e dentre outros.

*para mais informações a respeito do Apache Hadoop acessem o site [http://hadoop.apache.org/](http://hadoop.apache.org/)

### Let's get started

Neste tutorial você irá aprender à configurar o Apache Hadoop no modo *Multi Node*. Montando um ambiente capaz de executar seus primeiros *scripts* em *MapReduce*.

Primeiramente configure todas as máquinas que deseja montar um cluster Hadoop no modo *single node*. Para configurar basta seguir as instruções disponíveis no seguinte [link]({{ site.baseurl }}/config-hadoop-single-node).

Outro ponto importante é a configuração do ssh sem senha entre todas máquinas que você pretende montar o cluster, o que inclui ela mesma (*localhost*). Para realizar essa tarefa basta seguir as intruções do tutorial disponibilizado no portal [Viva o Linux](https://www.vivaolinux.com.br/dica/SSH-sem-senha/).

Após realizada essa primeira etapa vamos fazer as devidas modificações para que o Hadoop funcione em modo *cluster*. Para isso esse tutorial foi dividido em três tipos de configurações, nó *master*, *slaves* e para ambos.

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/programming-fire.gif" style="width: 50%;"/>
</p>

### Para o nó Master

Para o nó *master* basta editar o arquivo de configuração etc/hadoop/hdfs-site.xml:

```bash
~# vim hdfs-site.xml
```

No caso do nó *master* iremos manter apenas a configuração do *namenode*, removendo a propriedade do *datanode*:

```xml
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
   <value>file:/opt/bigdata/hadoop-2.7.3/hadoop_store/hdfs/namenode</value>
 </property>
```

*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

Vamos criar o arquivo com os endereços IP's das máquinas *slaves* do Hadoop no diretório etc/hadoop:

```bash
~# touch slaves
```

Vamos adicionar o IP de todas as máquinas *slaves* em etc/hadoop/slaves:

```bash
~# vim slaves
```

Adicione os endereços IP's ou *hostname* de cada uma das máquinas slaves:

```
IP_NODE_01
IP_NODE_02
IP_NODE_03
IP_NODE_04
IP_NODE_05
IP_NODE_0N
.
.
.
```

*ps: as informações deste arquivo devem ser apenas os IP's ou Hostname das máquinas separadas por quebra de linha (enter)

### Para os nós Slaves

Para os nós *slaves* iremos manter a configuração do *datanode*, removendo a propriedade *namenode*:

```xml
 <property>
  <name>dfs.replication</name>
  <value>1</value>
  <description>Default block replication.
  The actual number of replications can be specified when the file is created.
  The default is used if replication is not specified in create time.
  </description>
 </property>
 
 <property>
   <name>dfs.datanode.data.dir</name>
   <value>file:/opt/bigdata/hadoop-2.7.3/hadoop_store/hdfs/datanode</value>
 </property>
```

*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

### Para ambos

Para ambas as máquinas tanto *master* e *slave* iremos manter a configuração do diretório tmp e iremos configurar o IP do nó *master* (*Namenode*) com a propriedade fs.defaultFS:

```xml
 <property>
  <name>hadoop.tmp.dir</name>
  <value>/opt/bigdata/hadoop-2.7.3/tmp</value>
  <description>Temporary Directory.</description>
 </property>

 <property>
  <name>fs.defaultFS</name>
  <value>hdfs://IP_MASTER:9000</value>
  <description>Use HDFS as file storage engine</description>
 </property>
```

*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

Vamos editar e adicionar as seguintes propriedades ao arquivo yarn-site.xml:

```xml
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
```

*ps: a tag ```</property>``` deve ficar dentro de uma tag ```<configuration></configuration>```

## Finalização

Após as devidas configurações, logue via ssh na máquina master e formate o *namenode*:

```bash
~# hadoop namenode -format
```

Agora vamos iniciar o Hadoop e verificar se ele está funcionando em modo cluster:

```bash
~# start-dfs.sh && start-yarn.sh
```

Se tudo der certo, verifique os processos Hadoop que estão executando no nó master:

```bash
~# jps
```

Como você está acessando o nó *master* deve aparecer para você apenas os seguintes processos:

```bash
5417 Jps
4656 NameNode
5123 ResourceManager
4952 SecondaryNameNode
```

Acesse cada um dos nós *slaves* e verifique os processos que Hadoop que estão executando:

```bash
~# jps
```

Como você está acessando os nós *slave* deve aparecer para você apenas os seguintes processos:

```bash
14372 Jps
13987 DataNode
14168 NodeManager
```

Se tudo estiver ok, pronto temos o Apache Hadoop configurado em modo *Multi Node* ;)

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/sheldon-fuck.gif" style="width: 60%;"/>
</p>

## Disclaimer

Material desenvolvido durante o meu Mestrado no Instituto de Ciências Matemáticas e de Computação da Universidade de São Paulo (<a href="http://icmc.usp.br/" target="_blank">ICMC-USP</a>). Além disso, faço um agradecimento em especial ao Laboratório de Sistemas Distribuídos e Programação Concorrente (<a href="http://lasdpc.icmc.usp.br/" target="_blank">LaSDPC</a>), o qual faço parte que me permitiu a criação deste material. Por fim, informo que é permitido livremente a reprodução integral desse material desde que sejam feitas as devidas referências ao autor ;)

## Referências

[1] Apache Foundation. Hadoop: ***Setting up a Single Node Cluster***. Acessado em Agosto de 2016. Disponível em: [http://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html](http://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html)

[2] Apache Foundation. Hadoop: ***Hadoop Cluster Setup***. Acessado em Outubro de 2016. Disponível em:
[http://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html](http://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html)

[3] Michael G. Noll. Running Hadoop on Ubuntu Linux (Multi-Node Cluster). Acessado em Outbro de 2016. Disponível em: [http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-multi-node-cluster/](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-multi-node-cluster/)