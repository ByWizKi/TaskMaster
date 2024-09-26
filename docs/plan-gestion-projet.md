# Plan de gestion du projet - TaskMaster

## 1. Méthodologie
Nous utilisons la méthode Agile, avec des sprints de deux semaines. Chaque sprint a des objectifs clairs et suit les étapes suivantes :
1. Planification
2. Développement
3. Test
4. Revue et rétroaction

## 2. Organisation sur GitHub Projects
### a. Tableau Kanban
- **Backlog** : Toutes les fonctionnalités non encore planifiées.
- **To Do** : Les tâches à réaliser dans le sprint en cours.
- **In Progress** : Tâches en cours de développement.
- **In Review** : Tâches terminées mais en attente de validation.
- **Done** : Tâches complètement terminées.

### b. Assignation des tâches
Chaque développeur ou designer se voit attribuer des tâches en fonction de son domaine d'expertise (backend, frontend web, mobile).

### c. Priorisation des tâches
- **Haute priorité** : Bugs critiques ou fonctionnalités principales.
- **Priorité moyenne** : Améliorations et optimisations.
- **Faible priorité** : Fonctionnalités optionnelles ou mineures.

## 3. Branching model (GitFlow)
Nous utilisons GitFlow pour organiser le développement.
- `main` : Version stable.
- `develop` : Branche principale pour le développement.
- `feature/xxx` : Branche pour chaque nouvelle fonctionnalité.
- `hotfix/xxx` : Branche pour corriger les bugs en production.
