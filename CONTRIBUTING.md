# Contribuer à EdGraph

Guide pour démarrer sur le repo et travailler à deux (Louis et Jules), avec Claude Code.
Les règles du projet sont dans le [`README.md`](README.md) et dans [`CLAUDE.md`](CLAUDE.md). Ce guide explique seulement **comment** on travaille au quotidien.

> **Le repo est public.** Tout ce qui est commité est visible par n'importe qui, historique compris. Aucune donnée personnelle réelle, aucun compte rendu d'entretien nominatif, aucune clé API. En cas de doute, on ne commite pas et on demande.

---

## 1. Mise en route (une seule fois)

### 1.1 Compte GitHub et accès au repo
1. Créer un compte sur https://github.com/signup (ou utiliser un compte existant).
2. Envoyer son **nom d'utilisateur GitHub** à Louis, qui t'ajoute comme collaborateur.
3. Accepter l'invitation : elle arrive par mail, ou sur https://github.com/gourgul/EdGraph/invitations.

### 1.2 Outils à installer (macOS)
```bash
# Homebrew, si absent : https://brew.sh
brew install git gh
gh auth login          # choisir GitHub.com, SSH, se connecter via le navigateur
```
Puis installer Claude Code : https://claude.com/claude-code (application de bureau ou terminal).

### 1.3 Récupérer le projet
```bash
gh repo clone gourgul/EdGraph
cd EdGraph
cp .env.example .env   # clés API à renseigner plus tard (P2). Le .env n'est jamais commité.
git config user.name "Prénom Nom"
git config user.email "ton-email@exemple.com"
```

### 1.4 Lire avant de commencer
1. `README.md` : le projet en une page.
2. `docs/methodologie.md` : la source de vérité (cadrage, registre des décisions, phases).
3. `docs/backlog-p0.md` : ce qu'on fait maintenant, et qui fait quoi.

---

## 2. Le cycle de travail : issue → branche → PR → relecture → merge

Toute tâche suit ce cycle, même une correction de doc. C'est ce qui permet de travailler à deux sans se marcher dessus.

### 2.1 Une issue par tâche
- Les tâches sont des **issues** GitHub, rangées par phase dans des **milestones** (`P0`, `P1`…).
- Pour en créer une : onglet *Issues* → *New issue* → choisir un modèle :
  - **Tâche** : travail de dev ou de doc ;
  - **Nouvelle source** : évaluer une source de données avant toute ingestion ;
  - **Décision** : une question à trancher (registre §2 de la méthodologie).
- Chaque issue a un **critère d'acceptation** : on sait quand c'est fini.
- On s'assigne l'issue avant de commencer, pour que l'autre sache qui fait quoi.

### 2.2 Une branche par issue
Ne jamais travailler directement sur `main`.
```bash
git switch main && git pull                 # partir de la dernière version
git switch -c p0/3-guide-entretien          # format : p<phase>/<num issue>-<mot-clé>
```

### 2.3 Travailler et sauvegarder
```bash
git status                                  # voir ce qui a changé
git add docs/entretiens/guide-etudiant.md   # ajouter des fichiers précis plutôt que "git add ."
git commit -m "Guide étudiant : questions sur la césure (#3)"
git push -u origin p0/3-guide-entretien     # la première fois ; ensuite "git push" suffit
```
Des commits petits et fréquents valent mieux qu'un gros commit en fin de journée. Mettre `#<num>` dans le message relie le commit à l'issue.

### 2.4 Ouvrir une pull request (PR)
```bash
gh pr create --fill     # ou depuis la page GitHub : bouton "Compare & pull request"
```
- Le modèle de PR s'affiche : expliquer **en langage simple** ce qui change, comment le vérifier, ce qui reste ouvert.
- Écrire `Closes #3` dans la description : l'issue se fermera toute seule au merge.
- Demander la relecture de l'autre associé (*Reviewers*).

### 2.5 Relire et merger
- L'autre associé relit, commente, puis approuve (*Approve*) ou demande des modifications.
- Pour corriger après une remarque : modifier, commiter et pousser sur **la même branche**, la PR se met à jour toute seule.
- Une fois la PR approuvée : *Squash and merge*, puis supprimer la branche.
- Ensuite, en local :
  ```bash
  git switch main && git pull
  ```

---

## 3. Travailler avec Claude Code

Claude Code lit automatiquement `CLAUDE.md` à la racine : il connaît les règles du projet.

- Ouvrir le dossier `EdGraph` dans Claude Code, puis lui donner l'issue : *« Prends l'issue #3, propose un plan, puis fais-la sur une branche dédiée. »*
- Il peut créer la branche, écrire, commiter, pousser et ouvrir la PR. **Relire ce qu'il produit avant de valider**, surtout tout ce qui touche aux données et aux sources.
- Une issue par session : quand la PR est ouverte, on clôt la session et on en ouvre une nouvelle pour la tâche suivante.
- Pour apprendre : lui demander d'expliquer chaque commande git avant de la lancer.

---

## 4. Où mettre quoi

| Je veux… | Où |
|---|---|
| Proposer ou suivre une tâche | Une issue, modèle *Tâche* |
| Évaluer une source de données | Issue *Nouvelle source* + fiche `docs/sources/<source>.md` (modèle `_modele.md`) |
| Trancher une question structurante | Issue *Décision* + ADR `docs/decisions/ADR-XXXX-<slug>.md` |
| Consigner un entretien | `docs/entretiens/`, **anonymisé** (aucun nom, aucune école + promo + stage permettant de reconnaître quelqu'un) |
| Discuter d'un point précis du code ou d'un doc | Un commentaire dans la PR, sur la ligne concernée |

Les notes brutes d'entretien et tout ce qui contient des données personnelles restent **hors du repo** (drive partagé privé).

---

## 5. En cas de pépin

| Situation | Que faire |
|---|---|
| `git push` refusé (*rejected*) | `git pull` sur ta branche, puis `git push` |
| Conflit au `git pull` | Ouvrir les fichiers signalés, garder la bonne version entre les marqueurs `<<<<<<<` et `>>>>>>>`, puis `git add` et `git commit`. Ou demander à Claude Code. |
| J'ai travaillé sur `main` par erreur | `git switch -c p0/<num>-<slug>` : les changements non commités suivent sur la nouvelle branche |
| J'ai commité un secret ou une donnée personnelle | **Ne pas pousser.** Si c'est déjà poussé, prévenir Louis tout de suite : il faut révoquer la clé et réécrire l'historique. Un simple commit de suppression ne suffit pas, l'historique est public. |
