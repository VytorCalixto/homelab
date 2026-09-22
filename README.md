# Homelab

Infraestrutura como código para o homelab e a VPS.

O repositório contém os Docker Compose, configurações de infraestrutura e documentação necessários para manter o ambiente reproduzível e facilitar a migração de serviços do homelab para a VPS.

## Arquitetura

O ambiente é dividido em dois hosts:
- **VPS** — entrada pública, Caddy, WireGuard, Authentik, Komodo e serviços migrados.
- **Homelab** — serviços que permanecem na infraestrutura local.

O tráfego entre os hosts usa WireGuard. Serviços públicos são publicados pelo Caddy em vez de exporem diretamente suas portas de aplicação.

```text
                         Internet
                            │
                       ┌────▼────┐
                       │   VPS   │
                       │ Caddy   │
                       │ Authentik
                       │ Komodo  │
                       │ Apps    │
                       └────┬────┘
                            │
                       WireGuard
                            │
                       ┌────▼────┐
                       │ Homelab │
                       │  Apps   │
                       └─────────┘
```

## Estrutura

```text
.
├── config/
│   └── caddy/              # Configuração do reverse proxy
├── hosts/
│   ├── homelab/            # Documentação do host local
│   └── vps/                # Documentação da VPS
├── stacks/
│   ├── homelab/            # Stacks que permanecem no homelab
│   ├── vps/                # Stacks executadas na VPS
│   ├── authentik/           # Identidade / SSO
│   ├── komodo/              # Gerenciamento das aplicações
│   └── postgres/            # PostgreSQL de infraestrutura
└── docs/                    # Documentação geral
```

Cada aplicação deve, sempre que possível, ter sua própria stack. Dados persistentes ficam fora do Git, em diretórios do host.

## Documentação

- [Documentação geral](docs/README.md)
- [Arquitetura](docs/architecture.md)
- [Operação](docs/operations.md)
- [Migração de serviços](docs/migrations.md)
- [Segurança](docs/security.md)
- [Caddy](docs/caddy.md)

Também existem documentos específicos em [hosts](hosts/) e, quando necessário, READMEs dentro das próprias stacks.

## Deploy

As aplicações Docker são gerenciadas pelo Komodo. Os Compose versionados neste repositório são a fonte de configuração das stacks.

Para operações manuais ou troubleshooting, consulte [Operação](docs/operations.md).

## Secrets

Secrets e arquivos `.env` reais não são versionados.

O repositório deve conter somente arquivos `.env.example` com os nomes e formatos das variáveis necessárias. Secrets de produção são configurados no host ou no Komodo.

## Princípios

- Configuração versionada, dados fora do Git.
- Serviços públicos atrás do Caddy.
- Comunicação privada entre VPS e homelab via WireGuard.
- Uma aplicação por stack quando isso simplificar operação e rollback.
- Backups antes de migrações e alterações de banco.
- Migração incremental, com validação antes de desligar a origem.
- Secrets nunca entram no Git.

## Status

A infraestrutura está em migração gradual do homelab para a VPS. A documentação representa o estado desejado e deve ser atualizada junto com as mudanças.