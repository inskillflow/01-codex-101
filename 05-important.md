Dans le tutoriel précédent, il y avait déjà ces éléments :

```text
/plan
```

et aussi :

```text
Ne code pas encore.
Je veux d’abord lire et valider ta stratégie.
```

et :

```text
Lis reports/columns_analysis.csv et reports/eda_report.md.
Propose une stratégie colonne par colonne.
```

Donc oui, l’idée était là. Mais pour ton cours, il faut renforcer cela avec une règle centrale :

```text
Claude ou Codex ne doit jamais décider avant d’avoir lu les données.
```

Il faut imposer une logique en 5 phases.

# La bonne logique obligatoire

```text
Phase 1 — Créer la structure du projet
Phase 2 — Inspecter le fichier brut
Phase 3 — Faire l’EDA
Phase 4 — Décider colonne par colonne selon les vraies données
Phase 5 — Coder le nettoyage et l’encodage seulement après validation
```

Le point très important est celui-ci :

```text
La décision ne dépend pas seulement du type de colonne.
Elle dépend du contenu réel de la colonne.
```

Par exemple, une colonne `string` peut être :

```text
un pays
un identifiant
un nom d’organisation
une date mal reconnue
un code métier
un texte libre
une catégorie faible cardinalité
une catégorie forte cardinalité
```

Donc Claude ou Codex doit d’abord analyser :

```text
nombre de valeurs uniques
pourcentage de valeurs manquantes
exemples de valeurs
rôle probable de la colonne
risque d’explosion du nombre de colonnes
utilité pour le ML
```

---

# À ajouter absolument dans `CLAUDE.md`

Voici la version plus forte à mettre dans `CLAUDE.md`.

```md
# CLAUDE.md — Règles obligatoires du projet EDA

## Rôle

Tu agis comme data engineer senior et machine learning engineer.

Le projet sert à préparer un gros fichier tabulaire pour l’analyse exploratoire, le nettoyage, l’encodage et une éventuelle modélisation machine learning.

## Règle principale

Tu ne dois jamais transformer les données avant de les avoir analysées.

Tu dois toujours suivre cet ordre :

1. inspecter le fichier ;
2. faire l’EDA ;
3. produire un rapport ;
4. proposer une stratégie colonne par colonne ;
5. attendre validation ;
6. seulement ensuite écrire le code de nettoyage ou d’encodage.

## Décision dépendante des données

Pour chaque colonne, tu dois décider selon les données réelles, pas seulement selon le type technique.

Avant de transformer une colonne, tu dois vérifier :

- son nom ;
- son type détecté ;
- son nombre de valeurs uniques ;
- son pourcentage de valeurs manquantes ;
- des exemples de valeurs ;
- son rôle probable ;
- son risque pour le machine learning ;
- son risque mémoire.

## Interdiction

Il est interdit de faire automatiquement du one-hot encoding sur toutes les colonnes string.

## Règles d’encodage

- Si une colonne texte a peu de catégories utiles, proposer one-hot encoding.
- Si une colonne texte a trop de valeurs uniques, ne pas faire de one-hot encoding direct.
- Si une colonne est un identifiant, la supprimer du dataset ML ou la garder séparément.
- Si une colonne est une date, extraire année, mois, jour, trimestre.
- Si une colonne est un nom d’organisation, éviter le one-hot encoding naïf.
- Si une colonne est presque vide, proposer sa suppression.
- Si une colonne est constante, proposer sa suppression.
- Si une colonne est numérique, vérifier si elle est vraiment quantitative ou seulement un code.

## Étape de validation obligatoire

Avant de créer `03_clean_data.py` ou `04_encode_features.py`, tu dois produire une table de décision avec :

| colonne | type | valeurs uniques | valeurs manquantes | exemples | rôle probable | décision | justification |

Les décisions possibles sont :

- keep_numeric
- drop_identifier
- drop_empty
- drop_constant
- parse_date
- one_hot_encode
- frequency_encode
- group_rare_then_one_hot
- clean_text_keep
- ignore_for_ml

Tu dois attendre la validation humaine avant de coder.
```

---

# Prompt corrigé à donner à Claude Opus 4.8 ou Codex

Celui-ci est meilleur que le précédent, parce qu’il impose clairement la planification et la décision selon les données.

```text
/model claude-opus-4-8
/effort xhigh

Tu vas m’aider à créer un projet EDA complet pour un gros fichier tabulaire.

Très important :
tu ne dois pas décider les transformations à l’avance.
Tu dois d’abord analyser les vraies données.

Le fichier brut sera placé dans :

data/raw/

Structure attendue :

big-file-eda-codex/
│
├── data/
│   ├── raw/
│   ├── processed/
│
├── notebooks/
│   └── 01_eda.ipynb
│
├── src/
│   ├── 01_inspect_file.py
│   ├── 02_eda_profile.py
│   ├── 03_clean_data.py
│   ├── 04_encode_features.py
│   └── 05_train_ready_dataset.py
│
├── reports/
│   ├── eda_report.md
│   └── columns_analysis.csv
│
├── AGENTS.md
├── CLAUDE.md
├── requirements.txt
└── README.md

Tu dois travailler en phases.

Phase 1 :
Créer uniquement la structure du projet.

Phase 2 :
Créer src/01_inspect_file.py pour inspecter le fichier brut sans le charger entièrement.

Phase 3 :
Créer src/02_eda_profile.py pour analyser les colonnes.

Phase 4 :
Lire les rapports générés :
- reports/columns_analysis.csv
- reports/eda_report.md

Ensuite, proposer une stratégie colonne par colonne.

Pour chaque colonne, tu dois produire une table avec :

1. nom de colonne ;
2. type détecté ;
3. nombre de valeurs uniques ;
4. pourcentage de valeurs manquantes ;
5. exemples de valeurs ;
6. rôle probable ;
7. décision proposée ;
8. justification.

Les décisions possibles sont :
- keep_numeric
- drop_identifier
- drop_empty
- drop_constant
- parse_date
- one_hot_encode
- frequency_encode
- group_rare_then_one_hot
- clean_text_keep
- ignore_for_ml

Phase 5 :
Attendre ma validation avant de créer :
- src/03_clean_data.py
- src/04_encode_features.py
- src/05_train_ready_dataset.py

Règles obligatoires :

1. Ne jamais modifier data/raw.
2. Ne jamais faire du one-hot encoding automatique sur toutes les colonnes string.
3. Décider selon les vraies données.
4. Si une colonne contient trop de valeurs uniques, éviter le one-hot encoding direct.
5. Si une colonne ressemble à un identifiant, ne pas l’utiliser comme variable ML.
6. Si une colonne ressemble à une date, proposer une extraction année/mois/jour.
7. Si une colonne est presque vide, proposer sa suppression.
8. Si une colonne est constante, proposer sa suppression.
9. Si une colonne contient des noms d’organisations, éviter l’encodage naïf.
10. Toujours justifier chaque décision.

Maintenant, commence uniquement par le plan.
Ne crée aucun fichier avant que je dise oui.
```

---

# La phrase clé à ajouter dans le cours

Tu peux écrire ceci dans ton cours :

```text
Dans ce projet, Codex ou Claude ne doit pas appliquer une recette automatique.
Il doit d’abord observer les données, produire un rapport, puis décider colonne par colonne.
La transformation dépend du contenu réel des colonnes.
```

Et encore plus simple pour les débutants :

```text
On ne transforme pas parce que la colonne est du texte.
On transforme parce qu’on a compris ce que cette colonne représente.
```

---

# Donc la réponse claire

Oui, c’était présent dans l’idée, mais maintenant il faut le rendre **obligatoire** dans le tutoriel.

La meilleure formulation à imposer est :

```text
Analyse d’abord.
Décide ensuite.
Code seulement après validation.
```

Pour ton projet, c’est vraiment la règle centrale.
