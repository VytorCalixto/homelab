# Migração de serviços

A migração é feita uma aplicação por vez.

## Estratégia

```text
inventário
   ↓
backup/export
   ↓
copiar dados
   ↓
subir destino
   ↓
restore/import
   ↓
validar
   ↓
DNS / Caddy
   ↓
observar
   ↓
desligar origem
```

Não desligar a origem antes de confirmar o destino.

## Antes da migração

Confirmar imagem, versão, variáveis, dependências, portas, volumes, banco, uploads, jobs, domínio, autenticação e integrações.

O Compose é a fonte da verdade para mounts e portas.

## PostgreSQL

Para mover PostgreSQL entre containers, usar dump lógico em formato custom.

Origem:

```bash
docker exec <container-db> \
  pg_dump -U <usuario> -d <database> -Fc \
  -f /tmp/database.dump

docker cp <container-db>:/tmp/database.dump ./database.dump
```

Copiar para o destino pelo canal privado disponível. Depois verificar o hash com <code>sha256sum</code>.

Destino:

```bash
docker cp database.dump <container-db>:/tmp/database.dump

docker exec <container-db> \
  pg_restore -U <usuario> -d <database> \
  --no-owner --no-acl --exit-on-error \
  /tmp/database.dump
```

Para uma migração limpa, restaurar em um banco vazio. Durante o restore, manter os consumidores do banco parados.

Depois:

- verificar tabelas;
- verificar registros importantes;
- verificar extensões;
- iniciar a aplicação;
- verificar migrations;
- executar teste funcional.

## Securo

O Securo é uma stack independente. Quando for migrado sem usar o PostgreSQL compartilhado, o banco do próprio Compose do Securo recebe o dump.

```text
PostgreSQL Securo no Homelab
        │ pg_dump -Fc
        ▼
    securo.dump
        │ WireGuard / SCP
        ▼
PostgreSQL do Securo na VPS
        │ pg_restore
        ▼
      backend
```

Durante o restore, manter backend, worker e demais consumidores parados.

## SQLite

Para SQLite:

1. parar a aplicação;
2. copiar o banco e os arquivos persistentes relacionados;
3. iniciar no destino;
4. validar.

Se a aplicação tiver uploads ou outros diretórios separados, eles também precisam ser migrados.

## Validação

Uma migração só está concluída quando o container está saudável, os dados aparecem na aplicação, login e arquivos importantes funcionam e o domínio público funciona quando aplicável.
