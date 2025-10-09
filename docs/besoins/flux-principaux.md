---
title: Analyse des besoins - Flux principaux
---

# Flux principaux

## Objectif
Décrire les flux d’interaction entre les acteurs et le système.

## Diagrammes
### Flux d'activité : Recherche de cours

Voici le diagramme d'activité UML représentant le processus de recherche de cours :

![Diagramme de recherche de cours](Diagramme_activite.png)


### Description des flux complexes

### Flux : Ouverture de la plateforme

L'étudiant fait le choix de cours et entre dans la palteforme. Le système redirige l’étudiant vers la page d’accueil personnalisée.

---

### Flux : Affichage des cours obligatoires

Lorsque l’étudiant accède à son tableau de cheminement personnel, le système identifie automatiquement son programme d’études (ex. Baccalauréat en informatique) grâce à l’authentification. Ensuite, il interroge une base de données institutionnelle ou un fichier de règles programmé (ex. table de correspondance programme → cours obligatoires), indépendamment de l’API Planifium, pour récupérer la liste des cours obligatoires et des préalables requis.Les cours sont d’abord affichés avec les informations de base, telles que la description du cours, le nom du professeur, l'horaire et des commentaires généraux.Chaque cours est accompagné d’un bouton “Détails” permettant à l’étudiant de consulter des indicateurs approfondis, incluant la charge de travail moyenne, le taux d'échec et l'évaluation globale donnée par les étudiants. À partir de cette fiche détaillée, l’étudiant peut comparer plusieurs cours obligatoires entre eux via la vue comparative intégrée. Il peut ainsi identifier les cours présentant un risque élevé ou une charge importante, et adapter sa sélection en fonction de ses préférences.

---

### Flux : Recherche de cours

Si l’étudiant souhaite ajouter un cours hors programme, il peut utiliser la fonction de recherche à partir d’un mot-clé (sigle ou nom). Le système interroge l’API Planifium et affiche les cours correspondants avec les informations de base comme le nom, l'horaire, le professeur ou la description. Ensuite, en cliquant sur un cours spécifique, l’étudiant accède aux informations détaillées (évaluation, charge, taux d’échec, commentaires). Comme pour les cours obligatoires, une vue comparative est accessible après cette étape pour aider à la prise de décision.

---

### Flux : Personnalisation

Si l’étudiant a rempli son profil (préférence pour les cours pratiques, intérêt en IA, etc.), le système classe les résultats de recherche en fonction de ces préférences, grâce à un module de recommandation. Le système affiche le nom du cours sur l’interface. Lorsqu’un étudiant est intéressé, il peut cliquer pour consulter les détails. Ce mécanisme d’interaction simple et intuitif facilite l’identification rapide des cours pertinents selon les préférences individuelles. Le module apprend des interactions passées pour améliorer les recommandations.

---

### Flux : Comparaison

L’étudiant peut sélectionner plusieurs cours (ex: IFT2255, IFT2035) et accéder à une vue comparative. Cette fonctionnalité est disponible à partir de la fiche détaillée d’un cours (après avoir consulté l’évaluation, la charge de travail et le taux d’échec) ou depuis la vue du panier avant la validation finale.  Le système génère alors un tableau croisé comparant les éléments suivants : charge horaire, moyenne d'évaluation, taux d'échec et commentaires anonymes d'étudiants. En parallèle, le système interroge l’API Planifium pour récupérer les horaires des cours sélectionnés (jour, heure, groupe, salle, session). Une vue de type calendrier (similaire à Google Agenda) permet à l’étudiant d’identifier les conflits, chevauchements ou plages libres, facilitant la construction d’un horaire optimal. Enfin, les commentaires qualitatifs d’anciens étudiants sont affichés en bas de l’interface comparative. Ceux-ci offrent des informations utiles sur le style d’enseignement, la difficulté perçue, ou l’organisation des cours. Lorsque plusieurs commentaires partagent un contenu similaire, ils sont regroupés automatiquement pour éviter la redondance. L’étudiant peut cliquer pour les développer individuellement s’il souhaite lire le détail. Cette combinaison d’indicateurs quantitatifs et qualitatifs permet à l’étudiant de faire un choix éclairé, en fonction de ses préférences personnelles et contraintes académiques.


---


