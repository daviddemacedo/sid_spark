# Installation Cluster Hadoop avec HDFS, YARN et SPARK

Vous trouverez sur cette page, un guide afin d'installer une architecture Hadoop sous ubuntu avec 6 Instances.
Toutes les actions sont à répéter sur chaque instance.

## Préparation des Instances

Au préalable, il est nécessaire d'effectuer des actions sur les instances afin que ces dernières puissent fonctionner en mode cluster et puissent communiquer entre elles. 

Editer le fichier `/etc/hosts` et insérer l'ip de l'instance (172.28.100.xx / xx correspond à l'ip de la machine correspondante) et son nom d'host. Egalement y insérer les ips et nom d'host des autres instances. A la fin, le fichier devrait ressembler à cela : 
```
ubuntu@master:~$ cat /etc/hosts
127.0.0.1 localhost
#Machine Hadoop Cluster
172.28.100.89 slave5
172.28.100.71 slave4
172.28.100.78 slave3
172.28.100.91 slave2
172.28.100.75 slave1
172.28.100.136 master
``` 

Il est également nécessaire que les machines puissent se connecter entre elles. Pour que la connexion se fasse de manière sécurisée et que le mot de passe ne soit pas demandé, nous allons faire des connexions via clés ssh. 
Dans un premier temps, il est nécessaire de générer des clés: <br>
```
ubuntu@slave1:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
```
Copier le contenu de votre clé publique dans le fichier authorized_keys: <br>
`cat id_rsa.pub >> authorized_keys `
Copier également le contenu de la clé publique sur le fichier authorized_keys de toutes les autres instances. 

Certains packages sont nécessaires en pré requis :
```
sudo apt-get update
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev zip unzip
```
##  Installation de Java 

Hadoop nécessite l'installation de Java et Ubuntu avec l'installation minimale n'a pas Java par défaut. Vous pouvez vérifier cela avec la commande :
```
java -version

Command 'java' not found, but can be installed with:

sudo apt install default-jre            
sudo apt install openjdk-11-jre-headless
sudo apt install openjdk-8-jre-headless 
```

Nous allons ignorer ces suggestions et installer Java d'une manière différente.<br>
Hadoop fonctionne sans problème avec Java 8, mais peut rencontrer des bogues avec les versions plus récentes de Java. Nous allons donc installer Java 8 spécifiquement. Pour gérer plusieurs versions de Java, nous allons installer l'utilitaire [sdkman](https://sdkman.io/install) :<br>
`curl -s "https://get.sdkman.io" | bash`

A la fin de l'installation, il faut penser à "sourcer" le bash ou bien fermer le terminal et le réouvrir de nouveau afin que les variables d'environnements soient prises en compte: <br>
`source ~/.bashrc`
Nous allons maintenant utiliser SDKMAN pour installer Java. Vous pouvez lister toutes les versions disponibles de JAVA via :
```
ubuntu@master:~$ sdk list java
================================================================================
Available Java Versions
================================================================================
 Vendor        | Use | Version      | Dist    | Status     | Identifier
--------------------------------------------------------------------------------
 AdoptOpenJDK  |     | 14.0.0.j9    | adpt    |            | 14.0.0.j9-adpt      
               |     | 14.0.0.hs    | adpt    |            | 14.0.0.hs-adpt      
               |     | 13.0.2.j9    | adpt    |            | 13.0.2.j9-adpt      
               |     | 13.0.2.hs    | adpt    |            | 13.0.2.hs-adpt      
               |     | 12.0.2.j9    | adpt    |            | 12.0.2.j9-adpt      
               |     | 12.0.2.hs    | adpt    |            | 12.0.2.hs-adpt      
               |     | 11.0.6.j9    | adpt    |            | 11.0.6.j9-adpt      
               |     | 11.0.6.hs    | adpt    |            | 11.0.6.hs-adpt      
               |     | 8.0.242.j9   | adpt    |            | 8.0.242.j9-adpt     
               |     | 8.0.242.hs   | adpt    |            | 8.0.242.hs-adpt     
 Amazon        |     | 11.0.7       | amzn    |            | 11.0.7-amzn         
....
```

Nous allons installer la version de java 8.0.242.hs de AdoptOpenJDK via la commande suivante :<br> 
`ubuntu@master:~$ sdk install java 8.0.242.hs-adpt`

A la fin de l'installation, nous pouvons vérifier la version de java installer : 
```
ubuntu@master:~$ java -version
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_242-b08)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.242-b08, mixed mode)
```

L'avantage avec sdkman, c'est qu'il est possible simplement d'installer plusieurs version de java. Il suffit ensuite d'indiquer la version de java à utiliser : 
`sdk use java 13.0.1.hs-adpt`
Pour ce tp, nous resterons sur la version de java 8. 

Nous devons également définir explicitement la variable d'environnement JAVA_HOME en l'ajoutant au fichier ~/.bashrc : <br>
`echo "export JAVA_HOME=\$(readlink -f \$(which java) | sed 's:bin/java::')" >> ~/.bashrc`

echo JAVA_HOME devrait maintenant nous donner le chemin vers le répertoire SDKMAN (penser à ce sourcer de nouveau):
```
ubuntu@master:~$ echo $JAVA_HOME
/home/ubuntu/.sdkman/candidates/java/8.0.242.hs-adpt/
```

## Installation de Python

Comme pour Java,  nous allons installer un utilitaire qui permet de gérer plusieurs versions de python sur nos instances, pyenv: 
```
curl https://pyenv.run | bash
```
Ajouter les lignes suivantes à ~/.bashrc:
```
export PATH="/home/ubuntu/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
Et enfin réinitialiser votre bash: <br> 
`exec $SHELL`
Nous allons maintenant installer Python via pyenv: 
```
pyenv install 3.8.2
pyenv global 3.8.2
```
Vérification que python est bien installé : 
```
ubuntu@slave1:/opt/hadoop/etc/hadoop$ python -V
Python 3.8.2
ubuntu@slave1:/opt/hadoop/etc/hadoop$ pip -V
pip 19.2.3 from /home/ubuntu/.pyenv/versions/3.8.2/lib/python3.8/site-packages/pip (python 3.8)
```

## Installation de Hadoop 

Une fois Java installé, l'étape suivante consiste à installer Hadoop. Vous pouvez [obtenir la version la plus récente de Hadoop sur le site web d'Apache](https://hadoop.apache.org/releases.html). Au moment où nous écrivons ces lignes, cette version est Hadoop 3.2.1 (sortie le 22 septembre 2019). Nous allons télécharger cette version via wget : <br>
`ubuntu@master:~$ wget https://mirrors.ircam.fr/pub/apache/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz`

Décompressez l'archive avec tar vers le répertoire /opt/ : <br>
`sudo tar -xvf hadoop-3.2.1.tar.gz -C /opt/`

Supprimez le fichier d'archive et déplacez-vous dans le répertoire /opt/ :
```
ubuntu@master:~$ rm hadoop-3.2.1.tar.gz && cd /opt
ubuntu@master:/opt$ 
```
Renommez le répertoire Hadoop et modifiez ses autorisations afin qu'il vous appartienne : <br>
`ubuntu@master:/opt$ sudo mv hadoop-3.2.1 hadoop && sudo chown ubuntu: -R hadoop`

Enfin, définissez la variable d'environnement HADOOP_HOME et ajoutez les binaires Hadoop à votre PATH en faisant echo des lignes suivantes et en les concaténant à votre fichier ~/.bashrc :
```
ubuntu@master:/opt$ echo "export HADOOP_HOME=/opt/hadoop" >> ~/.bashrc
ubuntu@master:/opt$ echo "export PATH=\$PATH:\$HADOOP_HOME/bin:\$HADOOP_HOME/sbin" >> ~/.bashrc
```
Maintenant, lorsque vous sourcez votre ~/.bashrc (ou que vous ouvrez un nouveau shell), vous devriez pouvoir vérifier que Hadoop a été installé correctement :
```
ubuntu@master:~$ hadoop version
Hadoop 3.2.1
Source code repository https://gitbox.apache.org/repos/asf/hadoop.git -r b3cbbb467e22ea829b3808f4b7b01d07e0bf3842
Compiled by rohithsharmaks on 2019-09-10T15:56Z
Compiled with protoc 2.5.0
From source with checksum 776eaf9eee9c0ffc370bcbc1888737
This command was run using /opt/hadoop/share/hadoop/common/hadoop-common-3.2.1.jar
```
Pour que le HDFS fonctionne correctement par la suite, nous devons également définir JAVA_HOME dans le fichier /opt/hadoop/etc/hadoop/hadoop-env.sh. Trouvez la ligne dans ce fichier qui commence par `# export JAVA_HOME=` 
et l'éditer pour qu'elle corresponde à la variable JAVA_HOME que nous avons définie précédemment : <br>
`export JAVA_HOME=/home/ubuntu/.sdkman/candidates/java/8.0.242.hs-adpt/`

## Installation de Spark

Le dernier logiciel que nous voulons installer est Apache Spark. Nous allons l'installer de la même manière que nous avons installé Hadoop, ci-dessus. Tout d'abord, récupérez le fichier *.tgz le plus récent sur le site web de [Apache Spark](https://spark.apache.org/). Nous avons téléchargé la version de Spark 3.0.0 (Dec 23 2019) pré-built pour Apache Hadoop 3.2 et plus avec la commande : <br>
`ubuntu@master:~$ wget http://apache.crihan.fr/dist/spark/spark-3.0.0-preview2/spark-3.0.0-preview2-bin-hadoop3.2.tgz`

Comme pour Hadoop, décompressez l'archive avec tar vers le répertoire /opt/ :<br>
`ubuntu@master:~$ sudo tar xvf spark-3.0.0-preview2-bin-hadoop3.2.tgz -C /opt/`

Supprimez le fichier d'archive et déplacez-vous dans le répertoire /opt/ : <br>
`ubuntu@master:~$ rm spark-3.0.0-preview2-bin-hadoop3.2.tgz && cd /opt/`

Renommez le répertoire Spark et modifiez ses autorisations afin qu'il vous appartienne: <br>
`ubuntu@master:/opt$ sudo mv spark-3.0.0-preview2-bin-hadoop3.2/ spark && sudo chown ubuntu: -R spark`

Enfin, définissez la variable d'environnement SPARK_HOME et ajoutez les binaires Spark à votre PATH en faisant echo des lignes suivantes et en les concaténant à votre fichier ~/.bashrc :
```
ubuntu@master:/opt$ echo "export SPARK_HOME=/opt/spark" >> ~/.bashrc
ubuntu@master:/opt$ echo "export PATH=\$PATH:\$SPARK_HOME/bin" >> ~/.bashrc
```
Maintenant, lorsque vous sourcez votre ~/.bashrc (ou ouvrez un nouveau shell), vous devriez pouvoir vérifier que Spark a été installé correctement :
```
ubuntu@master:~$ spark-shell --version
...
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 3.0.0-preview2
      /_/
                        
Using Scala version 2.12.10, OpenJDK 64-Bit Server VM, 1.8.0_242
Branch HEAD
```
Pour pouvoir faire fonctionner spark avec l'interpréteur Python _Pyspark_, il est nécessaire d'ajouter les 2 lignes suivantes dans le fichier `/opt/spark/conf/spark-env.sh` : <br>
```
export PYSPARK_PYTHON=/usr/bin/python3
export PYSPARK_DRIVER_PYTHON=/usr/bin/python3 
```
On peut tester que cela fonctionne en tapant directement : 
```
ubuntu@master:/opt/spark/conf$ pyspark 
Python 3.6.9 (default, Apr 18 2020, 01:56:04) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
2020-04-29 22:56:14,266 WARN yarn.Client: Neither spark.yarn.jars nor spark.yarn.archive is set, falling back to uploading libraries under SPARK_HOME.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 3.0.0-preview2
      /_/

Using Python version 3.6.9 (default, Apr 18 2020 01:56:04)
SparkSession available as 'spark'.
>>> 
```

## Configuration du Cluster 

Créez le répertoire suivant et modifiez ses autorisations afin qu'il vous appartienne: 
```
$ sudo mkdir -p /opt/hadoop_tmp/hdfs
$ sudo chown ubuntu: –R /opt/hadoop_tmp
```
Pour que le système de fichiers distribués Hadoop (HDFS) soit opérationnel, nous devons modifier certains fichiers de configuration. Tous ces fichiers se trouvent dans `/opt/hadoop/etc/hadoop` <br>
Le premier est `core-site.xml`, ce dernier permet d'indiquer l'host et le port du système de fichier HDFS. Modifiez-le pour qu'il ressemble à ce qui suit :
```
<configuration>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://master:9000</value>
  </property>
</configuration>
```
Le suivant est `hdfs-site.xml` qui configure où le NameNode va stocker l'historique des transactions et où les DataNode vont stocker leurs blocks. C'est également ici où le coefficient de réplication est configuré. Ce dernier devrait ressembler à :
```
<configuration>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/opt/hadoop_tmp/hdfs/datanode</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/opt/hadoop_tmp/hdfs/namenode</value>
  </property>
  <property>
    <name>dfs.replication</name>
    <value>3</value>
  </property>
</configuration> 
```
La valeur 3 correspond aux nombres de réplications voulues. 

Le fichier suivant est `mapred-site.xml`, nous allons configurer les paramètres spécifiques à MapReduce tel que les paramètres mémoire, ainsi qu'utiliser YARN comme implémentation de MapReduce. Le fichier devrait ressembler à celui-ci :
```
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
  <property>
    <name>yarn.app.mapreduce.am.resource.mb</name>
    <value>512</value>
  </property>
  <property>
    <name>mapreduce.map.memory.mb</name>
    <value>256</value>
  </property>
  <property>
    <name>mapreduce.reduce.memory.mb</name>
    <value>256</value>
  </property>
</configuration> 
```
et enfin `yarn-site.xml`. Ce fichier contient les paramètres de configuration relatifs à YARN. Ce denier devrait ressembler à :
```
<configuration>
  <property>
    <name>yarn.acl.enable</name>
    <value>0</value>
  </property>
  <property>
    <name>yarn.resourcemanager.hostname</name>
    <value>master</value>
  </property>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
  <property>
    <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>  
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
  </property>
  <property>
    <name>yarn.nodemanager.resource.memory-mb</name>
    <value>3072</value>
  </property>
  <property>
    <name>yarn.scheduler.maximum-allocation-mb</name>
    <value>3072</value>
  </property>
  <property>
    <name>yarn.scheduler.minimum-allocation-mb</name>
    <value>256</value>
  </property>
  <property>
    <name>yarn.nodemanager.vmem-check-enabled</name>
    <value>false</value>
  </property>
</configuration>
```

Ensuite, nous devons créer deux fichiers dans `/opt/hadoop/etc/hadoop/` qui indiquent à Hadoop quelle instance doit être utilisée comme worker (slave) et quelle instance doit être le noeud maître (NameNode). Créez un fichier nommé master dans le répertoire susmentionné et n'ajoutez qu'une seule ligne : <br>
```
master
```
Ensuite, créez un fichier nommé `workers`  dans le même répertoire et ajoutez tous les autres instances :
```
slave1
slave2
slave3
slave4
slave5
```
Enfin, redémarrez les instances pour que ces changements prennent effet. Lorsque tous les instances ont redémarré, sur le master lancez la commande :
```
hdfs namenode -format -force
```
Nous démarrons ensuite le HDFS avec les deux commandes suivantes: 
```
start-dfs.sh && start-yarn.sh
```
Nous pouvons tester le cluster en mettant des fichiers dans le HDFS à partir de n'importe quelles instances (en utilisant `hadoop fs -put`) et en nous assurant qu'ils apparaissent sur d'autres instances (en utilisant `hadoop fs -ls`). Vous pouvez également vérifier que le cluster est opérationnel en ouvrant un navigateur web et en naviguant sur http://master:9870. Cette interface web vous donne accès à un explorateur de fichiers ainsi qu'à des informations sur la santé du cluster.

![hadoop img web](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/hadoop.png)

## Explication des paramètres mémoire 

L'allocation de la mémoire peut être délicate sur des instances avec peu de RAM car les valeurs par défaut ne conviennent pas aux instances avec moins de 8 Go de RAM. Nous avons donc essayé de personnaliser les paramètres mémoire pour nos instances qui possèdent 4go de RAM. 

Un job YARN est exécuté avec deux types de ressources :

* Un _Application Master_ (AM) est responsable du suivi de l'application et de la coordination des exécuteurs répartis dans le cluster.
* Certains exécuteurs créés par l'AM exécutent le job. Pour un job MapReduce, ils exécuteront l'opération de type map ou reduce en parallèle.

Les deux sont exécutés dans des conteneurs sur des nœuds type _worker_. Chaque nœud worker exécute un démon NodeManager qui est responsable de la création des conteneurs sur le nœud. L'ensemble du cluster est géré par un ResourceManager qui planifie l'allocation des conteneurs sur tous les nœuds worker, en fonction des besoins de capacité et de la charge actuelle.

Quatre types d'allocation de ressources doivent être configurés correctement pour que le cluster fonctionne. Ces types sont les suivants :
1. Quelle quantité de mémoire peut être allouée aux conteneurs YARN sur un seul nœud. Cette limite doit être supérieure à toutes les autres ; sinon, l'allocation des conteneurs sera rejetée et les demandes échoueront. Toutefois, il ne doit pas s'agir de la totalité de la mémoire vive du nœud.
Cette valeur est configurée dans `yarn-site.xml` avec `yarn.nodemanager.resource.memory-mb`.
2. La quantité de mémoire qu'un seul conteneur peut consommer et l'allocation minimale de mémoire autorisée. Un conteneur ne sera jamais plus grand que le maximum, sinon l'allocation échouera et sera toujours allouée comme un multiple de la quantité minimale de mémoire vive. Ces valeurs sont configurées dans `yarn-site.xml` avec `yarn.scheduler.maximum-allocation-mb` et `yarn.scheduler.minimum-allocation-mb`. 
3. La quantité de mémoire qui sera allouée à l'_Application Master_. Il s'agit d'une valeur constante qui doit correspondre à la taille maximale du conteneur. Ceci est configuré dans `mapred-site.xml` avec `yarn.app.mapreduce.am.resource.mb`.
4. La quantité de mémoire qui sera allouée à chaque opération map ou reduce. Celle-ci doit être inférieure à la taille maximale. Ceci est configuré dans `mapred-site.xml` avec les propriétés `mapreduce.map.memory.mb` et `mapreduce.reduce.memory.mb`.

La relation entre toutes ces propriétés est illustrée dans la figure suivante :
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/memory.png)

## Configurer Spark en cluster

Une partie de la configuration que nous avons effectuée ci-dessus était pour Hadoop YARN. Il s'agit d'un "négociateur de ressources" pour HDFS, qui orchestre la manière dont les fichiers sont déplacés et analysés dans le cluster. Pour que Spark puisse communiquer avec YARN, nous devons configurer deux autres variables d'environnement dans `~/.bashrc`:
```
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export LD_LIBRARY_PATH=$HADOOP_HOME/lib/native:$LD_LIBRARY_PATH
```
`$HADOOP_CONF_DIR` est le répertoire qui contient tous les fichiers de configuration *-site.xml que nous avons édités ci-dessus. Ensuite, nous créons le fichier de configuration Spark : 
```
cd $SPARK_HOME/conf
sudo mv spark-defaults.conf.template spark-defaults.conf
```
Beaucoup de paramètres peuvent être personnalisés dans ce fichier, notamment ceux relatif à la mémoire allouée aux conteneurs spark. En effet, ces derniers pour fonctionner dans des conteneurs YARN peut échouer si l'allocation de la mémoire n'est pas configurée correctement. <br>
Si la mémoire demandée est supérieure au maximum autorisé, YARN refusera la création du conteneur et notre application Spark ne démarrera pas. Il est donc nécessaire au préable de s'assurer que la valeur de `yarn.scheduler.maximum-allocation-mb` dans `$HADOOP_CONF_DIR/yarn-site.xml` (ce paramètre correspond à la valeur maximale autorisée en MB pour un seul conteneur) est supérieure à l'allocation mémoire configurée pour spark. 

Nous avons observé qu'en laissant les paramètres par défaut la mémoire allouée pour spark ne dépassait pas les 3gb configurée pour YARN. Dans un premier temps, nous ne personnalisons pas ces paramètres, nous y reviendrons plus tard dans le TP. 

Lors d'un lancement d'un job, spark démarre une interface web sur le port 4040 afin de suivre l'évolution de l'application. Cette interface web est stoppée à la fin du job spark. 
Afin d'activer en permanence cette interface, nous allons activer le service _history server_ de spark. 
Pour cela, dans le fichier `spark-defaults.conf`, nous allons ajouter les paramètres suivants : 
```
spark.eventLog.enabled           true
spark.eventLog.dir               hdfs://master:9000/logs/
spark.history.fs.logDirectory    hdfs://master:9000/logs/
```
Et enfin démarrer le script : `start-history-server.sh` présent dans `/opt/spark/sbin`.
Cela crée une interface web à l'adresse `http://<server-url>:18080` par défaut, qui historisent les différents lancements d'application spark. 

Enfin pour que ces changement prennent effet il faut redémarrer le cluster : 
```
stop-dfs.sh && stop-yarn.sh

start-dfs.sh && start-yarn.sh
```






