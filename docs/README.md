# Documentação

Esta documentação descreve a arquitetura, operação e manutenção do homelab e da VPS.

## Comece por aqui

- [Arquitetura](architecture.md) — hosts, rede, publicação e responsabilidades.
- [Operação](operations.md) — como subir, atualizar, verificar e diagnosticar stacks.
- [Migração de serviços](migrations.md) — como mover aplicações e dados do homelab para a VPS.
- [Segurança](security.md) — firewall, SSH, WireGuard, Caddy e proteção contra abuso.
- [Caddy](caddy.md) — publicação dos serviços e organização da configuração.

## Estrutura

```text
.
├── config/caddy/       # Configuração do Caddy
├── hosts/              # Documentação por host
├── stacks/             # Docker Compose por aplicação/infraestrutura
└── docs/               # Documentação geral
```

## Princípios

1. Configuração fica versionada; dados persistentes ficam nos hosts.
2. Cada aplicação deve ter uma stack independente sempre que possível.
3. Serviços públicos passam pelo Caddy.
4. A comunicação VPS ↔ homelab usa WireGuard.
5. Migrações são feitas uma aplicação por vez.
6. Segredos nunca são versionados. O Git contém apenas exemplos.

A documentação acompanha a infraestrutura real. Quando uma decisão arquitetural mudar, atualize a documentação junto com a configuração.
