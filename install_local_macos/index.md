# Installez Spark sur votre machine locale MacOs

##  Step 1 : Install Homebrew
Ouvrir un terminal et taper la commande suivante : <br>
`/usr/bin/ruby -e “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)”`

Entrer votre mot de passe lorsqu'il est demandé. 

```
==> Cleaning up /Library/Caches/Homebrew...
==> Migrating /Library/Caches/Homebrew to /Users/apple/Library/Caches/Homebrew..
==> Deleting /Library/Caches/Homebrew...
Already up-to-date.
==> Installation successful!
 
==> Homebrew has enabled anonymous aggregate user behaviour analytics.
Read the analytics documentation (and how to opt-out) here:
  http://docs.brew.sh/Analytics.html
 
==> Next steps:
- Run `brew help` to get started
- Further documentation: 
    http://docs.brew.sh
```

## Step 2 : Install xcode-select
Pour installer Java, Scala et Apache Spark en ligne de commande via le terminal, nous allons installer xcode-select. Entrez et exécutez la commande suivante dans Terminal : <br>
`xcode-select –install`

## Step 3 : Install Java
Pour installer Java en ligne de commande, entrez et exécutez la commande suivante dans le Terminal :<br>
`brew cask install java`

Vous devriez obtenir l'écran suivant : 
```
If your Java application still asks for JRE installation, you might need
to reboot or logout/login.
 
Installing this Cask means you have AGREED to the Oracle Binary Code
License Agreement for Java SE at
 
  https://www.oracle.com/technetwork/java/javase/terms/license/index.html
 
==> Satisfying dependencies
==> Downloading http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda
######################################################################## 100.0%
==> Verifying checksum for Cask java
==> Installing Cask java
==> Running installer for java; your password may be necessary.
==> Package installers may write to any location; options such as --appdir are i
Password:
==> installer: Package name is JDK 8 Update 144
==> installer: Upgrading at base path /
==> installer: The upgrade was successful.
? java was successfully installed!
```

## Step 4 : Installation de Scala

Pour installer Scala via la ligne de commande, entrez et exécutez la commande suivante dans le terminal :<br>
`brew install scala`

Vous devriez obtenir l'écran suivant : 
```
apples-MBP:~ Prasanth$ brew install scala
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/core).
==> Updated Formulae
grafana                    idris                      passenger
 
==> Using the sandbox
==> Downloading https://downloads.lightbend.com/scala/2.12.3/scala-2.12.3.tgz
######################################################################## 100.0%
==> Downloading https://raw.githubusercontent.com/scala/scala-tool-support/0a217
######################################################################## 100.0%
==> Caveats
To use with IntelliJ, set the Scala home to:
  /usr/local/opt/scala/idea
 
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
? /usr/local/Cellar/scala/2.12.3: 44 files, 20.1MB, built in 1 minute 1 second
```

## Step 5 : Install Spark

Pour installer Apache Spark via la ligne de commande, entrez et exécutez la commande suivante dans le Terminal : <br>
`brew install apache-spark`

Vous devriez obtenir l'écran suivant : 
```
apples-MBP:~ Prasanth$ brew install apache-spark
==> Using the sandbox
==> Downloading https://www.apache.org/dyn/closer.lua?path=spark/spark-2.2.0/spa
==> Best Mirror http://www-eu.apache.org/dist/spark/spark-2.2.0/spark-2.2.0-bin-
######################################################################## 100.0%
? /usr/local/Cellar/apache-spark/2.2.0: 1,318 files, 221.5MB, built in 12 minutes 8 seconds
```

## Step 6 : Vérifier l'installation

Pour vérifier si l'installation est réussie, lancez spark en utilisant la commande suivante dans Terminal :<br>
`spark-shell`

Vous devriez obtenir l'écran suivant : 
```
apples-MBP:~ Prasanth$ spark-shell
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
17/08/01 21:52:51 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
17/08/01 21:52:58 WARN ObjectStore: Version information not found in metastore. hive.metastore.schema.verification is not enabled so recording the schema version 1.2.0
17/08/01 21:52:58 WARN ObjectStore: Failed to get database default, returning NoSuchObjectException
17/08/01 21:52:59 WARN ObjectStore: Failed to get database global_temp, returning NoSuchObjectException
Spark context Web UI available at http://192.168.1.101:4040
Spark context available as 'sc' (master = local[*], app id = local-1501604572582).
Spark session available as 'spark'.
Welcome to
      __              __
     / _/_  _ __/ /_
    \ \/ _ \/ _ `/ _/  '_/
   /_/ ._/\,// //\\   version 2.2.0
      /_/
        
Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_144)
Type in expressions to have them evaluated.
Type :help for more information.
 
scala>
```

## Etape Complémentaire - Installation de Python

Afin d'éviter des problèmes d'environnement entre la version d'origine de Python (2.4) installée par MacOs et une installation de Python3.x, il est conseillé de procéder de la manière suivante : <br>

```
$ brew install pyenv
🍺  /usr/local/Cellar/pyenv/1.2.10: 634 files, 2.4MB
```
Cet outil gérera plusieurs versions de Python et est décrit comme "simple, discret, et suit la tradition Unix des outils à usage unique qui font une chose bien".

Installons maintenant la dernière version de Python (3.7.5 au moment où nous écrivons ces lignes) :
```
$ pyenv install 3.7.5
python-build: use openssl 1.0 from homebrew
python-build: use readline from homebrew
Downloading Python-3.7.5.tar.xz...
-> https://www.python.org/ftp/python/3.7.5/Python-3.7.5.tar.xz
Installing Python-3.7.5...
## further output not included ##
```

Maintenant que Python 3 est installé via pyenv, nous voulons le définir comme notre version globale par défaut pour les environnements pyenv : <br>
```
$ pyenv global 3.7.5
# and verify it worked
$ pyenv version
3.7.5 (set by /Users/mbbroberg/.pyenv/version)
```
Pour qu'il fonctionne correctement, nous devons ajouter les éléments suivants à notre fichier de configuration (.zshrc pour moi, éventuellement .bash_profile pour vous) :<br>
`$ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc`

Nous devons également supprimer les alias que nous avons utilisés dans les sections ci-dessus car ils empêcheraient d'utiliser correctement pyenv. Après les avoir supprimés, nous pouvons confirmer que pyenv gère notre version de Python 3 :
```
# I start by resetting the current shell
$ exec $0
$ which python
/Users/mbbroberg/.pyenv/shims/python
$ python -V
Python 3.7.5
$ pip -V
pip 19.0.3 from /Users/mbbroberg/.pyenv/versions/3.7.5/lib/python3.7/site-packages/pip (python 3.7)
```


[Page Suivante](https://daviddemacedo.github.io/sid_spark/install_local_win/)

[Retour Sommaire](https://daviddemacedo.github.io/sid_spark/)
