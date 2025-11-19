---
title: Analyse des besoins - Cas d'utilisation
---

# Cas d'utilisation

## Vue d’ensemble

Les cas d’utilisation décrivent les interactions principales entre les différents acteurs  
(étudiants, TGDEs, professeurs, administrateur) et la plateforme **OptiCoursUdeM**.  
Ils traduisent les besoins en fonctionnalités concrètes, du point de vue des utilisateurs.

## Identification des acteurs

- **Étudiant** : utilisateur principal qui recherche des cours, consulte les avis et résultats, compare des cours et personnalise son profil.
- **TGDE (Technicien·ne en gestion des dossiers étudiants)** : utilise le système pour vérifier le cheminement d’un étudiant et valider ses choix de cours.
- **Professeur / Chargé de cours** : consulte les statistiques agrégées de ses propres cours (résultats académiques, taux d’échec, avis étudiants).
- **Administrateur du système** : gère l’import des données académiques et la configuration de la synchronisation des avis Discord.
- **Service d’authentification de l’UdeM** : service externe qui authentifie les utilisateurs avant qu’ils accèdent à la plateforme.
- **Bot Discord** : transmet les avis étudiants anonymisés vers le backend (processus technique, pas un acteur humain initiant un cas d’utilisation).

## Liste des cas d’utilisation

| ID   | Nom                                                | Acteurs principaux                              | Description |
|------|----------------------------------------------------|-------------------------------------------------|------------|
| CU01 | Se connecter à la plateforme                       | Étudiant, TGDE, Professeur, Administrateur      | Permet à un utilisateur de s’authentifier via le service d’authentification de l’UdeM. |
| CU02 | Gérer le profil étudiant                           | Étudiant                                        | Permet à l’étudiant de renseigner ou modifier ses préférences (théorie/pratique, intérêts, charge souhaitée). |
| CU03 | Rechercher un cours                                | Étudiant, TGDE                                  | Permet de trouver des cours selon différents critères (code, titre, mot-clé, cycle, session). |
| CU04 | Consulter la fiche détaillée d’un cours            | Étudiant, TGDE, Professeur                      | Affiche toutes les informations d’un cours (description, horaires, professeur, prérequis, éligibilité). |
| CU05 | Consulter les avis étudiants                       | Étudiant, TGDE, Professeur                      | Affiche les avis agrégés d’étudiants pour un cours, si le nombre d’avis valides est suffisant. |
| CU06 | Consulter les résultats académiques d’un cours     | Étudiant, TGDE, Professeur                      | Affiche les statistiques académiques agrégées (moyenne, nombre d’inscrits, échecs) pour un cours. |
| CU07 | Comparer plusieurs cours                           | Étudiant                                        | Permet de comparer plusieurs cours selon la charge de travail, les avis et les résultats académiques. |
| CU08 | Consulter le cheminement et les cours obligatoires | Étudiant, TGDE                                  | Permet de visualiser les cours obligatoires, complétés ou non, et ceux restant à suivre. |
| CU09 | Importer les résultats académiques                 | Administrateur                                  | Permet à l’administrateur d’importer un fichier CSV de résultats académiques agrégés dans la plateforme. |



## Diagramme de cas d’utilisation

![Diagramme des cas d'utilisation](![alt text](<cas d utilisateur diageramme.png>))

---

## Détail des cas d’utilisation

### CU01 – Se connecter à la plateforme

**But** : Permettre à tout utilisateur autorisé (étudiant, TGDE, professeur, administrateur) d’accéder à ses fonctionnalités.  

**Acteurs principaux** : Étudiant, TGDE, Professeur, Administrateur  
**Acteur secondaire** : Service d’authentification de l’UdeM  

**Préconditions** :  
- L’utilisateur possède un compte UdeM valide.  
- La plateforme est accessible.  

**Postconditions** :  
- L’utilisateur est authentifié et redirigé vers son tableau de bord (étudiant) ou son écran d’accueil (TGDE, professeur, administrateur).



#### Scénario principal

1. L’utilisateur accède à la page de connexion d’OptiCoursUdeM.  
2. L’utilisateur saisit son identifiant UdeM et son mot de passe.  
3. Le système envoie les informations saisies au **service d’authentification de l’UdeM**.  
4. Le service d’authentification valide les informations et renvoie une réponse de succès.  
5. Le système crée une session utilisateur et récupère son rôle (étudiant, TGDE, professeur, administrateur).  
6. Le système affiche le tableau de bord correspondant au rôle de l’utilisateur.

#### Scénarios alternatifs

3a. Les identifiants sont incorrects.  
  3a.1. Le service d’authentification renvoie un échec.  
  3a.2. Le système affiche un message du type : « Identifiant ou mot de passe invalide ».  
  3a.3. L’utilisateur peut saisir à nouveau ses identifiants ou choisir « Mot de passe oublié ».  

3b. Le compte est suspendu.  
  3b.1. Le service d’authentification signale que le compte est suspendu.  
  3b.2. Le système affiche : « Votre compte est suspendu, veuillez contacter le support ».  
  3b.3. L’utilisateur ne peut pas accéder aux autres fonctionnalités.  

3c. Erreur technique (service d’authentification indisponible).  
  3c.1. Le système détecte l’indisponibilité du service externe.  
  3c.2. Le système affiche : « Service d’authentification momentanément indisponible, veuillez réessayer plus tard ».  

---

### CU02 – Gérer le profil étudiant

**But** : Permettre à l’étudiant de configurer ses préférences pour personnaliser les recherches et recommandations de cours.  

**Acteurs principaux** : Étudiant  

**Préconditions** :  
- CU01 – Se connecter (inclus).  

**Postconditions** :  
- Les préférences sont sauvegardées et seront utilisées lors des recherches et comparaisons.  

#### Scénario principal

1. L’étudiant ouvre la page « Mon profil » depuis le tableau de bord.  
2. Le système affiche le formulaire de préférences actuel (théorie/pratique, domaines d’intérêt, charge de travail souhaitée, plages horaires préférées, etc.).  
3. L’étudiant modifie ou renseigne ses préférences.  
4. L’étudiant clique sur « Enregistrer ».  
5. Le système valide les données (format, champs obligatoires).  
6. Le système sauvegarde les préférences dans la base de données.  
7. Le système affiche un message de confirmation : « Vos préférences ont été mises à jour ».  

#### Scénarios alternatifs

5a. Certains champs facultatifs sont laissés vides.  
  5a.1. Le système complète ces champs avec des **valeurs par défaut** (ex. « charge de travail moyenne »).  
  5a.2. Le système sauvegarde les préférences avec ces valeurs par défaut.  

5b. Une erreur technique survient lors de la sauvegarde.  
  5b.1. Le système n’arrive pas à enregistrer les données.  
  5b.2. Le système affiche : « Erreur lors de l’enregistrement de vos préférences. Veuillez réessayer ».  

---

### CU03 – Rechercher un cours

**But** : Permettre à un étudiant ou un TGDE de trouver des cours à partir de différents critères.  

**Acteurs principaux** : Étudiant, TGDE  
**Acteurs secondaires** : API Planifium, base de données interne (résultats académiques, avis)  

**Préconditions** :  
- CU01 – Se connecter (inclus).  

**Postconditions** :  
- Une liste de cours correspondant aux critères de recherche est affichée avec les informations principales et un badge d’éligibilité.  



#### Scénario principal

1. L’utilisateur accède à la page « Rechercher un cours ».  
2. Le système affiche un formulaire de recherche (champ code/titre/mot-clé + filtres : session, cycle, charge estimée, domaine).  
3. L’utilisateur saisit un critère (ex. « IFT2255 » ou « apprentissage ») et sélectionne éventuellement des filtres.  
4. L’utilisateur clique sur « Rechercher ».  
5. Le système interroge l’API Planifium pour récupérer la liste des cours correspondants (code, titre, horaires, session, professeur).  
6. Le système interroge la base de données interne pour associer, à chaque cours, les statistiques agrégées (résultats académiques, nombre d’avis valides).  
7. Le système calcule, pour chaque cours, l’éligibilité de l’étudiant (prérequis, corequis, cycle, contraintes de programme).  
8. Le système affiche la liste de résultats, chaque ligne contenant au minimum :  
   - code, titre, session, professeur ;  
   - badge d’éligibilité (« Éligible », « Non éligible », « Prérequis à compléter ») ;  
   - indicateurs de base (charge estimée, nombre d’avis valides, présence ou non de résultats académiques).  

#### Scénarios alternatifs

5a. Aucune donnée trouvée.  
  5a.1. L’API Planifium ne retourne aucun cours correspondant.  
  5a.2. Le système affiche : « Aucun cours trouvé pour ces critères ».  
  5a.3. Le système propose à l’utilisateur de :  
        - élargir ses filtres (ex. autre session, retirer certains critères) ;  
        - revenir au formulaire de recherche.  

6a. Données partielles ou manquantes pour certains cours.  
  6a.1. Le système détecte que :  
        - il y a **moins de 5 avis valides** pour un cours ; et/ou  
        - aucun résultat académique n’est disponible pour la session sélectionnée.  
  6a.2. Le système affiche un **pictogramme d’avertissement** à côté du cours et un texte explicite, par exemple :  
        - « Avis étudiants insuffisants (moins de 5 avis valides) » ;  
        - « Aucune donnée académique disponible pour cette session ».  
  6a.3. L’utilisateur peut quand même ouvrir la fiche détaillée du cours (CU04) pour voir les informations disponibles.  

---

### CU04 – Consulter la fiche détaillée d’un cours

**But** : Permettre de voir l’ensemble des informations détaillées sur un cours.  

**Acteurs principaux** : Étudiant, TGDE, Professeur  
**Acteurs secondaires** : API Planifium, base de données interne  

**Préconditions** :  
- CU03 – Rechercher un cours (étendu).  

**Postconditions** :  
- L’utilisateur visualise la fiche complète du cours et peut ensuite lancer CU05, CU06 ou CU07.  



#### Scénario principal

1. Depuis la liste de résultats de CU03, l’utilisateur clique sur un cours.  
2. Le système récupère les détails du cours (description, objectifs, nombre de crédits, langue, session, groupe, salle, horaire, professeur).  
3. Le système calcule ou affiche l’éligibilité pour l’étudiant sélectionné (si acteur = étudiant ou TGDE).  
4. Le système affiche une fiche structurée en sections :  
   - Informations générales (code, titre, crédits, description, professeur) ;  
   - Pré-requis / co-requis ;  
   - Horaire et groupes ;  
   - Liens vers « Avis étudiants », « Résultats académiques », « Ajouter à la comparaison ».  

#### Scénarios alternatifs

2a. Une erreur survient lors de la récupération des informations détaillées.  
  2a.1. Le système affiche : « Impossible de charger la fiche détaillée de ce cours pour le moment ».  
  2a.2. L’utilisateur peut revenir à la liste des résultats.  

---

### CU05 – Consulter les avis étudiants

**But** : Afficher les avis agrégés des étudiants sur un cours.  

**Acteurs principaux** : Étudiant, TGDE, Professeur  
**Acteurs secondaires** : Base de données interne  

**Préconditions** :  
- CU04 – Consulter la fiche détaillée d’un cours (étendu).  

**Postconditions** :  
- L’utilisateur visualise les indicateurs agrégés (note globale, difficulté perçue, charge estimée) et, si nécessaire, un message expliquant les limites des données.  

**Relations avec les autres cas d’utilisation** :  
- **Étend** (`<<extend>>`) CU04 – Consulter la fiche détaillée d’un cours.  

#### Scénario principal

1. Depuis la fiche d’un cours, l’utilisateur clique sur l’onglet ou le bouton « Avis étudiants ».  
2. Le système vérifie le nombre d’avis **valides** disponibles pour ce cours.  
3. Si le nombre d’avis valides est **≥ 5**, le système calcule les agrégats (moyenne de satisfaction, perception de la difficulté, charge de travail estimée).  
4. Le système affiche :  
   - les indicateurs agrégés ;  
   - éventuellement quelques exemples de commentaires anonymisés ;  
   - la session ou la période à laquelle les avis se rapportent.  

#### Scénarios alternatifs

2a. Nombre d’avis valides < 5.  
  2a.1. Le système n’affiche **aucun indicateur agrégé**, pour éviter des données non représentatives.  
  2a.2. Le système affiche un message clair, par exemple :  
        « Avis étudiants insuffisants (moins de 5 avis valides) pour ce cours ».  

2b. Aucun avis disponible.  
  2b.1. Le système affiche : « Aucun avis étudiant disponible pour ce cours ».  

---

### CU06 – Consulter les résultats académiques d’un cours

**But** : Afficher les statistiques académiques agrégées d’un cours.  

**Acteurs principaux** : Étudiant, TGDE, Professeur  
**Acteurs secondaires** : Base de données interne (fichiers CSV importés)  

**Préconditions** :  
- CU04 – Consulter la fiche détaillée d’un cours (étendu).  

**Postconditions** :  
- L’utilisateur voit les statistiques académiques pertinentes pour le cours et la session choisie.  

 

#### Scénario principal

1. Depuis la fiche d’un cours, l’utilisateur clique sur « Résultats académiques ».  
2. Le système recherche dans les données importées (CSV) les résultats pour ce cours et la session sélectionnée.  
3. Le système calcule, si nécessaire, la moyenne, le nombre d’inscrits et le nombre d’échecs.  
4. Le système affiche un tableau récapitulatif avec : session, moyenne, nombre d’inscrits, nombre d’échecs et éventuellement un commentaire sur la cohorte.  

#### Scénarios alternatifs

2a. Aucune donnée académique pour ce cours et cette session.  
  2a.1. Le système affiche :  
        « Aucune donnée académique disponible pour ce cours à la session [H2024] ».  
  2a.2. Le système propose, si possible, de consulter une session antérieure disposant de données.  

---

### CU07 – Comparer plusieurs cours

**But** : Permettre à l’étudiant de comparer plusieurs cours pour estimer la charge globale et choisir les plus pertinents.  

**Acteurs principaux** : Étudiant  

**Préconditions** :  
- CU03 – Rechercher un cours (étendu) et avoir sélectionné au moins deux cours.  

**Postconditions** :  
- Un tableau comparatif est affiché ; l’étudiant peut ajuster sa sélection.  



#### Scénario principal

1. Depuis la liste de résultats ou une fiche de cours, l’étudiant clique sur « Ajouter à la comparaison » pour plusieurs cours.  
2. Le système maintient une liste des cours sélectionnés pour la comparaison.  
3. L’étudiant clique sur « Voir comparaison ».  
4. Le système récupère, pour chaque cours sélectionné :  
   - nombre de crédits et charge de travail estimée ;  
   - statut d’éligibilité ;  
   - indicateurs d’avis (si disponibles) ;  
   - résultats académiques (si disponibles).  
5. Le système affiche un **tableau comparatif** (une ligne par cours, une colonne par critère).  
6. Le système calcule et affiche la **charge totale de travail estimée** si l’étudiant suivait tous les cours sélectionnés.  

#### Scénarios alternatifs

2a. Un cours sélectionné ne possède ni avis ni résultats académiques.  
  2a.1. Le système affiche dans les colonnes concernées :  
        « Aucune donnée disponible ».  
  2a.2. Le système met en évidence cette absence avec un pictogramme d’avertissement.  

1b. L’étudiant retire un cours de la comparaison.  
  1b.1. L’étudiant clique sur « Retirer » à côté du cours dans le tableau.  
  1b.2. Le système met à jour le tableau et recalcule la charge totale.  

---

### CU08 – Consulter le cheminement et les cours obligatoires

**But** : Permettre à l’étudiant (ou au TGDE) de visualiser le cheminement d’un programme et l’état d’avancement (cours complétés, en cours, restants).  

**Acteurs principaux** : Étudiant, TGDE  
**Acteurs secondaires** : Base de données des programmes et cheminements  

**Préconditions** :  
- CU01 – Se connecter (inclus).  
- Le programme de l’étudiant est connu (profil ou données institutionnelles).  

**Postconditions** :  
- L’utilisateur voit la liste des cours obligatoires et optionnels, avec leur statut (complété, en cours, à faire).  



#### Scénario principal

1. Depuis le tableau de bord, l’étudiant ou le TGDE clique sur « Mon cheminement » ou « Cheminement de l’étudiant ».  
2. Le système récupère les informations de programme associées à l’étudiant (blocs de cours, crédits, contraintes).  
3. Le système récupère la liste des cours déjà réussis, en cours et échoués.  
4. Le système calcule, pour chaque bloc ou catégorie (ex. cours obligatoires, optionnels, hors programme), ce qui est complété et ce qui reste à faire.  
5. Le système affiche une vue structurée du cheminement :  
   - cours complétés (avec session et note) ;  
   - cours en cours ;  
   - cours restants avec prérequis et suggestions d’inscription.  

#### Scénarios alternatifs

2a. Le programme de l’étudiant est inconnu ou incomplet.  
  2a.1. Le système affiche : « Impossible de déterminer votre programme. Veuillez contacter un TGDE ».  
  2a.2. L’utilisateur ne peut pas accéder à la vue de cheminement.

3a. Certaines données de résultats sont manquantes.  
  3a.1. Le système marque certains cours comme « statut inconnu ».  
  3a.2. Le système affiche un message discret :  
        « Certaines informations de résultats sont manquantes, veuillez vérifier auprès du Centre étudiant ».  

---

### CU09 – Importer les résultats académiques

**But** : Permettre à l’administrateur de charger un fichier CSV de résultats académiques agrégés dans le système.  

**Acteurs principaux** : Administrateur  
**Acteurs secondaires** : Base de données interne  

**Préconditions** :  
- CU01 – Se connecter (inclus).  
- L’administrateur dispose d’un fichier CSV conforme au format attendu (données agrégées, anonymes).  

**Postconditions** :  
- Les nouvelles données académiques sont intégrées et disponibles pour CU06 et CU07.  



#### Scénario principal

1. L’administrateur ouvre la page « Importer les résultats académiques ».  
2. Le système affiche un formulaire permettant de téléverser un fichier CSV et décrit le format attendu.  
3. L’administrateur sélectionne le fichier CSV sur son ordinateur et clique sur « Importer ».  
4. Le système vérifie la structure du fichier (en-têtes, types de colonnes, anonymisation).  
5. Si la structure est valide, le système insère ou met à jour les données dans la base interne.  
6. Le système affiche un récapitulatif : nombre de cours mis à jour, nombre de lignes ignorées, etc.  

#### Scénarios alternatifs

4a. Le fichier ne respecte pas le format attendu.  
  4a.1. Le système interrompt l’import.  
  4a.2. Le système affiche un message explicite, par exemple :  
        « Format de fichier invalide : colonne “moyenne” manquante ».  

4b. Le fichier contient des données non agrégées ou identifiantes.  
  4b.1. Le système détecte la présence de colonnes interdites (ex. numéro étudiant, nom).  
  4b.2. Le système refuse l’import et affiche :  
        « Import refusé : les données doivent être agrégées et anonymes (Loi 25) ».  
