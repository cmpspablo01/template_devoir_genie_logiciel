---
title: Analyse des besoins - Risques
---

# Analyse des risques

## Identification des risques

---

## Risque 1 – Qualité variable/biaisée des avis étudiants  
**Justification :** Les avis recueillis sur Discord ou par d'autres moyens sont subjectifs et pourait contenir du spam, des doublonsou des opinions extrêmes. Une mauvaise qualité d’avis peut affecter la fiabilité du système.  

- **Probabilité :** Moyenne  
- **Impact :** Élevé  
- **Plan de mitigation :**  
  - N’afficher les statistiques que si **n ≥ 5** avis distincts.  
  - Filtrer les doublons et avis suspects.  
  - Isoler les commentaires textuels des statistiques chiffrées.  
  - Mettre un message “avis non représentatifs” au besoin.  

---

## Risque 2 – Données officielles incomplètes, obsolètes ou changeantes  
**Justification :** On sait que les cours, les horaires, les prérequis et les résultats académiques changent d’une session à l’autre. Les sources officielles qu'on utilisent pourraient ne pas être à jour ou avoir des informations éronnées. Le format des CSV peut également évoluer. On doit donc faire attention a tout ca. 

- **Probabilité :** Moyenne  
- **Impact :** Élevé  
- **Plan de mitigation :**  
  - Afficher la session associée à chaque donnée.  
  - Marquer certaines données comme “non disponibles” si nécessaire.  
  - Encapsuler Planifium et les CSV dans des composants séparés.  
  - Valider le format lors de l’importation des données.  

---

## Risque 3 – Indisponibilité ou instabilité des services externes (Planifium, Discord)  
**Justification :** Le système dépend d’une API externe (Planifium) et de collecte d’avis externe (Discord). Si il y a un problème avec un de ces deux éléments ca peut empêcher la recherche de cours, l’importation d’horaires ou la réception d’avis.  

- **Probabilité :** Moyenne  
- **Impact :** Élevé  
- **Plan de mitigation :**  
  - Utiliser des données locales simulées lorsque les services externes sont inaccessibles.  
  - Implémenter un mécanisme de cache local pour les données de cours.  
 
---

## Risque 4 – Incohérence entre les différentes sources de données  
**Justification :** Un même cours peut avoir plusieurs sources d’information (Planifium, résultats académiques qui disent une chose, avis Discord qui disent une autre). Ces sources peuvent se contredire, ce qui peut causer un mauvais conseil pour l’étudiant qui sera plus mélangé que guidé.  

- **Probabilité :** Moyenne  
- **Impact :** Moyen à Élevé  
- **Plan de mitigation :**  
  - Définir une source d’autorité par exemple les résultats abrégés sont plus importants que les opinions de discord.
  - Indiquer clairement dans l’interface la provenance des données affichées.  

---

## Risque 5 – Confusion dans l’interprétation des règlements (prérequis, cycles, admissibilité)  
**Justification :** Les règles pour être admissible à un cours peuvent êtres complexes : plusieurs prérequis, des équivalences, cycles particuliers acceptés seulement. Une mauvaise logique pourrait indiquer par erreur qu’un étudiant est admissible alors qu’il ne l’est pas (ou le contraire).  

- **Probabilité :** Moyenne  
- **Impact :** Très élevé  
- **Plan de mitigation :**  
  - Implémenter une a vérification de l’éligibilité.  
  - Tester les règles avec plusieurs cas limites et rares (prérequis multiples, exceptions).  
  - Afficher la liste exacte des raisons d’admissibilité ou. inadmissibilité.  

---

## Risque 6 – Problèmes liés à la confidentialité et à la Loi 25  
**Justification :** Le système manipule des données provenant d’étudiants. Même si elles sont agrégées, on doit faire attention a ce que aucune donnée permete l’identification d’une personne et donc on a besoin de contraintes qui s'en occupe.  

- **Probabilité :** Faible à Moyenne  
- **Impact :** Très élevé  
- **Plan de mitigation :**  
  - Ne jamais stocker d’identifiants Discord.  
  - Utiliser uniquement des données agrégées et anonymes.  
  - Documenter l’ensemble des données manipulées pour valider leur conformité.  

---

## Risque 7 – Erreurs d’agrégation ou de calcul dans les statistiques  
**Justification :** Des erreurs dans les moyennes, le calcul de difficulté ou la charge de travail pourraient fournir des informations incorrectes et induire les étudiants en erreur.  

- **Probabilité :** Moyenne  
- **Impact :** Élevé  
- **Plan de mitigation :**  
  - Valider les statistiques en comparant avce les fichiers sources.  
  - Tester les agrégations.  
 

---