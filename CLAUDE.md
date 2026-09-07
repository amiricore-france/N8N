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
- Les workflows et identifiants n8n sont stockés dans le volume Docker `n8n_n8n_data`, pas dans ce dossier.
