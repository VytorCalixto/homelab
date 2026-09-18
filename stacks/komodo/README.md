# Komodo

Manage all our docker applications

Run with:

```bash
cp .env.example .env
docker compose -p komodo -f komodo/mongo.compose.yaml --env-file komodo/.env up -d
```
