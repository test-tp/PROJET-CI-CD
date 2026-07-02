# 04 - Preuve image GHCR

## Image publiée

- Nom de l'image : ghcr.io/test-tp/PROJET-CI-CD/nginx-web
- Tag principal : latest
- Digest : sha256:24fb6d3444aa06364814d8dd568c49210afb276596bcd8b7b2fc5d36e370cb01
- Lien GHCR ou capture : https://github.com/test-tp/PROJET-CI-CD/pkgs/container/projet-ci-cd%2Fnginx-web

## Explication

Expliquer pourquoi le tag et le digest sont utiles pour la traçabilité et le rollback.

Traçabilité : Le tag associe l'image au commit exact pour identifier l'origine du code. Le digest (SHA256) sert de signature inviolable pour garantir l'intégrité de l'artéfact.

Rollback : En cas d'anomalie en production, on peut redéployer l'image précédente via son tag/digest. Le retour arrière est instantané, sécurisé et sans aucune re-compilation
