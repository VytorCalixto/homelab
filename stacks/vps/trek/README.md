# TREK

Aplicação de planejamento e gerenciamento de viagens.

## Deployment

Compose: `compose.yaml`

Container: `trek`

Porta local:

```text
127.0.0.1:3000 → 3000
```

A aplicação é publicada externamente pelo Caddy.

## Persistência

O Compose monta:

```text
/srv/homelab/trek/data    → /app/data
/srv/homelab/trek/uploads → /app/uploads
```

O banco SQLite e os arquivos da aplicação ficam no volume `data`. Uploads ficam em `uploads`.

Ambos precisam ser incluídos no backup.

## Configuração

Variáveis importantes incluem:

- `ENCRYPTION_KEY`;
- `ALLOWED_ORIGINS`;
- `APP_URL`;
- parâmetros OIDC, quando o SSO estiver habilitado;
- `TRUST_PROXY`.

Não colocar secrets reais no Compose.

## Segurança

O container usa filesystem read-only, remove capabilities e habilita `no-new-privileges`. A porta fica limitada ao loopback da VPS.

## Deploy

```bash
docker compose config
docker compose up -d
docker compose ps
docker compose logs --tail=200 app
```

## Migração

Para migrar uma instalação existente:

1. parar o TREK na origem;
2. copiar o conteúdo de `data` e `uploads`;
3. restaurar no destino;
4. iniciar o container;
5. validar login e viagens;
6. validar uploads;
7. validar o domínio pelo Caddy.

