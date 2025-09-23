---
title: Analyse des besoins - Risques
---

# Analyse des risques

## Identification des risques

### Risque 1 – Manque de familiarité avec les outils techniques (MkDocs / Git / API)
- **Probabilité** : Élevée  
- **Impact** : Moyen  
- **Plan de mitigation** :  
  - Désigner un responsable pour l’installation et la configuration initiale  
  - Partager un guide interne d’utilisation des outils  
  - Prévoir du temps tampon pour résoudre les problèmes techniques en amont  

### Risque 2 – Difficultés d’intégration avec l’API Planifium ou le bot Discord
- **Probabilité** : Moyenne  
- **Impact** : Élevé  
- **Plan de mitigation** :  
  - Utiliser des données simulées (CSV/JSON) pour la Phase 1  
  - Reporter l’intégration réelle à la Phase 2  
  - Prévoir un processus alternatif d’import manuel des données  

### Risque 3 – Biais ou manipulation des avis étudiants
- **Probabilité** : Moyenne  
- **Impact** : Élevé  
- **Plan de mitigation** :  
  - N’afficher les avis qu’à partir de n≥5  
  - Détection et filtrage des doublons  
  - Modération manuelle des cas extrêmes  

### Risque 4 – Sous-estimation de la charge de travail étudiante
- **Probabilité** : Moyenne  
- **Impact** : Moyen  
- **Plan de mitigation** :  
  - Comparaison multi-cours avec estimation d’heures/semaine  
  - Documentation claire des critères d’estimation  
  - Mise à jour continue selon les retours des étudiants  

### Risque 5 – Disponibilité limitée des membres de l’équipe
- **Probabilité** : Moyenne  
- **Impact** : Moyen  
- **Plan de mitigation** :  
  - Répartition claire des tâches  
  - Suivi hebdomadaire de l’avancement  
  - Documentation commune pour faciliter la reprise par un autre membre  

## Modification du processus opérationnel

>Non applicable pour la phase 1
risques.md
2 Ko