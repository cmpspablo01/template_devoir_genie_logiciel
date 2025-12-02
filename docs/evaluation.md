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

## Plan de test

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

## Critères d'évaluation

- Les tests unitaires doivent :
  - passer tous sans erreur quand on fait `mvn test`,
  - couvrir les cas nominaux et plusieurs cas limites comme : liste vide, ID invalide, prérequis manquants, erreur simulée de l’API
- Le système est jugé satisfiasant ssi :
  - les CUs *Recherche de cours*, *Détails d’un cours* et *Comparer des cours* sont couverts par au moins un test unitaire chacun,
  - les scénarios principaux fonctionnent aussi via l’API REST et la CLI.

---

## Résultats des tests

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

## Évaluation du système

- **Qualité globale :**
  - Le backend s'occupe des CUs du devoir 2 : recherche de cours, détails, comparaison.
  - L’ajout de la logique d’éligibilité et de la CLI rend l'utilisatiion assez claire

- **Facilité d’utilisation :**
  - L’API REST reste simple (endpoints `/courses`, `/courses/{id}`, `/courses/comparer`, `/courses/{id}/eligibility`)
  - La CLI propose un menu clair pour faire les scénarios

- **Performance :**
  - Les appels vers Planifium sont simples et filtrés.
  