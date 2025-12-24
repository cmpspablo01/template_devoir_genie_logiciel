---
title: Conception - Modèle de données
---

# Modèle de données
Cette section décrit les entités métier réellement implémentées dans le backend REST et utilisées par les services et contrôleurs.

## Entités principales

- **Étudiant** : utilisateur de la plateforme (profil, préférences, informations de connexion).  
- **Cours** : informations officielles d’un cours (code, titre, crédits, prérequis, cycle).  
- **Avis** : retour d’un étudiant sur un cours (difficulté perçue, charge de travail, commentaire).  
- **Résultat académique** : données globales d’un cours (moyenne, nombre d’inscrits, nombre d’échecs).  
- **Profil** : préférences de l’étudiant (théorie/pratique, champs d’intérêt).  
- **Program** : représente un programme académique et les ensembles de cours associés.

## Relations entre entités

- Un **étudiant** peut avoir plusieurs **avis**.  
- Un **cours** peut avoir plusieurs **avis** et plusieurs **résultats académiques** (par trimestre).  
- Un **profil** est lié à un seul **étudiant**.  


## Éligibilité

- **EligibilityResult**  
  Représente le résultat d’une vérification d’éligibilité à un cours.
  Contient :
  - un indicateur d’éligibilité.
  - la liste des prérequis manquants, le cas échéant.

## Comparaison de cours

- **CourseSet**  
  Représente un ensemble de cours sélectionnés pour une comparaison.

- **CourseComparisonItem**  
  Contient les informations comparables d’un cours
  (crédits, horaires, contraintes).

- **CourseComparisonResult**  
  Représente le résultat global d’une comparaison :
  - charge totale estimée.
  - conflits potentiels d’horaires.
  - synthèse des cours comparés.

## Avis étudiants et résultats académiques

- **Review**  
  Représente un avis individuel soumis par un étudiant pour un cours.

- **ReviewAggregate**  
  Contient les données agrégées des avis étudiants(moyennes, statistiques).

- **AcademicResult**  
  Représente les résultats académiques issus des fichiers CSV, tels que la moyenne, le nombre d’inscriptions et le nombre d’échecs.


## Contraintes métier

- Un avis n’est visible que si au moins **5 avis** existent pour ce cours 
- Un étudiant doit être **authentifié** pour publier un avis ou personnaliser son profil.  
- Les résultats académiques sont **agrégés** et donc pas de données individuelles.  

## Évolution potentielle du modèle

- Ajouter des **statuts** pour les cours ex : populaire, difficile, recommandé.  
- Gérer des **combinations de cours** pour estimer la charge de travail totale.  
- Étendre la personnalisation exemple : recommandations automatiques selon le profil.  
