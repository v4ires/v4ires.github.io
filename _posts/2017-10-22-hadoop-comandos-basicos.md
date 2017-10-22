---
layout: post
title: Comandos básicos do Apache Hadoop 2.x
type: article
description: Neste tutorial você aprenderá a aplicar alguns comandos básicos para o gerênciamento do Apache Hadoop
background: /assets/images/hadoop-cmd-basicos.png
thumbnail: /assets/images/hadoop-cmd-basicos-thumbnail.png
img_post: /assets/images/hadoop-cmd-basicos-post.png
slug: hadoop-cmd-basicos
---

## Comandos básicos do Apache Hadoop 2.x

#### Requisitos iniciais:

- 1) Tenha o Apache Hadoop configurado em modo *<a href="{{ site.baseurl }}/config-hadoop-single-node/" target="_blank">single node</a>* ou *<a href="{{ site.baseurl }}/config-hadoop-multi-node/" target="_blank">multi node</a>*

*ps: Este tutorial foi testado com o sistema operacional Ubuntu 16.04 e 14.04. Caso deseje configurar em uma versão mais antiga, recente ou em outro SO, pequenas mudanças podem ocorrer nos procedimentos apresentados.

### TL;DR

O projeto Apache Hadoop é um *software* de código aberto mantido pela Apache Foundation que tem como propósito fornecer uma implementação de código aberto do modelo de programação *MapReduce* de forma confiável e escalável. O Hadoop é projetado para ampliar o processamento de um único servidor em milhares de máquinas, onde cada uma das máquinas oferecem poder de processamento e armazenamento local. Esta ferramenta é utilizada para processamento em *batch* de grandes volumes de dados (*Big Data*). Atualmente, o Apache Hadoop é uma das ferramentas mais conhecidas para processamento distribuído, mas existem outras ferramentas similares que se integram ao Hadoop, como o **Apache Spark**, **Apache Storm** e dentre outros.

*ps: para mais informações a respeito do Apache Hadoop acessem o site [http://hadoop.apache.org/](http://hadoop.apache.org/)

### Let's get started

Neste tutorial você irá aprender à utilizar alguns comandos básicos do Apache Hadoop. Além disso, será mostrado como gerenciar arquivos no sistema de arquivos *Hadoop Distributed File System* (HDFS).

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/trash-pc.gif" style="width: 50%;"/>
</p>

### Hadoop Scripts Tools

O Apache Hadoop fornece por padrão alguns *scripts* para o gerênciamento das instâncias da ferramenta. O diretório padrão onde se encontram os *scripts* é o ```bash hadoop/sbin```.

Como pode ser observado na pasta ```bash hadoop/sbin``` encontra-se os seguintes *scripts* que inclui sua versão para sistemas *bash* (Linux e MacOS) como também para o *prompt* de comando do sistema operacional MS Windows.

```bash
distribute-exclude.sh    start-all.cmd        stop-balancer.sh
hadoop-daemon.sh         start-all.sh         stop-dfs.cmd
hadoop-daemons.sh        start-balancer.sh    stop-dfs.sh
hdfs-config.cmd          start-dfs.cmd        stop-secure-dns.sh
hdfs-config.sh           start-dfs.sh         stop-yarn.cmd
httpfs.sh                start-secure-dns.sh  stop-yarn.sh
kms.sh                   start-yarn.cmd       yarn-daemon.sh
mr-jobhistory-daemon.sh  start-yarn.sh        yarn-daemons.sh
refresh-namenodes.sh     stop-all.cmd
slaves.sh                stop-all.sh
```

Por comodidade é mais prático configurarmos este diretório no arquivo ```bash ~/.bashrc``` ou no ```bash /etc/profile ```. Para isso devemos especificar o caminho ```bash hadoop/sbin``` no *Path* do sistema, adicionando o seguinte comando.

```bash
export HADOOP_INSTALL=/opt/hadoop
export PATH=$PATH:$HADOOP_INSTALL/sbin
```

Os comandos básicos mais utilizados é o de iniciar o sistema de arquivos HDFS e do Hadoop YARN. Como por exemplo:

* Iniciar o sistema de arquivos distribuído HDFS

```bash
~$ start-dfs.sh
```

* Iniciar o Hadoop Yarn

```bash
~$ start-yarn.sh
```

* Finalizar os processos do sistema de arquivos HDFS

```bash
~$  stop-dfs.sh
```

* Finalizar os processos o Hadoop Yarn

```bash
~$ stop-yarn.sh
```

Existem alguns *scripts deprecated* neste diretório, mas que ainda funcionam perfeitamente com o Hadoop:

* Inicia todos os processos relacionados com Hadoop e o HDFS

```bash
~$ start-all.sh
```

* Finaliza todos os processos relacionados com Hadoop e o HDFS

```bash
~$ stop-all.sh
```

### Comando Básicos do Hadoop

O Apache Hadoop fornece alguns arquivos binários onde podem ser realizadas algumas operações importantes. O diretório padrão onde se encontra os binários é o ```bash hadoop/bin```, nele você encontra os seguintes binários:

```bash
container-executor  hdfs      mapred.cmd               yarn
hadoop              hdfs.cmd  rcc                      yarn.cmd
hadoop.cmd          mapred    test-container-executor
```

Por comodidade é mais prático configurarmos este diretório no arquivo ```bash ~/.bashrc``` ou no ```bash /etc/profile ```. Para isso devemos especificar o caminho ```bash hadoop/bin``` no *Path* do sistema, adicionando o seguinte comando.

```bash
export HADOOP_INSTALL=/opt/hadoop
export PATH=$PATH:$HADOOP_INSTALL/bin
```

*ps: Não se esqueça de atualizar as variáveis de ambiente com o comando ```bash source ~/.bashrc``` our ```bash /etc/profile```

O comando mais utilizado é o HDFS, com ele é possível executar as rotinas *MapReduce*. Para executar um código *MapReduce* basta executar o seguinte comando:

```bash
~$ hadoop jar WordCount.jar WordCount /input /output
```

Com o comando HDFS também é possível gerenciar os arquivos que estão no sistema de arquivos do Hadoop. A seguir e demonstrado alguns exemplos básicos de comando básicos no sistema de arquivos HDFS, note que todos os comando apresentados se assemelham ao comandos do *bash*.

*ps: Todos os comando inciam com o prefixo ```bash hdfs dfs```, mas existe também a possibilidade de utilizar o comando depreciado ```bash hadoop fs```.

Criar um diretório:

```bash
#Comando
hdfs dfs -mkdir <paths>
#Exemplo
~$ hdfs dfs -mkdir /user/hduser/input
```

Listar diretórios:

```bash
#Comando
hdfs dfs -ls <args>
#Exemplo
~$ hdfs dfs -ls /user
```

Fazer *upload* de um arquivo:

```bash
#Comando
hdfs dfs -put <file_path> <hdfs_path>
#Exemplo
~$ hdfs dfs -put my_file.txt /user/hduser/input
```

Fazer *download* de um arquivo:

```bash
#Comando
hdfs dfs -get <hdfs_path> <local_path>
#Exemplo
~$ hdfs dfs -get /user/hduser/input/my_file.txt ~/
```

Remover um arquivo:

```bash
#Comando
hdfs dfs -r <file_name>  
#Exemplo
~$ hdfs dfs -r /user/hduser/input/my_file.txt
```

Remover um diretório:

```bash
#Comando
hdfs dfs -rmr <hdfs_path>
#Exemplo
~$ hdfs dfs -rmr /user/hduser/input
```

Renomear um arquivo:

```bash
#Comando
hdfs dfs -mv <file_name> <file_name>
#Exemplo
~$ hdfs dfs -mv /user/hduser/input/my_file.txt /user/hduser/input/file.txt
```

Mover um arquivo:

```bash
#Comando
hdfs dfs -mv <hdfs_src> <hdfs_dest>
#Exemplo
~$ hdfs dfs -mv /user/hduser/input/my_file.txt /user/hduser
```

Mover um diretório:

```bash
#Comando
hdfs dfs -mv <hdfs_dir> <hdfs_dir>
#Exemplo
~$ hdfs dfs -mv /user/hduser/input /user
```

Mostrar o tamanho do arquivo em *bytes*:

```bash
#Comando
hdfs dfs -du <file or directory>
#Exemplo
~$ hdfs dfs -du /user/hduser/input/my_file.txt
~$ hdfs dfs -du /user/hduser/input/
```

Visualizar o conteúdo de um arquivo:

```bash
#Comando
hdfs dfs -cat <file>
#Exemplo
~$ hdfs dfs -cat /user/hduser/input/my_file.txt
```

Agora você aprendeu alguns comandos básicos do Hadoop. Agora você já pode gerenciar arquivos no HDFS e aplicar algumas operações básicas :)

<p style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/images/coding-awesome.gif" style="width: 60%;"/>
</p>

## Disclaimer

Material desenvolvido durante o meu Mestrado no Instituto de Ciências Matemáticas e de Computação da Universidade de São Paulo (<a href="http://icmc.usp.br/" target="_blank">ICMC-USP</a>). Além disso, faço um agradecimento em especial ao Laboratório de Sistemas Distribuídos e Programação Concorrente (<a href="http://lasdpc.icmc.usp.br/" target="_blank">LaSDPC</a>), o qual faço parte que me permitiu a criação deste material. Por fim, informo que é permitido livremente a reprodução integral desse material desde que sejam feitas as devidas referências ao autor ;)

## Referências

Apache Foundation. Hadoop: **Apache Hadoop**. Acessado em Agosto de 2016. Disponível em: [http://hadoop.apache.org](http://hadoop.apache.org)
Dzone. Dzone/Big Data Zone. ***Top 10 Hadoop Shell Commands to Manage HDFS***. Acessado em Outubro de 2016. Disponível em [https://dzone.com/articles/top-10-hadoop-shell-commands](https://dzone.com/articles/top-10-hadoop-shell-commands)