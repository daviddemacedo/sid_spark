# Description du problème à résoudre à partir des données

## Problématique
<strong>Connaître les 20 communes où se sont déroulés le plus d’accidents sur les dix dernières années.</strong>
<br>
![](https://user-images.githubusercontent.com/54117403/80631935-cd099800-8a56-11ea-8059-3573ba99ae42.png)

## Sélection des fichiers de données
Nous sélectionnons les fichiers annuels Caractéristiques.csv qui comportent les champs nécessaires à notre étude :<br>
<strong>An</strong> : Année de l’accident <br>
<strong>Dep</strong> : Département : Code INSEE (Institut National de la Statistique et des Etudes Economiques) du département suivi d’un 0 (zéro). <br>
<strong>Com</strong> : Le numéro de commune est un code donné par l‘INSEE. Le code comporte 3 chiffres calés à droite. <br>
<strong>Num_Acc</strong> : Numéro d’identifiant de l’accident (numéro unique). <br><br>
![](https://user-images.githubusercontent.com/54117403/80632093-03471780-8a57-11ea-928a-9c15ed3b9d11.jpg)
<br><br>
En complément nous avons également besoin du fichier de données de l’INSEE qui permet d’associer les numéros de départements et de communes au nom littéral des communes.
https://public.opendatasoft.com/explore/dataset/code-postal-code-insee-2015/table/

Dans ce fichier nous utiliserons les champs : <br>
<strong>INSEE_COM</strong> : Le code INSEE de la commune <br>
<strong>Code_postal</strong> : Le code postal de la commune <br>
<strong>NOM_COM</strong> : Le nom de la commune <br>
<br>
![](https://user-images.githubusercontent.com/54117403/80632835-18707600-8a58-11ea-8dd8-c0cc5c959257.PNG)


