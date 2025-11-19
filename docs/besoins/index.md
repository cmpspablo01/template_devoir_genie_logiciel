---
title: Analyse des besoins - Présentation générale
---

# Présentation du projet

## Méthodologie pour la cueillette des données
Les besoins ont été identifiés à partir :

- de l’énoncé officiel du projet fourni par l’enseignant ;
- de l’analyse de la liste des souhaits fournie comme base de départ ;
- de discussions internes dans l’équipe (brainstorming) ;
- d’une revue des sources de données disponibles (Planifium API, fichiers CSV de résultats académiques, avis Discord).

## Description du domaine

### Fonctionnement actuel avant notre application

Le domaine étudié est celui du **choix de cours des étudiants de l'Université de Montréal**, plus particulièrement au DIRO. Avant notre système, un étudiant doit combiner plusieurs outils et sources pour planifier sa session, comprendre les contraintes de son programme et estimer la charge de travail. Voici une description simplifiée du processus actuel :

- L'étudiant consulte les outils fournies par l’UdeM (centre étudiant, catalogue de cours, fiches de programme) pour identifier les cours disponibles à la prochaine session.
- Il identifie les cours qu'il doit suivre parmi les cours obligatoires, les cours à option et les cours hors programme, en tenant compte des blocs de son cheminement qu'il n'a pas complété.
- Il vérifie les prérequis, les corequis, les cycles et les horaires des cours pour lesquels il est admissible.
- Il consulte l’horaire d’un cours et le personnel enseignant qui donne le cours, mais pour obtenir des informations sur la charge de travail, la difficulté ou la qualité de l’enseignement, il doit utiliser des outils externes telles que les serveurs Discord de son programme, les discussions avec d’autres étudiants, ou des sites comme RateMyProfessor.
- Lorsque disponibles, il tente de trouver les **résultats académiques agrégés** afin d’estimer la performance historique des cohortes dans ce cours.
- Il construit un horaire sans conflits et essaie d’équilibrer sa charge de travail en fonction de ses contraintes personnelles et des exigences des cours.
- Au besoin, il contacte les **TGDE** pour valider son admissibilité, clarifier des règles de programme ou planifier son cheminement.


### Acteurs du système

- **Étudiant**  
  Utilise la plateforme pour choisir ses cours, consulter les avis, les résultats académiques, comparer des cours et personnaliser son profil.

- **TGDE (Technicien·ne en gestion des dossiers étudiants)**  
  Utilise la plateforme pour visualiser le cheminement d’un étudiant, vérifier son admissibilité et valider la cohérence de sa sélection de cours.

- **Professeur / Chargé de cours**  
  Consulte les statistiques agrégées de ses cours (résultats académiques, avis, charge de travail perçue) pour ajuster son enseignement ou préparer les sessions suivantes.

- **Administrateur du système**  
  Responsable de l’import des fichiers CSV de résultats académiques, de la configuration de la synchronisation avec Discord et de la gestion technique de la plateforme.

- **Service d’authentification de l’UdeM**  
  Service externe qui authentifie les utilisateurs (étudiants, TGDEs, professeurs, administrateurs) avant qu’ils puissent accéder à la plateforme.

- **Bot Discord**  
  Service externe qui transmet périodiquement les avis étudiants anonymisés vers la plateforme, selon les paramètres définis par l’administrateur.


### Dépendances
- Accès aux données officielles via l’API Planifium.  
- Disponibilité des résultats académiques agrégés (format CSV).  
- Participation des étudiants pour fournir des avis via Discord.  
- Outils de documentation et de modélisation (MkDocs, Git/GitHub).  

## Hypothèses et contraintes

### Hypothèses
- Les avis étudiants sont collectés uniquement avec un bot Discord.  
- Les avis ne sont affichés que si n ≥ 5.  
- Les résultats académiques sont fournis sous format CSV agrégé.  
- Pour la Phase 1, toutes les données externes sont simulées par des fichiers statiques.  


### Contraintes du domaine

- Les résultats académiques publiés par l’UdeM sont **agrégés** et anonymes et ne couvrent pas nécessairement tous les cours ni toutes les sessions.
- Les avis étudiants sont non vérifiés et vont être évidement subjectifs ce qui peut entraîner des biais ou des informations contradictoires.
- L’Université selon la Loi 25 ne peut pas diffuser aucune donnée permettant d’identifier un étudiant donc toute donnée utilisée doit être anonymisée.
- Les informations officielles (horaires, prérequis, plans de cours) sont publiées dans différentes plateformes.
- Les règles de programme (prérequis, blocs, cycles) doivent être strictement respectées pour que l’étudiant puisse s’inscrire à un cours.

