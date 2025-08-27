# Remise Ã  niveau SQL â€“ Projet LÃ©on

Bienvenue !  
Tu viens de rejoindre le projet **LÃ©on**, une base de donnÃ©es qui permet de stocker des informations sur des **films**, **musiques**, **livres**, etc.

Ce TP a Ã©tÃ© conÃ§u pour t'aider Ã  te replonger dans les bases de **SQL** et de la **modÃ©lisation des donnÃ©es**.  
Tu disposes dâ€™un **schÃ©ma de base de donnÃ©es** au format `.png` et `.drawio`, mais attention : il nâ€™est plus tout Ã  fait Ã  jour !

---

> Si tu bloques Ã  un moment donnÃ© :
> - Tu peux chercher sur internet
> - Demander de lâ€™aide
> - Passer temporairement Ã  la suite
>
> Pense aussi Ã  **prendre des notes** Ã  chaque Ã©tape (requÃªtes intermÃ©diaires, dÃ©finitions, observations sur les tables, etc.).

---

## Ressources utiles

- [Documentation MySQL officielle](https://dev.mysql.com/doc/)
- [Cours SQL (SQL.sh)](https://sql.sh/cours/)

---

## Ã‰tape 1 â€“ Interroger la base

Pour interroger la base de donnÃ©es, vous allez devoir utiliser un langage de requÃªte nommÃ© SQL.
Un requÃªte se compose d'instructions et se termine par un `;`.

En plus des diffÃ©rentes instructions, le langage permet d'utiliser diffÃ©rents types de fonctions.

La base de donnÃ©es quant Ã  elle se compose de diffÃ©rentes tables (ex: films) et chaque table se compose de colonnes (ex: titre). Les donnÃ©es vont Ãªtre rangÃ©es dans les colonnes de la table correspondante. Vous pouvez voir Ã§a comme un classeur Excel oÃ¹ chaque table est une feuille avec diffÃ©rentes donnÃ©es.

### Ma premiÃ¨re requÃªte

`select colonne, colonne,... from table;`

- `select` est une instruction qui permet d'indiquer que vous voulez rÃ©cupÃ©rer les donnÃ©es de la base. Vous indiquez Ã  la suite les colonnes que vous voulez rÃ©cupÃ©rer ;
- `from` est une instructions qui permet d'indiquer de quel table vous rÃ©cupÃ©rer les donnÃ©es.

Liste les titres des films

```sql
show tables;
select Titre from films;
```

Il est possible d'ajouter des commentaires comme dans tous les langages de programmation.
```sql
select titre from films; -- Selectionne les titres des films
```
Il existe diffÃ©rents mÃ©thodes pour Ã©crire un commentaire :
- `-- commentaire` -> commentaire jusquâ€™Ã  la fin de la ligne ;
- `# commentaire` -> commentaire jusquâ€™Ã  la fin de la ligne ;
- `/* commentaire */` ->  commentaire multi-ligne Ã  lâ€™avantage de pouvoir indiquer oÃ¹ commence et oÃ¹ se termine le commentaire. Il est donc possible de lâ€™utiliser en plein milieu dâ€™une requÃªte SQL sans problÃ¨me.

Pour rÃ©cupÃ©rer toutes les colonnes de la table, vous pouvez utiliser `*`.
```sql
select * from table; -- SÃ©lÃ©ctionne toutes les colonnes de la table
```

Afficher toutes les donnÃ©es de la table films
```sql
select * from films;
```
En effet, la table comporte beaucoup plus de colonnes que sur le schÃ©ma qui n'est donc pas Ã  jour.
Il est recommandÃ© de commencer avec une requÃªte gÃ©nÃ©rique et de filtrer petit Ã  petit sur les donnÃ©es qui vous intÃ©ressent.

### Alias

Dans le langage SQL, il est possible de renommer des colonnes ou des tables avec des alias. Cela se fera uniquement pour votre requÃªte.

Alias sur une table
```sql
select * from table as table_alias;
```

Alias sur une colonne
```sql
select colonne as colonne_alias from table;
```

Renommer toute la table films en anglais (colonne et nom de de la table)
```sql
select id, titre as title, duree as duration, date_de_sortie as release_date, synopsis, budget, realisateur_id as director_id from films as movies;
```

### Ajouter des conditions

L'instruction `where` dans une requête SQL permet d'ajouter une ou plusiseurs conditions. Cela permet d'obtenir uniquement les informations désirées.
```sql
select colonne from table where colonne opérateur condition;
```

Pour cela, vous pouvez aussi vous servir de differents operateurs :
- `=` -> Egal ;
- `!=` -> pas Egal ;
- `and` & `or` -> Opérateurs logiques pour ajouter d'autres conditions ;
- `in` -> VÃ©rifie si la valeur est Ã©gale Ã  celle dans la liste fourni (ex: `where colonne in ( valeur1, valeur2, valeur3, ... )`) ;
- `between` -> permet de sÃ©lectionner dans une intervalle de  donnÃ©es ;
- `like` -> permet de filtrer sur un modèle particulier (plus simple que les regex).

```sql
select colonne from table where colonne = "toto" or colonne = "tata";
```

Liste les acteurs ayant le prÃ©nom `josé`
```sql
select * from acteurs where prenom = "José";
``` 

Liste les titres de films sortis entre 1986 & 1992
```sql
select titre from films where date_de_sortie between 1986 and 1992;
```

Liste les titres de films sortis entre 1986 & 1992, mais pas ceux de 1990
```sql
select titre from films where date_de_sortie between 1986 and 1992 and date_de_sortie <> 1990;
```

Liste les genres commençant par la lettre `r`
```sql
select * from genres where nom like "r%";
```

Liste les acteurs avec `o` pour troisième lettre du prénom
```sql
select * from acteurs where prenom like "__o%";
```

Liste les réalisateurs avec un nom ne finissant pas par `ood`
```sql
select * from realisateurs where nom not like "%ood";
```

### Mettre de l'ordre

`order by` permet de trier par ordre ascendant ou descendant sur une ou plusieurs colonnes.

```sql
select colonnne from table order by colonne ASC, colonne_2 DESC, colonne_3 ASC, ...;
```

Liste les titres de films par ordre descendant
```sql
select titre from films order by titre DESC;
```

Liste le nom et le prénom des réalisateurs par ordre ascendant pour le nom et desendant pour le prenom si le prénom ne comporte pas la lettre `a`.
```sql
select prenom, nom from realisateurs where prenom not like "%a%" order by prenom DESC, nom ASC;
```

### Groupe

`group by` permet de grouper un ou plusieurs résultats. Cette instruction est souvent utilisée avec des fonctions.

Afficher toutes les dates de sorties
```sql

```

Fonctionne aussi avec un `distinct`, il permet de retourner que les valeurs uniques.
```sql

```

### Fonctions SQL

#### Min/Max

La fonction `min()` permet de retourner la plus petite valeur. La fonction `max()` retourne la plus grande valeur.
```sql
select min(colonne) from table;
```
Par dÃ©faut, si la colonne n'est pas renommÃ©, elle gardera le nom de la fonction `min(colonne)`. Il est recommandÃ© de la renommer pour avoir une meilleure comprÃ©hension.

Afficher le film avec la plus petite duree en renommant la colonne
```sql

```

Afficher le film avec le plus grand budget en renommant la colonne pour les films sortis en 1983, 1954 et 1974
```sql

```

#### Moyenne

La fonction `avg()` permet d'obtenir la moyenne.

Afficher la moyenne de durÃ©e des films
```sql

```

#### Compter le nombre d'enregistrement

La fonction `count()` permet de compter le nombre d'enregistrement/ligne.
```sql
select count(*) from table where ...;
```

Combien de film dure plus d'1h30 ?
```sql

```

#### Somme

La fonction `sum()` permet d'additioner les valeurs d'une colonne.

Affichez le prix de tout les films du realisateur `Verhoeven Paul`
```sql

```

#### Jouer avec les chaines de caractÃ¨res

##### Concat

La fonction `concat()` permet de concatÃ©ner les valeur de plusieurs colonnes pour ne former quâ€™une seule chaÃ®ne de caractÃ¨re.

Affichez le nom et le prenom des acteurs dans une seule colonne nommÃ©e acteur (ex: `MCDOWELL MALCOLM`)
```sql

```

##### Replace

La fonction `replace()` permet de remplacer des caractÃ¨res dans une chaÃ®ne de caractÃ¨re.

A partir de la requÃªte prÃ©cÃ©dente, affichez le nom et le prenom des acteurs dans une seule colonne nommÃ©e acteur (ex: `MCDOWELL MALCOLM`) en affichant que le prÃ©nom `Alain` et en remplaÃ§ant les `a` par des `o`. 
```sql

``` 

##### Substring

La fonction `substring()` est utilisÃ©e pour segmenter une chaÃ®ne de caractÃ¨re. Cela permet dâ€™extraire une partie dâ€™un chaÃ®ne, par exemple pour tronquer un texte.

Affichez les 15 premiers caractÃ¨res du synopsis de chaque film
```sql

```

Affichez les caractÃ¨res du synopsis jusqu'au premier point de chaque film (vous aurez besoin d'une autre fonction pour y parvenir)
```sql

```

## Etape 2 - RequÃªtes avancÃ©es

Les tables d'une mÃªme base de donnÃ©es peuvent Ãªtre reliÃ©es entre elles. Cela s'appelle des relations et se manifeste par la prÃ©sence de clÃ© Ã©trangÃ¨re.

Pour l'Ã©tape 2, vous Ãªtes laissÃ© Ã  vous-mÃªme. Le but est d'Ã©valuer rapidement votre niveau. Vous Ãªtes autorisÃ©s Ã  chercher sur Internet (n'oubliez pas de tout prendre en note).

### Sous-requÃªte

En utilisant une sous-requÃªte, affichez la durÃ©e moyenne des films rÃ©alisÃ©s par `Spielberg`
```sql

```

### Fusion de tables

Affichez les informations des acteurs et des rÃ©alisateurs (c'est Ã  vous de chercher comment afficher les donnÃ©es des deux tables en une)
```sql

```

### Les jointures

Sans utiliser de sous-requÃªte, afficher tout le nom et le prÃ©nom des acteurs qui ont jouÃ© avec le rÃ©alisateur `Stanley Kubrick`, ainsi que le nom du film.
```sql





```

## Etape 3 - Conception de la base de donnÃ©es

En vous baladant dans ce monde de donnÃ©es, vous remarquez que le schÃ©ma qui vous a Ã©tÃ© fourni a Ã©tÃ© pensÃ© lors du tout dÃ©but de la base de donnÃ©es et qu'il n'a pas Ã©tÃ© mis Ã  jour au fil des changement dans le dÃ©veloppement.
En tant que nouvel arrivant sur ce projet, vous allez devoir le mettre Ã  jour.

Pour rÃ©aliser cette tÃ¢che, vous allez devoir rÃ©cupÃ©rer les secrets des diffÃ©rentes tables Ã  l'aide de requÃªtes SQL.
Ensuite, vous allez devoir reconstruire le schÃ©ma, mais pas avec n'importe quelle mÃ©thode ! Vous allez devoir rÃ©aliser le MCD de la base de donnÃ©es.

### Pour commencer, que signifie MCD ?

Un MCD est un ...




### A quoi Ã§a sert le MLD ?

Un MLD est un ...





## Explorer le schÃ©ma de la base via des requÃªtes SQL

> Objectif : Reconstituer la structure de la base de donnÃ©es **uniquement Ã  partir de requÃªtes SQL**, sans consulter de code ou de diagramme.

Cette Ã©tape te permet de mieux comprendre comment les donnÃ©es sont organisÃ©es *en partant directement de la base existante*.

Pour tâ€™aider, tu peux consulter la documentation officielle de MySQL :
[Documentation MySQL - Information Schema](https://dev.mysql.com/doc/refman/8.4/en/information-schema.html)

### âš ï¸ Ã€ ne pas prendre en compte

Certaines tables sont gÃ©nÃ©rÃ©es automatiquement par Laravel et **ne concernent pas le modÃ¨le mÃ©tier**.
Tu peux les exclure de tes rÃ©sultats SQL Ã  lâ€™aide de ce filtre :

```sql
WHERE table_name NOT IN (
  'cache',
  'cache_locks',
  'failed_jobs',
  'jobs',
  'job_batches',
  'migrations',
  'password_reset_tokens',
  'sessions',
  'users'
)
```

## RequÃªtes utiles

### 1. Lister toutes les bases de donnÃ©es disponibles

```sql

```

> Pour ce TP, la base de donnÃ©es utilisÃ©e est nommÃ©e : `laravel`

---

### 2. Lister toutes les tables de la base `laravel`

```sql

```

---

### 3. Lister les colonnes d'une table spÃ©cifique

```sql

```

---

### 4. Lister les colonnes de **toutes les tables**

```sql

```

---

### 5. Lister les colonnes avec leurs **types**

```sql

```

---

### 6. Lister les colonnes avec les **clÃ©s primaires, secondaires** et les **contraintes dâ€™unicitÃ©**

```sql

```

* `PRI` â†’ ClÃ© primaire
* `UNI` â†’ ClÃ© unique
* `MUL` â†’ ClÃ© Ã©trangÃ¨re (ou utilisÃ©e dans une jointure)

---

### 7. Lister les **clÃ©s Ã©trangÃ¨res** et leurs rÃ©fÃ©rences

```sql

```

---

### 8. Lister les colonnes avec **auto-incrÃ©ment**

```sql

```

> En gÃ©nÃ©ral, seules les clÃ©s primaires sont auto-incrÃ©mentÃ©es.

---

Avec lâ€™ensemble de ces requÃªtes, tu peux **reconstruire le MLD de la base de donnÃ©es**, et ainsi prÃ©parer la phase suivante : la conception du **MCD**.


## Objectif : Repartir de la Pratique vers la ModÃ©lisation

Tu as dÃ©jÃ  manipulÃ© la base de donnÃ©es en SQL, ce qui tâ€™a permis de comprendre comment les donnÃ©es sont rÃ©ellement organisÃ©es. Maintenant, il est temps de **remonter la chaÃ®ne de conception**, depuis le modÃ¨le logique jusquâ€™au modÃ¨le conceptuel.

### Ã‰tape 1 : RecrÃ©er le **MLD** (ModÃ¨le Logique de DonnÃ©es)

Ã€ partir de tes requÃªtes SQL et de lâ€™analyse de la structure actuelle de la base :

* Reconstitue le **MLD** : quelles sont les tables ? les relations ? les clÃ©s primaires et Ã©trangÃ¨res ?
* Formalise-le proprement pour en faire un modÃ¨le clair et exploitable.

*Pense Ã  bien traduire les contraintes observÃ©es dans la base (types de jointures, relations, unicitÃ©, etc.) en Ã©lÃ©ments du MLD.*

### Ã‰tape 2 : Revenir au **MCD** (ModÃ¨le Conceptuel de DonnÃ©es)

Une fois le MLD reconstruit :

* Remonte au **MCD**, câ€™est-Ã -dire au niveau conceptuel, plus abstrait.
* Identifie les entitÃ©s, leurs attributs, et les associations entre elles.
* ReprÃ©sente visuellement le MCD en utilisant lâ€™outil draw\.io comme base de travail.

### DerniÃ¨re Ã‰tape : **AmÃ©liorer le MCD**

Le MCD initial comporte sans doute des imprÃ©cisions ou des choix discutables (entitÃ©s mal dÃ©finies, relations superflues ou confuses, manque de normalisation...).

Ton objectif est donc de **rÃ©flÃ©chir Ã  une version amÃ©liorÃ©e** du MCD :

* Clarifie les rÃ´les de chaque entitÃ©.
* RÃ©duis les redondances et optimise la structure.
* Assure-toi que le modÃ¨le est **cohÃ©rent, comprÃ©hensible et fidÃ¨le aux besoins fonctionnels**.
