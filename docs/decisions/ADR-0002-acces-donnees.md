# ADR-0002 : accès aux données

Date : 24/11/2026
Statut : accepté

## Contexte
L'avantage potentiel sur JobTeaser repose sur les trajectoires réelles d'alumni reliées aux choix de scolarité. Le scraping de profils est exclu (sanction CNIL contre KASPR, 240 000 €, décembre 2024). Un partenariat avec l'école est trop lent pour un test fin avril 2027.

## Décision
- POC : données publiques uniquement.
- Produit : vendu aux écoles, qui apportent leurs données (alumni, stages, cours), dans un cadre contractuel (EdGraph sous-traitant, art. 28 RGPD).
- Les fonctions dépendant des données école (Q5, Q6) sont démontrées sur un jeu **synthétique** qui suit le schéma attendu des données école.

## Alternatives écartées
- Enquête alumni maison pendant le POC.
- Partenariat avec le service carrière ou ESSEC Alumni dès le POC.

## Conséquences
- Q1 à Q3 reposent sur des correspondances de compétences, pas sur des parcours observés : risque de décevoir les étudiants testeurs.
- Question ouverte : collecte consentie des stages et césures d'une trentaine d'étudiants (voir ADR-0005, à rédiger).
- Le test P8 comprend un volet école : les services carrière sont-ils prêts à partager leurs données et à payer ?
