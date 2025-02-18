TP nº2 : entrées-sorties
=====================

**L3 Informatique - Système**

### Instructions

1. Mettez à jour le dépôt `git` du cours : pour cela, placez-vous dans le
   répertoire du cours et lancez la commande `git pull`.
2. Déplacez-vous dans le répertoire [TP/TP2/](TP/TP2/).
3. Lancez la commande `make init`; ceci créera un répertoire
   [TP/TP2/work/](javascript:;) : c'est dans ce répertoire que vous
   devrez travailler (ne touchez pas au répertoire
   [TP/TP2/template](javascript:;), vous risqueriez ensuite d’avoir des
   problèmes pour mettre à jour le dépôt).
4. Déplacez-vous dans le répertoire [TP/TP2/work](javascript:;) pour
   travailler.


### Exercice 1 : les entrées/sorties de haut-niveau

Le répertoire [TP/TP2/work/ex-1-les-gens](javascript:;) contient un
programme `anne-sylvestre.c` dont l'exécution crée deux fichiers et
affiche leur contenu sur la sortie standard. Les écritures et lectures
nécessaires sont effectuées par l'intermédiaire d'un *flot* de type `FILE
*`, le type défini par la bibliothèque standard du C pour faciliter la
gestion des entrées/sorties.

Compiler ce programme, puis déterminer quels sont les appels systèmes
invoqués par les fonctions `fopen`, `(f)printf`, `fgets`, `fclose` : on
rappelle que (sous Linux) la commande `strace`, exécutée avec un
paramètre `cmd`, crée un nouveau processus exécutant `cmd` et affiche sur
sa sortie erreur standard une trace de tous les appels systèmes réalisés
par ce processus. 

Pour améliorer la lisibilité, il peut être judicieux de séparer la sortie
erreur standard et la sortie standard en redirigeant celle-ci sur un
autre terminal.

Comparer le nombre et l'ordre des appels systèmes réalisés avec le nombre
et l'ordre des appels correspondants aux fonctions de haut-niveau.
Remarquer en particulier que les stratégies d'écriture sur le terminal et
sur les deux fichiers ordinaires ne sont pas les mêmes.

Expliquer!


### Instructions complémentaires

Pour chacun des exercices suivants, vous trouverez dans le sous-répertoire 
`ex-N-blabla` une partie du code, à compléter, ainsi qu'un Makefile qui 
vous permettra :

 - de compiler votre code avec `make`,
 - de tester votre code avec `make test`.

À la fin de chaque exercice, avant de passer au suivant, assurez-vous que
votre programme passe *tous* les tests : lorsque vous exécutez `make
test`, toutes les lignes doivent dire `OK` !


### Exercice 2 : effet de quelques modes d'ouverture

Le répertoire correspondant est
[TP/TP2/work/ex-2-ouvertures](javascript:;).  

Compléter les fichiers `cree.c`, `ecrase.c`, `ajoute.c`,
`cree-ou-ecrase.c`, `cree-ou-ajoute.c` selon les directives indiquées en
commentaire dans ces fichiers.

En particulier,
   * `./cree "contenu de tutu" tutu` doit se comporter comme `echo
     "contenu de tutu" > tutu` lorsque l'option `noclobber` du shell est
     activée (`set -o noclobber` en bash) pour protéger les fichiers
     existants d'un écrasement intempestif;
   * `./cree-ou-ecrase "contenu de tutu" tutu` doit se comporter comme
     `echo "contenu de tutu" >| tutu`;
   * `./cree-ou-ajoute "un peu plus de contenu" tutu` doit se comporter comme
     `echo "un peu plus de contenu" >> tutu`.

Compiler avec `make` puis exécuter `make test` pour vérifier que tout est
correct.


### Exercice 3 : déplacement de la tête de lecture/écriture

Le répertoire correspondant est [TP/TP2/work/ex-3-alphabet](javascript:;). 
Le seul fichier à modifier est `cree-alphabet.c`.

En l'état, ce programme permet d'initialiser un fichier avec le texte
`"abcdefghijklmnopqrstuvwxyz"`. La tête de lecture/écriture est ensuite
replacée au début du fichier grâce à l'appel système `lseek`, dont le
comportement est similaire à `fseek`.

Compléter le fichier `cree-alphabet.c` selon les directives indiquées en
commentaire pour qu'après cette initialisation, `cree-alphabet` effectue
un 2e parcours du fichier, en passant certaines lettres en majuscule. Le
but ici étant seulement d'observer le comportement en situation normale,
il n'est pas demandé de gérer les potentielles erreurs.

Comparer ensuite le comportement de `cree-alphabet` avec celui de 
`cree-alphabet-append`.


### Exercice 4 : copycat

Le répertoire correspondant est [TP/TP2/work/ex-4-my-cat](javascript:;).
Le seul fichier à modifier est `my-cat.c`.

#### Écriture du programme `my-cat`

Compléter `my-cat.c` pour obtenir un programme qui lit en boucle depuis
l'entrée standard et recopie ce qu'il a lu sur la sortie standard. Une
fois la lecture terminée (fin de fichier détectée par `read`), le
programme doit terminer. En cas d'erreur, le programme doit terminer avec
le code d'erreur 1.

Faites attention à :

 - effectuer les lectures et écritures par blocs de taille `BUFFER_SIZE`
   (ni caractère par caractère, ni tout le fichier d'un coup)
 - après chaque appel système (`read()`, `write()`, etc.), vérifier s'il
   y a eu une erreur et agir en conséquence.

#### Impact de la taille des blocs de lecture

Votre programme lit sur l'entrée standard par blocs de `BUFFER_SIZE`
octets. Essayez de réduire la taille des blocs (100 octets, puis 10, 
puis 1) en testant à chaque fois le résultat avec `make test` : que
constatez-vous ?



### Exercice 5 : `my-dd`

Le répertoire correspondant est [TP/TP2/work/ex-5-my-dd](javascript:;). 
Le seul fichier à modifier est`my-dd.c`.

L'objectif de cet exercice est d'écrire un programme `my-dd` qui
reproduise une partie des fonctionnalités de l'utilitaire `dd` (cf. `man
dd`). Plus précisément `./my-dd -i <INFILE> -o <OUTFILE> -b <BLOCKSIZE>
-c <COUNT>` doit avoir un comportement équivalent à `dd if=<INFILE>
of=<OUTFILE> bs=<BLOCKSIZE> count=<COUNT> status=none`. Les arguments
`-i`, `-o`, `-b` et `-c` sont optionnels, avec le même comportement par
défaut que pour `dd`. En cas de succès, le programme doit terminer avec
le code 0, et en cas d'erreur, le code 1.


Compléter les portions de `my-dd.c` marquées avec `TODO [A] :`. Vous
obtiendrez ainsi une première version de `my-dd` qui ignore les arguments
`-i` et `-o`.

Vérifiez bien que si vous exécutez `make test`, le premier test est réussi.

Compléter ensuite le reste du code de de `my-dd.c` (c'est-à-dire les
portions marquées avec `TODO [B] :`).

Vérifiez que cette fois-ci, lorsque vous exécutez `make test`, tous les
tests sont réussis.


