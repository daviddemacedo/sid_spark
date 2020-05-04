# Conclusion

Au terme de ce TP, nous pouvons faire deux conclusions.

Dans un premier temps, nous obtenons bien la réponse à la problématique "Quelles sont les 20 communes ayant recensées le plus d'accidents corporels sur les dix dernières années ?", mais les résultats ne sont pas ceux auxquels nous nous attendions. En effet, on peut voir que la même ville apparaît plusieurs fois pour plusieurs codes postaux. Par exemple la ville de Toulouse apparaît six fois avec six codes postaux différents, mais le nombre d'accident est le même. Le traitement des fichiers additionne tous les accidents pour une même ville, mais garde les codes postaux différents. Nous avons essayé d'afficher uniquement la ville, mais cela multipliait le nombre d'accidents par le nombre de codes postaux existants pour cette même ville. Nous en sommes venus à la conclusion que le jeu de données que nous utilisions n'était peut-être pas adéquate par rapport à notre problématique et n'a donc pas permis d'obtenir totalement le résultat escompté.

Dans un second temps, après un grand nombre de tests: 
![](https://raw.githubusercontent.com/daviddemacedo/sid_spark/master/img/nbretestspark.png)
Il nous est apparu que les performances en local et en cluster sont similaires et même meilleure en local. Nous avons testé différentes configuration afin d'améliorer ces performances mais sans résultat significatif. Ceci s'explique par le fait que le jeu de données que nous avons utilisé est trop petit pour qu'une différence entre les deux installations soit significative. Si nous avions utilisé un jeu de données beaucoup important (plusieurs centaines de Go), il est sûr que les performances seraient bien meilleures en mode cluster.

Notre objectif lors de ce TP était de manipuler et expérimenter spark en mode cluster. Nous avons privilégié cela au résultat de notre problématique. Ainsi, nous avons testé le fonctionnement de Spark et sa configuration en mode réparti, et nous avons également réalisé des tests de robustesse qui se sont avérés concluants.





[Page Suivante](https://daviddemacedo.github.io/sid_spark/biblio/)

[Retour Sommaire](https://daviddemacedo.github.io/sid_spark/)
