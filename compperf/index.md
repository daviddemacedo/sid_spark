## Exécution du programme en stand-alone
Afin d'effectuer la comparaison de notre programme en mode local contre le mode cluster, nous allons dans un premier temps lancer notre script en local sur une des machines du cluster (en effet, il ne nous paraît pas très adéquate de comparer les performances entre le lancement de notre application sur nos pc personnels et le cluster via les instances fournis par l'université. Nos machines personnelles sont bien plus puissantes et l'environnement est totalement différent). 

Pour cela, nous allons lancer la commande suivante : <br>
`spark-submit --master yarn --deploy-mode client --py-files hdfs://master:9000/scripts/incidents_corp.py hdfs://master:9000/scripts/incidents_corp.py`

Nous obtenons bien le résultat voulu : 
```
+-----------+--------------------+-----+
|Code_postal|             NOM_COM|count|
+-----------+--------------------+-----+
|      75016|PARIS-16E-ARRONDI...| 7584|
|      75116|PARIS-16E-ARRONDI...| 7584|
|      97410|        SAINT-PIERRE| 7428|
|      17000|         LA ROCHELLE| 6525|
|      75012|PARIS-12E-ARRONDI...| 6424|
|      31100|            TOULOUSE| 6323|
|      31400|            TOULOUSE| 6323|
|      31000|            TOULOUSE| 6323|
|      31500|            TOULOUSE| 6323|
|      31200|            TOULOUSE| 6323|
|      31300|            TOULOUSE| 6323|
|      42100|       SAINT-ETIENNE| 6123|
|      97400|         SAINT-DENIS| 5964|
|      30900|               NIMES| 5906|
|      75017|PARIS-17E-ARRONDI...| 5606|
|      75013|PARIS-13E-ARRONDI...| 5136|
|      97490|         SAINT-DENIS| 4970|
|      75018|PARIS-18E-ARRONDI...| 4966|
|      75008|PARIS-8E-ARRONDIS...| 4798|
|      75020|PARIS-20E-ARRONDI...| 4756|
+-----------+--------------------+-----+
```
Et via l'interface web de spark, on peut voir différentes informations intéressantes: <br>
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/spark1.png)
Nous pouvons observer que l'application a mis 58s pour s'exécuter. 
En cliquant sur les différents onglets, on obtient le détaille de l'exécution des différentes tâches de notre programme : <br>
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/spark2.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/spark3.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/spark4.png)

## Exécution du programme en cluster (default)

Nous allons maintenant exécuter notre application en mode cluster. Pour faire fonctionner notre script sur notre cluster yarn, il est nécessaire au préalable d'effectuer quelques changement à ce dernier: 
```
ubuntu@master:~/script$ cat incidents_corp2.py 
import sys
import os
import pyspark
from functools import reduce
from pyspark.sql import DataFrame 
from pyspark.sql.functions import *
from pyspark.context import SparkContext
from pyspark.sql.session import SparkSession
sc = SparkContext()
spark = SparkSession(sc)


#On importe tous les fichiers caracteristiques*.csv dans le dataframe df
df = spark.read.csv("hdfs://master:9000/jdd/caracteristiques*.csv", header = "true", inferSchema= True, encoding = "ISO-8859-1")

#On importe le fichier caracteristiques2009 dans un dataframe différent car il a un header différent
df2009 = spark.read.csv("hdfs://master:9000/jdd/2009_caracteristiques.csv", header = "true", inferSchema= True, encoding = "ISO-8859-1", sep = '\t')

#On assemble les 2 dataframes
dfs = [df, df2009]
df_complete = reduce(DataFrame.unionAll, dfs)

# On importe le fichier code-postal-code-insee-2015.csv dans la dataframe com
com = spark.read.csv("hdfs://master:9000/jdd/code-postal-code-insee-2015.csv", header = "true", inferSchema= True, encoding = "ISO-8859-1", sep = ";")

# On créé des nouvelles dataframes avec les champs qui nous intéresse
com1 = com.select("INSEE_COM", "Code_postal", "NOM_COM")
df1 = df_complete.select("an", "com", "dep")

#Le champ dep de la DF df1 à un 0 qu'on doit supprimer afin de le faire correspondre avec un champ de la DF com1, on supprime le dernier 0 
df1 = df1.withColumn('dep1', when((expr("substring(dep, 3)") == 0) | (expr("substring(dep, 2)") == 0), expr("substring(dep, 1, length(dep)-1)")).otherwise(col("dep")))
# concat de 2 colonnes : 
df2=df1.select("*", concat(col("dep1"),lit(""),col("com")).alias("code_isee"))

#On crée une DF à partir des DF df2 & com1 
dfcom = df2.join(com1, df2.code_isee == com1.INSEE_COM, 'inner')

#On groupe les données selon la problématique
final = dfcom.groupBy("Code_postal", "NOM_COM").count().sort("count",ascending=False)
final.show()
```

Nous enlevons le paramètre `local` de  `SparkContext` afin de permettre l'exécution de notre script en cluster. Cette ligne permet d'instancier un objet de typeSparkContext, qui gère les propriétés globales de notre application. 
Et enfin, nous indiquons le chemin des jeux de données dans notre cluster hadoop, par exemple : `hdfs://master:9000/jdd/2009_ca....`

Pour exécuter l'application en mode cluster, nous utilisons la commande : <br>
`spark-submit --master yarn --deploy-mode cluster --py-files hdfs://master:9000/scripts/incidents_corp2.py hdfs://master:9000/scripts/incidents_corp2.py`

On peut voir dans les logs d'exécution de l'application que cette dernière s'est exécutée avec succès : 
```
2020-04-30 23:08:14,924 INFO yarn.Client: Application report for application_1588085365280_0024 (state: FINISHED)
2020-04-30 23:08:14,929 INFO yarn.Client: 
	 client token: N/A
	 diagnostics: N/A
	 ApplicationMaster host: slave4
	 ApplicationMaster RPC port: 33039
	 queue: default
	 start time: 1588280816537
	 final status: **SUCCEEDED**
	 tracking URL: http://master:8088/proxy/application_1588085365280_0024/
	 user: ubuntu
2020-04-30 23:08:14,986 INFO util.ShutdownHookManager: Shutdown hook called
```

L'interface web de spark nous montre : 
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkcluster1.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkcluster2.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkcluster3.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkcluster4.png)

On observe que notre programme met plus de temps à se finaliser en mode cluster qu'en mode local, certaine tâches mettent plus de temps en mode cluster.
Etant donnée qu'on utilise plus de ressources en mode cluster, on devrait en théorie avoir de meilleures performances en mode cluster. Néanmoins, notre jeux de données n'est pas assez volumineux pour qu'il y ait un intérêt à l'utiliser via une architecture hadoop / spark en cluster. Cette technologie a été crée pour traiter des données très importantes. C'est pour ce type de données très volumineuses qu'un cluster hadoop / spark prend tout son intérêt. 

## Exécution du programme en cluster (paramétrée)
Nous avons pu observer qu'en mode cluster uniquement 2 workers étaient utilisés, alors que potentiellement, spark pourrait en utiliser 6. On a donc 4 instances qui ne font rien, ce qui représente des ressources gâchées.   
Pour remédier à cela, nous avons éditer le fichier `spark-defaults.conf` et ajouter les paramètres suivants : 
```
spark.executor.cores    1
spark.executor.instances  11
```
`spark.executor.cores` indique à spark combien de core utiliser pour les _executor_. Nos instances possèdent 2 core, on indique donc à spark d'utiliser 1 core par _executor_.
`spark.executor.instances` indique à spark le nombre d'_executor_ à instancier. Ici on indique à spark d'initier 11 _executor_. On aura donc 2 _executor_ par instances  avec 1 core pour chaque _executor_. Une instance n'aura qu'un seul _executor_, car le _driver_ sera initialisé sur cette dernière, des ressources seront donc déjà utilisées. 
A noter qu'il est possible d'initialiser autant d'_executor_ tant qu'on a de la mémoire disponible. Lorsqu'on arrivera au maximum de mémoire utilisée spécifié dans les paramètres yarn, spark ne pourra plus initialiser des _executor_.

Exécutons notre application avec cette conf pour observer le résultat : 
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkclusterrep1.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkclusterrep2.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkclusterrep3.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkclusterrep4.png)

Nous remarquons qu'il a fallu encore plus de temps pour traiter notre application: **1,8 min** contre **1,1 min** avec la configuration précédente. 
Néanmoins, on observe que l'ensemble de nos instances ont été utilisées. 2 _executor_ ont étés répartis sur chaque instance comme prévu (et 1 `executor` pour l'instance qui porte le driver). Par contre, la plupart des tâches ont été exécutées uniquement par 2 executor. Il n'y a donc pas vraiment de valeur ajouté à répartir les différentes tâches de notre programme au sein des différents worker. 

## Test de résilience du cluster
Afin de tester la résilience de notre cluster, nos avons également fait un test de panne d'un worker pendant l'exécution de notre application : 
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkfailover1.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkfailover2.png)
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/sparkfailover3.png)

Nous avons pu observer que le cluster s'est rendu compte qu'un _executor_ ne répondait plus. Il a donc recommencé le traitement de notre programme en initialisant 2 nouveaux _executor_. 
Avec ce test, nous avons pu constater que notre cluster était tolérant à la panne d'un noeud. 
