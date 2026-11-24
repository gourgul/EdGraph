# RGPD et conformité

> **Statut** : BROUILLON. À faire relire avant toute collecte de données personnelles réelles.

## Principes retenus
- Pas de scraping de profils de personnes (la CNIL a sanctionné KASPR à hauteur de 240 000 € en décembre 2024 pour collecte de données LinkedIn).
- Minimisation : liste fermée des champs collectés (D10, à remplir ci-dessous).
- Données personnelles hors Git, stockées chiffrées, hébergement UE, accès limité aux deux associés.
- Effacement sur demande opérationnel dès la première collecte.
- Aucune statistique affichée sous un effectif minimal (risque de réidentification).

## Registre des traitements (POC)

| Traitement | Personnes | Données | Finalité | Base légale | Durée | Statut |
|---|---|---|---|---|---|---|
| Entretiens P0 | Étudiants MiM | Notes d'entretien anonymisées | Définir les questions | Consentement | Fin du POC | À préparer |
| Profils de test P8 | Étudiants testeurs | Parcours, expériences, préférences, CV fourni par eux | Tester les recommandations | Consentement | Fin du POC | À préparer |
| Collecte stages / césures (si ADR-0005 positif) | ~30 étudiants MiM | Stages, césures, secteurs | Parcours observés pour le POC | Consentement | À fixer | Question ouverte |

## Champs autorisés pour le profil étudiant (D10)
- (à fixer en P6)

## Champs jamais collectés
- Données sensibles au sens de l'art. 9 RGPD, numéro de sécurité sociale, notes détaillées, coordonnées de tiers.

## AI Act
- Outil d'orientation pour étudiants : a priori pas un système de recrutement.
- Si un jour vendu à des recruteurs pour trier ou classer des candidats : catégorie « emploi » de l'annexe III, haut risque (À VÉRIFIER, point 4). Décision D11 avant toute discussion commerciale avec des entreprises.

## Produit (hors POC)
- Données fournies par une école : l'école est responsable de traitement, EdGraph sous-traitant. Contrat art. 28 RGPD avant tout transfert.
