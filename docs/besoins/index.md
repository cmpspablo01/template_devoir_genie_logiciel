---
title: Analyse des besoins - Présentation générale
---

# Présentation du projet

## Méthodologie pour la cueillette des données
Les besoins ont été identifiés à partir :

- de l’énoncé officiel du projet fourni par l’enseignant ;
- de l’analyse de la liste des souhaits (fournie comme base de départ) ;
- de discussions internes dans l’équipe (brainstorming) ;
- d’une revue des sources de données disponibles (Planifium API, fichiers CSV de résultats académiques, avis Discord).

## Description du domaine

### Fonctionnement
Le système proposé est une plateforme web visant à aider les étudiants de l’Université de Montréal à prendre des décisions éclairées lors du choix de cours.  
Elle centralise les informations officielles (Planifium, résultats académiques agrégés) et non officielles (avis étudiants via Discord).  
L’étudiant peut rechercher un cours, vérifier son admissibilité (prérequis, cycle, etc.), consulter un tableau de bord du cours (résultats + avis), comparer plusieurs cours pour estimer la charge de travail, et personnaliser l’affichage selon son profil.

### Acteurs
- **Étudiant invité** : peut consulter les cours et leurs informations de base.  
- **Étudiant authentifié** : peut configurer son profil, comparer des cours, et consulter les avis agrégés.  
- **Administrateur** : gère l’ingestion des données (CSV, Discord, Planifium) et la validation minimale des avis.  
- **Systèmes externes** :  
  - *Planifium API* (catalogue officiel des cours et horaires)  
  - *Fichiers CSV* (résultats académiques agrégés)  
  - *Bot Discord* (collecte des avis étudiants)

### Dépendances
- Accès aux données officielles via l’API Planifium.  
- Disponibilité des résultats académiques agrégés (format CSV).  
- Participation des étudiants pour fournir des avis via Discord.  
- Outils de documentation et de modélisation (MkDocs, Git/GitHub).  

## Hypothèses et contraintes

### Hypothèses
- Les avis étudiants sont collectés uniquement via un bot Discord.  
- Les avis ne sont affichés que si n ≥ 5.  
- Les résultats académiques sont fournis sous format CSV agrégé.  
- Pour la Phase 1, toutes les données externes sont simulées par des fichiers statiques.  

### Contraintes
- Respect de l’échéancier (Phase 1 à livrer le 28 Sep).  
- Pas d’implémentation finale ni d’optimisation de performance en Phase 1.  
- Le système reste un prototype documentaire et conceptuel à cette étape.