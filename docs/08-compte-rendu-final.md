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

Capture de la validation : 
****************

Capture après la validation : 
<img width="1395" height="540" alt="image" src="https://github.com/user-attachments/assets/47c4fe23-13a8-485f-9381-a31756953d3a" />


## 3. Conteneurisation C12

Le Dockerfile : Basé sur une image légère nginx:alpine. Il copie le dossier local docs/ (qui contient notre index.html) directement dans le répertoire de publication par défaut de Nginx (/usr/share/nginx/html).

L'image produite : Nommée ghcr.io/[votre-nom]/[votre-depot]/nginx-web, optimisée (quelques mégaoctets seulement grâce à Alpine) et totalement indépendante de la machine hôte.

Exécution : Le conteneur s'exécute de manière isolée en exposant un port spécifique (ex: 8080), rendant l'application web immédiatement accessible via http://localhost:8080.

Preuves : 
**************

## 4. Orchestration et scaling C13

Fichier compose.yml : Il orchestre l'ensemble de notre application locale de manière déclarative. Il définit les volumes, les réseaux internes isolés et configure les variables d'environnement nécessaires au lancement uniforme de nos conteneurs.

Le second service : Ajout d'un service complémentaire (par exemple une base de données légere ou une API fictive) connecté sur le même réseau privé virtuel, permettant aux conteneurs de communiquer entre eux de manière sécurisée par leur simple nom de service.

Simulation de scaling : Testé localement via la commande docker compose up --scale nginx-web=3. Docker Compose instancie alors trois conteneurs identiques en parallèle à partir de la même image.

Limites : Compose gère le scaling sur une seule et unique machine physique (locale). Il n'a pas de Load Balancer automatique pour distribuer le trafic externe sur les 3 conteneurs, pas de gestion des pannes (si le PC s'éteint, tout s'arrête), et aucune capacité d'auto-guérison sur un cluster de serveurs (contrairement à Kubernetes).

## 5. Automatisation et sécurité C14

Workflows : Découpés en trois fichiers YAML distincts (01-ci, 02-publish, 03-promote) pour séparer strictement les responsabilités de développement, publication et déploiement.

GHCR et GITHUB_TOKEN : Le jeton éphémère GITHUB_TOKEN est généré à la volée par GitHub pour authentifier de manière chiffrée le workflow auprès du registre GHCR, sans stocker de clé permanente.

Permissions & Secrets : Application stricte du moindre privilège (contents: read, packages: write). Aucun mot de passe ni clé d'API n'est écrit en clair dans le code source pour éviter toute fuite historique majeure.

Rollback et Sauvegarde : L'immuabilité de l'image (via son Digest) permet un retour arrière instantané et sécurisé sans re-build. La sauvegarde repose sur l'exportation du code Git (GitOps) permettant une reconstruction complète de l'usine logicielle à la demande.

## 6. Production réelle

Expliquer obligatoirement :
- gestion des secrets ;
- rollback ;
- sauvegarde/restauration.
Ajouter au moins deux autres éléments pertinents pour une production réelle.

Gestion des secrets : Bannissement total des variables en clair. Utilisation obligatoire d'un coffre-fort managé chiffré (type HashiCorp Vault ou AWS Secrets Manager) couplé à GitHub Secrets.

Rollback : Procédure automatisée consistant à modifier le pointeur de version (Tag/Digest de production) vers l'ancienne image saine déjà présente sur GHCR. Le déploiement se fait sans aucune modification ou re-compilation du code.

Sauvegarde/restauration : Sauvegardes automatisées, externalisées et régulières (solutions "3-2-1") du code source Git, des configurations d'infrastructure sous forme de code, ainsi qu'une réplication du registre d'images GHCR sur un stockage cloud tiers.

Éléments complémentaires (au moins deux) : Supervision, Haute disponibilité

## 7. Preuves

Lister les liens vers les runs GitHub Actions, GHCR, tags, digest et captures utiles.

****************
****************
****************
****************
****************

## 8. Difficultés et apprentissages

Expliquer les difficultés rencontrées et ce que vous avez compris techniquement.

Difficultés rencontrées : Gestion des conflits Git lors du rapatriement des fichiers modifiés localement par rapport à l'historique distant de GitHub (erreur push rejected) + Compréhension de la notion d'artéfact immuable où l'image Docker ne doit jamais être re-compilée entre la recette et la production + Découverte au niveau de la prise en main et de la compréhension de Github

Apprentissages techniques : Maîtrise des fondamentaux des concepts de l'intégration et du déploiement continus (CI/CD) + Compréhension de l'intérêt d'isoler les environnements grâce à la conteneurisation Docker + Sensibilisation aux règles de sécurité essentielles en DevOps (permissions minimales des tokens, protection des secrets et mise en place de stratégies de rollback fiables).

Autres définitions : 

GHCR (GitHub Container Registry) : C'est un service de stockage en ligne sécurisé proposé par GitHub qui permet d'héberger, de gérer et de partager vos images Docker au même endroit que votre code source.

Le Tag : C'est une étiquette textuelle (comme latest ou un numéro de version) lisible par l'humain, attachée à une image Docker pour identifier facilement une version spécifique du logiciel.

Le Digest : C'est l'empreinte numérique unique et chiffrée (SHA256) générée automatiquement par Docker qui garantit de manière absolue que le contenu de l'image n'a pas été modifié ou falsifié.
