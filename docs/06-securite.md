# 🔒 Fiche Sécurité Minimale du Projet

## 1. Choix de l'image de base (Surface d'attaque)
[cite_start]Pour le fichier `Dockerfile`, le choix s'est porté sur l'image officielle `nginx:alpine`[cite: 9, 29, 51]. 
* [cite_start]**Justification :** La distribution Alpine Linux est minimaliste et ne pèse que quelques Mo. En éliminant les utilitaires, compilateurs et packages non nécessaires au fonctionnement de Nginx, on réduit drastiquement la "surface d'attaque"[cite: 26, 29]. Moins il y a de composants installés, moins il y a de vulnérabilités potentielles exploitables par un attaquant.

## 2. Gestion des privilèges et limites du conteneur
* **Constat actuel :** Par défaut, l'image Nginx officielle s'exécute avec les privilèges de l'utilisateur `root` à l'intérieur du conteneur. Dans le cadre de ce TP pédagogique, cela suffit à faire fonctionner le serveur web, mais représente une limite sécuritaire.
* **Risque :** Si un attaquant parvient à exploiter une faille de sécurité au sein du site web ou de Nginx, il obtiendrait les droits `root` dans le conteneur, ce qui pourrait lui faciliter une éventuelle évasion vers l'hôte (le serveur ou le PC Windows).
* **Correction pour la production :** En production réelle, il est indispensable de modifier le comportement en créant un utilisateur non-privilégié (ex: `USER nginx`) disposant uniquement des droits de lecture sur les fichiers web, et configuré pour écouter sur un port supérieur à 1024 (comme le port 8080).

## 3. Automatisation des contrôles de sécurité (DevOps)
Pour respecter une démarche de sécurité continue (DevSecOps), l'amélioration majeure à apporter à notre pipeline GitHub Actions serait l'intégration d'un outil de scan de vulnérabilités automatisé comme **Trivy** ou **Aqua Security**. 
* [cite_start]Cet outil analyserait l'image Docker à chaque commit (au moment du Build)[cite: 13, 23, 56].
* [cite_start]Le pipeline serait configuré pour s'interrompre et bloquer la publication sur GHCR si une vulnérabilité critique (CVE) était détectée dans l'image de base ou dans les dépendances[cite: 16].
