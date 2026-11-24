# Ontologie v0

> **Statut** : BROUILLON (P1). À valider contre `questions-de-competence.md` : chaque question doit s'écrire comme un chemin dans ce schéma.

## Nœuds

| Nœud | Identifiant de référence | Source POC |
|---|---|---|
| Etudiant | id interne (pseudonyme) | déclaratif, avec consentement |
| Programme / Majeure / Chaire | code interne | site ESSEC (À VÉRIFIER) |
| Cours | code cours ESSEC (À VÉRIFIER) | catalogue public (À VÉRIFIER) |
| Competence | URI ESCO et/ou code ROME 4.0 (ADR à prendre) | ESCO, ROME |
| Experience | id interne | déclaratif |
| Entreprise | SIREN | SIRENE |
| Secteur | code NAF | SIRENE |
| Poste | intitulé normalisé | déclaratif, rapports |
| Metier | code ROME / URI ESCO | ROME, ESCO |
| Trajectoire | id interne | produit seulement (données école) |
| Indicateur | id interne | CGE, rapports ESSEC, SIES |

## Arêtes

| Arête | De → vers | Exemple de question servie |
|---|---|---|
| SUIT | Etudiant → Cours | Q1 |
| DEVELOPPE | Cours / Experience → Competence | Q1, Q3 |
| A_EFFECTUE | Etudiant → Experience | Q1 |
| CHEZ | Experience / Poste → Entreprise | Q2, Q6 |
| APPARTIENT_A | Entreprise → Secteur | Q2, Q4 |
| CORRESPOND_A | Poste → Metier | Q2 |
| MOBILISE | Metier → Competence | Q1, Q2, Q3 |
| MENE_A | Poste → Poste | Q5 (produit) |
| A_RESULTAT | Metier / Secteur / Programme → Indicateur | Q4 |

## Propriétés obligatoires sur chaque arête

```yaml
source: str            # identifiant de la fiche docs/sources/
millesime: str         # ex. "2026"
methode: officiel | declare | extrait_llm | infere
confiance: float       # 0–1
date_ingestion: date
# si donnée personnelle :
base_legale: str
consentement_id: str
```

## Décisions à prendre en P1
- ROME 4.0, ESCO ou les deux (ESCO couvre mieux les intitulés anglais et les postes à l'étranger, ROME est la référence française).
- Granularité des compétences exposées à l'étudiant (trop fin = illisible).
