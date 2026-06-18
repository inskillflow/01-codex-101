# Tutoriel débutant — Créer un projet EDA complet avec Codex dans VS Code

## Objectif

On veut préparer un gros fichier de données comme celui dans Excel.

Le projet devra permettre de faire :

```text
1. Inspection du fichier
2. EDA simple
3. Nettoyage des données
4. Transformation des colonnes texte
5. Encodage intelligent
6. Création d’un dataset final prêt pour machine learning
```

Le point important est celui-ci :

```text
On ne demande pas à Codex de tout transformer automatiquement.
On demande à Codex d’abord d’analyser, ensuite de proposer un plan, puis seulement après de coder.
```

---

# PARTIE 1 — Créer le dossier du projet

## Étape 1 — Ouvrir VS Code

Ouvrir Visual Studio Code.

Ensuite ouvrir un terminal dans VS Code :

```text
Terminal > New Terminal
```

Sur Windows, le terminal peut être PowerShell.

---

## Étape 2 — Créer le dossier principal

Dans le terminal, taper :

```powershell
mkdir big-file-eda-codex
cd big-file-eda-codex
code .
```

Explication :

```text
mkdir big-file-eda-codex
```

crée le dossier du projet.

```text
cd big-file-eda-codex
```

entre dans le dossier.

```text
code .
```

ouvre ce dossier dans VS Code.

---

# PARTIE 2 — Demander à Codex de créer la structure

Dans le panneau Codex de VS Code, écrire d’abord :

```text
/status
```

Puis :

```text
/plan
```

Ensuite, coller ce prompt.

## Prompt 1 — Créer la structure complète du projet

```text
Tu vas m’aider à créer un projet complet pour analyser et préparer un gros fichier de données tabulaires.

Le public est débutant, donc je veux une structure simple, claire et professionnelle.

Crée l’arborescence suivante :

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
├── requirements.txt
└── README.md

Important :
1. Ne code pas encore la logique complète.
2. Crée seulement les dossiers et les fichiers de base.
3. Mets un commentaire simple dans chaque fichier Python pour expliquer son rôle.
4. Prépare un README.md avec une explication simple du projet.
5. Prépare un requirements.txt avec les librairies nécessaires pour EDA, gros fichiers et machine learning.
6. Prépare AGENTS.md avec les règles que Codex devra respecter dans ce projet.

Avant de créer les fichiers, donne-moi le plan exact de ce que tu vas faire.
```

Après que Codex donne le plan, écrire :

```text
Oui, applique maintenant ce plan et crée les dossiers et fichiers.
```

---

# PARTIE 3 — Créer la structure manuellement si Codex ne le fait pas

Si Codex ne crée pas les dossiers correctement, les étudiants peuvent le faire avec PowerShell.

## Commandes PowerShell

```powershell
mkdir data
mkdir data\raw
mkdir data\processed
mkdir notebooks
mkdir src
mkdir reports

New-Item notebooks\01_eda.ipynb
New-Item src\01_inspect_file.py
New-Item src\02_eda_profile.py
New-Item src\03_clean_data.py
New-Item src\04_encode_features.py
New-Item src\05_train_ready_dataset.py
New-Item reports\eda_report.md
New-Item reports\columns_analysis.csv
New-Item AGENTS.md
New-Item requirements.txt
New-Item README.md
```

Ensuite vérifier :

```powershell
dir
```

---

# PARTIE 4 — Expliquer la structure aux débutants

Voici ce que les étudiants doivent comprendre.

```text
data/raw
```

C’est ici qu’on place le fichier original.
On ne modifie jamais ce fichier.

Exemple :

```text
data/raw/grants.csv
```

ou :

```text
data/raw/grants.xlsx
```

---

```text
data/processed
```

C’est ici qu’on place les fichiers transformés.

Exemples :

```text
cleaned_data.parquet
encoded_dataset.parquet
final_dataset.parquet
final_dataset_sample.csv
```

---

```text
notebooks
```

C’est ici qu’on place les notebooks Jupyter pour explorer les données visuellement.

---

```text
src
```

C’est ici qu’on place les scripts Python.

Chaque script a un rôle précis.

| Fichier                     | Rôle                                                     |
| --------------------------- | -------------------------------------------------------- |
| `01_inspect_file.py`        | Vérifie le fichier brut sans tout charger.               |
| `02_eda_profile.py`         | Analyse les colonnes, les types, les valeurs manquantes. |
| `03_clean_data.py`          | Nettoie les données.                                     |
| `04_encode_features.py`     | Transforme les textes en nombres de façon intelligente.  |
| `05_train_ready_dataset.py` | Produit le dataset final pour le machine learning.       |

---

```text
reports
```

C’est ici qu’on sauvegarde les rapports.

Exemples :

```text
initial_file_report.md
eda_report.md
columns_analysis.csv
cleaning_report.md
encoding_report.md
final_dataset_report.md
```

---

```text
AGENTS.md
```

C’est le fichier qui donne les règles à Codex.

---

```text
requirements.txt
```

C’est la liste des librairies Python nécessaires.

---

```text
README.md
```

C’est la documentation du projet.

---

# PARTIE 5 — Créer l’environnement Python

Dans le terminal VS Code :

```powershell
py -3.12 -m venv .venv
```

Activer l’environnement :

```powershell
.venv\Scripts\activate
```

Mettre pip à jour :

```powershell
python -m pip install --upgrade pip
```

---

# PARTIE 6 — Demander à Codex de remplir `requirements.txt`

Dans Codex, écrire :

```text
Remplis le fichier requirements.txt pour ce projet.

Le projet doit permettre :
1. lire des fichiers CSV ;
2. lire des fichiers Excel ;
3. traiter de gros fichiers ;
4. faire une EDA ;
5. nettoyer les données ;
6. encoder les variables catégorielles ;
7. sauvegarder en Parquet ;
8. préparer un dataset pour machine learning.

Utilise des librairies simples pour débutants, mais ajoute aussi des outils efficaces pour gros fichiers.

Ne mets pas de versions strictes pour l’instant.
```

Le fichier devrait ressembler à ceci :

```txt
pandas
numpy
polars
duckdb
pyarrow
openpyxl
scikit-learn
matplotlib
jupyter
```

Ensuite installer :

```powershell
pip install -r requirements.txt
```

---

# PARTIE 7 — Créer le fichier `AGENTS.md`

Dans Codex, demander :

```text
Remplis le fichier AGENTS.md.

Ce fichier doit expliquer à Codex comment travailler dans ce projet.

Le projet concerne un gros fichier tabulaire.

Règles obligatoires :
1. Ne jamais modifier le fichier brut dans data/raw.
2. Ne jamais charger un fichier gigantesque entièrement sans inspection.
3. Toujours commencer par analyser la taille, les colonnes et les types.
4. Toujours produire un rapport avant transformation.
5. Ne jamais faire du one-hot encoding sur toutes les colonnes string automatiquement.
6. Identifier les colonnes ID, dates, montants, catégories, noms, textes libres.
7. Appliquer one-hot encoding seulement aux colonnes avec peu de catégories.
8. Utiliser frequency encoding ou suppression pour les colonnes avec trop de valeurs uniques.
9. Sauvegarder les fichiers transformés dans data/processed.
10. Sauvegarder les rapports dans reports.
11. Écrire un code clair, simple et commenté pour débutants.

Ajoute aussi une section "Workflow attendu" avec les scripts à exécuter dans l’ordre.
```

---

# PARTIE 8 — Placer le fichier brut

Les étudiants doivent placer le fichier dans :

```text
data/raw/
```

Exemple :

```text
data/raw/grants.xlsx
```

ou :

```text
data/raw/grants.csv
```

Important :

```text
Le fichier original reste toujours dans data/raw.
On ne le modifie jamais.
```

---

# PARTIE 9 — Demander à Codex de créer le premier script

## Script 1 — Inspection du fichier

Dans Codex :

```text
Crée le fichier src/01_inspect_file.py.

Ce script doit être très simple et adapté aux débutants.

Objectif :
inspecter le fichier placé dans data/raw sans le transformer.

Le script doit :
1. chercher automatiquement le premier fichier dans data/raw ;
2. afficher le nom du fichier ;
3. afficher son extension ;
4. afficher sa taille en MB ;
5. détecter si le fichier est CSV, Excel ou Parquet ;
6. si c’est un CSV, lire seulement les 10 premières lignes ;
7. si c’est un Excel, afficher les noms des feuilles et lire seulement les 10 premières lignes de la première feuille ;
8. afficher les noms des colonnes ;
9. afficher les types détectés ;
10. sauvegarder un rapport dans reports/initial_file_report.md.

Contraintes :
- ne pas charger tout le fichier si le fichier est très gros ;
- mettre des commentaires simples dans le code ;
- ajouter des messages print clairs pour les débutants ;
- gérer les erreurs si aucun fichier n’est trouvé.
```

Puis exécuter :

```powershell
python src/01_inspect_file.py
```

---

# PARTIE 10 — Demander à Codex de créer le script EDA

## Script 2 — EDA des colonnes

Dans Codex :

```text
Crée le fichier src/02_eda_profile.py.

Objectif :
faire une première analyse exploratoire des données.

Le script doit :
1. lire le fichier depuis data/raw ;
2. utiliser un échantillon si le fichier est très volumineux ;
3. calculer le nombre de colonnes ;
4. afficher les noms des colonnes ;
5. détecter les types de colonnes ;
6. calculer le pourcentage de valeurs manquantes ;
7. calculer le nombre de valeurs uniques par colonne ;
8. afficher quelques exemples de valeurs pour chaque colonne ;
9. détecter les colonnes constantes ;
10. détecter les colonnes presque vides ;
11. détecter les colonnes qui ressemblent à des identifiants ;
12. détecter les colonnes qui ressemblent à des dates ;
13. détecter les colonnes catégorielles avec peu de valeurs ;
14. détecter les colonnes catégorielles avec beaucoup de valeurs.

Le script doit sauvegarder :
- reports/columns_analysis.csv
- reports/eda_report.md

Important :
- ne fais aucun nettoyage ;
- ne fais aucun encodage ;
- fais seulement l’analyse.
- écris le code de manière simple pour débutants.
```

Puis :

```powershell
python src/02_eda_profile.py
```

---

# PARTIE 11 — Demander à Codex d’analyser avant de transformer

C’est ici que le mode plan est très important.

Dans Codex :

```text
/plan

Lis les résultats produits par :
- reports/columns_analysis.csv
- reports/eda_report.md

Je veux que tu proposes une stratégie de transformation colonne par colonne.

Pour chaque colonne, donne une table avec :

1. nom de la colonne ;
2. type détecté ;
3. nombre de valeurs uniques ;
4. pourcentage de valeurs manquantes ;
5. rôle probable de la colonne ;
6. décision proposée ;
7. justification.

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

Important :
Ne code pas encore.
Je veux d’abord lire et valider ta stratégie.
```

Là, Codex doit réfléchir avant d’écrire du code.

---

# PARTIE 12 — Explication importante pour les débutants

Il faut leur expliquer ceci clairement.

## Mauvaise approche

```text
Toutes les colonnes string → one-hot encoding
```

C’est mauvais.

Pourquoi ?

Parce qu’une colonne comme :

```text
recipient_legal_name
```

peut contenir des milliers d’organisations différentes.

Exemple :

```text
LE CHENAIL INC.
UPSTREAM MUSIC ASSOCIATION
The Take Action Society
```

Si on fait du one-hot encoding dessus, on peut créer des milliers de colonnes.

---

## Bonne approche

Codex doit décider selon le rôle de la colonne.

| Type de colonne     | Exemple                                      | Décision possible              |
| ------------------- | -------------------------------------------- | ------------------------------ |
| Pays                | `recipient_country`                          | One-hot encoding               |
| Type d’accord       | `agreement_type`                             | One-hot encoding               |
| Date                | `agreement_date`                             | Extraire année, mois, jour     |
| Numéro de référence | `ref_number`                                 | Supprimer ou garder comme ID   |
| Nom légal           | `recipient_legal_name`                       | Nettoyer ou frequency encoding |
| Montant             | `agreement_value`                            | Garder numérique               |
| Texte libre         | `description`                                | Garder pour NLP ou ignorer     |
| Colonne vide        | `research_organization_name` si presque vide | Supprimer                      |

---

# PARTIE 13 — Créer le script de nettoyage

Après validation de la stratégie :

```text
Crée le fichier src/03_clean_data.py.

Objectif :
nettoyer les données sans encore faire l’encodage final.

Le script doit :
1. lire le fichier brut depuis data/raw ;
2. ne jamais modifier le fichier original ;
3. nettoyer les noms de colonnes :
   - minuscules ;
   - remplacer espaces par underscore ;
   - supprimer caractères spéciaux inutiles ;
4. supprimer les colonnes vides selon l’analyse ;
5. supprimer les colonnes constantes selon l’analyse ;
6. nettoyer les colonnes texte :
   - enlever les espaces au début et à la fin ;
   - remplacer les chaînes vides par des valeurs manquantes ;
   - uniformiser les textes si nécessaire ;
7. convertir les colonnes dates détectées ;
8. convertir les colonnes numériques mal reconnues ;
9. sauvegarder le résultat dans :
   data/processed/cleaned_data.parquet
10. produire un rapport :
   reports/cleaning_report.md

Le code doit être simple, commenté et compréhensible par un débutant.
```

Puis :

```powershell
python src/03_clean_data.py
```

---

# PARTIE 14 — Créer le script d’encodage intelligent

Dans Codex :

```text
Crée le fichier src/04_encode_features.py.

Objectif :
transformer les données nettoyées en données numériques utilisables pour machine learning.

Le script doit lire :
data/processed/cleaned_data.parquet

Règles d’encodage :
1. Garder les colonnes numériques.
2. Pour les dates, créer des colonnes :
   - année ;
   - mois ;
   - jour ;
   - trimestre.
3. Supprimer les colonnes identifiants du dataset ML.
4. Pour les colonnes catégorielles avec peu de valeurs uniques :
   - appliquer OneHotEncoder.
5. Pour les colonnes catégorielles avec beaucoup de valeurs uniques :
   - ne pas faire de one-hot encoding direct ;
   - utiliser frequency encoding ou regrouper les catégories rares.
6. Ne pas encoder naïvement les noms d’organisations.
7. Ne pas encoder naïvement les numéros de référence.
8. Sauvegarder :
   data/processed/encoded_dataset.parquet
9. Sauvegarder un rapport :
   reports/encoding_report.md

Ajoute des commentaires pédagogiques pour expliquer pourquoi certaines colonnes sont encodées et d’autres non.
```

Puis :

```powershell
python src/04_encode_features.py
```

---

# PARTIE 15 — Créer le dataset final

Dans Codex :

```text
Crée le fichier src/05_train_ready_dataset.py.

Objectif :
vérifier que le dataset final est propre pour un modèle machine learning.

Le script doit :
1. lire data/processed/encoded_dataset.parquet ;
2. vérifier le nombre de lignes ;
3. vérifier le nombre de colonnes ;
4. vérifier s’il reste des colonnes string ;
5. vérifier les valeurs manquantes ;
6. vérifier les colonnes constantes ;
7. sauvegarder le dataset final dans :
   data/processed/final_dataset.parquet
8. sauvegarder un petit échantillon CSV dans :
   data/processed/final_dataset_sample.csv
9. produire un rapport final :
   reports/final_dataset_report.md

Le rapport doit expliquer :
- nombre final de lignes ;
- nombre final de colonnes ;
- colonnes supprimées ;
- colonnes encodées ;
- colonnes conservées ;
- problèmes restants ;
- recommandations pour la suite.
```

Puis :

```powershell
python src/05_train_ready_dataset.py
```

---

# PARTIE 16 — Commandes finales à exécuter dans l’ordre

Les étudiants doivent toujours exécuter les scripts dans cet ordre :

```powershell
python src/01_inspect_file.py
python src/02_eda_profile.py
python src/03_clean_data.py
python src/04_encode_features.py
python src/05_train_ready_dataset.py
```

Après chaque étape, vérifier les rapports :

```powershell
dir reports
```

Et les fichiers transformés :

```powershell
dir data\processed
```

---

# PARTIE 17 — Demander à Codex de créer le README final

Dans Codex :

```text
Crée un README.md complet pour débutants.

Le README doit expliquer :

1. le but du projet ;
2. la structure des dossiers ;
3. où placer le fichier brut ;
4. comment créer l’environnement Python ;
5. comment installer les dépendances ;
6. comment exécuter les scripts dans l’ordre ;
7. ce que fait chaque script ;
8. ce que signifie EDA ;
9. pourquoi on ne transforme pas toutes les colonnes texte avec one-hot encoding ;
10. pourquoi les colonnes ID doivent être traitées avec prudence ;
11. pourquoi les dates doivent être transformées ;
12. où trouver les rapports ;
13. où trouver le dataset final ;
14. quelles sont les prochaines étapes possibles pour entraîner un modèle.

Le ton doit être pédagogique, clair et adapté à des débutants.
```

---

# PARTIE 18 — Prompt maître à donner à Codex

Celui-ci est le prompt complet que tu peux donner directement à tes étudiants.

```text
/plan

Tu es un assistant data engineer et machine learning engineer.

Je suis débutant et je veux créer un projet complet dans VS Code pour préparer un gros fichier tabulaire.

Le fichier brut sera placé dans data/raw.

Je veux que tu crées tout le projet étape par étape.

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
├── requirements.txt
└── README.md

Objectif du projet :
1. inspecter le fichier ;
2. faire une EDA ;
3. analyser les colonnes ;
4. nettoyer les données ;
5. transformer les colonnes texte en nombres quand c’est pertinent ;
6. faire du one-hot encoding seulement si la colonne a peu de catégories ;
7. éviter le one-hot encoding sur les identifiants, les noms, les descriptions longues et les colonnes à forte cardinalité ;
8. sauvegarder un dataset final prêt pour machine learning.

Règles :
1. Ne code pas tout d’un coup.
2. Commence par créer la structure.
3. Ensuite crée requirements.txt.
4. Ensuite crée AGENTS.md.
5. Ensuite crée les scripts un par un.
6. Après chaque script, explique comment l’exécuter.
7. Après chaque script, explique ce qu’il produit.
8. Écris un code simple, clair, commenté et adapté à des débutants.
9. Ne modifie jamais le fichier original dans data/raw.
10. Sauvegarde tous les résultats dans data/processed et reports.

Avant de créer les fichiers, donne-moi ton plan détaillé.
```

---

# PARTIE 19 — Version courte pour les étudiants

Tu peux leur donner cette consigne simple :

```text
Votre travail n’est pas de demander à Codex : “encode toutes les colonnes”.

Votre travail est de demander à Codex :
1. d’inspecter les données ;
2. d’analyser les colonnes ;
3. de proposer une stratégie ;
4. de justifier les transformations ;
5. de coder seulement après validation.

Un bon pipeline ML commence toujours par la compréhension des données.
```

---

# PARTIE 20 — Phrase importante à retenir

```text
Une colonne texte n’est pas automatiquement une colonne catégorielle utile.
```

Exemple :

```text
recipient_country
```

peut être encodée facilement.

Mais :

```text
recipient_legal_name
```

ne doit pas être one-hot encodée naïvement, car elle peut contenir trop de valeurs différentes.

Donc le vrai objectif est :

```text
Transformer intelligemment, pas transformer automatiquement.
```
