# 📝 Compte Rendu Final Personnel

## 1. Synthèse de la réalisation du projet
[cite_start]Ce projet de 5 jours m'a permis de mettre en pratique et d'industrialiser le déploiement d'une application web statique au sein d'une chaîne CI/CD automatisée[cite: 4, 6]. [cite_start]J'ai réalisé l'initialisation de l'environnement local sous Windows 11 avec Docker Desktop en configurant un `Dockerfile` pour empaqueter l'application Nginx, ainsi qu'un fichier `compose.yml` intégrant un service secondaire (`ping-service`) pour gérer et documenter une simulation d'orchestration et de mise à l'échelle (scaling)[cite: 9, 26, 29, 35, 36, 51].

[cite_start]Dans un second temps, j'ai déporté cette logique sur la plateforme cloud GitHub grâce à GitHub Actions[cite: 11, 23, 26]. [cite_start]J'ai écrit trois workflows distincts permettant de tester automatiquement le conteneur (`01-ci.yml`), de le publier de manière sécurisée et traçable sur GitHub Container Registry (`02-publish-ghcr.yml`) et enfin de permettre une promotion manuelle sans aucun "rebuild" vers un environnement de production simulé (`03-promote.yml`)[cite: 15, 16, 19, 51].

## 2. Difficultés rencontrées et solutions apportées
[cite_start]La principale difficulté technique de ce projet a résidé dans la gestion des différences environnementales entre mon poste de travail local sous Windows 11 (utilisant l'invite de commandes PowerShell) et l'environnement d'exécution virtuel basé sur Linux Ubuntu fourni par GitHub pour exécuter les pipelines Actions[cite: 26]. 
* *Exemple concret :* La gestion de la syntaxe des commandes de création de dossiers (`New-Item` vs `mkdir -p`), la sensibilité à la casse des noms de fichiers ou encore la configuration des barres obliques pour les chemins de fichiers (`\` sous Windows vs `/` sous Linux). 
* [cite_start]*Solution :* L'analyse minutieuse des journaux d'erreurs (logs) détaillés de GitHub Actions m'a permis d'adapter les syntaxes des scripts de tests HTTP (`curl`) et d'ajuster les commandes Git locales pour que l'historique et les fichiers de documentation soient poussés de manière totalement anonyme et conforme[cite: 15, 55].

## 3. Conclusion et perspectives DevOps
[cite_start]Ce module m'a fait prendre conscience qu'un pipeline CI/CD bien structuré apporte une immense valeur ajoutée à une entreprise comme Catal-Log : il élimine les tâches manuelles répétitives propices aux erreurs humaines, accélère le temps de mise en production et offre une traçabilité totale (auditabilité) grâce à l'usage des Digests et des environnements GitHub[cite: 6, 7, 8, 26, 39]. 

Bien que l'orchestration via Docker Compose reste confinée à un usage local, les concepts d'immuabilité de l'artéfact, de séparation des environnements et de promotion manuelle mis en œuvre dans ce TP sont les fondations exactes nécessaires pour évoluer vers des architectures de production de grande envergure gérées par **Kubernetes**.
