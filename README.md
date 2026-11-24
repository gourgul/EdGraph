# EdGraph

Knowledge graph d'orientation professionnelle pour étudiants d'école de commerce.
POC : étudiants ESSEC Grande École (MiM) qui choisissent leurs stages et leur césure.

> Statut : phase **P0 (cadrage)**. Rien n'est encore codé. La méthodologie complète est dans [`docs/methodologie.md`](docs/methodologie.md).

## Le principe en trois lignes

- Un graphe relie cours, expériences, compétences, métiers, entreprises, secteurs et indicateurs d'insertion.
- Chaque lien porte sa source, son millésime, sa méthode et un niveau de confiance.
- Une recommandation est un chemin dans ce graphe, affiché avec ses sources, pas une réponse opaque d'un LLM.

## Ce que le POC est, et n'est pas

- **Est** : un prototype sur **données publiques** uniquement, testé fin avril 2027 auprès de 10 à 20 étudiants MiM et de 2 à 3 services carrière.
- **N'est pas** : le produit. Le produit sera vendu aux écoles, qui apporteront leurs données (alumni, stages, cours). Les fonctions qui en dépendent sont montrées sur données **synthétiques**, présentées comme telles.

## Organisation du repo

| Dossier | Contenu |
|---|---|
| `docs/` | Méthodologie, questions de compétence, ontologie, RGPD, fiches sources, ADR, entretiens |
| `ontology/` | Schéma du graphe (à venir, P1) |
| `ingestion/` | Un module par source + `manifest.yaml` (P2–P3) |
| `pivot/` | Réconciliation des identifiants (P3) |
| `graph/` | Chargement et requêtes par question (P5) |
| `profiling/`, `reco/` | Profil étudiant et recommandation explicable (P6–P7) |
| `eval/` | Questions + réponses attendues, annotations qualité |
| `tests/fixtures/` | Données **synthétiques** uniquement |
| `data/` | Données téléchargées. **Jamais commité.** |

## Règles d'équipe

- `main` protégée ; une branche par issue (`px/<num>-<slug>`) ; PR relue par l'autre associé.
- Une phase = un milestone GitHub ; une tâche = une issue avec critère d'acceptation.
- Toute décision structurante = un ADR dans `docs/decisions/`.
- Aucune donnée personnelle réelle dans le repo, ni dans le code, ni dans les tests, ni dans les prompts.

## Démarrer

```bash
gh repo clone gourgul/EdGraph && cd EdGraph
cp .env.example .env   # renseigner les clés API
# installation et pipeline : à écrire en P2 (uv + DuckDB + Neo4j, voir docs/methodologie.md §5)
```

Avant de commencer : lire `docs/methodologie.md`, puis `docs/backlog-p0.md`.
Pour la mise en route et le cycle issue → branche → PR : [`CONTRIBUTING.md`](CONTRIBUTING.md).
