Nous avons choisit de coder en language Python. 
Python étant de plus en plus présent dans nos environnements de travail, il nous est paru intéressant de commencer à apprendre ce langage via ce TP. 

Le code afin de résoudre la problématique est le suivant : 
```
import sys
import os
import pyspark
from functools import reduce
from pyspark.sql import DataFrame 
from pyspark.sql.functions import *
from pyspark.context import SparkContext
from pyspark.sql.session import SparkSession
sc = SparkContext('local')
spark = SparkSession(sc)


#On importe tous les fichiers caracteristiques*.csv dans le dataframe df
df = spark.read.csv("caracteristiques*.csv", header = "true", inferSchema= True, encoding = "ISO-8859-1")

#On importe le fichier caracteristiques2009 dans un dataframe différent car il a un header différent
df2009 = spark.read.csv("2009_caracteristiques.csv", header = "true", inferSchema= True, encoding = "ISO-8859-1", sep = '\t')

# On assemble les 2 dataframes
dfs = [df, df2009]
df_complete = reduce(DataFrame.unionAll, dfs)

# On importe le fichier code-postal-code-insee-2015.csv dans la dataframe com
com = spark.read.csv("code-postal-code-insee-2015.csv", header = "true", inferSchema= True, encoding = "ISO-8859-1", sep = ";")

# On crée des nouvelles dataframes avec les champs qui nous intéresse
com1 = com.select("INSEE_COM", "Code_postal", "NOM_COM")
df1 = df_complete.select("an", "com", "dep")

#Le champ dep de la DF df1 à un 0 qu'on doit supprimer afin de le faire correspondre avec un champ de la DF com1, on supprime le dernier 0 
df1 = df1.withColumn('dep1', when((expr("substring(dep, 3)") == 0) | (expr("substring(dep, 2)") == 0), expr("substring(dep, 1, length(dep)-1)")).otherwise(col("dep")))
# concat de 2 colonnes : 
df2=df1.select("*", concat(col("dep1"),lit(""),col("com")).alias("code_isee"))

#On crée une DF à partir des DF df2 & com1 
dfcom = df2.join(com1, df2.code_isee == com1.INSEE_COM, 'inner')

# On groupe les données selon la problématique
final = dfcom.groupBy("Code_postal", "NOM_COM").count().sort("count",ascending=False)
final.show()
```

