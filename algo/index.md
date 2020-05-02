# Algorithme de résolution de ce problème

Nous avons deux « chaînes » de fichiers de données à exprimer : <br>
<ol>
<li>Chaîne de<strong> traitement des fichiers accidents Caractéristiques.csv</strong> du Ministère de l'Intérieur </li>
<li>Chaîne de<strong> traitement du fichier INSEE</strong> des références de communes</li>
</ol>
Au final les données devront être assemblées.
<br>

## Traitement données fichiers « CaractéristiquesXX.csv »
Notre jeu de données sont les fichiers "Caractéristiques[année].csv du Ministère de l'Intérieur. Le fichier de l’année 2009 est dans un format qui diffère des autres fichiers, il faudra l’adapter avant de l’assembler aux autres Caractéristiques.csv.

Nous devons ensuite sélectionner les champs année (an), commune  (com) et département (dep) en rapport avec notre problématique.

![](https://user-images.githubusercontent.com/54117403/80800147-0f42ee80-8ba9-11ea-967c-56c6e1ad9a9e.PNG)
<br>
## Traitement données fichier INSEE
Le fichier de l’Institut National de la Statistique et des Etudes Economiques va nous permettre d’afficher le nom de la commune en lien avec le code de cette dernière. Trois champs sont utiles pour notre étude : <br>
<strong>INSEE_COM</strong> : Le code INSEE de la commune<br>
<strong>Code_postal</strong> : Le code postal de la commune<br>
<strong>NOM_COM</strong> : Le nom de la commune<br>

![](https://user-images.githubusercontent.com/54117403/80802354-58963c80-8baf-11ea-85b7-247a7449daf8.PNG)

## Assemblage final
Avant d’assembler les données nous devons effectuer une adaptation du format du champ « dep » dont les numéros de départements de métropole apparaissent sur 3 chiffres avec un zéro à la fin. Ce dernier chiffre doit être supprimé (Ex Pour Paris 750 qui doit être transformé en 75).<br>
Nous effectuerons ensuite une jointure avec comme référence commune le numéro INSEE de commune.<br>
Et enfin le comptage des lignes avec le même numéro de commune.<br>

![](https://user-images.githubusercontent.com/54117403/80803867-cfcdcf80-8bb3-11ea-867a-a79e69afe6cb.PNG)
