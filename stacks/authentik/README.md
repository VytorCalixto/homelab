# Authentik

Stack de identidade e SSO da infraestrutura.

## Componentes

- `server` — interface e API do Authentik.
- `worker` — tarefas assíncronas.
- `redis` — fila/cache local da stack.
- PostgreSQL — banco externo à stack, conectado pela rede `homelab-backend`.

A configuração atual usa a imagem `ghcr.io/goauthentik/server:2026.8.2`.

## Dependências

Antes de iniciar:

1. criar a rede Docker externa `homelab-backend`;
2. disponibilizar o PostgreSQL;
3. criar o banco e usuário `authentik`;
4. configurar o `.env`;
5. configurar o DNS e o Caddy quando for publicar a interface.

A porta do servidor é publicada somente em `127.0.0.1:9000`.

## Secrets

O arquivo `.env.example` documenta:

- `AUTHENTIK_SECRET_KEY`;
- conexão com PostgreSQL.

O `.env` real não deve entrar no Git.

## Deploy

Pelo diretório da stack:

```bash
cp .env.example .env
docker compose config
docker compose up -d
docker compose ps
```

No ambiente gerenciado pelo Komodo, usar o stack cadastrado no projeto em vez de manter um deployment manual concorrente.

## Caddy

A publicação externa é feita pelo Caddy. A aplicação não publica a porta 9000 diretamente para a Internet.

Depois de alterar a configuração do Caddy, validar e recarregar:

```bash
sudo caddy validate --config /etc/caddy/Caddyfile
sudo systemctl reload caddy
```

## Persistência

A stack monta:

- `./data:/data`;
- `./custom-templates:/templates`;
- `./certs:/certs` no worker.

O PostgreSQL contém o estado principal da aplicação e precisa fazer parte da estratégia de backup.

## Troubleshooting

Ver estado:

```bash
docker compose ps
docker compose logs --tail=200 server
docker compose logs --tail=200 worker
docker compose logs --tail=100 redis
```

Se o Authentik não iniciar, verificar primeiro conectividade com PostgreSQL, `AUTHENTIK_SECRET_KEY` e o healthcheck do Redis.
