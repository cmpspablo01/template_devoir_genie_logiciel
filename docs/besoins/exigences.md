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
  - Description : L’étudiant peut indiquer ses préférences (ex théorie/pratique et ses centres d’intérêts).
  - Critères d’acceptation :
    - Un formulaire permet de saisir et enregistrer au moins 3 préférences.
    - Les résultats de recherche sont réordonnés selon ces préférences.
    - L’étudiant peut réinitialiser son profil à tout moment.

- [ ] **EF3 – Recherche de cours et éligibilité**
  - Description : L’étudiant peut rechercher un cours par code, titre du cours ou mot-clé et voir son éligibilité (prérequis, cycle, contraintes de programme).
  - Critères d’acceptation :
    - Recherche de “IFT2015” retourne la fiche du cours.
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

- [ ] **EF6 – Authentification et rôle admin**
  - Description : Connexion avec un profil valide d'étudiant; on peut se connecter avec le rôle Admin pour l’import des csv.
  - Critères d’acceptation :
    - Étudiant : profil et préférences enregistrés.
    - Admin : accès à l’import csv pour les résultas agrégés des cours.

---

# Exigences non fonctionnelles

---

## ENF1 – Conformité légale (Loi 25)

- Système ne doit jamais stocker de données qui permetrait d'identifier des étudiants (identifiant Discord, nom, courriel).  
- Les fichiers CSV et JSON qu'on utilisent doivent contenir uniquement des données agrégées et anonymes.  
- les données qui viennet d’une source externe doivent être vérifiée pour s’assurer qu’elle ne permetent pas d’identifier une personne.  
- **Vérification :** inspection manuelle des structures de données + revue de sécurité interne.

---

## ENF2 – Accessibilité et clarté

- L’interface doit permettre d’accéder aux informations essentielles d’un cours  
  (accéder aux **horaires**, **résultats académiques**, **avis étudiants**) en moins de 3 actions donc rapide et efficace.  
- Les blocs d’infos doivent être clairement labelled :  
  - “Résultats académiques”  
  - “Avis étudiants”  
  - “Éligibilité”  
- Les messages d’erreur qu'on met (ex. “Pas assez d’avis”, “Aucune donnée disponible”) doivent être claires et facil a comprendre.  
- **Vérification :** on va faire des tests utilisateurs pour vérifier tout ca  

---

## ENF3 – Fiabilité et exactitude des données

- Les statistiques calculées (moyenne, difficulté, charge de travail) doivent être proche aux données sources avec une incertitude maximale de ±0,1.  
- On recalcule automatiquement à chaque ajout d’avis ou de note les moyennes.  
- Le système doit indiquer clairement de quelle session les données affichées proviennent pour éviter les confusions.  
- **Vérification :** comparaison systématique entre données sources (CSV/JSON) et données affichées.

---

## ENF4 – Performance minimale

- Une recherche de cours sur un jeu de données contenant jusqu’à 1 000 cours par exemple doit retourner une réponse en moins de 2 secondes pour que ce soit efficace et rapide.  
- Le chargement du tableau de bord d’un cours incluant résultats t avis doit s’effectuer en 1 seconde si les données sont locales.  
- **Vérification :** mesures avec des tests manuels.

---

## ENF5 – Interopérabilité

- Toutes les fonctionnalités doivent être faites via une API REST utilisant le format JSON.  
- La structure des données doit être compatible avec une future interface web et un bot Discord.  
- Les URL doivent suivre un format standard (`/courses/{code}`, `/reviews`, `/compare`).  
- **Vérification :** tests d’appel API via curl / Postman.

---

## ENF6 – Maintenabilité et évolutivité

- Le code doit être organisé avec une architecture claire (ex. MVC : contrôleurs, services, stockage).  
- Le système doit permettre le remplacement plus tarde si nécessaire :  
  - de la source des cours (Planifium)  
  - du stockage des avis  
  - de la méthode d’authentification  
- **Vérification :** revue de code + documentation dans le README.

---

# Infrastructures

Section qui décrit les choix techniques liés au stockage, à l’hébergement et à l’évolution du système. 

---

## Phase 1 – Prototype local

- Le système fonctionne entièrement en local.  
- Les données sont stockées dans :  
  - Un csv pour les résultats académiques simulés  
  - JSON pour les avis étudiants  
- L’ensemble du projet est présenté avec MkDocs  
- Le site de documentation est hébergé via GitHub Pages.

---

## Phase 2 – Intégration externe (prévue)

- **Planifium API** ca va être la source officielle de tous les cours de l'UdeM et leurs horaires.  
- Un **bot Discord** sera capable d’envoyer automatiquement les avis au backend.  
- Le stockage évoluera vers une base de données relationnelle comme PostgreSQL ou SQLite afin de gérer plus de données.

---

## Hébergement et déploiement envisagés

- Le backend pourrait être déployé à l'interne sur un serveur de UdeM ou une machine virtuelle de l’équipe.  
- Le déploiement qu'on fera devra utiliser HTTPS pour sécuriser les échanges de données.  
- Les endpoints sensibles ont leur mettra une **authentification admin** pour que pas toute le monde puisse jouer avec.  
- La documentation restera sur GitHub pour faciliter l’accès à l’équipe et aux auxiliaires.

---

## Sécurité

- L’ensemble des communications externes avec Planifium, Discord vers le  backend devront utiliser **HTTPS**.  
- Les accès admins devront être protégés par d'authentification simple comme un mdp.  
- Les fichiers CSV importés devront être vérifiés pour empêcher l’injection de contenu dangereux .
