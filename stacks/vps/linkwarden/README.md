# Linkwarden

Gerenciador de bookmarks e páginas arquivadas.

## Deployment

Container: `linkwarden`

Porta local:

```text
127.0.0.1:5173 → 3000
```

A aplicação usa a rede Docker externa `homelab-backend`.

## Persistência

```text
/srv/homelab/linkwarden/data → /data/data
```

O diretório contém dados persistentes da aplicação.

O PostgreSQL utilizado pelo Linkwarden é configurado por variáveis do `.env`. A configuração real não deve ser versionada.

## Configuração

O `.env.example` deve ser preenchido antes do deployment.

Validar:

```bash
docker compose config
```

## Deploy

```bash
docker compose up -d
docker compose ps
docker compose logs --tail=200 linkwarden
```

## Migração

Para uma migração com banco PostgreSQL:

1. exportar o banco na origem;
2. copiar o dump;
3. restaurar o banco no destino;
4. copiar o diretório persistente;
5. configurar as credenciais do destino;
6. iniciar a aplicação;
7. validar links, usuários e arquivos arquivados.

Consultar [Migração de serviços](../../docs/migrations.md).

## Caddy

A porta do container fica exposta somente em loopback. O acesso público deve ocorrer pelo domínio configurado no Caddy.
