# Choix des Jeux de données

## Sources de données :
Nous choisissons un jeu de données issus du site gouvernemental www.data.gouv.fr. Ce jeu de données est fourni par le Ministère de l’Intérieur via la base des accidents corporels de la circulation disponible sur :<br>
https://www.data.gouv.fr/fr/datasets/base-de-donnees-accidents-corporels-de-la-circulation/ <br>
![](https://user-images.githubusercontent.com/54117403/80628998-7b5f0e80-8a52-11ea-82f3-a6b9dccb67c6.PNG)
<br>
## Contexte : <br>
Pour chaque accident corporel (soit un accident survenu sur une voie ouverte à la circulation publique, impliquant au moins un véhicule et ayant fait au moins une victime ayant nécessité des soins), des saisies d’information décrivant l’accident sont effectuées par l’unité des forces de l’ordre (police, gendarmerie, etc.) qui est intervenue sur le lieu de l’accident.
<br><br>
## Organisation des données.<br>
Les bases données sont disponibles par années et sont composées de 4 fichiers au format .csv : <br>
<ol>
<li><strong>Caractéristiques</strong> (Numéro accident, date, heure, département, commune, …) </li><br>
<li><strong>Lieux</strong> (Catégorie route, voie, régime de circulation, infrastructure, largeur chaussée, …) </li><br>
<li><strong>Véhicules</strong> (Catégorie véhicule, identifiant, sens de circulation, point de choc, obstacle heurté, …) </li><br>
<li><strong>Usagers</strong> ( Catégorie usager, année de naissance, sexe, place dans le véhicule, …) </li>
</ol>
Un fichier pdf donne un descriptif des informations et des champs disponibles dans ces bases de données : <br> https://www.data.gouv.fr/fr/datasets/r/8d4df329-bbbb-434c-9f1f-596d78ad529f
<br>


[Page Suivante](https://daviddemacedo.github.io/sid_spark/descpb/)

[Retour Sommaire](https://daviddemacedo.github.io/sid_spark/)
