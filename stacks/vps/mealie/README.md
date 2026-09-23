# Mealie

Aplicação de receitas e planejamento de refeições.

## Deployment

Container: `mealie`

Porta local:

```text
127.0.0.1:9925 → 9000
```

## Persistência

```text
/srv/homelab/mealie/data → /app/data
```

O banco SQLite e os demais dados persistentes ficam nesse diretório.

## Configuração

A stack usa `.env` para secrets e configuração de OIDC.

Também define:

- `ALLOW_SIGNUP=false`;
- `OIDC_AUTH_ENABLED=true`;
- `TZ=America/Sao_Paulo`;
- `BASE_URL=https://receitas.rotiav.com.br`.

O `.env` real nunca deve ser versionado.

## Deploy

```bash
docker compose config
docker compose up -d
docker compose ps
docker compose logs --tail=200 mealie
```

## Migração

A migração planejada usa o SQLite existente.

1. parar o Mealie na origem;
2. copiar o diretório de dados;
3. restaurar em `/srv/homelab/mealie/data`;
4. iniciar a stack;
5. validar receitas e usuários;
6. validar OIDC e URL pública.

## Caddy

O Caddy publica o serviço. A porta 9925 não deve ser exposta diretamente à Internet.
