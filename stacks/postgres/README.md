# PostgreSQL

PostgreSQL de infraestrutura para aplicações que compartilham o banco central.

## Imagem

A stack usa:

```text
pgvector/pgvector:0.8.6-pg16
```

A extensão `vector` está disponível para aplicações que precisam de pgvector.

> Aplicações podem usar PostgreSQL dedicado quando isso simplificar isolamento, restore ou manutenção. O Securo, por exemplo, não usa este banco compartilhado.

## Rede

O container participa da rede Docker externa:

```text
homelab-backend
```

A porta 5432 não deve ser publicada no host.

## Persistência

Os dados ficam em:

```text
/srv/homelab/postgres/data
```

Esse diretório faz parte da estratégia de backup da VPS.

## Configuração

Copiar o exemplo e preencher o secret:

```bash
cp .env.example .env
```

O arquivo real nunca deve ser versionado.

## Deploy

Criar a rede se necessário:

```bash
docker network create homelab-backend
```

Criar o diretório:

```bash
sudo mkdir -p /srv/homelab/postgres/data
```

Subir:

```bash
docker compose up -d
docker compose ps
```

Verificar:

```bash
docker compose exec postgres pg_isready -U postgres
```

## Bancos e roles

Cada aplicação deve ter seu próprio banco e role. Não usar o superusuário da infraestrutura na aplicação.

Exemplo:

```sql
CREATE ROLE app LOGIN PASSWORD '...';
CREATE DATABASE app OWNER app;
REVOKE ALL ON DATABASE app FROM PUBLIC;
GRANT CONNECT ON DATABASE app TO app;
```

As credenciais reais devem ser armazenadas fora do Git.

## Backup

Preferir dump lógico por banco para permitir restore independente:

```bash
docker compose exec -T postgres \
  pg_dump -U <usuario> -d <database> -Fc > database.dump
```

Para restore, seguir [Migração de serviços](../../docs/migrations.md).

## Troubleshooting

```bash
docker compose ps
docker compose logs --tail=200 postgres
docker compose exec postgres pg_isready -U postgres
```

Não expor 5432 temporariamente apenas para facilitar troubleshooting. Use a rede Docker ou o acesso privado administrativo.
