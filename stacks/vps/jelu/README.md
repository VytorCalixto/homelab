# Jelu

Aplicação de gerenciamento de biblioteca pessoal.

## Deployment

Container: `jelu`

Porta local:

```text
127.0.0.1:11111 → 11111
```

Imagem atual:

```text
wabayang/jelu:0.87.3
```

## Persistência

A stack usa:

```text
/srv/homelab/jelu/config       → /config
/srv/homelab/jelu/database     → /database
/srv/homelab/jelu/files/images → /files/images
/srv/homelab/jelu/files/imports → /files/imports
```

Todos esses diretórios devem ser considerados dados da aplicação e incluídos no backup.

## Deploy

```bash
docker compose config
docker compose up -d
docker compose ps
docker compose logs --tail=200 jelu
```

## Migração

Parar a origem antes da cópia para evitar alterações concorrentes.

Copiar os diretórios persistentes para os caminhos equivalentes na VPS e validar livros, imagens e imports depois do primeiro boot.

## Caddy

O acesso externo passa pelo Caddy. Não publicar a porta 11111 diretamente na Internet.
