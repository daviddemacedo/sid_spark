# Installez Spark sur votre machine locale Windows 10

##  Etape 1 : Téléchargez les composants

### a) Téléchargement de Spark
La version de Spark est disponible sur le site d'Apache https://spark.apache.org/downloads.html
![](https://user-images.githubusercontent.com/54117403/79692135-cf544100-8263-11ea-92fb-1b46cbde48a7.png)
 
### b) Téléchargez Java 8
La version 8 de Java https://www.java.com/fr/download/
![](https://user-images.githubusercontent.com/54117403/79693224-b058ad80-8269-11ea-9653-bacfdd78df2f.PNG)

### c) Télécharger Anaconda
Télécharger Anaconda version 3.7 https://www.anaconda.com/
![](https://user-images.githubusercontent.com/54117403/79695176-a76dd900-8275-11ea-862c-44e69e3c7dd6.png)

### d) Télécharger le fichier winutils.exe
Télécharger le fichier sur GitHub https://github.com/steveloughran/winutils/tree/master/hadoop-2.7.1/bin

![](https://user-images.githubusercontent.com/54117403/79695364-b739ed00-8276-11ea-8e3a-62a63b405112.png)

##  Etape 2 : Installez les composants

### a) Installation de Java 8
Lancez l'installation via le setup <br>
Puis Next<br>
![](https://user-images.githubusercontent.com/54117403/79695534-b2296d80-8277-11ea-8714-3ae182591d35.png)<br>
Puis Next<br>
![](https://user-images.githubusercontent.com/54117403/79695576-ec930a80-8277-11ea-84c4-8be9d9ce7d88.png)<br>
Installation...<br>
![](https://user-images.githubusercontent.com/54117403/79695595-0df3f680-8278-11ea-9849-bec4b6243d18.png)<br>
Choisissez l'emplacement d'installation<br>
![](https://user-images.githubusercontent.com/54117403/79695616-2b28c500-8278-11ea-8310-6112c63a6363.png)<br>
<br>
![](https://user-images.githubusercontent.com/54117403/79695783-0b45d100-8279-11ea-8fe0-3428789bbf7c.png)<br>

Ouvrez une console de commande Windows pour vérifier la version installée<br>
![](https://user-images.githubusercontent.com/54117403/79695896-a9d23200-8279-11ea-8c15-ec06e4fd18a1.png)

### b) Installation d'Anaconda
Lancez l'installateur _**Anaconda3.2020/02-Windows-x86_64.exe**_ <br>
![](https://user-images.githubusercontent.com/54117403/79696159-37fae800-827b-11ea-9729-f863568f7601.png)<br>

Sélectionnez All users<br>
![](https://user-images.githubusercontent.com/54117403/79696191-6d9fd100-827b-11ea-8737-4b38c36630a1.png)<br>
<br>
![](https://user-images.githubusercontent.com/54117403/79696208-96c06180-827b-11ea-9153-139de96a93e7.png)
<br>
Vérifiez l'installation de Python avec une fenêtre commande windows <br>
![](https://user-images.githubusercontent.com/54117403/79696601-ec960900-827d-11ea-9b11-ef4e13c5be7e.png)
 
### c) Installation de winutils.exe
Créez sous C: un dossier _**hadoop**_. Puis sous hadoop un dossier **_bin_**.<br>
Puis déposez dans _**bin**_ le fichier _**winutils.exe**_ <br>


### d) Installation de Spark
Décompressez sous C: le fichier téléchargé _**spark-2.4.5-bin-hadoop2.7.gz**_ <br>
![](https://user-images.githubusercontent.com/54117403/79696914-d2f5c100-827f-11ea-8281-e8adf8e7bbb8.png)<br>
<br>
Dans le répertoire conf supprimez les _**.template**_ dans les noms des fichiers suivants :<br>
(Exemple : _**docker.properties.template**_  =>  _**docker.properties**_ )<br>
![](https://user-images.githubusercontent.com/54117403/79698188-8c0bc980-8287-11ea-8218-765364a84d07.png)<br>

Allez dans Panneau de configuration / Système et sécurité / Système / Paramètres système avancés <br>
Cliquez sur ![](https://user-images.githubusercontent.com/54117403/79698366-c164e700-8288-11ea-951e-ae5debf4efb6.png)<br>
![](https://user-images.githubusercontent.com/54117403/79698407-ff620b00-8288-11ea-8c7d-3c3fd4dc01a0.png)<br>

Dans Variables utilisateurs cliquez sur Nouvelle<br> 
Procédez aux créations suivantes :<br>
Nom de la variable : **JAVA_HOME**    Valeur de la variable : **C:\Program Files\Java\jre1.8.0_241**<br>
Nom de la variable : **HADOOP_HOME**  Valeur de la variable : **C:\hadoop\bin**<br>
Nom de la variable : **SPARK_HOME**   Valeur de la variable : **C:\spark-2.4.5-bin-hadoop2.7**<br>
<br>
Dans les **Variables systèmes** appuyez sur Nouvelle et procédez à la création suivante <br>
Nom de la variable : **SPARK_PYTHON**   Valeur de la variable : **C:\ProgramData\Anaconda3\python.exe** <br>
<br>
Toujours dans les **Variables systèmes** positionnez-vous sur **Path** et cliquez sur Modifier <br>
Ajoutez les lignes suivantes via le bouton **Nouveau** <br>
%SPARK_HOME%\bin <br>
%HADOOP_HOME%\bin <br>
%JAVA_HOME%\bin <br>

Refermez les fenêtres et testez le lancment de Spark avec Python via la console Anaconda <br>
![](https://user-images.githubusercontent.com/54117403/79699621-f117ed00-8290-11ea-8854-8ac556a21ec6.png)
<br>
Dans la console tapez **pyspark** <br> et vérifiez que le message "**Using Python**" est présent <br>
![](https://user-images.githubusercontent.com/54117403/79699755-b2366700-8291-11ea-9bf8-94c17d452cc3.png)



[Page Suivante](https://daviddemacedo.github.io/sid_spark/choixjdd/)

[Retour Sommaire](https://daviddemacedo.github.io/sid_spark/)










