---
title: Analyse des besoins - Exigences
---

# Exigences

## Exigences fonctionnelles

- [ ] **EF1 – Agrégation et affichage des avis étudiants**
  - Description : Le système agrège les avis étudiants d’un cours et n’affiche les résultats que si n ≥ 5 avis valides sont disponibles.
  - Critères d’acceptation :
    - Si un cours contient ≥ 5 avis, un bloc “Avis agrégés” (moyenne, dispersion) est affiché.
    - Si un cours contient < 5 avis, aucun avis n’est affiché et un message “Pas assez d’avis” apparaît.

- [ ] **EF2 – Personnalisation du profil étudiant**
  - Description : L’étudiant peut indiquer ses préférences (ex. théorie/pratique, centres d’intérêt).
  - Critères d’acceptation :
    - Un formulaire permet de saisir et enregistrer au moins 3 préférences.
    - Les résultats de recherche sont réordonnés selon ces préférences.
    - L’étudiant peut réinitialiser son profil à tout moment.

- [ ] **EF3 – Recherche de cours et éligibilité**
  - Description : L’étudiant peut rechercher un cours (par code, titre ou mot-clé) et voit clairement son éligibilité (prérequis, co-requis, cycle, contraintes de programme/statut).
  - Critères d’acceptation :
    - Recherche “IFT2015” retourne la fiche du cours.
    - Chaque résultat inclut un badge “Éligible / Non éligible / Incomplet”.

- [ ] **EF4 – Comparaison multi-cours**
  - Description : L’étudiant peut comparer plusieurs cours et estimer la charge totale de travail d’une combinaison.
  - Critères d’acceptation :
    - Ajout de 2 cours ou plus → affichage d’un résumé de charge totale (heures/semaine estimées).
    - Le comparateur permet de retirer un cours déjà ajouté.

- [ ] **EF5 – Résultats académiques agrégés**
  - Description : Pour chaque cours, le système affiche la moyenne finale, le nombre d’inscrits et le nombre d’échecs (si disponibles).
  - Critères d’acceptation :
    - Bloc “Résultats académiques” visible si données présentes.
    - Si aucune donnée n’est disponible, un message explicite s’affiche.

---

## Exigences non fonctionnelles

- [ ] **ENF1 – Conformité légale**
  - Le système respecte la Loi 25 (protection des données).
  - Vérification : données anonymisées, accès journalisé, consentement explicite.

- [ ] **ENF2 – Accessibilité et clarté**
  - Les informations sont présentées de manière claire et utile, avec une interface simple.
  - Vérification : test utilisateur de base.

- [ ] **ENF3 – Fiabilité des données**
  - Les agrégations sont cohérentes et exactes.
  - Vérification : tests avec jeux de données simulés.

- [ ] **ENF4 – Performance minimale**
  - Les recherches retournent un résultat en moins de 2 secondes (prototype local).
  - Vérification : test manuel.

- [ ] **ENF5 – Maintenabilité**
  - Le projet doit être documenté (README, structure claire).
  - Vérification : relecture par un pair, existence de documentation minimale.

---

## Priorisation

- **Critique** : EF1 (avis), EF3 (recherche + éligibilité), EF5 (résultats académiques)  
- **Important** : EF2 (profil), EF4 (comparaison)  
- **Souhaitable** : optimisations de performance (ENF4), accessibilité avancée (ENF2)  

## Types d'utilisateurs

> Identifier les différents profils qui interagiront avec le système.

| Type d’utilisateur      | Description                               | Exemples de fonctionnalités accessibles |
|--------------------------|-------------------------------------------|------------------------------------------|
| Étudiant invité          | Consultation simple sans profil           | Recherche de cours, voir fiche de base   |
| Étudiant authentifié     | Profil personnel enregistré               | Personnalisation, comparaison, avis      |
| Administrateur           | Gestion avancée (Phase 1 limitée à l’import) | Import CSV, validation des avis, configuration |
<!-- TODO: Détailler selon le périmètre du projet. -->


## Infrastructures

- **Phase 1** : Prototype local avec MkDocs (site statique).  
- **Données** : fichiers CSV (résultats académiques simulés), JSON (avis étudiants simulés).  
- **Intégrations prévues (Phase 2)** : API Planifium, Bot Discord.  
- **Hébergement cible** : GitHub Pages ou serveur interne UdeM. 