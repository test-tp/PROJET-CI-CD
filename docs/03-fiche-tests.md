# 03 - Fiche tests

## Test automatisé GitHub Actions

- Workflow concerné : 01-ci.yml
- Lien vers le run réussi : A compléter
- Ce qui est testé : Le workflow construit l'image Docker à partir du Dockerfile pour valider l'absence d'erreurs de build. Il démarre ensuite le conteneur sur le runner Ubuntu et utilise une commande curl pour effectuer un test de connectivité HTTP (Healthcheck). Cela vérifie que le serveur Nginx répond correctement avec un code d'état 200.
- Résultat : Le test est validé avec succès (statut au vert). Le site web est fonctionnel et accessible, ce qui autorise le déclenchement du workflow suivant pour la publication.

## Test local Docker ou Docker Compose

Renseigner l'une des deux situations.

### Situation A - Test réalisé

Commandes utilisées :

```bash
docker build -t projet-cicd-nginx:local .
docker run --rm -p 8080:80 projet-cicd-nginx:local
```

ou :

```bash
docker compose up --build
```

Résultat observé : Le build de l'image s'exécute sans erreur en local. Lors du lancement, les conteneurs démarrent correctement. En ouvrant un navigateur web à l'adresse http://localhost:8080, la page d'accueil du site statique de l'entreprise Catal-Log s'affiche instantanément. Dans le terminal, les logs Nginx confirment la bonne réception des requêtes HTTP avec un code 200. De plus, le service secondaire (ping-service) parvient à joindre le serveur web sur le réseau interne Docker.

Image du site ci-dessous : 
<img width="1275" height="461" alt="image" src="https://github.com/user-attachments/assets/eabb8ea2-3952-49ad-9005-a0966e561d71" />


### Situation B - Test local impossible

Justification : Non applicable (le test a pu être réalisé avec succès sur l'environnement Docker Desktop sous Windows 11).

## Simulation de scaling

Si l'environnement le permet :

```bash
docker compose up -d --scale web=2
docker compose ps
```

Résultat observé : La commande s'exécute avec succès. Docker Compose duplique le service web et crée instantanément deux instances distinctes (par exemple projet-ci-cd-web-1 et projet-ci-cd-web-2) qui tournent en parallèle sur le port configuré. La commande "docker compose ps" confirme que les deux réplicas sont actifs et en bonne état (State: Running). Le trafic généré est maintenant réparti entre ces deux conteneurs, simulant une infrastructure locale à haute disponibilité.

## Limites de la simulation

Expliquer les limites : absence de vrai load balancer, absence de haute disponibilité, absence de supervision, dépendance à l'environnement local, etc.

Pas de Load Balancer : Répartition des flux purement interne, sans gestionnaire de trafic professionnel (comme Traefik ou HAProxy).

Pas de Haute Disponibilité : Tout tourne sur une seule machine locale ; si votre PC s'éteint, tout le site se coupe.

Pas de Supervision : Aucun outil (type Prometheus/Grafana/Zabbix) pour alerter en cas de panne ou analyser les logs.

Pas d'Auto-guérison : Docker Compose ne sait pas recréer automatiquement un conteneur défaillant sur un autre serveur.
