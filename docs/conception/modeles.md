---
title: Conception - Modèle de données
---

# Modèle de données

## Entités principales

- **Étudiant** : utilisateur de la plateforme (profil, préférences, informations de connexion).  
- **Cours** : informations officielles d’un cours (code, titre, crédits, prérequis, cycle).  
- **Avis** : retour d’un étudiant sur un cours (difficulté perçue, charge de travail, commentaire).  
- **Résultat académique** : données globales d’un cours (moyenne, nombre d’inscrits, nombre d’échecs).  
- **Profil** : préférences de l’étudiant (théorie/pratique, champs d’intérêt).  

## Relations entre entités

- Un **étudiant** peut avoir plusieurs **avis**.  
- Un **cours** peut avoir plusieurs **avis** et plusieurs **résultats académiques** (par trimestre).  
- Un **profil** est lié à un seul **étudiant**.  

## Contraintes métier

- Un avis n’est visible que si au moins **5 avis** existent pour ce cours 
- Un étudiant doit être **authentifié** pour publier un avis ou personnaliser son profil.  
- Les résultats académiques sont **agrégés** et donc pas de données individuelles.  

## Évolution potentielle du modèle

- Ajouter des **statuts** pour les cours ex : populaire, difficile, recommandé.  
- Gérer des **combinations de cours** pour estimer la charge de travail totale.  
- Étendre la personnalisation exemple : recommandations automatiques selon le profil.  
