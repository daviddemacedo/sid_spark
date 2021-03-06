# Création de machines virtuelles sur OpenStack

Pour réaliser la partie cluster de ce TP, il est nécessaire de créer plusieurs machines virtuelles (VM) hébergées sur OpenStack (http://cloud.univ-lille.fr/).<br>
Notre cluster étant composé d'une VM master et de 5 VM slaves que nous avons nommées : master, slave1, slave2, slave3, slave4 et slave5. Dans le cadre du TP, seules 4 machines nous étaient demandées, slave4 et slave5 sont donc facultatives.

Pour cela, suivez le tutoriel suivant : (http://cloud.univ-lille.fr/cloud_ULille_tutoriel.pdf) avec les informations ci-dessous :
- Source : "ubuntu-18.04"
- Gabarit : "puissante"

Lors de leur création, les instances récupèrent chacune une adresse IP dans le même réseau : 172.28.100.0/16. Elles peuvent donc communiquer entre elles directement.

Les VM sont accessibles en SSH :
- Depuis Windows vous pouvez par exemple utiliser l'outil Putty (https://www.putty.org/) ou Mobaxterm (https://mobaxterm.mobatek.net/download.html)
- Depuis Linux/Mac il suffit simplement de lancer un terminal puis de lancer la commande suivante :
`ssh ubuntu@172.28.100.136`


[Page Suivante](https://daviddemacedo.github.io/sid_spark/installcluster/)

[Retour Sommaire](https://daviddemacedo.github.io/sid_spark/)
