---
title: Analyse des besoins - Flux principaux
---

# Flux principaux

## Objectif
Décrire les flux d’interaction entre les acteurs et le système.

## Diagrammes

### Flux 1 — Recherche et consultation d’un cours
```mermaid
flowchart TD
  A[Étudiant] -->|Saisit code/titre/mots-clés| B[Recherche]
  B --> C{API Planifium disponible ?}
  C -- Non --> X[Message d'erreur : service indisponible]
  C -- Oui --> D[Interrogation Planifium]
  D --> E[Résultats : cours trouvés]
  E --> F[Étudiant sélectionne un cours]
  F --> G[Affichage fiche du cours]
  G --> H{Avis étudiants ≥ 5 ?}
  H -- Oui --> I[Afficher avis agrégés (moyenne, dispersion)]
  H -- Non --> J[Afficher message « Pas assez d’avis »]
  G --> K[Afficher résultats académiques (moyenne, inscrits, échecs) si dispo]
```