---
title: Conception - Présentation générale
---

# Conception

Cette section explique les choix faits pour construire le système.

## Approche utilisée

Nous avons choisi une **architecture client-serveur** :  
- **Frontend (interface web)** : ce que l’étudiant voit et utilise (recherche de cours, comparaison, avis).  
- **Backend (API REST)** : gère la logique (règles, calculs, agrégation des données).  
- **Base de données** : garde les informations (résultats académiques, profils étudiants, avis).  

Ce modèle ressemble au **MVC** :  
- Vue = l’interface,  
- Contrôleur = l’API backend,  
- Modèle = la base de données.  

## Contraintes prises en compte

- **Techniques** :  
  - Hébergement sur un serveur web simple.  
  - Base de données relationnelle (PostgreSQL) ou NoSQL (MongoDB).  
  - Utilisation des formats standards : JSON (API) et CSV (résultats).  

- **Exigences** :  
  - **Sécurité** : connexion sécurisée, protection des données personnelles (Loi 25).  
  - **Rapidité** : réponse en moins de 2 secondes pour les recherches et comparaisons.  
  - **Accessibilité** : site simple, clair, qui fonctionne aussi sur mobile.  
  - **Évolutivité** : possibilité d’ajouter d’autres données ou nouvelles fonctions plus tard.  
