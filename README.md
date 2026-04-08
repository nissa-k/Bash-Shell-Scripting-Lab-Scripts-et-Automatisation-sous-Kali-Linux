# Bash Shell Scripting Lab – Scripts et Automatisation sous Kali Linux

### TP2 – Scripting Shell

**Autrice :** Karadag Nissa  
**Formation :** Bachelor 2 Informatique – Ynov Campus  
**Module :** Systèmes Linux  
**Date :** Décembre 2025

---

## Sommaire

1. [Objectif](#objectif)
2. [Environnement de travail](#environnement-de-travail)
3. [Operateurs de comparaison](#operateurs-de-comparaison)
4. [Exercice 1 – Boucle while](#exercice-1--boucle-while--compter-de-1-à-10)
5. [Exercice 2 – Boucle for](#exercice-2--boucle-for--parcourir-une-liste-de-noms)
6. [Exercice 3 – Jeu du nombre à deviner](#exercice-3--jeu-du-nombre-à-deviner)
7. [Exercice 4 – Vérifier l'existence d'un fichier](#exercice-4--vérifier-lexistence-dun-fichier-ou-dossier)
8. [Exercice 5 – Script de sauvegarde](#exercice-5--script-de-sauvegarde-simple)
9. [Exercice 6 – Menu interactif](#exercice-6--menu-interactif-bash)
10. [Bilan](#bilan-des-scripts-réalisés)

---

## Objectif

Maîtriser les fondamentaux du scripting Bash à travers six exercices progressifs couvrant les boucles, les structures conditionnelles, les interactions utilisateur, la manipulation de fichiers et l'automatisation de tâches. L'environnement utilisé est une machine virtuelle Kali Linux sous VirtualBox.

---

## Environnement de travail

- **OS :** Kali Linux (VM VirtualBox)
- **Répertoire de travail :** `~/tp_shell`
- **Editeur :** nano
- **Workflow :** `nano script.sh` → `chmod +x script.sh` → `./script.sh`

---

## Operateurs de comparaison

| Operateur | Signification |
|---|---|
| `-lt` | Strictement inférieur à ( < ) |
| `-le` | Inférieur ou égal à ( <= ) |
| `-eq` | Egal à ( = ) |
| `-ne` | Différent de ( != ) |
| `-gt` | Strictement supérieur à ( > ) |
| `-ge` | Supérieur ou égal à ( >= ) |

---

## Exercice 1 – Boucle while : compter de 1 à 10

**Objectif :** créer un script utilisant une boucle while pour afficher les nombres de 1 à 10.

**Concepts :** boucle conditionnelle contrôlée par une variable compteur, incrémentation arithmétique.

**Syntaxe :**

```bash
while [ condition ]
do
    commandes
done
```

**Script – `ex1_while.sh` :**

```bash
#!/bin/bash
# Exercice 1 : afficher les nombres de 1 à 10 avec une boucle while

i=1

while [ "$i" -le 10 ]
do
    echo "$i"
    i=$((i + 1))
done
```

**Résultat :** affichage des nombres 1 à 10, un par ligne.

---

## Exercice 2 – Boucle for : parcourir une liste de noms

**Objectif :** créer un script utilisant une boucle for pour itérer sur une liste de noms et afficher un message personnalisé pour chacun.

**Concepts :** itération sur une liste statique de chaînes, gestion de variables.

**Syntaxe :**

```bash
for var in $liste
do
    commandes
done
```

**Script – `ex2_for.sh` :**

```bash
#!/bin/bash
# Exercice 2 : parcourir une liste de noms avec une boucle for

noms="Nissa Alice Emma Hugo David Jean Sofiane"

for nom in $noms
do
    echo "Bonjour $nom, bienvenue dans le script Bash !"
done
```

**Résultat :**

```
Bonjour Nissa, bienvenue dans le script Bash !
Bonjour Alice, bienvenue dans le script Bash !
...
Bonjour Sofiane, bienvenue dans le script Bash !
```

---

## Exercice 3 – Jeu du nombre à deviner

**Objectif :** script dans lequel l'utilisateur entre un nombre secret entre 1 et 20, puis le devine. Chaque tentative est enregistrée dans `joueur_log.txt`.

**Concepts :** boucle infinie, comparaisons numériques (-lt, -gt), compteur de tentatives, journalisation par redirection (>>).

**Elements cles :**

```bash
> fichier.txt              # Vider un fichier
read -p "Message : " var   # Lire une entrée utilisateur
echo "texte" >> fichier    # Ecrire dans un fichier sans écraser
```

**Script – `ex3_jeu.sh` :**

```bash
#!/bin/bash
# Exercice 3 : jeu du nombre à deviner (1 à 20) avec log des tentatives

log="joueur_log.txt"
> "$log"

echo " Jeu nombre à deviner entre 1 et 20"
read -p "Entrez le nombre à deviner (entre 1 et 20) : " secret

if [ "$secret" -lt 1 ] || [ "$secret" -gt 20 ]; then
    echo "Le nombre doit être entre 1 et 20."
    exit 1
fi

clear
echo "Le nombre secret a été choisi ! À vous de deviner."
tentative_nb=0

while true
do
    read -p "Votre proposition : " proposition
    tentative_nb=$((tentative_nb + 1))

    if [ "$proposition" -lt "$secret" ]; then
        echo "C'est plus grand !"
        echo "Tentative $tentative_nb : $proposition → trop petit" >> "$log"
    elif [ "$proposition" -gt "$secret" ]; then
        echo "C'est plus petit !"
        echo "Tentative $tentative_nb : $proposition → trop grand" >> "$log"
    else
        echo "Bravo ! Vous avez trouvé en $tentative_nb tentative(s) !"
        echo "Tentative $tentative_nb : $proposition → trouvé !" >> "$log"
        break
    fi
done

echo "Les tentatives ont été enregistrées dans le fichier : $log"
```

**Résultat :** partie jouée en 3 tentatives (4 → trop petit, 20 → trop grand, 14 → trouvé).

---

## Exercice 4 – Vérifier l'existence d'un fichier ou dossier

**Objectif :** script qui demande un chemin et indique s'il existe, s'il s'agit d'un fichier ou d'un dossier.

**Concepts :** tests de fichiers Bash : `-e` (existence), `-f` (fichier régulier), `-d` (répertoire).

**Syntaxe :**

```bash
if [ condition ]; then
    commandes
elif [ condition ]; then
    commandes
else
    commandes
fi
```

**Script – `ex4_check_file.sh` :**

```bash
#!/bin/bash
# Exercice 4 : vérifier l'existence d'un fichier ou d'un dossier

read -p "Entrez le nom d'un fichier ou d'un dossier : " chemin

if [ -e "$chemin" ]; then
    echo "Le chemin '$chemin' existe."

    if [ -f "$chemin" ]; then
        echo "Il s'agit d'un fichier."
    elif [ -d "$chemin" ]; then
        echo "Il s'agit d'un dossier."
    else
        echo "Ce n'est ni un fichier régulier ni un dossier."
    fi
else
    echo "Le chemin '$chemin' n'existe pas."
fi
```

**Résultats testés :**

| Chemin saisi | Résultat |
|---|---|
| `tp_shell` | N'existe pas |
| `Musique` | N'existe pas |
| `/etc` | Existe – il s'agit d'un dossier |

---

## Exercice 5 – Script de sauvegarde simple

**Objectif :** script qui demande un dossier source et un dossier de destination, copie tous les fichiers et affiche le nombre de fichiers copiés.

**Concepts :** vérification d'existence d'un répertoire, création avec `mkdir -p`, boucle sur répertoire, détection de fichiers réguliers (-f), copie avec `cp`, compteur.

**Syntaxe :**

```bash
for fichier in "$rep"/*
do
    commandes
done

compteur=$((compteur + 1))
```

**Script – `ex5_backup.sh` :**

```bash
#!/bin/bash
# Exercice 5 : script de sauvegarde simple

read -p "Dossier source : " src
read -p "Dossier de destination : " dest

if [ ! -d "$src" ]; then
    echo "Le dossier source n'existe pas."
    exit 1
fi

if [ ! -d "$dest" ]; then
    echo "Le dossier de destination n'existe pas. Création... "
    mkdir -p "$dest"
fi

compteur=0

for fichier in "$src"/*
do
    if [ -f "$fichier" ]; then
        cp "$fichier" "$dest"
        compteur=$((compteur + 1))
    fi
done

echo "Sauvegarde terminée."
echo "Nombre de fichiers copiés : $compteur"
```

**Résultat :** copie de `f1.txt` et `f2.txt` depuis `source_test` vers `dest_test` — 2 fichiers copiés.

---

## Exercice 6 – Menu interactif Bash

**Objectif :** script affichant un menu en boucle avec 4 options (date, répertoire courant, liste des fichiers, quitter).

**Concepts :** boucle `while true`, structure `case` pour dispatcher les actions, commande `break` pour sortir de la boucle.

**Syntaxe case :**

```bash
case $variable in
    valeur1)
        commandes
        ;;
    valeur2)
        commandes
        ;;
    *)
        commandes
        ;;
esac
```

**Script – `ex6_menu.sh` :**

```bash
#!/bin/bash
# Exercice 6 : menu interactif Bash

while true
do
    echo "===== MENU ====="
    echo "1) Afficher la date"
    echo "2) Afficher le répertoire courant"
    echo "3) Lister les fichiers"
    echo "4) Quitter"
    echo "---------------"
    read -p "Votre choix : " choix

    case "$choix" in
        1)
            echo "Date actuelle :"
            date
            ;;
        2)
            echo "Répertoire courant :"
            pwd
            ;;
        3)
            echo "Liste des fichiers :"
            ls
            ;;
        4)
            echo "Au revoir !"
            break
            ;;
        *)
            echo "Choix invalide, veuillez réessayer."
            ;;
    esac
    echo
done
```

**Résultat :** menu affiché en boucle, chaque option exécute la commande correspondante, option 4 quitte proprement.

---

## Bilan des scripts réalisés

| Exercice | Fichier | Concept clé | Statut |
|---|---|---|---|
| Exercice 1 | `ex1_while.sh` | Boucle while, incrémentation | Réalisé |
| Exercice 2 | `ex2_for.sh` | Boucle for, liste de chaînes | Réalisé |
| Exercice 3 | `ex3_jeu.sh` | Boucle infinie, comparaisons, log fichier | Réalisé |
| Exercice 4 | `ex4_check_file.sh` | Tests fichiers -e -f -d, if/elif/else | Réalisé |
| Exercice 5 | `ex5_backup.sh` | Boucle for répertoire, cp, compteur | Réalisé |
| Exercice 6 | `ex6_menu.sh` | while true, case, menu interactif | Réalisé |

---

*Karadag Nissa — Bachelor 2 Informatique — Ynov Campus — 2025*
