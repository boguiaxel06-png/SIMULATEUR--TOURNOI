# Cahier des charges — Simulateur de Tournoi (v1)

## 1. Objectif

Le programme conçoit la structure d'un tournoi à élimination directe en fonction du nombre d'équipes choisi au préalable, puis simule ce tournoi.

## 2. Utilisateurs

- Un club
- Un organisateur d'évènement
- Une personne qui veut simplement simuler un tournoi

## 3. Périmètre

### Inclus dans la v1

- Tournoi à **élimination directe** uniquement
- Nombre d'équipes : **2, 4, 8, 16 ou 32**
- Placement des équipes par **tirage au sort**
- Vainqueur de chaque match choisi **au hasard**
- Affichage du tournoi **tour par tour**

### Hors périmètre (plus tard)

- Phases de poules
- Repêchages
- Nombre d'équipes quelconque (avec équipes exemptées au premier tour)
- Pronostic basé sur la forme des équipes et des joueurs
- Historique des tournois d'une équipe
- Choix manuel des affrontements du premier tour (à la place du tirage au sort)

## 4. Fonctionnalités

1. Demander à l'utilisateur le nombre d'équipes.
2. Demander les noms des équipes, un par un (`Équipe 1 :`, `Équipe 2 :`, ...).
3. Placer les équipes par tirage au sort.
4. Simuler chaque match au hasard, tour par tour, jusqu'au champion.
5. Afficher chaque tour avec ses matchs et ses vainqueurs, puis le champion.

## 5. Règles métier

- Pour _n_ équipes, il y a **n - 1 matchs** en tout (chaque match élimine une équipe, et il faut en éliminer n - 1 pour obtenir un champion).
- Le nombre de tours est le nombre de fois qu'on divise _n_ par 2 pour arriver à 1 (32 équipes : 5 tours ; 8 équipes : 3 tours).
- Le vainqueur d'un match est l'une des deux équipes qui y jouent, choisie au hasard.
- Chaque tour oppose les vainqueurs du tour précédent. La finale oppose les deux vainqueurs des demi-finales.
- Noms des tours selon le nombre d'équipes encore en lice :

| Équipes en lice | Nom du tour         |
| --------------- | ------------------- |
| 32              | Seizièmes de finale |
| 16              | Huitièmes de finale |
| 8               | Quarts de finale    |
| 4               | Demi-finales        |
| 2               | Finale              |

## 6. Entrées et sorties

### Entrées

- Le nombre d'équipes (2, 4, 8, 16 ou 32)
- Le nom de chaque équipe, saisi au clavier

### Sortie (exemple avec 4 équipes)

```
Demi-finales
  Équipe A vs Équipe B -> vainqueur : Équipe A
  Équipe C vs Équipe D -> vainqueur : Équipe C
Finale
  Équipe A vs Équipe C -> vainqueur : Équipe C
Champion : Équipe C, félicitations !
```

## 7. Cas d'erreur

| Cas                                                                                                                            | Réaction du programme                      |
| ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------ |
| Nombre d'équipes différent de 2, 4, 8, 16, 32 (y compris 1, 0, un nombre négatif)                                              | Message d'erreur, le nombre est redemandé  |
| Nombre d'équipes saisi sous forme de texte (`abc`)                                                                             | Message d'erreur, le nombre est redemandé  |
| Nom d'équipe vide ou composé uniquement d'espaces                                                                              | Message d'erreur, le nom est redemandé     |
| Nom déjà utilisé (comparaison sans tenir compte de la casse ni des espaces au début et à la fin : `a` et `A` sont le même nom) | Message d'erreur, un autre nom est demandé |

Règles sur les noms :

- Les espaces au début et à la fin d'un nom sont ignorés (`"Lions "` et `"Lions"` sont le même nom).
- Les espaces à l'intérieur d'un nom sont autorisés (ex. : `Real Madrid`).

La simulation ne démarre que lorsque **toutes** les équipes ont un nom valide.

## 8. Contraintes

- Langage : Python
- Interface : console, sans bibliothèque externe
- Plafond : 32 équipes

## 9. Critères de validation

- **CV-1** : si 4 équipes sont saisies, alors 2 tours et 3 matchs sont joués.
- **CV-2** : si 8 équipes sont saisies, alors 3 tours et 7 matchs sont joués.
- **CV-3** : si 32 équipes sont saisies, alors 5 tours et 31 matchs sont joués, le premier tour s'appelant « Seizièmes de finale ».
- **CV-4** : si 2 équipes sont saisies, alors un seul match est joué : la finale.
- **CV-5** : si l'utilisateur saisit 6, 1 ou `abc` comme nombre d'équipes, alors un message d'erreur s'affiche et le nombre est redemandé.
- **CV-6** : si l'utilisateur saisit un nom déjà utilisé (y compris avec une casse ou des espaces de début et de fin différents), un nom vide ou un nom composé uniquement d'espaces, alors un message d'erreur s'affiche et le nom est redemandé ; un nom comme `Real Madrid` est accepté.
- **CV-7** : le champion est toujours l'une des équipes saisies.
- **CV-8** : chaque match oppose deux équipes différentes, et son vainqueur est l'une d'elles.
- **CV-9** : le nombre total de matchs affichés est toujours égal à n - 1.
