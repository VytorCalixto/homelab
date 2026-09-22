# Arquitetura

## Visão geral

O ambiente é dividido em dois hosts:

- **VPS** — ponto de entrada público, reverse proxy, serviços migrados e componentes de gerenciamento.
- **Homelab** — serviços que permanecem em casa por dependerem de armazenamento, hardware ou rede local.

A comunicação privada entre os hosts usa WireGuard.

```text
                         Internet
                            │
                     ┌──────┴──────┐
                     │     VPS     │
                     │             │
                     │ Caddy       │
                     │ Authentik   │
                     │ Komodo Core │
                     │ Apps VPS    │
                     └──────┬──────┘
                            │
                      WireGuard VPN
                            │
                     ┌──────┴──────┐
                     │   Homelab   │
                     │             │
                     │ Apps locais │
                     └─────────────┘
```

## VPS

A VPS concentra Caddy, WireGuard, Authentik, Komodo Core e os serviços que foram migrados.

As aplicações Docker são preferencialmente gerenciadas pelo Komodo. O Caddy roda diretamente no host e termina TLS antes de encaminhar o tráfego.

## Homelab

O homelab mantém serviços que não serão migrados ou que precisam dos recursos locais.

Quando um serviço do homelab precisar ser acessado externamente:

```text
Internet → Caddy (VPS) → WireGuard → serviço no Homelab
```

A aplicação não deve precisar expor sua porta diretamente à Internet.

## Rede

A rede WireGuard deve ser tratada como uma rede privada entre os hosts. Antes de alterar firewall ou rotas, confirme os endereços reais em:

```bash
sudo wg show
```

Não usar o túnel como rota padrão sem uma necessidade explícita. Rotas devem ser adicionadas de forma deliberada.

## Docker

Aplicações que precisam conversar entre si podem compartilhar uma rede Docker externa, como <code>homelab-backend</code>.

Essa rede não substitui o firewall. Portas que só precisam ser acessadas pelo Caddy devem ficar vinculadas ao loopback do host.

## Persistência

Dados persistentes ficam em caminhos do host, normalmente abaixo de <code>/srv/homelab/&lt;serviço&gt;/</code>.

Não versionar bancos, uploads, mídia, arquivos <code>.env</code> reais ou dumps.

## Publicação

Padrão para aplicação na VPS:

```text
DNS → Caddy :443 → 127.0.0.1:<porta> → container
```

Padrão para aplicação no homelab:

```text
DNS → Caddy :443 (VPS) → WireGuard → IP privado do Homelab:<porta>
```
