# Operação

## Fluxo padrão

Antes de alterar uma aplicação:

1. verificar o estado atual;
2. revisar o Compose e as dependências;
3. fazer backup;
4. aplicar a alteração;
5. verificar healthcheck e logs;
6. validar a aplicação;
7. registrar mudanças importantes.

## Verificar uma stack

```bash
docker compose ps
docker compose config
docker compose logs --tail=100
```

Para acompanhar um serviço:

```bash
docker compose logs -f <serviço>
```

## Atualização

Preferir versões de imagem conhecidas e reproduzíveis.

Fluxo:

```text
backup → atualização → recriação → healthcheck → logs → teste
```

Se houver migration de banco, fazer backup antes e seguir as instruções da aplicação.

## Rollback

Rollback pode exigir tanto a versão anterior da imagem quanto a restauração do banco. Trocar apenas a tag da imagem não desfaz migrations de schema.

## Caddy

Depois de alterar a configuração:

```bash
sudo caddy validate --config /etc/caddy/Caddyfile
sudo systemctl reload caddy
sudo systemctl status caddy
```

## WireGuard

Verificar o túnel:

```bash
sudo wg show
```

Antes de fechar o SSH público ou alterar regras de firewall, confirme o handshake do WireGuard e teste uma segunda sessão administrativa pelo endereço privado.

## Secrets

Arquivos <code>.env</code> reais não pertencem ao Git. O repositório deve conter somente <code>.env.example</code>.

Nunca versionar senhas, tokens, chaves privadas, secrets de OIDC ou chaves de criptografia.

## Incidentes

Em caso de indisponibilidade:

1. não apagar dados;
2. registrar o estado;
3. verificar conectividade;
4. verificar containers;
5. verificar logs;
6. verificar Caddy/WireGuard quando aplicável;
7. restaurar backup somente depois de entender o estado dos dados;
8. documentar causa e correção.
