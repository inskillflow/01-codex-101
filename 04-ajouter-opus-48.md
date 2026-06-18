# Tutoriel complet — Ajouter Claude Opus 4.8 dans VS Code et l’utiliser pour un gros fichier EDA

## 0. Objectif du tutoriel

À la fin, l’étudiant saura :

```text
1. Installer Claude Code.
2. Ajouter Claude Code dans VS Code.
3. Sélectionner Claude Opus 4.8.
4. Créer un projet EDA propre.
5. Demander à Claude de planifier avant de coder.
6. Créer les dossiers et fichiers du projet.
7. Préparer un pipeline Python pour gros fichier.
8. Faire l’inspection, l’EDA, le nettoyage et l’encodage.
9. Vérifier le dataset final.
```

Claude Code est un outil de développement agentique qui peut lire un codebase, modifier des fichiers, exécuter des commandes et s’intégrer aux outils de développement. ([Claude API Docs][1])

---

# PARTIE 1 — Vérifier les prérequis

## 1.1 Installer ou vérifier VS Code

Ouvrir VS Code.

Dans le terminal VS Code, vérifier :

```powershell
code --version
```

Claude Code pour VS Code demande **VS Code 1.98.0 ou plus récent**. Il faut aussi un compte Anthropic avec abonnement Claude compatible ou un compte Claude Console. ([Claude API Docs][2])

---

## 1.2 Vérifier Git

Dans le terminal :

```powershell
git --version
```

Si Git n’est pas installé, il faut l’installer avant de continuer.

Pourquoi ?

Parce que Git permet de suivre les changements créés par Claude ou Codex.

---

## 1.3 Vérifier Python

Dans le terminal :

```powershell
py --version
```

ou :

```powershell
python --version
```

Pour ce cours, on utilise Python 3.12 si possible.

---

## 1.4 Vérifier Node.js

Claude Code peut être installé par plusieurs méthodes. Si on utilise l’installation par `npm`, Node.js est nécessaire. La documentation Anthropic précise que l’installation globale avec `npm install -g @anthropic-ai/claude-code` nécessite Node.js 18 ou plus récent. ([Claude API Docs][3])

Vérifier :

```powershell
node --version
npm --version
```

---

# PARTIE 2 — Installer Claude Code

## 2.1 Méthode recommandée sur Windows PowerShell

Dans PowerShell :

```powershell
irm https://claude.ai/install.ps1 | iex
```

Cette commande est donnée dans le quickstart officiel de Claude Code pour Windows PowerShell. ([Claude API Docs][4])

---

## 2.2 Méthode macOS, Linux ou WSL

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

---

## 2.3 Méthode avec npm

```powershell
npm install -g @anthropic-ai/claude-code
```

Pour mettre à jour ensuite :

```powershell
npm install -g @anthropic-ai/claude-code@latest
```

Anthropic recommande cette commande pour mettre à niveau une installation npm, plutôt que `npm update -g`. ([Claude API Docs][3])

---

## 2.4 Vérifier l’installation

Dans le terminal :

```powershell
claude --version
```

Puis lancer :

```powershell
claude
```

Si Claude s’ouvre dans le terminal, l’installation CLI fonctionne.

---

# PARTIE 3 — Installer Claude Code dans VS Code

## 3.1 Installer l’extension

Dans VS Code :

```text
Ctrl + Shift + X
```

Chercher :

```text
Claude Code
```

Installer l’extension officielle **Claude Code**.

La documentation Anthropic indique que l’installation dans VS Code se fait depuis la vue Extensions avec `Ctrl + Shift + X`, puis en cherchant “Claude Code”. ([Claude API Docs][2])

---

## 3.2 Redémarrer VS Code

Après installation :

```text
Fermer VS Code
Rouvrir VS Code
```

Si l’extension n’apparaît pas, utiliser :

```text
Ctrl + Shift + P
Developer: Reload Window
```

Anthropic recommande de redémarrer VS Code ou d’utiliser “Developer: Reload Window” si l’extension n’apparaît pas après installation. ([Claude API Docs][2])

---

## 3.3 Ouvrir Claude dans VS Code

Il y a plusieurs méthodes.

Méthode 1 :

```text
Cliquer sur l’icône Spark dans la barre latérale
```

Méthode 2 :

```text
Ctrl + Shift + P
Claude Code: Open in New Tab
```

Méthode 3 :

```text
Cliquer sur Claude Code dans la barre de statut
```

La documentation indique que Claude Code peut être ouvert depuis l’icône Spark, la Command Palette ou la barre de statut. ([Claude API Docs][2])

---

## 3.4 Se connecter

Quand le panneau Claude s’ouvre :

```text
Sign in
```

Le navigateur s’ouvre.

Se connecter avec le compte Claude.

Revenir dans VS Code.

La première ouverture du panneau Claude Code déclenche un écran de connexion. ([Claude API Docs][2])

---

# PARTIE 4 — Comprendre extension VS Code vs CLI

Important pour les étudiants :

```text
L’extension Claude Code dans VS Code permet d’utiliser Claude dans un panneau graphique.
La commande claude dans le terminal nécessite aussi l’installation CLI autonome.
```

Anthropic précise que l’extension VS Code embarque sa propre copie du CLI pour le panneau de chat, mais que taper `claude` dans le terminal VS Code demande une installation CLI séparée. ([Claude API Docs][2])

Donc pour ce cours, on utilise les deux :

```text
1. Claude Code extension dans VS Code
2. Claude CLI dans le terminal intégré
```

---

# PARTIE 5 — Sélectionner Claude Opus 4.8

## 5.1 Vérifier que Opus 4.8 est disponible

Dans Claude Code, écrire :

```text
/status
```

Puis :

```text
/model
```

Choisir :

```text
Opus 4.8
```

ou :

```text
claude-opus-4-8
```

La documentation Anthropic liste Opus 4.8 avec l’identifiant `claude-opus-4-8`. ([Centre d'aide Claude][5])

---

## 5.2 Lancer directement Claude Code avec Opus 4.8

Dans le terminal VS Code :

```powershell
claude --model claude-opus-4-8
```

La documentation officielle indique que le modèle d’une session peut être choisi avec le flag `--model`, par exemple `claude --model claude-opus-4-8`. ([Centre d'aide Claude][5])

---

## 5.3 Définir Opus 4.8 comme modèle par défaut

Sur Windows PowerShell :

```powershell
setx ANTHROPIC_MODEL "claude-opus-4-8"
```

Ensuite :

```text
1. Fermer VS Code.
2. Fermer le terminal.
3. Rouvrir VS Code.
4. Rouvrir le projet.
5. Relancer claude.
```

Sur macOS avec zsh :

```bash
echo 'export ANTHROPIC_MODEL="claude-opus-4-8"' >> ~/.zshrc
source ~/.zshrc
```

Sur Linux avec bash :

```bash
echo 'export ANTHROPIC_MODEL="claude-opus-4-8"' >> ~/.bashrc
source ~/.bashrc
```

Anthropic documente `ANTHROPIC_MODEL` comme variable permettant de définir le modèle par défaut pour les futures sessions. ([Centre d'aide Claude][5])

---

# PARTIE 6 — Activer un raisonnement plus profond

Pour un projet de gros fichier EDA, on veut que Claude réfléchisse avant de coder.

Dans Claude Code :

```text
/effort
```

Choisir :

```text
xhigh
```

ou, pour une tâche très complexe :

```text
max
```

Opus 4.8 supporte les niveaux `low`, `medium`, `high`, `xhigh` et `max`. Le niveau par défaut d’Opus 4.8 est `high`, et `xhigh` donne un raisonnement plus profond avec une consommation plus élevée. ([Claude API Docs][6])

Pour les débutants, recommandation :

```text
/effort xhigh
```

Puis demander :

```text
Planifie d’abord. Ne code pas encore.
```

---

# PARTIE 7 — Créer le projet EDA dans VS Code

## 7.1 Créer le dossier principal

Dans le terminal VS Code :

```powershell
mkdir big-file-eda-codex
cd big-file-eda-codex
code .
```

---

## 7.2 Initialiser Git

```powershell
git init
```

Puis :

```powershell
git status
```

Pourquoi ?

Parce qu’avant que Claude ou Codex modifie le projet, Git doit pouvoir montrer les changements.

---

# PARTIE 8 — Demander à Claude de créer la structure

Dans Claude Code, écrire :

```text
/model claude-opus-4-8
/effort xhigh
```

Puis donner ce prompt.

## Prompt 1 — Création du projet

```text
Tu vas m’aider à créer un projet complet pour analyser et préparer un gros fichier tabulaire.

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
├── CLAUDE.md
├── requirements.txt
└── README.md

Important :
1. Ne code pas encore la logique complète.
2. Crée seulement les dossiers et les fichiers de base.
3. Mets un commentaire simple dans chaque fichier Python pour expliquer son rôle.
4. Prépare un README.md simple.
5. Prépare un requirements.txt.
6. Prépare AGENTS.md pour Codex.
7. Prépare CLAUDE.md pour Claude Code.

Avant de créer les fichiers, donne-moi le plan exact de ce que tu vas faire.
```

Quand Claude donne le plan :

```text
Oui, applique maintenant ce plan et crée les dossiers et fichiers.
```

---

# PARTIE 9 — Créer la structure manuellement si nécessaire

Si Claude ne crée pas correctement les fichiers, faire manuellement.

Dans PowerShell :

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
New-Item CLAUDE.md
New-Item requirements.txt
New-Item README.md
```

Vérifier :

```powershell
tree /F
```

---

# PARTIE 10 — Expliquer la structure aux étudiants

## Dossier `data/raw`

```text
data/raw
```

C’est le dossier du fichier original.

Exemples :

```text
data/raw/grants.csv
data/raw/grants.xlsx
```

Règle :

```text
On ne modifie jamais le fichier original.
```

---

## Dossier `data/processed`

```text
data/processed
```

C’est le dossier des fichiers générés par le pipeline.

Exemples :

```text
cleaned_data.parquet
encoded_dataset.parquet
final_dataset.parquet
final_dataset_sample.csv
```

---

## Dossier `src`

```text
src
```

C’est le dossier des scripts Python.

| Script                      | Rôle                                            |
| --------------------------- | ----------------------------------------------- |
| `01_inspect_file.py`        | Inspecter le fichier sans tout charger.         |
| `02_eda_profile.py`         | Faire l’analyse exploratoire.                   |
| `03_clean_data.py`          | Nettoyer les données.                           |
| `04_encode_features.py`     | Transformer les variables texte intelligemment. |
| `05_train_ready_dataset.py` | Créer le dataset final.                         |

---

## Dossier `reports`

```text
reports
```

C’est le dossier des rapports.

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

## Fichier `AGENTS.md`

Ce fichier donne les règles à Codex.

---

## Fichier `CLAUDE.md`

Ce fichier donne les règles à Claude Code.

---

# PARTIE 11 — Créer `CLAUDE.md`

Dans Claude Code, demander :

```text
Remplis le fichier CLAUDE.md.

Ce fichier doit donner à Claude Code les règles du projet.

Contexte :
Le projet sert à analyser un gros fichier tabulaire, faire une EDA, nettoyer les données, encoder les colonnes et produire un dataset final pour machine learning.

Règles :
1. Toujours commencer par analyser avant de coder.
2. Ne jamais modifier le fichier brut dans data/raw.
3. Ne jamais charger un fichier gigantesque entièrement sans inspection.
4. Toujours produire un rapport avant transformation.
5. Ne jamais one-hot encoder toutes les colonnes string automatiquement.
6. Identifier le rôle des colonnes : ID, date, montant, pays, province, nom d’organisation, texte libre, catégorie.
7. Appliquer one-hot encoding seulement pour les colonnes à faible cardinalité.
8. Pour les colonnes à forte cardinalité, proposer frequency encoding, regroupement des catégories rares ou suppression.
9. Écrire du code clair pour débutants.
10. Après chaque modification, expliquer comment tester.

Ajoute aussi une section "Ordre d’exécution des scripts".
```

---

# PARTIE 12 — Créer `AGENTS.md` pour Codex

Dans Claude ou Codex, demander :

```text
Remplis le fichier AGENTS.md.

Ce fichier doit guider Codex dans ce projet EDA.

Règles :
1. Ne jamais modifier data/raw.
2. Ne jamais tout encoder automatiquement.
3. Toujours lire les rapports avant de modifier un script.
4. Toujours proposer un plan avant une grosse modification.
5. Toujours expliquer les changements après modification.
6. Toujours vérifier les risques mémoire.
7. Toujours vérifier le risque d’explosion de colonnes après one-hot encoding.
8. Garder le code simple pour débutants.

Ajoute une section avec les commandes à exécuter dans l’ordre.
```

---

# PARTIE 13 — Créer l’environnement Python

Dans le terminal VS Code :

```powershell
py -3.12 -m venv .venv
```

Activer :

```powershell
.venv\Scripts\activate
```

Mettre pip à jour :

```powershell
python -m pip install --upgrade pip
```

Créer ou remplir `requirements.txt` :

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

Installer :

```powershell
pip install -r requirements.txt
```

---

# PARTIE 14 — Placer le fichier brut

Mettre le fichier Excel ou CSV dans :

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

Ne pas le renommer si ce n’est pas nécessaire.

Ne pas l’ouvrir et l’enregistrer dans Excel juste avant le traitement, car Excel peut parfois modifier les formats.

---

# PARTIE 15 — Demander à Claude de créer le script 1

## Objectif du script 1

Le premier script inspecte le fichier.

Il ne nettoie rien.

Il n’encode rien.

Il sert seulement à répondre :

```text
Quel est le fichier ?
Quelle est sa taille ?
Quelles sont les colonnes ?
Quels sont les types ?
Est-ce CSV, Excel ou Parquet ?
```

## Prompt 2 — `01_inspect_file.py`

```text
Crée le fichier src/01_inspect_file.py.

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
- mettre des commentaires simples ;
- afficher des messages clairs ;
- gérer l’erreur si data/raw est vide.
```

Exécuter :

```powershell
python src/01_inspect_file.py
```

Vérifier :

```powershell
dir reports
```

---

# PARTIE 16 — Demander à Claude de créer le script 2

## Objectif du script 2

Le deuxième script fait l’EDA.

EDA signifie :

```text
Exploratory Data Analysis
```

En français :

```text
Analyse exploratoire des données
```

On observe les colonnes avant de décider comment les transformer.

## Prompt 3 — `02_eda_profile.py`

```text
Crée le fichier src/02_eda_profile.py.

Objectif :
faire une analyse exploratoire des données.

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
- fais seulement l’analyse ;
- écris le code de manière simple pour débutants.
```

Exécuter :

```powershell
python src/02_eda_profile.py
```

Ouvrir :

```text
reports/eda_report.md
reports/columns_analysis.csv
```

---

# PARTIE 17 — Faire réfléchir Claude avant transformation

Après l’EDA, ne pas demander directement :

```text
Encode everything.
```

C’est une mauvaise instruction.

Demander plutôt :

```text
/effort xhigh

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

Décisions possibles :
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

C’est l’étape la plus importante du tutoriel.

---

# PARTIE 18 — Expliquer one-hot encoding simplement

Dire aux étudiants :

```text
Une colonne texte n’est pas automatiquement une bonne colonne pour one-hot encoding.
```

Exemple de bonne colonne :

```text
recipient_country
```

Si elle contient :

```text
CA
US
FR
```

Alors one-hot encoding est raisonnable.

Exemple de colonne dangereuse :

```text
recipient_legal_name
```

Si elle contient :

```text
LE CHENAIL INC.
UPSTREAM MUSIC ASSOCIATION
THE TAKE ACTION SOCIETY
```

Elle peut avoir des milliers de valeurs différentes.

Donc one-hot encoding peut créer des milliers de colonnes.

---

# PARTIE 19 — Demander le script 3 : nettoyage

Après validation de la stratégie :

```text
Crée le fichier src/03_clean_data.py.

Objectif :
nettoyer les données sans faire l’encodage final.

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

Exécuter :

```powershell
python src/03_clean_data.py
```

Vérifier :

```powershell
dir data\processed
dir reports
```

---

# PARTIE 20 — Demander le script 4 : encodage intelligent

```text
Crée le fichier src/04_encode_features.py.

Objectif :
transformer les données nettoyées en données numériques utilisables pour machine learning.

Le script doit lire :
data/processed/cleaned_data.parquet

Règles d’encodage :
1. Garder les colonnes numériques.
2. Pour les dates, créer :
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

Exécuter :

```powershell
python src/04_encode_features.py
```

---

# PARTIE 21 — Demander le script 5 : dataset final

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

Exécuter :

```powershell
python src/05_train_ready_dataset.py
```

---

# PARTIE 22 — Exécuter tout le pipeline

Quand tous les scripts sont prêts :

```powershell
python src/01_inspect_file.py
python src/02_eda_profile.py
python src/03_clean_data.py
python src/04_encode_features.py
python src/05_train_ready_dataset.py
```

Vérifier les fichiers générés :

```powershell
dir reports
dir data\processed
```

---

# PARTIE 23 — Utiliser Claude pour faire une revue finale

Dans Claude Code :

```text
/effort xhigh

Fais une revue complète du projet.

Vérifie :
1. la structure ;
2. le risque mémoire ;
3. la qualité de l’EDA ;
4. la logique de nettoyage ;
5. la logique d’encodage ;
6. le risque d’explosion du nombre de colonnes ;
7. la clarté pour des débutants ;
8. les améliorations prioritaires.

Ne modifie rien pour l’instant.
Donne-moi seulement ton diagnostic.
```

---

# PARTIE 24 — Utiliser Codex dans le même projet

Tu peux aussi ouvrir Codex dans VS Code et lui donner ce prompt :

```text
Lis le projet big-file-eda-codex.

Je veux que tu agisses comme reviewer technique.

Ne modifie rien.

Compare les scripts :
- src/01_inspect_file.py
- src/02_eda_profile.py
- src/03_clean_data.py
- src/04_encode_features.py
- src/05_train_ready_dataset.py

Vérifie :
1. si les scripts s’exécutent dans le bon ordre ;
2. si les chemins sont corrects ;
3. si data/raw n’est jamais modifié ;
4. si les rapports sont bien produits ;
5. si l’encodage évite les colonnes à forte cardinalité ;
6. si le projet est compréhensible pour un débutant.

Donne-moi les corrections recommandées avant de modifier.
```

Workflow recommandé :

```text
Claude Opus 4.8 = planification, raisonnement, stratégie, revue critique.
Codex = génération de code, correction locale, revue de diff.
```

---

# PARTIE 25 — Créer le README final

Dans Claude Code :

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

# PARTIE 26 — Vérifier avec Git

Dans le terminal :

```powershell
git status
```

Voir les changements :

```powershell
git diff
```

Ajouter les fichiers :

```powershell
git add .
```

Créer un commit :

```powershell
git commit -m "Create EDA pipeline project with Claude Opus 4.8"
```

---

# PARTIE 27 — Prompt maître complet à donner aux étudiants

Voici le prompt unique qu’ils peuvent utiliser au début.

```text
/model claude-opus-4-8
/effort xhigh

Tu es un assistant data engineer senior et machine learning engineer.

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
├── CLAUDE.md
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
4. Ensuite crée CLAUDE.md.
5. Ensuite crée AGENTS.md.
6. Ensuite crée les scripts un par un.
7. Après chaque script, explique comment l’exécuter.
8. Après chaque script, explique ce qu’il produit.
9. Écris un code simple, clair, commenté et adapté à des débutants.
10. Ne modifie jamais le fichier original dans data/raw.
11. Sauvegarde tous les résultats dans data/processed et reports.
12. Avant chaque transformation, explique pourquoi cette transformation est pertinente.

Avant de créer les fichiers, donne-moi ton plan détaillé.
```

---

# Résumé final

La bonne méthode n’est pas :

```text
Claude, encode tout.
```

La bonne méthode est :

```text
Claude, analyse d’abord.
Propose une stratégie.
Justifie les transformations.
Puis code étape par étape.
```

Pour ce projet, **Claude Opus 4.8** sert surtout à réfléchir profondément sur les colonnes, les risques mémoire et la stratégie d’encodage. **Codex** peut ensuite aider à générer, corriger et revoir le code.

[1]: https://docs.anthropic.com/en/docs/claude-code/overview?utm_source=chatgpt.com "Overview - Claude Code Docs"
[2]: https://docs.anthropic.com/en/docs/claude-code/ide-integrations "Use Claude Code in VS Code - Claude Code Docs"
[3]: https://docs.anthropic.com/en/docs/claude-code/setup?utm_source=chatgpt.com "Advanced setup - Claude Code Docs"
[4]: https://docs.anthropic.com/en/docs/claude-code/quickstart "Quickstart - Claude Code Docs"
[5]: https://support.anthropic.com/en/articles/11940350-claude-code-model-configuration "Claude Code model configuration | Claude Help Center"
[6]: https://docs.anthropic.com/en/docs/claude-code/model-config "Model configuration - Claude Code Docs"
