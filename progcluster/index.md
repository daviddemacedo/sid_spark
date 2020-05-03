# Programme porté sur cluster YARN, données stockées sur le système de fichier HDFS

Nous avons au préalable récupéré nos jeux de données sur nos instances. 

Afin de porter notre programme sur le cluster yarn, il faut d'abord y insérer notre jeux de données : 
```
hdfs dfs -mkdir /jdd
hdfs dfs -put 2009_caracteristiques.csv /jdd
hdfs dfs -put caracteristiques* /jdd
hdfs dfs -put code-postal-code-insee-2015.csv /jdd
hdfs dfs -ls /jdd
Found 12 items
-rw-r--r--   1 ubuntu supergroup    4792491 2020-04-23 11:03 /jdd/2009_caracteristiques.csv
-rw-r--r--   1 ubuntu supergroup    4851687 2020-04-23 11:04 /jdd/caracteristiques-2017.csv
-rw-r--r--   1 ubuntu supergroup    4638375 2020-04-23 11:04 /jdd/caracteristiques-2018.csv
-rw-r--r--   1 ubuntu supergroup    5254620 2020-04-23 11:04 /jdd/caracteristiques_2008.csv
-rw-r--r--   1 ubuntu supergroup    4997218 2020-04-23 11:04 /jdd/caracteristiques_2010.csv
-rw-r--r--   1 ubuntu supergroup    4800826 2020-04-23 11:04 /jdd/caracteristiques_2011.csv
-rw-r--r--   1 ubuntu supergroup    4477777 2020-04-23 11:04 /jdd/caracteristiques_2012.csv
-rw-r--r--   1 ubuntu supergroup    4225215 2020-04-23 11:04 /jdd/caracteristiques_2013.csv
-rw-r--r--   1 ubuntu supergroup    4355745 2020-04-23 11:04 /jdd/caracteristiques_2014.csv
-rw-r--r--   1 ubuntu supergroup    4369670 2020-04-23 11:04 /jdd/caracteristiques_2015.csv
-rw-r--r--   1 ubuntu supergroup    4557000 2020-04-23 11:04 /jdd/caracteristiques_2016.csv
-rw-r--r--   1 ubuntu supergroup  116056748 2020-04-23 11:04 /jdd/code-postal-code-insee-2015.csv
```

Nous avons fait de même avec notre script Python, et enfin nous avons également crée un répertoire logs afin que les logs d'exécutions de spark y soient présents:
```
ubuntu@master:/opt/hadoop_tmp/hdfs$ hdfs dfs -ls /
Found 5 items
drwxr-xr-x   - ubuntu supergroup          0 2020-04-18 15:59 /input
drwxr-xr-x   - ubuntu supergroup          0 2020-04-23 11:04 /jdd
drwxr-xr-x   - ubuntu supergroup          0 2020-04-29 13:48 /logs
drwxr-xr-x   - ubuntu supergroup          0 2020-04-24 00:13 /scripts
drwxr-xr-x   - ubuntu supergroup          0 2020-04-18 23:56 /user
```

Il est également possible de visualiser le cluster hadoop via une interface web, ceci en utilisant l'ip du master port 9870, `http://172.28.100.136:9870` : 
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/hdfs.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/hdfs1.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/hdfs2.png)



[Page Suivante](https://daviddemacedo.github.io/sid_spark/compperf/)

[Retour Sommaire](https://daviddemacedo.github.io/sid_spark/)
