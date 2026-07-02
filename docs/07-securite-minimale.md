# 07 - Sécurité minimale

## Permissions GitHub Actions

Indiquer les permissions utilisées dans les workflows et expliquer pourquoi elles sont limitées.

- 01-ci.yml : contents: read
- 02-publish-ghcr.yml : contents: read, packages: write
- 03-promote.yml : contents: read, packages: write

## Gestion des secrets

Pas de secrets dans le code : Stocker des mots de passe ou des clés d'API en clair dans le code expose le système à des fuites massives (surtout si le dépôt devient public). Une fois poussé dans l'historique Git, un secret est visible par tous et très difficile à effacer définitivement.

Expliquer l'usage du GITHUB_TOKEN dans ce projet : C'est un jeton secret généré automatiquement par GitHub au début de chaque run et détruit dès qu'il se termine. Dans ce projet, il est utilisé par le workflow 02 pour s'authentifier de manière totalement sécurisée et transparente auprès du registre GHCR, sans que vous n'ayez à créer ou configurer de mot de passe manuel.

Indiquer quels éléments devraient être placés dans GitHub Secrets ou dans un coffre de secrets en production réelle : En production réelle : Les éléments sensibles comme les clés de connexion aux serveurs de production, les certificats SSL, les mots de passe de bases de données ou les jetons d'accès Cloud tiers doivent impérativement être stockés dans GitHub Secrets (chiffrés) ou dans un coffre-fort de secrets professionnel (comme HashiCorp Vault, AWS Secrets Manager...)

## Rollback

Expliquer comment revenir à une version précédente à partir d'un tag, d'un digest ou de la promotion d'une image antérieure.

Par Tag / Digest : Il faut relancer le déploiement en ciblant précisément l'identifiant (le tag ou le Digest) d'une version précédente reconnue comme saine et stockée sur GHCR.

Par promotion antérieure : Comme les images sur GHCR sont immuables (figées dans le temps), effectuer un rollback consiste à pointer de nouveau vers l'ancien artéfact. Le déploiement est instantané et totalement sécurisé puisqu'il ne nécessite aucune re-compilation du code source, éliminant ainsi le risque d'introduire un nouveau bug.

## Sauvegarde / restauration

À sauvegarder : Le dépôt Git (code, historique, documentation), la configuration de l'infrastructure (.github/workflows/, docker-compose.yml) et les images Docker publiées sur GHCR.

Comment restaurer : Réimporter le dépôt Git sauvegardé sur une nouvelle organisation GitHub. Les fichiers de configuration (GitOps) permettent de relancer automatiquement les pipelines et de republier instantanément les images Docker à partir du code source.

## Deux éléments complémentaires

Choisir et expliquer au moins deux éléments parmi : répartition de charge, supervision, haute disponibilité, registre d'images maîtrisé, gestion des accès, journalisation, contrôle des vulnérabilités, politique de déploiement, validation manuelle avant production, séparation stricte des environnements.

Supervision (Monitoring) : C'est le tableau de bord de votre infrastructure. Elle consiste à installer des outils (comme Prometheus ou Grafana) pour surveiller en temps réel la santé de vos conteneurs (utilisation du CPU, de la mémoire, taux d'erreurs HTTP). En production, la supervision permet d'alerter instantanément les équipes techniques en cas de panne ou de comportement anormal avant même que les utilisateurs ne s'en aperçoivent.

Haute Disponibilité (High Availability - HA) : C'est la garantie que votre site reste accessible même en cas de panne matérielle. Au lieu de faire tourner votre application sur une seule machine locale, elle est déployée en plusieurs exemplaires (réplicas) sur un cluster de serveurs distincts, épaulée par un répartiteur de charge (Load Balancer). Si un serveur physique tombe en panne, le trafic est instantanément redirigé vers les serveurs sains, assurant un service continu sans aucune coupure pour les clients.
