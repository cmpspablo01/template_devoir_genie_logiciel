---
title: Analyse des besoins - Risques
---

# Analyse des risques

## Identification des risques

---

## Risque 1 – Qualité variable/biaisée des avis étudiants  
**Justification :** Les avis recueillis sur Discord ou par d'autres moyens sont subjectifs et pouraient contenir du spam, des doublons ou des opinions extrêmes. Une mauvaise qualité d’avis peut affecter la fiabilité du système.  

- **Probabilité :** Moyenne  
- **Impact :** Élevé  
- **Plan de mitigation :**  
  - N’afficher les statistiques que si **n ≥ 5** avis distincts.  
  - Filtrer les doublons et avis suspects.  
  - Isoler les commentaires textuels des statistiques chiffrées.  
  - Mettre un message “avis non représentatifs” au besoin.  

---

## Risque 2 – Données officielles incomplètes, obsolètes ou changeantes  
**Justification :** On sait que les cours, les horaires, les prérequis et les résultats académiques changent d’une session à l’autre. Les sources officielles qu'on utilise pourraient ne pas être à jour ou avoir des informations erronées. Le format des CSV peut également évoluer.  

- **Probabilité :** Moyenne  
- **Impact :** Élevé  
- **Plan de mitigation :**  
  - Afficher la session associée à chaque donnée.  
  - Marquer certaines données comme “non disponibles” si nécessaire.  
  - Encapsuler Planifium et les CSV dans des composants séparés.  
  - Valider le format lors de l’importation des données.  

---

## Risque 3 – Indisponibilité ou instabilité des services externes (Planifium, Discord)  
**Justification :** Le système dépend d’une API externe (Planifium) et de la collecte d’avis via Discord. Une indisponibilité temporaire de ces services peut empêcher la recherche de cours, la récupération des horaires ou la réception d’avis.  

- **Probabilité :** Moyenne  
- **Impact :** Élevé  
- **Plan de mitigation :**  
  - Implémenter mécanisme de mise en cache local temporaire pour les données Planifium.  
  - Détecter l’erreur et afficher un message clair à l’étudiant (“Service temporairement indisponible”).  
  - Permettre la continuité minimale avec les données déjà chargées.  

## Risque 4 – Mauvaise intégration ou fusion des données dans le système  
**Justification :** Même si les différentes sources de données ne sont pas contradictoires entre elles, leur intégration dans notre app peut introduire des erreurs. Par exemple, afficher des résultats académiques d’une mauvaise session, associer par erreur des avis au mauvais cours. Ce type de mauvaise fusion de l’information pourrait mener l’étudiant à mal interpréter les données et prendre de mauvaises décisions.  

- **Probabilité :** Moyenne  
- **Impact :** Moyen à Élevé  
- **Plan de mitigation :**  
  - Vérifier systématiquement l’association cours/session avant d’afficher une donnée.  
  - Normaliser les formats d’affichage pour éviter les confusions.  
  


## Risque 5 – Confusion dans l’interprétation des règlements (prérequis, cycles, admissibilité)  
**Justification :** Les règles pour être admissible à un cours peuvent être complexes (prérequis multiples, équivalences, cycles particuliers). Le risque provient d’une mauvaise implémentation du moteur d’admissibilité.  

- **Probabilité :** Faible  
- **Impact :** Très élevé  
- **Plan de mitigation :**  
  - Implémenter une vérification d’éligibilité centralisée et bien définie.  
  - Tester les règles avec plusieurs cas limites (prérequis multiples, exceptions).  
  - Afficher la liste exacte des raisons d’admissibilité ou d’inadmissibilité.  

---

## Risque 6 – Problèmes liés à la confidentialité et à la Loi 25  
**Justification :** Le système manipule des données provenant d’étudiants. Même si elles sont agrégées, on doit faire attention à ce qu’aucune donnée ne permette l’identification d’une personne.  

- **Probabilité :** Faible à Moyenne  
- **Impact :** Très élevé  
- **Plan de mitigation :**  
  - Ne jamais stocker d’identifiants Discord.  
  - Utiliser uniquement des données agrégées et anonymes.  
  - Documenter l’ensemble des données manipulées pour valider leur conformité.  

---

## Risque 7 – Erreurs d’agrégation ou de calcul dans les statistiques  
**Justification :** Des erreurs dans les moyennes, le calcul de difficulté ou la charge de travail pourraient fournir des informations incorrectes et induire les étudiants en erreur.  

- **Probabilité :** Faible  
- **Impact :** Élevé  
- **Plan de mitigation :**  
  - Valider les statistiques en comparant avec les fichiers sources.  
  - Tester les fonctions d’agrégation.  

