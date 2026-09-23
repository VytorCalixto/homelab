# Nametag

Aplicação de gerenciamento de contatos e cartões.

## Deployment

A stack possui dois serviços:

- `app` — aplicação principal;
- `cron` — tarefas agendadas.

A aplicação usa a rede externa `homelab-backend`.

## Porta

```text
127.0.0.1:3030 → 3000
```

## Persistência

```text
/srv/homelab/nametag/photo_data → /app/data/photos
```

As fotos devem fazer parte do backup.

O banco da aplicação deve ser exportado separadamente conforme o mecanismo utilizado pelo Nametag.

## Cron

O container `cron` executa:

- envio de lembretes;
- purge de registros deletados;
- sincronização CardDAV;
- geocoding.

As tarefas usam `CRON_SECRET` para autenticar contra o serviço principal.

## Secrets

`CRON_SECRET` e demais credenciais devem ser configurados no `.env`/Komodo e nunca no Git.

## Deploy

```bash
docker compose config
docker compose up -d
docker compose ps
docker compose logs --tail=200 app
docker compose logs --tail=200 cron
```

## Migração

Antes de copiar dados, parar app e cron. Migrar o banco e `photo_data`, iniciar a stack e validar contatos, fotos e tarefas agendadas.

## Caddy

A porta 3030 fica restrita ao loopback. O acesso público deve passar pelo Caddy.
