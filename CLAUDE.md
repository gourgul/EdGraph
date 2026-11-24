# CLAUDE.md : contexte pour Claude Code

## Projet
EdGraph : knowledge graph d'orientation professionnelle pour étudiants d'école de commerce.
POC : ESSEC Grande École (MiM), choix de stages et de césure, sur données publiques uniquement.
Méthodologie complète et registre des décisions : `docs/methodologie.md`. La lire avant toute tâche non triviale.

## Phase en cours
P0, cadrage. Milestone actif : `P0`. Backlog : `docs/backlog-p0.md`.
Ne pas écrire de code d'ingestion tant que `docs/questions-de-competence.md` n'est pas figé (critère de sortie P0).

## Règles non négociables
1. Ne jamais commiter `data/`, `.env`, ni aucun fichier contenant des données personnelles réelles.
2. Jamais de donnée personnelle réelle dans le code, les tests, les fixtures ou les prompts. Les fixtures sont synthétiques.
3. Toute arête du graphe porte `source`, `millesime`, `methode` (`officiel` | `declare` | `extrait_llm` | `infere`), `confiance` (0–1), `date_ingestion`.
4. Aucun chiffre affiché sans source, millésime et effectif. Sous l'effectif minimal, pas de statistique.
5. Pas de scraping de profils de personnes (LinkedIn ou autre). Toute nouvelle source passe d'abord par une fiche `docs/sources/<source>.md`.
6. Le LLM extrait et reformule ; il ne fabrique pas la recommandation, qui est un chemin dans le graphe.
7. Tests avant merge ; une issue par session ; une branche par issue.

## Façon de travailler
- Lire l'issue, puis les fichiers qu'elle cite (fiche source, ADR, section de la méthodologie).
- Proposer un plan court avant de coder ; signaler toute contradiction avec `docs/methodologie.md`.
- Toute décision structurante prise en cours de route : proposer un ADR (`docs/decisions/ADR-XXXX-<slug>.md`, modèle dans la méthodologie §7) plutôt que de trancher en silence.
- Expliquer la PR en langage simple dans sa description : ce qui change, comment le tester, ce qui reste ouvert. Un des deux associés monte en compétence en développement.
- Affirmation non vérifiée sur source primaire : la marquer `À VÉRIFIER`.

## Commandes
À compléter en P2 (installation `uv`, pipeline, tests, base graphe via `docker compose`).
