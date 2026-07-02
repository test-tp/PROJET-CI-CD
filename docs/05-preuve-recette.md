# 05 - Preuve recette simulée

## Workflow de validation recette

- Workflow concerné : 03-promote.yml
- Environnement GitHub : recette
- Tag source validé : latest
- Digest observé : sha256:24fb6d3444aa06364814d8dd568c49210afb276596bcd8b7b2fc5d36e370cb01
- Lien du run : https://github.com/test-tp/PROJET-CI-CD/actions/runs/28616416578

## Résultat

Décrire le résultat du test HTTP réalisé pendant la validation recette.
La validation se fait par le déploiement de l'artéfact immuable sans re-compilation. Le script récupère l'image depuis GHCR et simule son installation. Le terminal affiche avec succès le message de confirmation avec l'URL complète de l'image de recette.
