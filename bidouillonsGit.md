 Nous utiliserons la console et des commandes en lignes

Logiciels utilisés:
git
	$ git --version
	git version 2.17.1
éditeur de texte de votre choix

Fortement inspiré de 
https://gitlab.in2p3.fr/grasland/formation-git/tree/master
Les 3 premières étapes (Intro Version et TP 1 et 2)

# Git en solo (sur des fichiers que vous êtes le seul à modifier)

Nous produisons du texte, nous corrigeons, reprenons, amendons,
revenons en arrière sur du texte.... *(parfois à plusieurs)*.

Il nous faut naviguer entre des versions parfois éloignées, et retrouver facilement :
	-la version qui introduit/termine une fonctionnalité, un bug
	-la version qui a été livrée à telle date
	-la version qui a été modifiée à telle date, par untel.

# Il faut pour cela utiliser un système de gestion de version !!!!!
(cvs, svn, mercurial (hg), bazaar (bzr)....)
Bien adapté aux fichiers texte (basé sur diff), utilisable pour les autres
Ne devrait pas être utilisé comme système de sauvegarde ou d'archivage .....

#C'est parti pour les 1er pas

https://github.com/jmfGithub/Git_juin2019
		::: Bouton vert "Clone or download" :::

$ git clone https://github.com/jmfGithub/Git_juin2019.git
$ cd Git_juin2019
$ ls -al
$ ls -al .git

$ git log
$ git status
$ git help [command] ou git command --help
$ man git
$ man giteveryday

#Git manipule trois arborescences
« Passé »   : La version d’origine sur laquelle on se base (Version)
« Présent » : Le contenu actuel du Répertoire de travail              
« Futur »   : La version que l’on est en train de créer    (Index)	

 →  Répertoire de travail -- git  add fichier ->	Index (staging area) →
↑ 																													 				    ↓
 ← Répertoire de travail <- git reset fichier -- Index (staging area)  ←

 → Index (staging area)      -- git  commit ->	    			Version (HEAD) →
↑ 																													 				      ↓
 ← Index (staging area)  <- git reset --soft HEAD~ --		  Version (HEAD) ←
       
   Répertoire de travail <- git reset --hard HEAD~  --   Version (HEAD) → 
	    ↑																														       ↓
			 ←											←										←							     ←


__git est volubile__

$ touch toto
$ git status
"Votre branche est à jour":  VRAI tant qu'on a pas créé de nouvelle version
"toto n'est pas suivi"    :  il n'est pas dans l'index
"utilisez "git add" pour les suivre", merci git !

$ git add toto
$ git status

"Modifications qui seront validées"  == PRÊTES pour la nouvelle version

On peut avoir les 2 messages
$ touch tata
$ git status

On peut ne pas suivre certains fichiers, pour ne pas les voir avec *git status*:
$ git status -uno
ni même avec *git status*: les mettre dans un fichier .gitignore:

On peut ajouter et retirer des fichiers/dossiers de l'index

pour lister les fichiers suivis:
$ git ls-files

Pour détruire et renommer des fichiers suivis:
$ git rm  (parfois git rm -f)
arrêter de suivre un fichier sans le détruire:
$ git rm --cached

$ git mv

Mais si je modifie un fichier ?

cat > toto
1ere étape
CTRL d

$ git status
Git dit presque tout:
toto est un nouveau fichier .. et on peut le déindexer: git reset HEAD toto
toto a été modifié          .. on accepte la modif    : git add toto
                            .. on n'accepte pas       : git checkout -- toto

mieux que git add (marche sur plusieurs fichiers, mais avec un grain fin):
$ git add -p
liste toutes les modifs de tous les fichers suivis, on peut les accepter ou pas, individuellement.

Ainsi, *git add* joue 2 rôles:
 - la 1ere fois il indique que désormais le fichier est suivi
 - les suivantes il indique l'acceptation des modifications sur le fichier


# Et enfin, on créé la nouvelle version
$ git commit -m "création de toto"
$ git log

Il est possible, mais pas conseillé d'utiliser:
$ git commit -am "création de toto"
Ça va créer la nouvelle version en acceptant toutes les modifications de tous les fichiers suivis (économise des git add, ou 1 git add -p)

Et maintenant on peut naviguer dans les versions
git log --oneline
08f6dde (HEAD -> master) création de toto
c9ec61b (origin/master, origin/HEAD) Initial commit

$ git checkout c9ec61b
Ne vous inquiétez pas de l'état "Détaché", et là encore git vous explique que faire. On parlera des branches dans un prochain __Bidouillons Ensemble__.

Pour revenir à l'état normal:
$ git checkout master

#Historique linéaire

Voir les différences entre le Répertoire de travail et l'index

modifier toto
cat >> toto
2eme étape
CTRL d

$ git diff
à alterner avec
$ git add -p

on créé donc une nouvelle version progressivement, quand tout va bien on "committe"

Voir les différences entre le Répertoire de travail et la Version
$ git diff HEAD

Voir les différences qui seront inclues dans la prochaine Version
$ git diff --cached

*git diff* peut ne s'appliquer qu'à un fichier (ou dossier)
$ git diff fichier

entre 2 commits
$ git diff commit1 commit2

$ git show [commit]

HEAD		désigne le dernier commit enregistré
HEAD~		est son parent
HEAD~2	est son grand-parent 
HEAD~N  est son Nième parent

$ git tag nomme un commit

#Annuler un jeu de commits, sans danger
$ git revert HEAD~

créé un nouveau commit, on ne peut donc pas perdre accidentellement des données en utilisant revert. Ne convient pas pour effacer quelque chose de "critique" sur le serveur... on n'efface pas un *push*


Si git vous ennuie en ne voulant pas faire quelque chose tant vous n'avez pas *committé*, mettez les modifs effectuées depuis le dernier commit sous le tapis, utilisez:
$ git stash

$ git stash pop
pour les retrouver.

#Tout est OK pour être sauvegardé sur un serveur distant *(et donc partagé)*
$ git push  (ne marche que si vous y êtes autorisé)

l'inverse = récupérer depuis le dépot, par exemple ce que d'autres y ont *pushé*

$ git pull
à faire souvent, au moins chaque fois que vous que vous commencez une session de travail.

En espérant que vous n'en arriviez pas là:
https://explainxkcd.com/wiki/index.php/1597:_Git


