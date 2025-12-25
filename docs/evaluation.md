---
title: Évaluation et tests
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Tests et évaluation

## Plan de test (Pour devoir 2)

- Types de tests réalisés :
  - **Tests unitaires** sur la couche service (`CourseService`, `UserService`)
  - **Tests manuels** avec l’API REST et la CLI
  - **Tests d’intégration**
- Outils utilisés :
  - **JUnit 5** pour les tests unitaires
  - **Maven** pour l’exécution automatique des tests (`mvn test`)
  - **Javalin** pour exposer l’API REST
  - **CLI Java** pour valider les scénarios utilisateur en ligne de commande

---
## Oracle de tests 

### 1. `testGetAllCourses_retourneListeDeCoursQuandApiOk`

- **Cas d’utilisation lié :**  
  CU *Recherche de cours* (`getAllCourses`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` est set up de facon a ce qu'on renvoie deux cours :
    - `IFT1015 – Programmation 1`
    - `IFT2035 – Concepts des langages de programmation`
  - Paramètres de recherche : `name = "programmation"`.

- **Résultat attendu :**
  - La liste retournée n’est pas nulle.
  - La liste contient exactement 2 cours
  - Le premier cours a l’ID `IFT1015`
  - Le 2e cours a l’ID `IFT2035`

- **Effets de bord attendus :**
  - Aucun effet de bord   
  - La méthode ne fait qu’appeler le client HTTP et retourner la liste.

---

### 2. `testGetAllCourses_retourneListeVideQuandApiVide`

- **Cas d’utilisation lié :**  
  CU *Recherche de cours* (`getAllCourses`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` renvoie une liste vide de cours
  - Paramètres de recherche : map vide

- **Résultat attendu :**
  - La liste retournée n’est pas nulle
  - La liste est vide (`isEmpty() == true`).

- **Effets de bord attendus :**
  - Aucun effet de bord.
  - Vérifie que le service gère correctement le cas ou on a “0 résultats” sans erreur

---

### 3. `testGetCourseById_retourneCoursQuandTrouve`

- **Cas d’utilisation lié :**  
  CU *Voir les détails d’un cours* (`getCourseById`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` renvoie un cours :
    - ID `IFT2035`
    - nom `"Concepts des langages de programmation"`
    - crédits `3.0`
    - texte de prérequis `"Préalable : IFT1025"`
  - Appel de `getCourseById("IFT2035")`.

- **Résultat attendu :**
  - L’`Optional` retourné est présent (`isPresent()`).
  - L’ID du cours est `IFT2035`.
  - Le nombre de crédits est `3.0`.
  - Le texte de prérequis est `"Préalable : IFT1025"`.

- **Effets de bord attendus :**
  - Aucun effet de bord.
  - Vérifie que le service donne bien toutes les infos d’un cours quand l’API répond bien 

---

### 4. `testCompareCourses_retourneListeVideQuandListeIdsNulleOuVide`

- **Cas d’utilisation lié :**  
  CU *Comparer des cours* (`compareCourses`)

- **Arguments / contexte :**
  - Deux appels successifs :
    1. `compareCourses(null)`
    2. `compareCourses(List.of())`
  - Aucun besoin de configurer le `FakeHttpClientApi` : la méthode doit shutdiwn avant l’appel HTTP quand la liste d’IDs est vide ou nulle.

- **Résultat attendu :**
  - Pour `null` : le résultat n’est pas nul, mais la liste est vide.
  - Pour la liste vide : résultat non nul et vide aussi

- **Effets de bord attendus :**
  - Aucun appel réseau 
  - Aucun effet de bord.
  - Vérifie que la méthode est forte face à une entrée invalide et qu’elle ne lance pas d’exception

---

### 5. `testCheckEligibility_etudiantEligibleQuandTousPrerequisOK`

- **Cas d’utilisation lié :**  
  CU *Évaluer l’éligibilité d’un étudiant à un cours* (`checkEligibility`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` renvoie un cours `IFT2035` dont la liste de prérequis est :
    - `["IFT1025", "IFT1015"]`.
  - La liste de cours complétés par l’étudiant est :
    - `["IFT1025", "IFT1015"]`.
  - Appel de `checkEligibility("IFT2035", completed)`.

- **Résultat attendu :**
  - `result.isEligible()` est `true`.
  - `result.getMissingPrerequisites()` est une liste vide.

- **Effets de bord attendus :**
  - Aucun effet de bord (lecture seule).
  - Vérifie le cas nominal : tous les prérequis sont complétés, l’étudiant est bien déclaré éligible.

---

### 6. `testCheckEligibility_nonEligibleQuandPrerequisManquants`

- **Cas d’utilisation lié :**  
  CU *Évaluer l’éligibilité d’un étudiant à un cours* (`checkEligibility`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` renvoie le même cours `IFT2035` avec les prérequis :
    - `["IFT1025", "IFT1015"]`.
  - La liste de cours complétés par l’étudiant est :
    - `["IFT1025"]` seulement.
  - Appel de `checkEligibility("IFT2035", completed)`.

- **Résultat attendu :**
  - `result.isEligible()` est `false`.
  - `result.getMissingPrerequisites()` contient exactement `["IFT1015"]`.

- **Effets de bord attendus :**
  - Aucun effet de bord.
  - Vérifie que le service repère correctement les prérequis manquants et signale la non-éligibilité.

---

### 7. `testCompareCourses_retourneCoursQuandIdsValides`

- **Cas d’utilisation lié :**  
  CU *Comparer des cours* (`compareCourses(ids, params)`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` est configuré pour renvoyer deux cours :
    - `IFT1015 – Programmation 1` (3 crédits),
    - `IFT2035 – Concepts des langages de programmation` (3 crédits).
  - Liste d’IDs passée au service :
    - `["IFT1015", "IFT2035"]`.
  - Paramètres passés à `compareCourses` :
    - `include_schedule = "true"`,
    - `schedule_semester = "A25"`.

- **Résultat attendu :**
  - La liste retournée n’est pas nulle.
  - La liste contient exactement 2 cours.
  - Le premier cours a l’ID `IFT1015`.
  - Le second cours a l’ID `IFT2035`.

- **Effets de bord attendus :**
  - Aucun effet de bord persistant.
  - Vérifie le cas nominal : des IDs valides produisent bien une liste de cours correspondante.

---

### 8. `testGetAllCourses_filtreParNom`

- **Cas d’utilisation lié :**
  - CU *Recherche de cours*  (`getAllCourses via getCourseById`)

-	**Arguments / contexte :**
  - Le FakeHttpClientApi est configuré pour renvoyer un seul cours :
	- `IFT1015 – Programmation 1`
  - Aucun paramètre particulier n’est passé au service.
  - Appel : `getCourseById("IFT1015")`

-	**Résultat attendu :**
	- Le résultat retourné est non vide (`isPresent()`).
	- L’ID du cours retourné est exactement `IFT1015`. 

- **Effets de bord attendus :**
	- Aucun effet de bord.
	- Le test vérifie simplement que le service retourne bien le bon cours lorsque l’API renvoie une valeur valide. 

---

### 9. `testGetCourseById_coursInexistantRetourneEmpty`

- **Cas d’utilisation lié :**
  CU *Voir les détails d’un cours* (`getCourseById`)

- **Arguments / contexte :**
  - Le FakeHttpClientApi renvoie :
	  - courseToReturn = null
	  - throwOnGetCourse = false
	- Appel : `getCourseById("IFT9999")` (ID qui n’existe pas)

- **Résultat attendu :**
	- Le résultat est `Optional.empty()`
	- Le service ne lève aucune exception.

- **Effets de bord attendus :**
	- Aucun effet de bord.
	- Le test valide le comportement attendu lorsque l’API Planifium ne trouve pas le cours demandé.

---

### 10. `testCompareCourses_retourneListeVideQuandApiVide`

- **Cas d’utilisation lié :**
  CU *Comparer des cours* (`compareCourses`)

- **Arguments / contexte :**
	- Le FakeHttpClientApi renvoie une liste vide :
	  - coursesToReturn = `List.of()`
	  - Liste des IDs fournie : `"IFT1015", "IFT2035"`

- **Résultat attendu :**
  - Le résultat n’est pas null.
	- Mais la liste doit être vide car l’API n’a renvoyé aucun cours.

- **Effets de bord attendus :**
	- Aucun effet secondaire.
	- Vérifie que compareCourses gère correctement une API vide sans planter.

---
### 11. `testCompareCourses_retourneListeVideQuandIdsVides`

- **Cas d’utilisation lié :**  
  CU *Comparer des cours* (`compareCourses`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` est configuré avec deux cours :
    - `IFT1015 – Programmation 1` (3 crédits)
    - `IFT2035 – Concepts des langages de programmation`
  - Liste d’IDs fournie :  
    `["IFT1015", "IFT2035"]`
  - Paramètres :  
    - `include_schedule = "true"`  
    - `schedule_semester = "A25"`

- **Résultat attendu :**
  - Le résultat retourné n’est pas nul (`assertNotNull(result)`).
  - Le test valide que la méthode accepte les paramètres et exécute l’appel sans erreur.

- **Effets de bord attendus :**
  - Aucun effet de bord.
  - Le service ne fait qu’appeler le client HTTP avec les paramètres fournis.

---

### 12. `testCompareCourses_ignoreIdsInvalides`

- **Cas d’utilisation lié :**  
  CU *Comparer des cours* (`compareCourses`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` renvoie un seul cours :
    - `IFT1015 – Programmation 1` (3 crédits)
  - Liste des IDs transmise :
    - `"   "` (chaîne vide / blanche)
    - `null`
    - `"IFT1015"` (seul ID valide)
  - Aucun paramètre additionnel.

- **Résultat attendu :**
  - La liste retournée n’est pas nulle.
  - La liste contient **exactement 1 cours**, correspondant à l’ID valide.
  - Le cours retourné a l’ID `IFT1015`.

- **Effets de bord attendus :**
  - Aucun appel réseau pour les IDs invalides.
  - Aucun effet de bord.
  - Vérifie que la méthode filtre correctement les IDs vides ou nuls avant l’appel API.

---

### 13. `testCompareCourses_retourneListeVideQuandApiException`

- **Cas d’utilisation lié :**  
  CU *Comparer des cours* (`compareCourses`)

- **Arguments / contexte :**
  - Le `FakeHttpClientApi` est configuré pour lever une exception :
    - `throwOnGetCourse = true`
  - Liste d’IDs fournie :  
    `["IFT1015", "IFT2035"]`
  - Aucun paramètre additionnel.

- **Résultat attendu :**
  - La liste retournée n’est pas nulle.
  - La liste est vide (`isEmpty() == true`).
  - Aucune exception n’est propagée par `compareCourses`.

- **Effets de bord attendus :**
  - Aucun effet de bord.
  - Vérifie que le service capture l’exception API et renvoie proprement une liste vide, sans planter.

---

## Critères d'évaluation

- Les tests unitaires doivent :
  - passer tous sans erreur quand on fait `mvn test`,
  - couvrir les cas nominaux et plusieurs cas limites comme : liste vide, ID invalide, prérequis manquants, erreur simulée de l’API
- Le système est jugé satisfiasant ssi :
  - les CUs *Recherche de cours*, *Détails d’un cours* et *Comparer des cours* sont couverts par au moins un test unitaire chacun,
  - les scénarios principaux fonctionnent aussi via l’API REST et la CLI.

---

## Résultats des tests (Pour devoir 2)

- **Résumé qualitatif :**
  - Les tests unitaires confirment que :
    - la recherche de cours retourne les bonnes listes,
    - la récupération des détails d’un cours gère correctement cours trouvés et non trouvés,
    - la comparaison de cours gère les IDs invalides et les erreurs simulées de l’API,
    - le calcul d’éligibilité respecte bien la logique sur les prérequis.
  - Le comportement observé correspond aux attentes pour les CUs couverts.

- **Résumé quantitatif :**
  - **14 tests unitaires JUnit** au total :
    - 11 pour `CourseService`
    - 3 pour `UserService`
  - Tous les tests passent avewc succès (`Tests run: 14, Failures: 0, Errors: 0`).
  - Nous n’avons pas calculé la couverture exacte, mais les tests ciblent les branches principales de la logique métier.

---

## Évaluation du système (Pour devoir 2)

- **Qualité globale :**
  - Le backend s'occupe des CUs du devoir 2 : recherche de cours, détails, comparaison.
  - L’ajout de la logique d’éligibilité et de la CLI rend l'utilisatiion assez claire

- **Facilité d’utilisation :**
  - L’API REST reste simple (endpoints `/courses`, `/courses/{id}`, `/courses/comparer`, `/courses/{id}/eligibility`)
  - La CLI propose un menu clair pour faire les scénarios

- **Performance :**
  - Les appels vers Planifium sont simples et filtrés.
  
-------------------------------------------------------------------------------------------

## Plan de test (Pour devoir 3)
Les tests unitaires présentés ci-dessous ont été initialement développés lors du Devoir 2.
Ils ont été conservés et réutilisés pour le Devoir 3, car ils couvrent toujours les
fonctionnalités principales du système et respectent les exigences du présent devoir.
Pour le Devoir 3, nous sélectionnons 6 fonctionnalités distinctes parmi l’ensemble des tests
existants afin de satisfaire aux critères d’évaluation.


### 1. `testGetAllCourses_retourneListeDeCoursQuandApiOk`

- **Cas d’utilisation lié :**  
  CU *Recherche de cours* (`getAllCourses`)

- **Arguments / contexte :**
  -	Le FakeHttpClientApi est configuré pour renvoyer deux cours :
	    -	`IFT1015 – Programmation 1`
	    -	`IFT2035 – Concepts des langages de programmation`
	    -	Paramètres de recherche fournis au service : {`name": "programmation`}

- **Résultat attendu :**
  -	La liste retournée n’est pas nulle.
	- La liste contient exactement 2 cours.
	-	Le premier cours a l’ID IFT1015.
	-	Le deuxième cours a l’ID IFT2035..

- **Effets de bord attendus :**
  - Aucun effet de bord.
	-	Le service se contente de récupérer les données via le client HTTP simulé et de les retourner.

--- 

### 2. `testGetCourseById_retourneCoursQuandTrouve`

- **Cas d’utilisation lié :**
  CU *Voir les détails d’un cours* (`getCourseById`)

- **Arguments / contexte :**
	- Le FakeHttpClientApi renvoie un cours avec :
	- ID : `IFT2035`
	- Crédits : `3.0`
	- Texte de prérequis : `"Préalable : IFT1025"`
	- Appel de la méthode :`getCourseById("IFT2035")`

- **Résultat attendu :**
  - Le résultat retourné est un Optional présent.
	- L’ID du cours est IFT2035.
	- Le nombre de crédits est 3.0.
	- Le texte des prérequis correspond à celui fourni par l’API simulée.

- **Effets de bord attendus :**
	- Aucun effet de bord.
	- Le service agit en lecture seule et ne modifie aucune donnée persistante.

---

### 3. `testCheckEligibility_etudiantEligibleQuandTousPrerequisOK`

- **Cas d’utilisation lié :**
  CU *Évaluer l’éligibilité d’un étudiant à un cours* (`checkEligibility`)

- **Arguments / contexte :**
	- Le FakeHttpClientApi renvoie un cours avec les prérequis suivants : `["IFT1025", "IFT1015"]`
	- Liste des cours complétés par l’étudiant : `["IFT1025", "IFT1015"]`
	- Appel de la méthode : `checkEligibility("IFT2035", completedCourses)`

- **Résultat attendu :**
  - `result.isEligible()` retourne true.
	- La liste des prérequis manquants est vide.

- **Effets de bord attendus :**
	- Aucun effet de bord.
	- Le test valide uniquement la logique métier de comparaison des prérequis.

---

### 4. `testCompareCourses_ignoreIdsInvalides`

- **Cas d’utilisation lié :**
  CU *Comparer des cours* `(compareCourses)`

- **Arguments / contexte :**
	- Le FakeHttpClientApi est configuré pour renvoyer un seul cours valide :
		- `IFT1015 – Programmation 1`
	- Liste d’IDs fournie au service :
	  - `"   "` (chaîne vide)
	  - `null`
	  - `"IFT1015"` (ID valide)

- **Résultat attendu :**
  - La liste retournée n’est pas nulle.
	-	La liste contient exactement 1 cours.
	-	Le cours retourné a l’ID `IFT1015`.

- **Effets de bord attendus :**
	- Aucun effet de bord.
	- Les IDs invalides sont ignorés avant l’appel à l’API simulée.

---

### 5. `testCreateSet_ensembleValide`

- **Cas d’utilisation lié :**
  CU *Créer un ensemble de cours* `(createSet)`

- **Arguments / contexte :**
	- Trimestre fourni : `"H25"`
	- Liste de cours :
	  - `["IFT1015", "IFT2255", "IFT2015"]`
	- Appel de la méthode : `createSet("H25", courseIds)`

- **Résultat attendu :**
  - Le résultat retourné est un Optional présent.
	-	L’ensemble contient exactement 3 cours.
	-	Le trimestre est correctement assigné.
	-	Un identifiant unique d’ensemble est généré.

- **Effets de bord attendus :**
	- L’ensemble est stocké en mémoire dans le service.
	-	Aucun accès à des ressources externes.

--- 

### 6. ` testAddReview_avisValide`

- **Cas d’utilisation lié :**
  CU *Soumettre un avis étudiant* `(addReview)`

- **Arguments / contexte :**
	- Avis soumis :
	  - Course ID : `IFT2255`
	  - Difficulté :`4`
	  - Charge de travail : `3`
	  -	Commentaire : `"Cours intéressant"`
	- Le fichier de stockage JSON est vide au départ.

- **Résultat attendu :**
  - La méthode `addReview` retourne true.
	-	Un avis est présent dans la liste retournée par `getReviewsForCourse("IFT2255")`.

- **Effets de bord attendus :**
	- Écriture d’un nouvel avis dans le fichier JSON de stockage.
	-	Aucune modification d’autres avis existants.

---

## Critères d'évaluation (Pour devoir 3)

- **Résumé qualitatif :**
  - Les tests unitaires réalisés pour le devoir 3 confirment que :
	  -	la recherche de cours retourne correctement les listes correspondant aux critères fournis, y compris lorsque l’API renvoie une liste vide ;
	  - la consultation des détails d’un cours gère correctement les cas où le cours existe, n’existe pas ou lorsque l’ID est invalide ;
	  - la vérification de l’éligibilité applique correctement les règles de prérequis et distingue les cas éligibles et non éligibles ;
	  -	la comparaison de cours ignore les identifiants invalides et reste robuste face aux erreurs simulées de l’API ;
	  -	la création d’ensembles de cours respecte les contraintes métier (trimestre valide, nombre maximal de cours, sigles valides) ;
	  -	la gestion des avis étudiants valide les entrées, stocke correctement les avis et calcule les agrégats (moyennes et nombre d’avis).

- **Résumé quantitatif :**
	- Nombre total de tests unitaires JUnit :
	  - CourseServiceTest
	  - CourseSetServiceTest
	  -	ReviewServiceTest
	  -	UserServiceTest
	  -	Les tests couvrent plus de 6 fonctionnalités distinctes, incluant :
	  -	recherche,
	  -	détails,
	  -	éligibilité,
	  -	comparaison,
	  -	ensembles de cours,
	  -	avis étudiants.
  -	Tous les tests passent avec succès :
    - Tests run: XX, Failures: 0, Errors: 0

---

## Évaluation du système (Pour devoir 3)

- **Qualité globale :**
  - Le backend couvre l’ensemble des fonctionnalités requises pour le devoir 3 :
	  -	logique métier enrichie (éligibilité, ensembles de cours, avis),
	  -	séparation claire entre services, contrôleurs et modèles.
	  -	L’architecture modulaire facilite l’ajout de nouvelles fonctionnalités sans impact majeur sur les composants existants.

- **Facilité d’utilisation :**
  - L’API REST expose des endpoints clairs et cohérents :
		- /courses
	  - /courses/{id}
	  -	/courses/{id}/eligibility
	  -	/course-sets
	  -	/reviews
	- La CLI permet de tester les scénarios principaux sans interface graphique, ce qui facilite la validation fonctionnelle.

- **Performance :**
  - Le système est robuste face aux entrées invalides et aux erreurs simulées de services externes.
	-	Les appels externes sont centralisés et simulés dans les tests, ce qui améliore la testabilité et la stabilité globale du système.

---