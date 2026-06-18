# Cours complet — Configurer et utiliser Codex de A à Z

## 1. C’est quoi Codex ?

Codex est l’agent de programmation d’OpenAI. Son rôle est d’aider à **écrire du code, lire un projet, modifier des fichiers, lancer des commandes, faire des revues de code et préparer des corrections**. OpenAI le décrit comme un agent qui aide à “write, review, and ship code”. Codex est accessible via plusieurs surfaces : **CLI**, **extension IDE**, **application Codex**, et **Codex Web** selon ton plan et ton environnement. ([OpenAI Help Center][1])

La différence avec un chatbot classique est simple :
un chatbot répond surtout à des questions ; Codex travaille dans un dépôt de code, comprend les fichiers, propose des changements, exécute des commandes et peut revoir les différences Git.

---

## 2. Les façons d’utiliser Codex

Tu peux utiliser Codex de quatre façons principales :

| Interface               | Utilisation principale                                             |
| ----------------------- | ------------------------------------------------------------------ |
| **Codex CLI**           | Dans le terminal. Idéal pour développeurs, DevOps, CI/CD, scripts. |
| **Codex IDE extension** | Dans VS Code, Cursor, Windsurf ou forks compatibles.               |
| **Codex App**           | Application desktop pour gérer plusieurs agents et tâches.         |
| **Codex Web**           | Version cloud liée à ChatGPT et GitHub.                            |

OpenAI indique que l’extension VS Code est compatible avec la plupart des forks de VS Code, et que pour les autres IDE, on peut aussi utiliser Codex CLI dans le terminal intégré. ([OpenAI Help Center][1])

---

# PARTIE 1 — Installation de Codex CLI

## 3. Prérequis

Avant d’installer Codex, il faut avoir :

```bash
git --version
node --version
npm --version
```

Sur Windows, il est recommandé d’avoir :

```powershell
git --version
node --version
npm --version
```

Il faut aussi avoir un compte ChatGPT/OpenAI. Codex est inclus dans les plans ChatGPT Free, Go, Plus, Pro, Business, Edu et Enterprise, avec des limites selon le plan. ([OpenAI Help Center][1])

---

## 4. Installation sur Windows

Commande officielle PowerShell :

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

Cette commande installe Codex CLI sur Windows. La documentation officielle du dépôt OpenAI donne aussi l’installation par `npm` et Homebrew. ([GitHub][2])

Alternative avec npm :

```powershell
npm install -g @openai/codex
```

Vérifier l’installation :

```powershell
codex --version
```

Lancer Codex :

```powershell
codex
```

---

## 5. Installation sur macOS ou Linux

Commande officielle :

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

Alternative avec npm :

```bash
npm install -g @openai/codex
```

Alternative avec Homebrew :

```bash
brew install --cask codex
```

Puis :

```bash
codex
```

Le dépôt officiel indique qu’après installation, il suffit d’exécuter `codex` pour commencer. ([GitHub][2])

---

# PARTIE 2 — Première configuration

## 6. Connexion à ChatGPT

Après avoir lancé :

```bash
codex
```

Codex propose de se connecter avec ChatGPT. Le dépôt officiel recommande de choisir **Sign in with ChatGPT** pour utiliser Codex avec un plan ChatGPT compatible. ([GitHub][2])

Tu peux aussi utiliser :

```bash
codex login
```

La commande `codex login` permet d’authentifier Codex via ChatGPT OAuth, device auth, API key ou access token selon le mode choisi. ([OpenAI Développeurs][3])

Pour se déconnecter :

```bash
codex logout
```

---

## 7. Créer un projet de test

Exemple simple :

```bash
mkdir demo-codex
cd demo-codex
git init
echo "# Demo Codex" > README.md
```

Puis lancer Codex dans le projet :

```bash
codex
```

Tu peux écrire :

```text
Analyse ce projet et explique sa structure.
```

Ou directement depuis le terminal :

```bash
codex "Analyse ce projet et propose une structure propre."
```

---

# PARTIE 3 — Configuration avec `config.toml`

## 8. Où se trouve la configuration ?

Codex stocke la configuration utilisateur dans :

```bash
~/.codex/config.toml
```

Pour une configuration spécifique à un projet, il faut créer :

```bash
.codex/config.toml
```

La documentation officielle précise que la CLI et l’extension IDE partagent les mêmes couches de configuration, et que `~/.codex/config.toml` sert aux paramètres utilisateur tandis que `.codex/config.toml` sert au projet. ([OpenAI Développeurs][4])

---

## 9. Exemple de configuration de base

Créer le dossier :

```bash
mkdir -p ~/.codex
```

Créer le fichier :

```bash
nano ~/.codex/config.toml
```

Exemple :

```toml
model = "gpt-5.4"

approval_policy = "on-request"
sandbox_mode = "workspace-write"

[features]
goals = true
```

Explication :

| Élément           | Rôle                                          |
| ----------------- | --------------------------------------------- |
| `model`           | Modèle utilisé par défaut.                    |
| `approval_policy` | Définit quand Codex demande une confirmation. |
| `sandbox_mode`    | Définit ce que Codex peut lire/écrire.        |
| `[features]`      | Active certaines fonctionnalités.             |

OpenAI recommande de garder les préférences personnelles dans `~/.codex/config.toml`, le comportement spécifique au dépôt dans `.codex/config.toml`, et les overrides CLI seulement pour des cas ponctuels. ([OpenAI Développeurs][5])

---

## 10. Priorité des configurations

Codex applique les configurations dans cet ordre, du plus prioritaire au moins prioritaire :

1. Flags CLI, par exemple `--model`
2. `.codex/config.toml` du projet
3. Profil `~/.codex/<profile>.config.toml`
4. Configuration utilisateur `~/.codex/config.toml`
5. Configuration système
6. Valeurs par défaut

Cette priorité est documentée officiellement dans la page “Config basics”. ([OpenAI Développeurs][4])

Exemple d’override ponctuel :

```bash
codex --model gpt-5.4
```

Ou :

```bash
codex --config model='"gpt-5.4"'
```

La documentation précise que `--config` lit les valeurs comme du TOML, pas comme du JSON. ([OpenAI Développeurs][6])

---

# PARTIE 4 — Permissions et sandbox

## 11. Comprendre les permissions

Codex peut lire des fichiers, modifier le projet et lancer des commandes. Il faut donc contrôler son niveau d’autonomie.

Modes importants :

| Mode                 | Signification                                                     |
| -------------------- | ----------------------------------------------------------------- |
| `read-only`          | Codex lit seulement. Bon pour audit ou explication.               |
| `workspace-write`    | Codex peut modifier le dossier du projet. Bon pour développement. |
| `danger-full-access` | Accès très permissif. À éviter sauf environnement isolé.          |

Exemple conseillé pour un projet local :

```bash
codex --sandbox workspace-write --ask-for-approval on-request
```

La documentation officielle présente `--sandbox` avec les valeurs `read-only`, `workspace-write` et `danger-full-access`, et `--ask-for-approval` avec `untrusted`, `on-request` et `never`. ([OpenAI Développeurs][3])

À éviter sauf laboratoire isolé :

```bash
codex --yolo
```

Le flag `--dangerously-bypass-approvals-and-sandbox`, aussi appelé `--yolo`, exécute les commandes sans approbation ni sandbox ; OpenAI indique de l’utiliser seulement dans un environnement externe fortement isolé. ([OpenAI Développeurs][3])

---

# PARTIE 5 — Fichier `AGENTS.md`

## 12. À quoi sert `AGENTS.md` ?

`AGENTS.md` est un fichier d’instructions permanentes pour Codex. Il permet de dire au projet :

* comment lancer les tests ;
* quel style de code respecter ;
* quelles commandes éviter ;
* quelle architecture suivre ;
* quelles règles pédagogiques ou professionnelles appliquer.

OpenAI indique que Codex lit les fichiers `AGENTS.md` avant de commencer le travail, avec une logique de guidance globale et spécifique au projet. ([OpenAI Développeurs][7])

---

## 13. Créer un `AGENTS.md` global

Commande :

```bash
mkdir -p ~/.codex
nano ~/.codex/AGENTS.md
```

Exemple :

```md
# Instructions globales pour Codex

## Style de travail

- Toujours expliquer les changements avant de modifier plusieurs fichiers.
- Toujours vérifier les tests existants avant de proposer un commit.
- Ne jamais supprimer un fichier sans expliquer pourquoi.
- Préférer une solution simple, lisible et maintenable.
- Pour les projets pédagogiques, commenter clairement le code.
```

Tester :

```bash
codex --ask-for-approval never "Résume les instructions actives."
```

La documentation officielle donne précisément ce type de vérification pour confirmer que Codex charge bien les instructions globales. ([OpenAI Développeurs][7])

---

## 14. Créer un `AGENTS.md` dans un projet

À la racine du dépôt :

```bash
touch AGENTS.md
```

Exemple :

```md
# Instructions du projet

## Objectif

Ce projet est un exemple pédagogique. Le code doit être simple, lisible et facile à expliquer.

## Règles techniques

- Utiliser des noms de variables clairs.
- Ne pas ajouter de dépendance sans justification.
- Lancer les tests avant de finaliser une modification.
- Mettre à jour le README si le comportement change.

## Commandes utiles

- Installer les dépendances : npm install
- Lancer le projet : npm run dev
- Lancer les tests : npm test
```

Puis :

```bash
codex "Lis AGENTS.md et propose un plan d'amélioration du projet."
```

---

# PARTIE 6 — Commandes CLI essentielles

## 15. Commandes de base

| Commande           | Rôle                                               |
| ------------------ | -------------------------------------------------- |
| `codex`            | Lance l’interface terminal interactive.            |
| `codex "prompt"`   | Lance Codex avec une instruction directe.          |
| `codex login`      | Connexion à ChatGPT ou autre méthode d’auth.       |
| `codex logout`     | Supprime les identifiants stockés.                 |
| `codex doctor`     | Génère un diagnostic local.                        |
| `codex update`     | Vérifie et applique une mise à jour si disponible. |
| `codex resume`     | Reprend une session précédente.                    |
| `codex fork`       | Crée une branche de conversation.                  |
| `codex completion` | Génère l’autocomplétion shell.                     |

Ces commandes apparaissent dans la référence officielle des commandes CLI, qui liste notamment `codex`, `codex doctor`, `codex exec`, `codex login`, `codex logout`, `codex resume`, `codex fork`, `codex completion` et `codex update`. ([OpenAI Développeurs][3])

---

## 16. Commandes avec exemples

### Lancer Codex dans le dossier courant

```bash
codex
```

### Lancer Codex dans un dossier précis

```bash
codex --cd ./mon-projet
```

### Choisir un modèle

```bash
codex --model gpt-5.4
```

### Donner une consigne directe

```bash
codex "Analyse le code et trouve les problèmes de structure."
```

### Démarrer avec accès écriture au projet

```bash
codex --sandbox workspace-write --ask-for-approval on-request
```

### Faire une analyse sans modifier les fichiers

```bash
codex --sandbox read-only "Analyse ce projet sans rien modifier."
```

### Attacher une image

```bash
codex --image ./capture.png "Explique ce problème d'interface."
```

Le flag `--image` permet d’attacher une ou plusieurs images à la première instruction. ([OpenAI Développeurs][3])

---

# PARTIE 7 — `codex exec` pour automatiser

## 17. C’est quoi `codex exec` ?

`codex exec` sert à lancer Codex en mode non interactif. C’est utile pour :

* scripts ;
* CI/CD ;
* audit automatique ;
* génération de rapport ;
* revue de code automatisée.

OpenAI indique que `codex exec`, alias `codex e`, exécute Codex de manière non interactive et peut produire une sortie formatée ou JSONL. ([OpenAI Développeurs][3])

---

## 18. Exemples `codex exec`

### Analyse simple

```bash
codex exec "Analyse le projet et donne les 5 améliorations prioritaires."
```

### Écrire la réponse finale dans un fichier

```bash
codex exec "Génère un résumé du projet." --output-last-message rapport.md
```

### Utiliser JSONL pour automatisation

```bash
codex exec --json "Analyse les erreurs potentielles."
```

### Lire le prompt depuis stdin

```bash
cat prompt.txt | codex exec -
```

### Autoriser un dossier hors dépôt

```bash
codex exec --add-dir ../shared "Analyse le projet et le dossier shared."
```

### Exécution dans un dossier spécifique

```bash
codex exec --cd ./backend "Analyse l'API et propose des tests."
```

---

# PARTIE 8 — Commandes avancées Codex CLI

## 19. Vue d’ensemble des commandes documentées

| Commande                   | Rôle                                                          |
| -------------------------- | ------------------------------------------------------------- |
| `codex`                    | Lance le TUI terminal.                                        |
| `codex app`                | Lance l’application desktop Codex.                            |
| `codex app-server`         | Lance le serveur local Codex pour développement/debug.        |
| `codex apply` ou `codex a` | Applique localement un diff généré par une tâche Codex Cloud. |
| `codex archive`            | Archive une session.                                          |
| `codex unarchive`          | Restaure une session archivée.                                |
| `codex cloud`              | Gère les tâches Codex Cloud depuis le terminal.               |
| `codex completion`         | Génère l’autocomplétion shell.                                |
| `codex debug models`       | Affiche le catalogue brut des modèles vus par Codex.          |
| `codex doctor`             | Diagnostic installation/config/auth/Git/terminal.             |
| `codex exec` ou `codex e`  | Exécution non interactive.                                    |
| `codex execpolicy`         | Évalue des règles d’exécution.                                |
| `codex features`           | Liste, active ou désactive des feature flags.                 |
| `codex fork`               | Fork d’une session interactive.                               |
| `codex login`              | Connexion.                                                    |
| `codex logout`             | Déconnexion.                                                  |
| `codex mcp`                | Gestion des serveurs MCP.                                     |
| `codex mcp-server`         | Exécute Codex comme serveur MCP.                              |
| `codex plugin`             | Gestion des plugins.                                          |
| `codex plugin marketplace` | Gestion des marketplaces de plugins.                          |
| `codex remote-control`     | Active le support remote-control local.                       |
| `codex resume`             | Reprend une session.                                          |
| `codex sandbox`            | Lance des commandes dans les sandboxes Codex.                 |
| `codex update`             | Mise à jour Codex.                                            |

La référence officielle liste ces commandes et leur niveau de maturité, avec plusieurs commandes stables et d’autres expérimentales. ([OpenAI Développeurs][3])

---

# PARTIE 9 — Slash commands Codex CLI

## 20. C’est quoi une slash command ?

Une slash command est une commande tapée directement dans l’interface Codex, par exemple :

```text
/status
```

ou :

```text
/review
```

Les slash commands permettent de contrôler Codex sans quitter la session : changer le modèle, voir le statut, ajuster les permissions, faire une revue, résumer la conversation, etc. OpenAI indique qu’il suffit de taper `/` dans le composer pour ouvrir le menu des slash commands. ([OpenAI Développeurs][8])

---

## 21. Liste des slash commands CLI documentées

| Slash command           | Rôle                                                                |
| ----------------------- | ------------------------------------------------------------------- |
| `/permissions`          | Modifier ce que Codex peut faire sans demander.                     |
| `/ide`                  | Inclure le contexte de l’IDE : fichiers ouverts, sélection, etc.    |
| `/keymap`               | Modifier les raccourcis clavier du TUI.                             |
| `/vim`                  | Activer ou désactiver le mode Vim.                                  |
| `/sandbox-add-read-dir` | Ajouter un dossier lisible au sandbox Windows.                      |
| `/agent`                | Changer de thread agent actif.                                      |
| `/apps`                 | Parcourir les apps/connecteurs disponibles.                         |
| `/plugins`              | Gérer ou inspecter les plugins.                                     |
| `/hooks`                | Voir et gérer les lifecycle hooks.                                  |
| `/clear`                | Nettoyer le terminal et démarrer une nouvelle conversation.         |
| `/archive`              | Archiver la session courante et quitter.                            |
| `/compact`              | Résumer la conversation pour libérer du contexte.                   |
| `/copy`                 | Copier la dernière sortie terminée.                                 |
| `/diff`                 | Afficher le diff Git.                                               |
| `/exit`                 | Quitter la CLI.                                                     |
| `/quit`                 | Quitter la CLI.                                                     |
| `/experimental`         | Activer/désactiver des fonctionnalités expérimentales.              |
| `/approve`              | Approuver une action refusée par l’auto-review.                     |
| `/memories`             | Configurer l’utilisation des mémoires.                              |
| `/skills`               | Parcourir et utiliser les skills.                                   |
| `/feedback`             | Envoyer des logs/feedback.                                          |
| `/init`                 | Générer un squelette `AGENTS.md`.                                   |
| `/logout`               | Se déconnecter.                                                     |
| `/mcp`                  | Voir les outils MCP configurés.                                     |
| `/mention`              | Attacher un fichier ou dossier à la conversation.                   |
| `/model`                | Changer le modèle actif.                                            |
| `/fast`                 | Activer/désactiver le mode Fast si disponible.                      |
| `/plan`                 | Passer en mode planification.                                       |
| `/goal`                 | Définir, voir, mettre en pause, reprendre ou supprimer un objectif. |
| `/personality`          | Changer le style de communication.                                  |
| `/ps`                   | Voir les terminaux expérimentaux en arrière-plan.                   |
| `/stop`                 | Arrêter les terminaux en arrière-plan.                              |
| `/fork`                 | Créer une nouvelle branche de conversation.                         |
| `/side`                 | Lancer une conversation secondaire temporaire.                      |
| `/btw`                  | Alias de conversation secondaire temporaire.                        |
| `/review`               | Demander une revue du working tree.                                 |
| `/status`               | Afficher configuration, tokens, modèle, sandbox, etc.               |
| `/debug-config`         | Diagnostiquer les couches de configuration.                         |
| `/statusline`           | Configurer la ligne de statut du TUI.                               |
| `/title`                | Configurer le titre du terminal/onglet.                             |
| `/theme`                | Choisir un thème de coloration syntaxique.                          |
| `/raw`                  | Activer/désactiver le mode raw scrollback.                          |

Cette liste vient de la page officielle “Slash commands in Codex CLI”. La documentation précise aussi que certaines slash commands peuvent être mises en file d’attente avec `Tab` si une tâche est déjà en cours. ([OpenAI Développeurs][8])

---

## 22. Les slash commands les plus importantes à enseigner

### Voir l’état de la session

```text
/status
```

À utiliser pour vérifier :

* modèle actif ;
* usage du contexte ;
* permissions ;
* racine de travail ;
* sandbox actif.

---

### Afficher le diff Git

```text
/diff
```

À utiliser avant un commit :

```text
/diff
```

Puis demander :

```text
Explique chaque changement avant que je commit.
```

---

### Faire une revue de code

```text
/review
```

Utile après modification :

```text
/review
```

Puis :

```text
Vérifie les risques de bug, de sécurité et de maintenabilité.
```

---

### Planifier avant de coder

```text
/plan
```

Exemple :

```text
/plan Propose un plan pour ajouter une authentification JWT.
```

Le mode `/plan` sert à demander un plan avant l’implémentation. ([OpenAI Développeurs][8])

---

### Définir un objectif long

```text
/goal Refactoriser le module d'authentification sans casser les tests.
```

Commandes liées :

```text
/goal
/goal pause
/goal resume
/goal clear
```

La documentation indique qu’un objectif `/goal` reste attaché au thread actif pendant que le travail continue. ([OpenAI Développeurs][8])

---

### Changer les permissions

```text
/permissions
```

Scénario pédagogique :

```text
/permissions
```

Choisir :

```text
Read Only
```

Puis demander :

```text
Analyse le projet sans modifier les fichiers.
```

---

### Changer le modèle

```text
/model
```

À utiliser quand on veut passer d’un modèle rapide à un modèle plus robuste.

---

### Résumer une longue session

```text
/compact
```

À utiliser quand la conversation devient longue. La commande sert à résumer le contexte visible pour réduire l’utilisation des tokens. ([OpenAI Développeurs][8])

---

### Démarrer proprement

```text
/clear
```

Important : `/clear` démarre une nouvelle conversation dans la même session CLI, alors que `Ctrl+L` nettoie seulement l’affichage. ([OpenAI Développeurs][8])

---

# PARTIE 10 — Slash commands IDE et App

## 23. Slash commands dans l’extension IDE

Dans l’extension Codex IDE, les commandes documentées incluent :

| Slash command        | Rôle                                                  |
| -------------------- | ----------------------------------------------------- |
| `/auto-context`      | Active/désactive l’ajout automatique du contexte IDE. |
| `/cloud`             | Passe en mode cloud.                                  |
| `/cloud-environment` | Choisit l’environnement cloud.                        |
| `/feedback`          | Ouvre le dialogue de feedback.                        |
| `/goal`              | Définit un objectif persistant.                       |
| `/local`             | Passe en mode local.                                  |
| `/review`            | Lance une revue de code.                              |
| `/status`            | Affiche ID de thread, contexte, limites.              |

La documentation précise que les slash commands IDE servent à contrôler Codex sans quitter le champ de chat. ([OpenAI Développeurs][9])

---

## 24. Slash commands dans Codex App

Dans l’application Codex, les commandes documentées incluent :

| Slash command | Rôle                                |
| ------------- | ----------------------------------- |
| `/feedback`   | Envoyer feedback/logs.              |
| `/goal`       | Définir un objectif persistant.     |
| `/init`       | Générer un scaffold `AGENTS.md`.    |
| `/mcp`        | Voir le statut MCP.                 |
| `/plan`       | Activer le mode plan.               |
| `/review`     | Revue des changements.              |
| `/status`     | Afficher thread, contexte, limites. |

La documentation précise que les commandes disponibles peuvent varier selon l’environnement et l’accès. ([OpenAI Développeurs][10])

---

# PARTIE 11 — MCP avec Codex

## 25. Pourquoi connecter MCP ?

MCP signifie **Model Context Protocol**. Avec MCP, Codex peut se connecter à des outils externes : documentation, bases de données, outils internes, systèmes de fichiers, APIs, etc.

Exemple :

```bash
codex mcp
```

Ajouter un serveur MCP :

```bash
codex mcp add context7 -- npx -y @upstash/context7-mcp
```

OpenAI indique qu’on peut configurer MCP soit avec `codex mcp`, soit en modifiant `config.toml`, et que cette configuration est partagée entre CLI et extension IDE. ([OpenAI Développeurs][11])

Dans Codex :

```text
/mcp
```

Cette commande affiche les serveurs MCP actifs.

---

# PARTIE 12 — Skills

## 26. C’est quoi une skill ?

Une skill est un paquet d’instructions réutilisable. Elle peut contenir :

```text
my-skill/
  SKILL.md
  scripts/
  references/
  assets/
```

OpenAI explique qu’une skill donne à Codex des capacités ou expertises spécifiques, avec un fichier obligatoire `SKILL.md` contenant au minimum `name` et `description`. ([OpenAI Développeurs][12])

Dans Codex CLI :

```text
/skills
```

Ou mentionner une skill avec :

```text
$nom-de-la-skill
```

Cas d’usage :

| Skill              | Exemple                          |
| ------------------ | -------------------------------- |
| `spring-review`    | Revue d’un projet Spring Boot.   |
| `terraform-audit`  | Audit Terraform.                 |
| `readme-generator` | Génération README professionnel. |
| `test-writer`      | Création de tests unitaires.     |
| `security-review`  | Revue sécurité.                  |

---

# PARTIE 13 — Workflow professionnel recommandé

## 27. Workflow propre avec Git

Toujours commencer par :

```bash
git status
```

Créer une branche :

```bash
git checkout -b feature/codex-demo
```

Lancer Codex :

```bash
codex --sandbox workspace-write --ask-for-approval on-request
```

Demander :

```text
Analyse le projet, propose un plan, puis attends ma validation avant de modifier les fichiers.
```

Utiliser :

```text
/plan
```

Après modification :

```text
/diff
```

Puis :

```text
/review
```

Enfin :

```bash
git status
git add .
git commit -m "Improve project structure with Codex"
```

---

## 28. Workflow pour corriger un bug

```text
/plan Analyse ce bug et propose une stratégie de correction sans modifier le code.
```

Ensuite :

```text
Applique uniquement la solution minimale. Ne refactorise pas tout le projet.
```

Puis :

```text
/diff
```

Puis :

```text
/review
```

---

## 29. Workflow pour générer des tests

```text
Analyse les fichiers de test existants. Ajoute des tests uniquement pour les comportements non couverts.
```

Après génération :

```text
Explique chaque test ajouté et le scénario qu’il valide.
```

Puis :

```bash
npm test
```

Ou :

```bash
pytest
```

---

## 30. Workflow pour un projet étudiant

Prompt recommandé :

```text
Tu agis comme assistant pédagogique. 
Analyse ce projet comme si tu corrigeais un travail pratique.
Ne modifie aucun fichier pour l’instant.
Donne :
1. les forces du projet ;
2. les problèmes importants ;
3. les erreurs de structure ;
4. les corrections prioritaires ;
5. une note indicative sur 20 avec justification.
```

Ensuite :

```text
Maintenant propose des corrections minimales sans changer l’intention du projet.
```

---

# PARTIE 14 — Tableau résumé des commandes

## 31. Commandes terminal

```bash
codex
codex "prompt"
codex --cd ./projet
codex --model gpt-5.4
codex --sandbox read-only
codex --sandbox workspace-write
codex --ask-for-approval on-request
codex --image ./image.png "Explique cette capture"
codex login
codex logout
codex doctor
codex update
codex resume
codex fork
codex completion bash
codex completion zsh
codex completion fish
codex completion power-shell
codex exec "prompt"
codex exec --json "prompt"
codex exec --output-last-message rapport.md "prompt"
codex mcp
codex mcp --help
codex features list
codex features enable goals
codex features disable goals
```

---

## 32. Slash commands à mémoriser

```text
/status
/plan
/goal
/review
/diff
/permissions
/model
/compact
/clear
/init
/mcp
/skills
/mention
/fork
/side
/btw
/copy
/raw
/theme
/statusline
/title
/debug-config
/logout
/exit
/quit
```

---

# PARTIE 15 — Bonnes pratiques

## 33. Règles professionnelles

1. Ne jamais lancer Codex en mode permissif dans un projet sensible.
2. Toujours commencer par `/plan` pour une grosse modification.
3. Toujours vérifier `/diff` avant de commit.
4. Toujours utiliser `/review` après une modification importante.
5. Garder un `AGENTS.md` clair dans chaque dépôt.
6. Mettre les règles personnelles dans `~/.codex/config.toml`.
7. Mettre les règles de projet dans `.codex/config.toml`.
8. Éviter `--yolo` sauf dans un environnement isolé.
9. Utiliser `read-only` pour l’analyse.
10. Utiliser `workspace-write` seulement pour les projets de confiance.

OpenAI recommande de commencer avec des permissions par défaut si on débute avec les agents de code, puis de desserrer les permissions seulement pour des dépôts ou workflows de confiance. ([OpenAI Développeurs][5])

---

# PARTIE 16 — Mini-lab complet

## Objectif

Configurer Codex, créer un projet, ajouter `AGENTS.md`, lancer une analyse, demander une modification, vérifier le diff et faire une revue.

### Étape 1 — Créer le projet

```bash
mkdir codex-lab
cd codex-lab
git init
```

### Étape 2 — Créer un fichier Python

```bash
cat > app.py << 'EOF'
def add(a,b):
    return a+b

print(add(2,3))
EOF
```

### Étape 3 — Créer `AGENTS.md`

```bash
cat > AGENTS.md << 'EOF'
# Instructions pour Codex

- Le projet est pédagogique.
- Le code doit rester simple.
- Ajouter des commentaires uniquement s’ils aident à comprendre.
- Ne pas ajouter de dépendance externe.
- Proposer un plan avant toute modification importante.
EOF
```

### Étape 4 — Lancer Codex

```bash
codex --sandbox workspace-write --ask-for-approval on-request
```

### Étape 5 — Demander une analyse

```text
Lis le projet et explique ce qu’il fait. Ne modifie rien.
```

### Étape 6 — Demander un plan

```text
/plan Améliore légèrement ce projet en ajoutant une fonction subtract et des tests simples.
```

### Étape 7 — Appliquer

```text
Applique le plan de manière minimale.
```

### Étape 8 — Voir le diff

```text
/diff
```

### Étape 9 — Revue

```text
/review
```

### Étape 10 — Commit

```bash
git status
git add .
git commit -m "Add subtract function and simple tests"
```

---

# Conclusion

La configuration sérieuse de Codex repose sur quatre piliers : **CLI bien installé**, **permissions contrôlées**, **`config.toml` propre**, et **`AGENTS.md` clair**. Les slash commands donnent le contrôle pendant la session : `/plan` pour réfléchir avant de coder, `/diff` pour vérifier, `/review` pour contrôler la qualité, `/permissions` pour ajuster le niveau d’autonomie, et `/status` pour comprendre l’état réel de Codex.

[1]: https://help.openai.com/en/articles/11369540-using-codex-with-your-chatgpt-plan "Using Codex with your ChatGPT plan | OpenAI Help Center"
[2]: https://github.com/openai/codex "GitHub - openai/codex: Lightweight coding agent that runs in your terminal · GitHub"
[3]: https://developers.openai.com/codex/cli/reference "Command line options – Codex CLI | OpenAI Developers"
[4]: https://developers.openai.com/codex/config-basic "Config basics – Codex | OpenAI Developers"
[5]: https://developers.openai.com/codex/learn/best-practices "Best practices – Codex | OpenAI Developers"
[6]: https://developers.openai.com/codex/config-advanced "Advanced Configuration – Codex | OpenAI Developers"
[7]: https://developers.openai.com/codex/guides/agents-md "Custom instructions with AGENTS.md – Codex | OpenAI Developers"
[8]: https://developers.openai.com/codex/cli/slash-commands "Slash commands in Codex CLI | OpenAI Developers"
[9]: https://developers.openai.com/codex/ide/slash-commands "Slash commands – Codex IDE | OpenAI Developers"
[10]: https://developers.openai.com/codex/app/commands "Commands – Codex app | OpenAI Developers"
[11]: https://developers.openai.com/codex/mcp "Model Context Protocol – Codex | OpenAI Developers"
[12]: https://developers.openai.com/codex/skills "Agent Skills – Codex | OpenAI Developers"
