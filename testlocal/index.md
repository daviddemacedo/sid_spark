# Test du programme avec l'installation locale

L'exécution de notre script Python nous donne le résultat suivant : 
```
√ jdd % time spark-submit --master "local[1]" ./incidents_corp.py
20/04/29 15:06:23 WARN Utils: Your hostname, MacBook-Pro-de-David.local resolves to a loopback address: 127.0.0.1; using 192.168.1.127 instead (on interface en0)
20/04/29 15:06:23 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
20/04/29 15:06:23 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
20/04/29 15:06:24 INFO SparkContext: Running Spark version 2.4.5
20/04/29 15:06:24 INFO SparkContext: Submitted application: incidents_corp.py
20/04/29 15:06:24 INFO SecurityManager: Changing view acls to: daviddemacedo
20/04/29 15:06:24 INFO SecurityManager: Changing modify acls to: daviddemacedo
20/04/29 15:06:24 INFO SecurityManager: Changing view acls groups to: 
20/04/29 15:06:24 INFO SecurityManager: Changing modify acls groups to: 
20/04/29 15:06:24 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users  with view permissions: Set(daviddemacedo); groups with view permissions: Set(); users  with modify permissions: Set(daviddemacedo); groups with modify permissions: Set()
20/04/29 15:06:24 INFO Utils: Successfully started service 'sparkDriver' on port 60374.
20/04/29 15:06:24 INFO SparkEnv: Registering MapOutputTracker
20/04/29 15:06:24 INFO SparkEnv: Registering BlockManagerMaster
20/04/29 15:06:24 INFO BlockManagerMasterEndpoint: Using org.apache.spark.storage.DefaultTopologyMapper for getting topology information
20/04/29 15:06:24 INFO BlockManagerMasterEndpoint: BlockManagerMasterEndpoint up
.
.
.
20/04/29 15:09:36 INFO DAGScheduler: ResultStage 9 (showString at NativeMethodAccessorImpl.java:0) finished in 0,908 s
20/04/29 15:09:36 INFO DAGScheduler: Job 6 finished: showString at NativeMethodAccessorImpl.java:0, took 8,570877 s
20/04/29 15:09:36 INFO CodeGenerator: Code generated in 7.7283 ms
+-----------+--------------------+-----+
|Code_postal|             NOM_COM|count|
+-----------+--------------------+-----+
|      75116|PARIS-16E-ARRONDI...| 7584|
|      75016|PARIS-16E-ARRONDI...| 7584|
|      97410|        SAINT-PIERRE| 7428|
|      17000|         LA ROCHELLE| 6525|
|      75012|PARIS-12E-ARRONDI...| 6424|
|      31400|            TOULOUSE| 6323|
|      31000|            TOULOUSE| 6323|
|      31100|            TOULOUSE| 6323|
|      31500|            TOULOUSE| 6323|
|      31300|            TOULOUSE| 6323|
|      31200|            TOULOUSE| 6323|
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
only showing top 20 rows

20/04/29 15:09:36 INFO SparkContext: Invoking stop() from shutdown hook
20/04/29 15:09:36 INFO SparkUI: Stopped Spark web UI at http://192.168.1.127:4040
20/04/29 15:09:36 INFO MapOutputTrackerMasterEndpoint: MapOutputTrackerMasterEndpoint stopped!
20/04/29 15:09:36 INFO MemoryStore: MemoryStore cleared
20/04/29 15:09:36 INFO BlockManager: BlockManager stopped
20/04/29 15:09:36 INFO BlockManagerMaster: BlockManagerMaster stopped
20/04/29 15:09:36 INFO OutputCommitCoordinator$OutputCommitCoordinatorEndpoint: OutputCommitCoordinator stopped!
20/04/29 15:09:36 INFO SparkContext: Successfully stopped SparkContext
20/04/29 15:09:36 INFO ShutdownHookManager: Shutdown hook called
20/04/29 15:09:36 INFO ShutdownHookManager: Deleting directory /private/var/folders/5n/dg6h3vbd3b74jhhk21gnwf9w0000gn/T/spark-9856caad-78bf-4b75-823d-b74f38c1b678
20/04/29 15:09:36 INFO ShutdownHookManager: Deleting directory /private/var/folders/5n/dg6h3vbd3b74jhhk21gnwf9w0000gn/T/spark-f8f7b0bc-29a3-4ee8-8bff-589440127a70
20/04/29 15:09:36 INFO ShutdownHookManager: Deleting directory /private/var/folders/5n/dg6h3vbd3b74jhhk21gnwf9w0000gn/T/spark-9856caad-78bf-4b75-823d-b74f38c1b678/pyspark-7f669051-5192-493a-9c74-1b601260d46d
spark-submit ./incidents_corp.py  68,93s user 15,06s system 454% cpu 18,480 total
```

On retrouve bien le classement des 20 villes avec le plus d'incidents corporelles sur les 10 dernières années. <br>
A noter qu'on pourrait avoir en sortie plus de ville en mettant un chiffre dans () de final.show(). 
Par exemple final.show(30) nous donnerait en sortie les 3O villes avec le plus d'incidents corporels sur les 10 dernières années: 
```
+-----------+--------------------+-----+
|Code_postal|             NOM_COM|count|
+-----------+--------------------+-----+
|      75116|PARIS-16E-ARRONDI...| 7584|
|      75016|PARIS-16E-ARRONDI...| 7584|
|      97410|        SAINT-PIERRE| 7428|
|      17000|         LA ROCHELLE| 6525|
|      75012|PARIS-12E-ARRONDI...| 6424|
|      31200|            TOULOUSE| 6323|
|      31500|            TOULOUSE| 6323|
|      31100|            TOULOUSE| 6323|
|      31000|            TOULOUSE| 6323|
|      31400|            TOULOUSE| 6323|
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
|      75015|PARIS-15E-ARRONDI...| 4592|
|      75019|PARIS-19E-ARRONDI...| 4516|
|      34070|         MONTPELLIER| 4461|
|      34090|         MONTPELLIER| 4461|
|      34000|         MONTPELLIER| 4461|
|      34080|         MONTPELLIER| 4461|
|      75011|PARIS-11E-ARRONDI...| 4243|
|      35700|              RENNES| 4200|
|      35000|              RENNES| 4200|
|      35200|              RENNES| 4200|
+-----------+--------------------+-----+
only showing top 30 rows
```

On pourrait également écrire dans un fichier le contenu de la variable final via la commande `final.write.csv("/tmp/myresults.csv")`

On se retrouve avec plusieurs fichier dans le répertoire myresults.csv: 
```
-rw-r--r--   1 daviddemacedo  staff   2424 29 avr 15:28 part-00000-faa8b358-20b3-4fab-8226-33f2d7d1b8d9-c000.csv
-rw-r--r--   1 daviddemacedo  staff   2279 29 avr 15:28 part-00001-faa8b358-20b3-4fab-8226-33f2d7d1b8d9-c000.csv
-rw-r--r--   1 daviddemacedo  staff   2251 29 avr 15:28 part-00002-faa8b358-20b3-4fab-8226-33f2d7d1b8d9-c000.csv
-rw-r--r--   1 daviddemacedo  staff   2273 29 avr 15:28 part-00003-faa8b358-20b3-4fab-8226-33f2d7d1b8d9-c000.csv
-rw-r--r--   1 daviddemacedo  staff   2453 29 avr 15:28 part-00004-faa8b358-20b3-4fab-8226-33f2d7d1b8d9-c000.csv
-rw-r--r--   1 daviddemacedo  staff   2420 29 avr 15:28 part-00005-faa8b358-20b3-4fab-8226-33f2d7d1b8d9-c000.csv
-rw-r--r--   1 daviddemacedo  staff   2620 29 avr 15:28 part-00006-faa8b358-20b3-4fab-8226-33f2d7d1b8d9-c000.csv
-rw-r--r--   1 daviddemacedo  staff   2118 29 avr 15:28 part-00007-faa8b358-20b3-4fab-8226-33f2d7d1b8d9-c000.csv
...
```
La commande `cat part-000* > result.csv` permet de tout regrouper dans le fichier result.csv:
```
75016,PARIS-16E-ARRONDISSEMENT,7584
75116,PARIS-16E-ARRONDISSEMENT,7584
97410,SAINT-PIERRE,7428
17000,LA ROCHELLE,6525
75012,PARIS-12E-ARRONDISSEMENT,6424
31500,TOULOUSE,6323
31100,TOULOUSE,6323
31400,TOULOUSE,6323
31300,TOULOUSE,6323
31200,TOULOUSE,6323
31000,TOULOUSE,6323
42100,SAINT-ETIENNE,6123
97400,SAINT-DENIS,5964
30900,NIMES,5906
75017,PARIS-17E-ARRONDISSEMENT,5606
75013,PARIS-13E-ARRONDISSEMENT,5136
97490,SAINT-DENIS,4970
75018,PARIS-18E-ARRONDISSEMENT,4966
75008,PARIS-8E-ARRONDISSEMENT,4798
75020,PARIS-20E-ARRONDISSEMENT,4756
75015,PARIS-15E-ARRONDISSEMENT,4592
75019,PARIS-19E-ARRONDISSEMENT,4516
34000,MONTPELLIER,4461
34090,MONTPELLIER,4461
34080,MONTPELLIER,4461
34070,MONTPELLIER,4461
75011,PARIS-11E-ARRONDISSEMENT,4243
35000,RENNES,4200
35200,RENNES,4200
35700,RENNES,4200
75014,PARIS-14E-ARRONDISSEMENT,4065
67000,STRASBOURG,4054
67200,STRASBOURG,4054
67100,STRASBOURG,4054
76610,LE HAVRE,3796
49400,SAUMUR,3695
75010,PARIS-10E-ARRONDISSEMENT,3276
37100,TOURS,3274
37000,TOURS,3274
37200,TOURS,3274
11100,NARBONNE,3136
30000,NIMES,2953
44300,NANTES,2817
44200,NANTES,2817
...
```

Pour la suite de ce TP, nous n'allons pas écrire en disque les résultats. 


[Page Suivante](https://daviddemacedo.github.io/sid_spark/openstack/)

[Retour Sommaire](https://daviddemacedo.github.io/sid_spark/)
