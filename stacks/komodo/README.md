# Komodo

Komodo é a camada de gerenciamento das aplicações Docker do projeto.

## Componentes

A stack atual usa o Compose `mongo.compose.yaml`. O MongoDB é usado pelo próprio Komodo.

A configuração de aplicação e os secrets são separados: o repositório contém o Compose e exemplos; os valores reais devem ser configurados no ambiente de execução.

## Primeiro setup

Copiar o exemplo:

```bash
cp .env.example .env
```

Validar a configuração:

```bash
docker compose -p komodo -f mongo.compose.yaml --env-file .env config
```

Subir:

```bash
docker compose -p komodo -f mongo.compose.yaml --env-file .env up -d
```

## Operação

Verificar:

```bash
docker compose -p komodo -f mongo.compose.yaml ps
docker compose -p komodo -f mongo.compose.yaml logs --tail=200
```

Depois do bootstrap, os deployments das aplicações devem ser administrados pelo Komodo, evitando alterações manuais concorrentes.

## Stacks

As stacks de aplicação ficam versionadas neste repositório. O Komodo deve apontar para o branch e caminho de Compose definidos para cada stack.

A separação por stack facilita:

- deploy independente;
- atualização;
- rollback;
- observabilidade;
- migração.

## Periphery

O Periphery é o agente que permite ao Komodo operar Docker em hosts remotos.

A instalação e configuração do Periphery devem permanecer separadas da stack do Core. Não publicar a porta do Periphery na Internet sem necessidade.

## Backup

O estado do Komodo e seu banco precisam de backup. Não tratar apenas os arquivos do repositório como backup do Komodo.

Antes de uma alteração importante, garantir que o backup do estado do Komodo está disponível.
