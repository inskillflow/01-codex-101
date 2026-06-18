# Tutoriel — Ajouter OpenAI Codex à VS Code

## 1. Objectif

L’objectif est d’utiliser Codex directement dans **Visual Studio Code**, au lieu de passer seulement par le terminal.

Avec l’extension VS Code, Codex peut :

* lire les fichiers ouverts ;
* comprendre le projet ;
* utiliser le contexte de l’éditeur ;
* proposer des modifications ;
* expliquer du code ;
* faire une revue de code ;
* travailler localement ou déléguer certaines tâches au cloud.

OpenAI indique que l’extension Codex fonctionne avec **VS Code**, mais aussi avec plusieurs forks comme **Cursor** et **Windsurf**. Elle est disponible sur Windows, macOS et Linux. ([OpenAI Développeurs][1])

---

# 2. Vérifier les prérequis

Avant d’installer l’extension, vérifier que tu as :

```powershell
code --version
git --version
codex --version
```

Si `codex --version` ne fonctionne pas encore, installer Codex CLI d’abord :

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

Ou avec npm :

```powershell
npm install -g @openai/codex
```

Le dépôt officiel OpenAI indique que Codex CLI peut être installé avec le script Windows, avec npm ou avec Homebrew, puis lancé avec la commande `codex`. ([GitHub][2])

---

# 3. Installer l’extension Codex dans VS Code

## Méthode 1 — Depuis VS Code

Ouvrir VS Code.

Aller dans :

```text
Extensions
```

Ou utiliser le raccourci :

```text
Ctrl + Shift + X
```

Dans la barre de recherche, écrire :

```text
Codex
```

Chercher l’extension officielle :

```text
Codex – OpenAI's coding agent
```

Puis cliquer sur :

```text
Install
```

La page Marketplace décrit Codex comme l’agent de programmation d’OpenAI permettant d’écrire, revoir et livrer du code plus rapidement depuis l’IDE ou via délégation au cloud. ([Visual Studio Marketplace][3])

---

## Méthode 2 — Depuis Quick Open

Dans VS Code, ouvrir Quick Open :

```text
Ctrl + P
```

Puis coller la commande d’installation fournie par la Marketplace VS Code.

La Marketplace officielle indique que l’installation peut se faire depuis **VS Code Quick Open** avec `Ctrl+P`. ([Visual Studio Marketplace][3])

---

# 4. Redémarrer VS Code

Après installation, fermer puis rouvrir VS Code.

OpenAI précise que dans VS Code, Codex apparaît normalement dans la barre latérale droite, mais qu’il peut être nécessaire de redémarrer l’éditeur si Codex n’apparaît pas immédiatement. ([OpenAI Développeurs][1])

---

# 5. Ouvrir Codex dans VS Code

Après redémarrage, tu devrais voir une icône Codex dans la barre latérale.

Cliquer dessus.

Tu obtiens normalement un panneau de chat Codex intégré à VS Code.

Dans VS Code, Codex s’ouvre par défaut dans la barre latérale droite. ([OpenAI Développeurs][1])

---

# 6. Se connecter à Codex

Dans le panneau Codex, choisir :

```text
Sign in with ChatGPT
```

Le navigateur s’ouvre.

Se connecter avec le compte ChatGPT.

Revenir dans VS Code.

OpenAI précise qu’après installation, l’extension demande une connexion avec un compte ChatGPT ou une clé API. L’usage avec un plan ChatGPT permet d’utiliser Codex sans configuration API supplémentaire. ([OpenAI Développeurs][1])

---

# 7. Vérifier que Codex fonctionne

Créer ou ouvrir un projet.

Exemple :

```powershell
mkdir demo-codex-vscode
cd demo-codex-vscode
code .
```

Créer un fichier :

```text
app.py
```

Mettre ce code :

```python
def add(a, b):
    return a + b

print(add(2, 3))
```

Dans Codex, écrire :

```text
Explique ce fichier app.py simplement.
```

Puis tester :

```text
Ajoute une fonction subtract(a, b), mais explique d'abord ce que tu vas modifier.
```

---

# 8. Utiliser le contexte de VS Code

L’intérêt principal de Codex dans VS Code est qu’il peut utiliser le contexte de l’éditeur :

* fichiers ouverts ;
* sélection de code ;
* fichiers mentionnés ;
* projet courant ;
* diff Git.

Exemple :

```text
Analyse le fichier actuellement ouvert et propose trois améliorations.
```

Autre exemple :

```text
Explique uniquement la fonction que j’ai sélectionnée.
```

Autre exemple :

```text
Lis le projet et propose une architecture plus propre sans modifier les fichiers pour l’instant.
```

La documentation Codex IDE indique que l’extension peut utiliser les fichiers ouverts, les sélections et les références `@file` pour donner des réponses plus pertinentes avec des prompts plus courts. ([OpenAI Développeurs][1])

---

# 9. Utiliser les modes de Codex dans VS Code

Codex peut fonctionner avec différents niveaux d’autonomie.

## Mode Chat

Codex répond et explique, mais ne prend pas trop d’initiative.

Exemple :

```text
Explique-moi ce code ligne par ligne.
```

## Mode Agent

Codex peut proposer ou appliquer des modifications dans le projet.

Exemple :

```text
Ajoute des tests unitaires pour ce fichier.
```

## Mode Agent Full Access

Mode plus permissif. À utiliser seulement dans un projet de test ou un environnement contrôlé.

Exemple :

```text
Refactorise ce module et lance les tests.
```

OpenAI indique que l’extension permet de choisir un mode d’approbation entre **Chat**, **Agent** et **Agent Full Access**, selon le niveau d’autonomie souhaité. ([OpenAI Développeurs][1])

---

# 10. Ajouter un fichier `AGENTS.md`

À la racine du projet, créer :

```text
AGENTS.md
```

Contenu recommandé :

```md
# Instructions pour Codex

## Objectif du projet

Ce projet est utilisé pour apprendre à travailler avec Codex dans VS Code.

## Règles de travail

- Toujours expliquer les modifications avant de les appliquer.
- Ne jamais supprimer un fichier sans justification.
- Préférer du code simple, lisible et maintenable.
- Après une modification, proposer une commande de test.
- Ne pas ajouter de dépendance sans raison claire.

## Commandes utiles

- Lancer le projet Python : python app.py
- Lancer les tests : pytest
```

Ensuite, demander dans Codex :

```text
Lis AGENTS.md et confirme les règles que tu vas respecter.
```

---

# 11. Utiliser les slash commands dans VS Code

Dans le panneau Codex de VS Code, tu peux écrire des commandes avec `/`.

Les commandes principales dans l’extension IDE sont :

```text
/status
/review
/goal
/local
/cloud
/cloud-environment
/auto-context
/feedback
```

OpenAI documente ces slash commands pour contrôler Codex directement depuis le champ de chat de l’extension IDE. ([OpenAI Développeurs][4])

---

## `/status`

Afficher l’état de Codex.

```text
/status
```

Utile pour vérifier :

* mode local ou cloud ;
* contexte actif ;
* limites ;
* thread courant.

---

## `/review`

Demander une revue de code.

```text
/review
```

Exemple après des modifications :

```text
/review
```

Puis :

```text
Vérifie les bugs possibles, la lisibilité et les risques de sécurité.
```

---

## `/goal`

Définir un objectif persistant.

```text
/goal Améliorer progressivement ce projet sans casser les tests.
```

Exemple pédagogique :

```text
/goal Transformer ce petit script Python en mini-projet propre avec fonctions, tests et README.
```

---

## `/local`

Passer en mode local.

```text
/local
```

À utiliser si tu veux que Codex travaille avec ton environnement local.

---

## `/cloud`

Passer en mode cloud.

```text
/cloud
```

À utiliser pour déléguer une tâche plus longue.

La documentation indique que l’extension permet de déléguer de plus gros travaux à Codex dans le cloud, puis de suivre et revoir les résultats depuis l’IDE. ([OpenAI Développeurs][5])

---

## `/cloud-environment`

Choisir l’environnement cloud.

```text
/cloud-environment
```

Utile si tu as plusieurs environnements configurés.

---

## `/auto-context`

Activer ou désactiver l’ajout automatique du contexte IDE.

```text
/auto-context
```

À utiliser si tu veux contrôler si Codex doit prendre automatiquement le contexte des fichiers ouverts.

---

# 12. Commandes VS Code liées à Codex

Tu peux aussi utiliser la palette de commandes VS Code :

```text
Ctrl + Shift + P
```

Puis chercher :

```text
Codex
```

OpenAI précise que les commandes Codex peuvent être utilisées depuis la **Command Palette** et aussi associées à des raccourcis clavier. ([OpenAI Développeurs][6])

Exemples de commandes utiles :

```text
Codex: New Chat
Codex: Toggle Codex
Codex: Add Selection to Context
Codex: Add File to Context
```

Les noms peuvent légèrement varier selon la version installée, donc la méthode la plus sûre est de chercher simplement :

```text
Codex
```

dans la palette de commandes.

---

# 13. Créer un raccourci clavier Codex

Dans VS Code :

```text
Ctrl + Shift + P
```

Chercher :

```text
Preferences: Open Keyboard Shortcuts
```

Puis chercher :

```text
Codex
```

Choisir une commande Codex.

Cliquer sur le crayon.

Ajouter un raccourci, par exemple :

```text
Ctrl + Alt + C
```

OpenAI explique qu’on peut rechercher `Codex` ou l’ID de commande dans les raccourcis clavier pour assigner un raccourci. ([OpenAI Développeurs][6])

---

# 14. Configurer Codex pour VS Code

L’extension Codex utilise aussi la configuration partagée de Codex CLI.

Fichier utilisateur :

```text
~/.codex/config.toml
```

Sur Windows :

```text
C:\Users\VotreNom\.codex\config.toml
```

Exemple recommandé :

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[features]
goals = true
```

La documentation officielle indique que l’extension IDE utilise Codex CLI et que certains comportements, comme le modèle par défaut, les approbations et le sandbox, se configurent dans le fichier partagé `~/.codex/config.toml`. ([OpenAI Développeurs][7])

---

# 15. Exemple de workflow complet dans VS Code

## Étape 1 — Ouvrir le projet

```powershell
cd C:\Users\user1\demo-codex-vscode
code .
```

## Étape 2 — Ouvrir Codex dans la barre latérale

Cliquer sur l’icône Codex.

## Étape 3 — Définir l’objectif

```text
/goal Améliorer ce projet Python en gardant le code simple et pédagogique.
```

## Étape 4 — Demander une analyse

```text
Analyse le projet. Ne modifie aucun fichier. Donne-moi d’abord les problèmes principaux.
```

## Étape 5 — Demander un plan

```text
Propose un plan de correction en 5 étapes.
```

## Étape 6 — Appliquer uniquement une petite étape

```text
Applique seulement l’étape 1. Ne touche pas aux autres fichiers.
```

## Étape 7 — Revue

```text
/review
```

## Étape 8 — Vérifier Git

Dans le terminal VS Code :

```powershell
git status
git diff
```

## Étape 9 — Commit

```powershell
git add .
git commit -m "Improve project structure with Codex"
```

---

# 16. Exemple de prompts utiles dans VS Code

## Pour comprendre un projet

```text
Explique la structure de ce projet comme si tu formais un débutant.
```

## Pour corriger un bug

```text
Trouve la cause probable de cette erreur. Ne modifie rien avant de me proposer un plan.
```

## Pour générer des tests

```text
Ajoute des tests unitaires pour les fonctions principales. Garde les tests simples.
```

## Pour améliorer un README

```text
Améliore le README avec une section installation, exécution et tests.
```

## Pour faire une revue

```text
/review
```

Puis :

```text
Classe les problèmes par gravité : critique, important, mineur.
```

## Pour travailler avec une sélection

Sélectionner un bloc de code, puis demander :

```text
Explique uniquement le code sélectionné.
```

Ou :

```text
Refactorise uniquement la sélection, sans modifier le reste du fichier.
```

---

# 17. Problèmes fréquents

## Codex n’apparaît pas dans VS Code

Solutions :

```text
1. Redémarrer VS Code.
2. Vérifier que l’extension est bien installée.
3. Chercher "Codex" dans la Command Palette.
4. Mettre VS Code à jour.
```

## La connexion ne fonctionne pas

Solutions :

```text
1. Se déconnecter puis reconnecter.
2. Vérifier le compte ChatGPT.
3. Vérifier que Codex fonctionne dans le terminal avec codex.
4. Redémarrer VS Code.
```

## Codex ne voit pas le bon projet

Solution :

```text
Ouvrir VS Code directement dans le dossier racine du projet.
```

Exemple :

```powershell
cd C:\Users\user1\mon-projet
code .
```

## Codex modifie trop de fichiers

Utiliser des prompts plus stricts :

```text
Ne modifie qu’un seul fichier.
```

```text
Avant toute modification, donne-moi la liste exacte des fichiers que tu veux changer.
```

```text
Applique une correction minimale.
```

---

# 18. À retenir

Pour utiliser Codex proprement dans VS Code :

```text
1. Installer Codex CLI.
2. Installer l’extension Codex dans VS Code.
3. Se connecter avec ChatGPT.
4. Ouvrir un projet avec code .
5. Créer AGENTS.md.
6. Utiliser /status, /goal, /review, /local, /cloud.
7. Toujours vérifier git diff avant de commit.
```

Le plus important pour tes étudiants : **Codex dans VS Code n’est pas seulement un chat**. C’est un assistant de développement intégré à l’éditeur, capable d’utiliser le contexte du projet, de proposer des modifications et de participer au workflow Git.

[1]: https://developers.openai.com/codex/ide?utm_source=chatgpt.com "Codex IDE extension"
[2]: https://github.com/openai/codex "GitHub - openai/codex: Lightweight coding agent that runs in your terminal · GitHub"
[3]: https://marketplace.visualstudio.com/items?itemName=openai.chatgpt&utm_source=chatgpt.com "Codex – OpenAI's coding agent"
[4]: https://developers.openai.com/codex/ide/slash-commands?utm_source=chatgpt.com "Codex IDE extension slash commands"
[5]: https://developers.openai.com/codex/ide/features?utm_source=chatgpt.com "Codex IDE extension features"
[6]: https://developers.openai.com/codex/ide/commands?utm_source=chatgpt.com "Commands – Codex IDE"
[7]: https://developers.openai.com/codex/ide/settings?utm_source=chatgpt.com "Codex IDE extension settings"
