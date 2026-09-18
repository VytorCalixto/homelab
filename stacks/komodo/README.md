# Komodo

Manage all our docker applications

Run with:

```bash
cp .env.example .env
docker compose -p komodo -f mongo.compose.yaml --env-file .env up -d
```
