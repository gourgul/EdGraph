# Knowledge graph d'orientation professionnelle : méthodologie

> **Statut** : v0.3, 24/11/2026 (après 2 tours de questions). Document vivant, source de vérité du projet.
> **Usage** : à placer dans `docs/methodologie.md` du repo. Toute décision tranchée est reportée en §2 et, si elle est structurante, dans un ADR (`docs/decisions/`).
> **Légende** : `OUVERT` = à décider · `TRANCHÉ` = décidé · `À VÉRIFIER` = affirmation non vérifiée sur source primaire.

---

## 1. Cadrage

**Problème visé** : aider des étudiants d'école de commerce (POC : ESSEC) à passer des études à la vie professionnelle : quels métiers, quels secteurs, quels choix (électifs, stages, césure) y mènent, et ce que ça donne concrètement à l'entrée et quelques années après.

**Ce qu'il faut avoir en tête avant de coder**

1. **Le marché du premier emploi se durcit.** Enquête CGE 2026 : taux net d'emploi des écoles de management à 74,5 %, en baisse de 3,8 points sur un an. Le besoin d'aide à l'orientation pro est réel et croissant.
2. **Le concurrent n°1 est déjà dans les écoles.** JobTeaser se présente comme l'espace carrière officiel de 800+ établissements en Europe, avec un module d'orientation (Career Explorer) et un produit de matching (TalentMatch, janvier 2026). Si ESSEC l'utilise (À VÉRIFIER), notre outil doit apporter quelque chose que JobTeaser n'a pas.
3. **Ce que JobTeaser n'a probablement pas : les trajectoires réelles des alumni d'une école donnée**, reliées aux choix faits pendant la scolarité (électifs, chaires, stages, césure). C'est là que le graphe peut être meilleur. C'est aussi la donnée la plus difficile et la plus risquée juridiquement à obtenir (voir §3 P2 et conformité).
4. **Le scraping LinkedIn est exclu.** La CNIL a sanctionné KASPR (240 000 €, décembre 2024) pour collecte de données LinkedIn ; tout moissonnage de données personnelles exige une base légale et une information des personnes. Les trajectoires doivent venir d'un partenariat (école, association d'alumni) ou d'un consentement direct des personnes.
5. **L'API Offres d'emploi de France Travail n'est actuellement plus exposée** (catalogue data.gouv). La couche "offres en cours" doit donc reposer sur d'autres sources ou être reportée.

**Thèse** : l'actif défendable est (a) le graphe parcours → expériences → postes → trajectoires, propre à une école, sourcé et explicable ; (b) l'accès aux données qui l'alimentent. Si le graphe ne répond pas mieux que JobTeaser + le service carrière + le réseau informel sur des cas réels, on arrête.

**Conséquence des décisions D2 et D6 : le POC n'est pas le produit.** Le POC tourne sur données publiques ; le produit sera vendu aux écoles, qui apporteront leurs données (alumni, stages, cours). Le POC doit donc prouver deux choses distinctes :
1. **Côté étudiants** : même avec des données publiques, le graphe donne des pistes de stage / césure plus pertinentes et mieux justifiées que les outils actuels.
2. **Côté écoles** : un service carrière voit ce que le graphe deviendrait avec ses propres données, et serait prêt à les partager et à payer.

Limite assumée : les questions qui demandent des trajectoires d'alumni (« où sont ceux qui ont fait X ? ») ne peuvent pas être traitées dans le POC. On les montre en démonstration sur données **synthétiques**, clairement présentées comme telles.

---

## 2. Registre des décisions

| ID | Décision | Statut | Réponse | Date |
|---|---|---|---|---|
| D1 | Segment du MVP | TRANCHÉ | Étudiants d'école de commerce vers la vie professionnelle ; POC sur l'ESSEC | 24/11/2026 |
| D2 | Qui paie | TRANCHÉ | Prototype d'abord ; à terme, produit vendu aux écoles (B2B, service carrière) | 24/11/2026 |
| D3 | Équipe | TRANCHÉ | Louis et Jules codent tous les deux, avec Claude Code | 24/11/2026 |
| D4 | Horizon du premier test utilisateurs | TRANCHÉ | Fin avril 2027 | 24/11/2026 |
| D5 | Programme et moment ciblés | TRANCHÉ | ESSEC Grande École (MiM) ; choix de stages et de césure | 24/11/2026 |
| D6 | Accès aux données | TRANCHÉ | POC sur données publiques uniquement ; données des écoles dans le produit vendu | 24/11/2026 |
| D13 | Quels services carrière solliciter pour valider la proposition B2B pendant le POC (ESSEC + 1 à 2 autres écoles) | OUVERT | | |
| D7 | Seuils qualité (taux de rapprochement, précision des liens extraits) | OUVERT | | |
| D8 | Stack technique | OUVERT (proposition en §5) | | |
| D9 | Questions de compétence retenues (liste figée) | OUVERT | | |
| D10 | Profilage : données collectées sur l'étudiant, et celles jamais collectées | OUVERT | | |
| D11 | Positionnement AI Act (orientation vs outil utilisé par des recruteurs) | OUVERT | | |
| D12 | Règles employeurs des associés (EY, RBC) sur les activités extérieures | OUVERT | | |

---

## 3. Phases

Chaque phase a : objectif, tâches, livrables (chemins dans le repo), critère de sortie vérifiable, risques. Une phase n'est close que si son critère de sortie est atteint.

**Calendrier indicatif (D4, test fin avril 2027, soit ~22 semaines à temps partiel)** : P0–P1 fin décembre 2026 · P2–P3 mi-février · P4–P5 mi-mars · P6–P7 mi-avril · P8 fin avril. Le périmètre de chaque phase reste minimal ; tout ce qui n'est pas indispensable au test part en backlog.

### P0. Cadrage et questions de compétence

- **Objectif** : savoir à quelles questions le graphe doit répondre. Rien n'est ingéré qui ne serve une question.
- **Tâches**
  - Trancher D5, D6, D9, D12.
  - 8 à 10 entretiens courts avec des étudiants ESSEC du segment D5 : quelles questions se posent-ils vraiment, où cherchent-ils aujourd'hui (JobTeaser, service carrière, alumni, LinkedIn, bouche-à-oreille) ?
  - Écrire 5 à 8 questions de compétence, chacune avec : formulation étudiant, données nécessaires, réponse attendue sur un cas réel, et **faisable dans le POC (données publiques) ou produit seulement (données école)**. Exemples de départ :
    - Q1 (POC). « Avec mon parcours (électifs, expériences), quels types de stage ou de césure sont cohérents, et quelles compétences me manquent pour tel autre type ? »
    - Q2 (POC). « Je vise le métier Z à la sortie : quels types de stage, dans quels secteurs, s'en rapprochent le plus en compétences ? »
    - Q3 (POC). « Césure en entreprise, à l'étranger ou en projet perso : qu'est-ce que chaque option développe, au regard du métier visé ? »
    - Q4 (POC). « Dans quels secteurs travaillent les diplômés MiM, et avec quels salaires d'entrée ? » (rapports carrière ESSEC, CGE : agrégés)
    - Q5 (produit). « Les alumni qui ont fait un stage en Y, où sont-ils 3 à 5 ans après ? » → démonstration sur données synthétiques uniquement
    - Q6 (produit). « Quelles entreprises prennent des stagiaires ESSEC en césure sur ce type de poste ? » → données de stages de l'école
  - Benchmark : tester chaque question sur JobTeaser, le site carrière ESSEC, Welcome to the Jungle, APEC, et 2 à 3 outils IA.
- **Livrables** : `docs/questions-de-competence.md`, `docs/entretiens/`, `docs/benchmark.md`, ADR-0001 (segment), ADR-0002 (accès aux données).
- **Critère de sortie** : questions figées ; pour chaque question, la meilleure réponse disponible aujourd'hui est documentée. C'est la barre à battre.
- **Risques** : sauter la phase par envie de coder ; questions inventées au lieu d'être entendues en entretien.

### P1. Ontologie minimale

- **Nœuds** : Étudiant (profil), Programme / Majeure / Chaire, Cours (électif), Compétence, Expérience (stage, césure, alternance, association), Entreprise, Secteur, Poste (intitulé), Métier (référentiel), Trajectoire (séquence de postes d'un alumni), Indicateur (salaire, taux d'emploi).
- **Arêtes** : `SUIT` (étudiant → cours), `DEVELOPPE` (cours / expérience → compétence), `A_EFFECTUE` (étudiant / alumni → expérience), `CHEZ` (expérience / poste → entreprise), `APPARTIENT_A` (entreprise → secteur), `CORRESPOND_A` (poste → métier), `MOBILISE` (métier → compétence), `MENE_A` (poste → poste, dans les trajectoires), `A_RESULTAT` (métier / secteur / programme → indicateur).
- **Référentiels de métiers et compétences** : ROME 4.0 (France Travail) et/ou ESCO (référentiel européen, utile pour les postes internationaux et les intitulés anglais). Choix en P1, documenté en ADR.
- **Règle non négociable** : chaque arête porte `source`, `millesime`, `methode` (`officiel` | `declare` | `extrait_llm` | `infere`), `confiance` (0–1), `date_ingestion`. Les arêtes issues de données personnelles portent en plus `base_legale` et `consentement_id`.
- **Livrables** : `ontology/schema.yaml`, `ontology/constraints.cypher`, `docs/ontologie.md` (correspondance questions ↔ chemins du graphe).
- **Critère de sortie** : chaque question de compétence s'écrit comme une requête sur le schéma.

### P2. Collecte : inventaire et fiches sources

- **Sources candidates**

| Couche | Source | Apporte | Accès | Piège |
|---|---|---|---|---|
| Insertion agrégée | Enquête insertion CGE (annuelle, publiée en juin) | Taux d'emploi, salaires, secteurs par type d'école | Publications + rapports d'écoles | Agrégé par type d'école ; déclaratif |
| Insertion ESSEC | Rapports carrière / employment reports ESSEC | Secteurs, salaires, fonctions par programme | Public, parfois sous formulaire | Format PDF, périmètre variable selon programmes |
| Insertion administrative | SIES / InserSup : note flash juillet 2026 sur l'emploi salarié à 18 mois des bac+5 ingénieur et management | Taux d'emploi salarié en France | Public | Granularité par école À VÉRIFIER ; salariat en France seulement |
| Parcours scolaire | Catalogue des cours / électifs / chaires ESSEC | Cours → compétences | Site ESSEC ou partenariat (D6) | Descriptions textuelles → extraction LLM |
| Trajectoires alumni | ESSEC Alumni (annuaire) ou enquête maison auprès d'alumni consentants | Séquences de postes, lien avec les choix de scolarité | Partenariat ou collecte directe avec consentement (D6) | Donnée personnelle ; biais de réponse ; **pas de scraping** |
| Profil étudiant | Déclaratif + CV ou export LinkedIn fourni par l'étudiant lui-même | Parcours, expériences, préférences | Consentement de l'utilisateur | Minimisation ; droit à l'effacement |
| Métiers / compétences | API ROME 4.0 (France Travail) ; ESCO (UE) | Référentiels | API / téléchargement | Granularité ; correspondance ROME ↔ ESCO |
| Entreprises / secteurs | Base SIRENE (INSEE) | SIREN, code NAF, effectif | Open data | Rapprochement nom d'entreprise → SIREN (flou) |
| Marché du travail | API Marché du travail (France Travail) ; APEC (études, À VÉRIFIER pour l'accès aux données) | Tensions, recrutements cadres | API / publications | Granularité territoire × métier |
| Offres en cours | API Offres d'emploi France Travail | Postes ouverts | **Actuellement non exposée** | Hors MVP sauf source alternative |

- **Fiche source** (`docs/sources/<source>.md`) : URL, licence / CGU, producteur, champs utiles, clés, millésimes, fréquence, limites, base légale si données personnelles, question(s) servie(s).
- **Critère de sortie** : chaque question de P0 est couverte par au moins une source accessible légalement avant mi-mars ; sinon la question sort du MVP.
- **Périmètre POC (D6)** : seules les lignes publiques sont ingérées (CGE, rapports carrière ESSEC, SIES, catalogue de cours public, ROME / ESCO, SIRENE, API Marché du travail). Les lignes « Trajectoires alumni » et données de stages de l'école sont **hors POC**, simulées par un jeu synthétique (`tests/fixtures/alumni_synthetiques/`) qui suit exactement le schéma attendu des données école.
- **Point faible du POC** : aucune source publique ouverte ne décrit les stages et césures réellement effectués par les étudiants. Les réponses Q1–Q3 reposeront sur des correspondances de compétences (cours, expériences → métiers), pas sur des parcours observés. C'est le principal argument de vente auprès des écoles (« avec vos données, ça devient observé »), mais aussi le principal risque de décevoir les étudiants testeurs.
- **À vérifier en P2** : le catalogue de cours MiM est-il publié avec des descriptions exploitables ? Les rapports carrière ESSEC détaillent-ils stages et césures, ou seulement le premier emploi ?

### P3. Ingestion et réconciliation des identifiants

- **Objectif** : données reproductibles + **table pivot** reliant intitulés de postes → métiers (ROME / ESCO), noms d'entreprises → SIREN → secteur (NAF), cours → compétences.
- **Couches**
  1. `raw` : fichiers bruts par millésime, jamais modifiés, hash dans `ingestion/manifest.yaml`. Les données personnelles sont chiffrées au repos et ne quittent pas l'environnement prévu (§4).
  2. `silver` : normalisation par source.
  3. `pivot` : correspondances avec méthode (exact / règle / flou / LLM / manuel) et confiance.
  4. `gold` : nœuds et arêtes prêts à charger.
- **Le travail le plus lourd** : normaliser des intitulés de postes libres (« Analyst M&A », « Chargé d'affaires junior », « Consultant Strategy ») vers un référentiel de métiers, et des noms d'entreprises vers un SIREN.
- **Livrables** : `ingestion/sources/<source>/`, `pivot/`, `reports/rapprochement.md` généré.
- **Critère de sortie** : pipeline rejouable en une commande ; taux de rapprochement intitulés → métiers et entreprises → SIREN ≥ seuils D7.

### P4. Liens extraits et qualité

- **Objectif** : construire les liens non fournis proprement (cours → compétences, intitulé → métier, expérience → compétences).
- **Ordre de préférence** : correspondance officielle → déclaration par la personne → extraction LLM (taguée `extrait_llm`) → inférence (`infere`).
- **Contrôle** : échantillon annoté à la main par les deux associés (taille fixée en D7), mesure de précision et d'accord inter-annotateurs ; les liens sous le seuil ne sont pas montrés à l'utilisateur.
- **Livrables** : `eval/annotations/`, `reports/qualite-liens.md`.
- **Critère de sortie** : précision ≥ seuil D7 pour chaque type de lien exposé.

### P5. Chargement du graphe et réponse aux questions

- **Livrables** : `graph/load.py`, `graph/queries/qN.cypher`, `eval/questions/` (cas + réponse attendue).
- **Critère de sortie** : chaque question de compétence passe ses cas tests en CI, avec une réponse au moins aussi bonne que le benchmark P0.

### P6. Profilage de l'étudiant

- **Principe** : le profil est un sous-graphe ancré sur les mêmes nœuds (cours, expériences, compétences, secteurs visés), pas un score opaque.
- **Trois couches**
  - Faits : programme, année, majeure / chaire, électifs suivis, expériences (stages, césure, associations), langues, mobilité internationale envisagée.
  - Préférences déclarées : secteurs et métiers attirants ou exclus, critères (rémunération, sens, international, rythme de vie, taille d'entreprise).
  - Signaux d'usage : pistes explorées ou écartées → révision des poids.
- **Ingestion du profil** : saisie guidée + import d'un CV ou d'un export LinkedIn fourni par l'étudiant (ses propres données, avec son accord), parsé puis validé par lui avant d'entrer dans le graphe.
- **Garde-fous**
  - Pas de test de personnalité présenté comme prédictif.
  - Ne pas réduire la reco à « ce que font la plupart des ESSEC » : montrer aussi les trajectoires minoritaires, sinon l'outil renforce le tropisme conseil / finance au lieu d'élargir les options.
  - Minimisation : liste fermée des champs (D10), aucune donnée sensible.
- **Livrables** : `profiling/schema.yaml`, `profiling/elicitation.md`, `profiling/cv_parser/`, `docs/rgpd.md`.
- **Critère de sortie** : un profil se construit en moins de N minutes (à fixer) ; chaque champ sert au moins une question.

### P7. Recommandation explicable

- **Principe** : une recommandation = un ou plusieurs chemins dans le graphe, restitués en clair. Exemple : « Métier X : 12 alumni ayant suivi la chaire Y y sont arrivés en moins de 3 ans, souvent après un stage en Z (source : enquête alumni 2027, n = 12) ».
- **Règles** : le LLM reformule et dialogue, il ne fabrique pas la recommandation ; tout chiffre affiché a sa source, son millésime et son effectif ; en dessous d'un effectif minimal (à fixer), on n'affiche pas de statistique (risque de réidentification d'un alumni).
- **Livrables** : `reco/`, `reco/explain.py`, tests d'explicabilité.
- **Critère de sortie** : 100 % des recommandations du jeu de test ont un chemin, des sources et respectent le seuil d'effectif.

### P8. Test utilisateurs et go / no-go

- **Volet étudiants** : 10 à 20 étudiants MiM qui préparent un stage ou une césure ; chacun pose ses propres questions (Q1–Q4) et compare avec ce qu'il obtient via JobTeaser, le service carrière et son réseau. Avec leur accord explicite, leur propre parcours sert de profil de test.
- **Volet écoles (D13)** : démonstration à 2 ou 3 services carrière (ESSEC + 1 à 2 autres écoles), incluant Q5–Q6 sur données synthétiques. Questions à leur poser : quelles données ils ont réellement (format, qualité, base légale), s'ils seraient prêts à les partager, ce qu'ils paient aujourd'hui (JobTeaser, autres) et ce qu'ils paieraient pour ceci.
- **Mesures** : préférence déclarée des étudiants, pistes nouvelles jugées pertinentes, erreurs factuelles relevées ; côté écoles, intention de partager les données et ordre de grandeur de budget.
- **Livrables** : `docs/tests-utilisateurs/`, `docs/entretiens-ecoles/`, ADR go / no-go.
- **Critère de sortie** : décision écrite. Go seulement si les deux volets sont positifs : un graphe que les étudiants aiment mais qu'aucune école ne veut alimenter ne fait pas un produit B2B.

### Transverse : conformité (P0 → P8)

- **RGPD** : utilisateurs majeurs, mais données de parcours et de carrière = données personnelles. Registre de traitement, base légale par source, information des personnes, durée de conservation, droit d'effacement opérationnel. Si l'ESSEC fournit des données, elle est responsable de traitement et nous sous-traitants : contrat (art. 28) avant tout transfert.
- **Pas de scraping de profils** (voir §1, CNIL / KASPR).
- **AI Act** : un outil d'orientation pour étudiants n'est a priori pas un système de recrutement. Mais si le produit est un jour vendu à des recruteurs pour trier ou classer des candidats, il entrerait dans la catégorie « emploi » de l'annexe III (haut risque) (À VÉRIFIER sur le texte, point 4). D11 à trancher avant toute discussion commerciale avec des entreprises.
- **Activités extérieures** : D12 avant tout lancement public.

---

## 4. Organisation GitHub

### Structure du repo

```
EdGraph/
├── README.md                  # pitch, installation, commande unique pour rejouer le pipeline
├── CLAUDE.md                  # contexte et règles pour Claude Code (§6)
├── docs/
│   ├── methodologie.md        # ce document
│   ├── questions-de-competence.md
│   ├── entretiens/            # comptes rendus anonymisés
│   ├── benchmark.md
│   ├── ontologie.md
│   ├── rgpd.md
│   ├── sources/               # une fiche par source
│   ├── decisions/             # ADR-0001-segment.md, ...
│   └── tests-utilisateurs/
├── ontology/
├── ingestion/
│   ├── manifest.yaml
│   └── sources/<source>/      # download.py, parse.py, validate.py
├── pivot/
├── graph/
├── profiling/
├── reco/
├── eval/
├── reports/
├── tests/
│   └── fixtures/              # données synthétiques uniquement
├── data/                      # .gitignore : jamais commité
├── docker-compose.yml
├── .env.example
└── .github/
    ├── workflows/ci.yml
    ├── ISSUE_TEMPLATE/        # tâche, source, décision
    ├── pull_request_template.md
    └── CODEOWNERS
```

### Conventions

- **Repo privé.** Une phase = un milestone (P0 … P8). Une tâche = une issue avec critère d'acceptation. Un GitHub Project (tableau) pour suivre l'avancement.
- **Labels** : `phase:Px`, `type:source|code|doc|decision|rgpd`, `owner:louis|jules`, `bloquant`.
- **Branches** : `main` protégée ; une branche par issue (`px/<num>-<slug>`) ; PR obligatoire, relue par l'autre associé ; CI verte requise.
- **Répartition proposée (D3)** : chacun porte des modules entiers plutôt que des morceaux du même fichier. Proposition à valider : Louis sur ontologie, pivot et graphe ; Jules sur sources marché du travail / entreprises / salaires, fiches sources et protocole de test ; les deux sur les annotations (P4).
- **Données** : jamais dans Git, jamais de données personnelles réelles dans les tests (fixtures synthétiques). Reproductibilité par `manifest.yaml`. Données personnelles stockées dans un espace chiffré hébergé dans l'UE, accès limité aux deux associés.
- **Secrets** : `.env` local, `.env.example` versionné, secrets CI dans GitHub Actions.
- **Décisions** : toute décision structurante = un ADR court.

---

## 5. Stack proposée (D8)

| Besoin | Proposition | Pourquoi | Alternative |
|---|---|---|---|
| Langage | Python, gestion par `uv` | Écosystème data ; prise en main facilitée avec Claude Code | — |
| Transformations | DuckDB + Parquet | Gratuit, local, SQL lisible | Postgres |
| Base graphe | Neo4j Community (Docker) | Requêtes multi-sauts (trajectoires) naturelles en Cypher | Postgres + tables d'arêtes au début |
| Extraction / normalisation | LLM via API, prompts versionnés dans `prompts/` | Intitulés de postes, descriptions de cours, CV | Règles + NLP classique |
| Interface de test P8 | Streamlit ou page web simple | Suffisant pour un test à 20 utilisateurs | — |
| Tests / CI | pytest, GitHub Actions | Standard | — |

---

## 6. Travailler avec Claude Code

- **`CLAUDE.md` à la racine**, contenu de départ :
  - Objectif du projet en 3 lignes + lien vers `docs/methodologie.md`.
  - Phase et milestone actifs.
  - Règles : ne jamais commiter `data/` ni `.env` ; jamais de donnée personnelle réelle dans le code, les tests ou les prompts ; toute arête porte `source`, `millesime`, `methode`, `confiance` ; aucun chiffre sans source ; tests avant merge ; une issue par session.
  - Commandes : installer, rejouer le pipeline, lancer les tests, lancer la base graphe.
- **Mode de travail** : une session = une issue. Claude Code lit `CLAUDE.md` + la fiche source ou l'ADR concerné, propose un plan, code, teste, ouvre la PR ; l'autre associé relit et merge.
- **Pour Jules (montée en compétence)** : commencer par des issues bien bornées (une fiche source + son module `download/parse/validate`), demander à Claude Code d'expliquer chaque PR avant de l'ouvrir.
- **Ce qu'on ne délègue pas** : les décisions du registre (§2), l'annotation (P4), les entretiens et tests utilisateurs (P0, P8), tout ce qui touche à une donnée personnelle réelle.

---

## 7. Modèle d'ADR

```
# ADR-XXXX : <titre>
Date :
Statut : proposé | accepté | remplacé par ADR-YYYY
Contexte :
Décision :
Alternatives écartées (et pourquoi) :
Conséquences :
```

---

## Sources

- Enquête insertion CGE 2026 (synthèse) : https://www.planetegrandesecoles.com/enquete-insertion-professionnelle-cge
- Enquête CGE 2026 (CIDJ) : https://www.cidj.com/s-orienter/apres-le-bac/premier-emploi-l-insertion-des-diplomes-de-grandes-ecoles-ralentit-nettement
- SIES, note flash NF 2026-20 (emploi salarié à 18 mois, bac+5 ingénieur et management) : https://www.enseignementsup-recherche.gouv.fr/fr/le-taux-d-emploi-salarie-en-france-des-diplomes-de-2024-de-formation-ingenieur-et-en-management-de-103821
- JobTeaser, services carrière et Career Explorer : https://www.jobteaser.com/fr/corporate/services-carriere ; https://www.jobteaser.com/fr/corporate/guidez-vos-etudiants
- JobTeaser TalentMatch (janvier 2026) : https://presseagence.fr/?p=942496
- Étude EDHEC x JobTeaser 2026 : https://www.edhec.edu/sites/default/files/2026-02/2026_02_03_CP_Etude_EDHEC_JobTeaser_2026.pdf
- CNIL, sanction KASPR : https://www.cnil.fr/fr/aspiration-de-donnees-sanction-de-200-000-euros-lencontre-de-la-societe-kaspr
- CNIL, intérêt légitime et moissonnage : https://www.cnil.fr/fr/focus-interet-legitime-collecte-par-moissonnage
- API Offres d'emploi France Travail (statut) : https://www.data.gouv.fr/dataservices/672cf68826b31834a945cefe
- API Marché du travail France Travail : https://www.data.gouv.fr/dataservices/672cf65f739fcf87e0f02be2
- API ROME 4.0 : https://www.data.gouv.fr/dataservices/672cf64aa47b8d4c15d13d58
- AI Act, annexe III : https://ai-act-service-desk.ec.europa.eu/fr/ai-act/annex-3
