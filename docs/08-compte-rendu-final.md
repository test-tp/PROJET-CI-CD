# 📝 Compte Rendu Final Personnel

## 1. Synthèse 
Ce projet nous a permis de mettre en pratique le déploiement d'une application web statique au sein d'une chaîne CI/CD automatisée. Nous avons réalisé l'initialisation de l'environnement local sous Windows 11 avec Docker Desktop en configurant un `Dockerfile` pour empaqueter l'application Nginx, ainsi qu'un fichier `compose.yml` intégrant un service secondaire (`ping-service`) pour gérer et documenter une simulation d'orchestration et de mise à l'échelle (scaling).
Dans un second temps, nous avons déporté cette logique sur la plateforme cloud GitHub grâce à GitHub Actions. Nous avons écrit 3 workflows distincts permettant de tester automatiquement le conteneur (`01-ci.yml`), de le publier de manière sécurisée et traçable sur GitHub Container Registry (`02-publish-ghcr.yml`) et enfin de permettre une promotion manuelle sans aucun "rebuild" vers un environnement de production simulé (`03-promote.yml`).

## 2. Fonctionnement technique

Commit : Le développeur pousse son code sur la branche main de GitHub.

Build & Test (01-ci.yml) : GitHub Actions déclenche automatiquement la construction de l'image Docker de test, puis lance un conteneur temporaire pour vérifier sa conformité (test de réponse HTTP curl).

Publication GHCR (02-publish-ghcr.yml) : Si les tests réussissent, l'image finale est construite et poussée sur le registre GitHub Packages (GHCR).

Validation recette (03-promote.yml - Manuel Promotion) : L'opérateur déclenche manuellement la validation en environnement de recette. L'image existante est récupérée depuis GHCR pour simuler les tests de conformité, sans aucune re-compilation du code.

Promotion production-simulee (03-promote.yml) : Une fois validée, la même image exacte (artéfact immuable) change simplement de statut (tag) pour être déployée dans l'environnement de production simulée.

Capture de la validation

Capture après la validation : 
<img width="1395" height="540" alt="image" src="https://github.com/user-attachments/assets/47c4fe23-13a8-485f-9381-a31756953d3a" />


## 3. Conteneurisation C12

Présenter le Dockerfile, l'image produite, l'exécution conteneurisée et les preuves.

## 4. Orchestration et scaling C13

Présenter compose.yml, le second service, la simulation de scaling et les limites.

## 5. Automatisation et sécurité C14

Présenter les workflows, GHCR, le GITHUB_TOKEN, les permissions, l'absence de secrets dans le code, le rollback et la sauvegarde/restauration.

## 6. Production réelle

Expliquer obligatoirement :

- gestion des secrets ;
- rollback ;
- sauvegarde/restauration.

Ajouter au moins deux autres éléments pertinents pour une production réelle.

## 7. Preuves

Lister les liens vers les runs GitHub Actions, GHCR, tags, digest et captures utiles.

## 8. Difficultés et apprentissages

Expliquer les difficultés rencontrées et ce que vous avez compris techniquement.

