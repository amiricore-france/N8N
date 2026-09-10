# CLAUDE.md

Ce dossier contient la configuration Docker de n8n (automatisation), utilisé pour construire des workflows séparés du développement du CRM.

## Intégration avec le CRM

Certains workflows n8n injectent des données dans le CRM immobilier (`crm-immo`, projet distinct avec sa propre session Claude Code).

**Avant de construire ou modifier un workflow qui appelle l'API du CRM**, lire :

```
/home/renaud/crm-immo/API-N8N.md
```

Ce fichier décrit le flux d'authentification (compte `invite` en lecture/création, compte admin réservé aux apporteurs), les endpoints disponibles (`POST /api/prospects`, `POST /api/apporteurs`), les champs/enums attendus, et les codes d'erreur à gérer.

Ne pas dupliquer ce contenu ici : le CRM évolue dans son propre dépôt, ce fichier n'est qu'un pointeur pour éviter que cette session en perde la trace.

## n8n

- Instance locale via `docker-compose.yml`, accessible sur `http://localhost:5678` (port restreint à `127.0.0.1`, pas exposé sur le réseau local).
- `docker compose up -d` / `docker compose down` pour gérer le conteneur.
- Les workflows et identifiants n8n sont stockés dans le volume Docker `n8n_n8n_data` : c'est la source de vérité, ce dossier ne fait tourner aucun workflow.

## Sauvegarde des workflows

Le dossier `workflows/` contient des exports JSON de workflows n8n, committés manuellement à titre de sauvegarde (pas de sync automatique avec le volume Docker — un export peut donc être en retard sur l'état réel).

Pour exporter un workflow avant de le committer :

```
docker exec n8n-server n8n export:workflow --id=<workflow_id> --output=/tmp/export.json
docker cp n8n-server:/tmp/export.json workflows/<nom-du-workflow>.json
```

Ces exports contiennent des références d'identifiants (id/nom) mais pas de secrets — les identifiants n8n restent dans le volume Docker.
