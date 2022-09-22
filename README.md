# TP 4 - Gestion des paquets

## Exercice 1. Commandes de base

1. Commencez par mettre à jour votre système avec les commandes vues dans le cours.

Pour mettre à jour le système, il faut utiliser la commande `apt-get update` et `apt-get upgrade`.

2. Créez un alias "maj" de la ou des commande(s) de la question précédente. Où faut-il enregistrer cet alias pour qu’il ne soit pas perdu au prochain redémarrage ?

Pour créer un alias, il faut utiliser la commande `alias maj="apt-get update && apt-get upgrade"`. Pour que l'alias soit conservé au prochain redémarrage, il faut l'enregistrer dans le fichier `~/.bashrc`.

3. Utilisez le fichier /var/log/dpkg.log pour obtenir les 5 derniers paquets installés sur votre machine.

Pour obtenir les 5 derniers paquets installés, il faut utiliser la commande `tail -n 5 /var/log/dpkg.log`.

4. Listez les derniers paquets qui ont été installés explicitement avec la commande apt install

Pour lister les derniers paquets installés avec la commande `apt install`, il faut utiliser la commande `apt list --installed`.

5. Utilisez les commandes dpkg et apt pour compter de deux manières différentes le nombre de total de paquets installés sur la machine (ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage ? Pourquoi ne peut-on pas utiliser directement le fichier dpkg.log ?

Pour compter le nombre de paquets installés, il faut utiliser la commande `dpkg -l | wc -l` et `apt list --installed | wc -l`. La différence de comptage est due au fait que la commande `dpkg -l` compte aussi les paquets supprimés. On ne peut pas utiliser directement le fichier `dpkg.log` car il ne contient pas le nombre de paquets installés.

6. Combien de paquets sont disponibles en téléchargement sur les dépôts Ubuntu ?

Pour compter le nombre de paquets disponibles en téléchargement sur les dépôts Ubuntu, il faut utiliser la commande `apt-cache search . | wc -l`.

7. A quoi servent les paquets glances, tldr et hollywood ? Installez-les et testez-les.

Les paquets `glances`, `tldr` et `hollywood` servent respectivement à afficher des informations sur le système, à afficher des exemples d'utilisation de commandes et à afficher des films.

8. Quels paquets proposent de jouer au sudoku ?

Pour trouver les paquets proposant de jouer au sudoku, il faut utiliser la commande `apt-cache search sudoku`.

## Exercice 2.

A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule commande, pour n’importe quel programme ? Utilisez la réponse à cette question pour écrire un script appelé origine-commande (sans l’extension .sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.

Pour trouver le paquet installant la commande `ls`, il faut utiliser la commande `dpkg -S ls`. Pour obtenir cette information en une seule commande, il faut utiliser la commande `dpkg -S $(which ls)`.

```bash
#!/bin/bash

if [ $# -ne 1 ]; then
    echo "Usage: $0 <command>"
    exit 1
fi

dpkg -S $(which $1)
```

## Exercice 3.

Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.

Pour écrire une commande qui affiche "INSTALLÉ" ou "NON INSTALLÉ" selon le nom et le statut du package spécifié dans cette commande, il faut utiliser la commande `dpkg -s $1 | grep Status | cut -d " " -f 4`.

## Exercice 4.

Lister les programmes livrés avec coreutils. En particulier, on remarque que l’un deux se nomme [. De
quoi s’agit-il ?

Pour lister les programmes livrés avec coreutils, il faut utiliser la commande `apt-cache show coreutils | grep -E "Package:|Description:"`. L'un des programmes se nomme `.` et sert à vérifier si un fichier existe.

## Exercice 5. aptitude

1. Installez les paquets emacs et lynx à l’aide de la version graphique d’aptitude (et prenez deux minutes pour vous renseigner et tester ces paquets)

Pour installer les paquets `emacs` et `lynx`, il faut utiliser la commande `aptitude install emacs lynx`.

## Exercice 6. Installation d’un paquet par PPA

1. Installer la version Oracle de Java (avec l’ajout des PPA)

Pour installer la version Oracle de Java, il faut utiliser la commande `sudo add-apt-repository ppa:webupd8team/java` et `sudo apt-get update` et `sudo apt-get install oracle-java8-installer`.

2. Vérifiez qu'un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?

Pour vérifier qu'un nouveau fichier a été créé dans `/etc/apt/sources.list.d`, il faut utiliser la commande `ls /etc/apt/sources.list.d`. Ce fichier contient les nouveaux dépôts de paquets.

## Exercice 7. Installation d’un logiciel à partir du code source

1. Commencez par cloner le dépôt git suivant :

`git clone https://gitlab.com/jallbrit/cbonsai.git`

2. Rendez vous dans le dossier cbonsai. Un fichier README.md) est livré avec les sources, et vous explique
comment compiler le programme (vous pouvez installer un lecteur de Markdown pour Bash, comme
mdless pour vous faciliter la lecture de ce type de fichiers).

Un fichier Makefile est également présent. Un Makefile est un fichier utilisé par l’outil make, et contient toutes les directives de compilation d’un logiciel. Un Makefile définit un certain nombre de règles permettant de construire des cibles. Les cibles les plus communes étant install (pour la compilation et l’installation du logiciel) et clean (pour sa suppression).

En suivant les consignes du fichier README.md, et en installant les éventuels paquets manquants, compilez ce programme et installez le en local.

Pour compiler le programme, il faut utiliser la commande `make`. Pour installer le programme, il faut utiliser la commande `sudo make install`.

3. Malheureusement, cette installation "à la main" fait qu’on ne dispose pas des bénéfices de la gestion de paquets apportés par dpkg ou apt. Heureusement, il est possible de transformer un logiciel installé "à la main" en un paquet, et de le gérer ensuite avec apt ; c’est ce que permet par exemple l’outil checkinstall.

checkinstall permet de créer un paquet à partir d’un logiciel installé "à la main". Il permet de créer un paquet Debian, et de l’installer avec apt.

4. Recommencez la compilation à l’aide de checkinstall

Pour compiler le programme à l'aide de checkinstall, il faut utiliser la commande `checkinstall`. Un paquet a été créé (fichier `cbonsai_1.3.1_amd64.deb`), et le logiciel est à présent installé (tapez `cbonsai` depuis n’importe quel dossier pour vous en assurer) ; on peut vérifier par exemple avec `aptitude` qu’il provient bien du paquet qu’on a créé avec `checkinstall`.

## Exercice 8. Création de dépôt personnalisé

### Création d’un paquet Debian avec dpkg-deb

1. Dans le dossier scripts créé lors du TP 2, créez un sous-dossier origine-commande où vous créerez un sous-dossier DEBIAN, ainsi que l’arborescence usr/local/bin où vous placerez le script écrit à l’exercice 2

Pour créer un sous-dossier `origine-commande` dans le dossier `scripts`, il faut utiliser la commande `mkdir scripts/origine-commande`. Pour créer un sous-dossier `DEBIAN` dans le dossier `origine-commande`, il faut utiliser la commande `mkdir scripts/origine-commande/DEBIAN`. Pour créer l'arborescence `usr/local/bin` dans le dossier `origine-commande`, il faut utiliser la commande `mkdir -p scripts/origine-commande/usr/local/bin`.

2. Dans le dossier DEBIAN, créez un fichier control avec les champs suivants :

```
Package: origine-commande #nom du paquet
Version: 0.1 #numéro de version
Maintainer: Foo Bar #votre nom
Architecture: all #les architectures cibles de notre paquet (i386, amd64...)
Description: Cherche l'origine d'une commande
Section: utils #notre programme est un utilitaire
Priority: optional #ce n'est pas un paquet indispendable
```

3. Revenez dans le dossier parent de origine-commande (normalement, c’est votre $HOME) et tapez la commande suivante pour construire le paquet :

`dpkg-deb --build origine-commande`

### Création du dépôt personnel avec reprepro

1. Dans votre dossier personnel, commencez par créer un dossier repo-cpe. Ce sera la racine de votre dépôt

Pour créer un dossier `repo-cpe` dans le dossier personnel, il faut utiliser la commande `mkdir repo-cpe`.

2. Ajoutez-y deux sous-dossiers : conf (qui contiendra la configuration du dépôt) et packages (qui contiendra nos paquets)

Pour créer un sous-dossier `conf` dans le dossier `repo-cpe`, il faut utiliser la commande `mkdir repo-cpe/conf`. Pour créer un sous-dossier `packages` dans le dossier `repo-cpe`, il faut utiliser la commande `mkdir repo-cpe/packages`.

3. Dans conf, créez le fichier distributions suivant :

```
Origin: Un nom, une URL, ou tout texte expliquant la provenance du dépôt
Label: Nom du dépôt
// Suite: stable
Codename: focal #!! A MODIFIER selon la distribution cible !!
Architectures: i386 amd64 #(architectures cibles)
Components: universe #(correspond à notre cas)
Description: Une description du dépôt
```

4. Dans le dossier repo-cpe, générez l’arborescence du dépôt avec la commande :

`reprepro -b . export`

5. Copiez le paquet origine-commande.deb créé précédemment dans le dossier packages du dépôt, puis, à la racine du dépôt, exécutez la commande

`reprepro -b . includedeb cosmic origine-commande.deb`

6. Il faut à présent indiquer à apt qu’il existe un nouveau dépôt dans lequel il peut trouver des logiciels. Pour cela, créez (avec sudo) dans le dossier /etc/apt/sources.list.d le fichier repo-cpe.list contenant :

`deb file:/home/User/repo-cpe cosmic multiverse`

7. Lancez la commande sudo apt update

Le repo est bien ajouté.

### Signature du dépôt avec GPG

1. Commencez par créer une nouvelle paire de clés avec la commande

`gpg --gen-key`

Attention ! N’oubliez pas votre passphrase !!!

2. Ajoutez à la configuration du dépôt (fichier distributions la ligne suivante :

`SignWith: yes`

3. Ajoutez la clé à votre dépôt :

`reprepro --ask-passphrase -b . export`

4. Ajoutez votre clé publique à votre dépôt avec la commande :

`gpg --export -a "auteur" > public.key`

5. Enfin, ajoutez cette clé à la liste des clés fiables connues de apt :

`sudo apt-key add public.key`

Le dépôt est bien signé.
