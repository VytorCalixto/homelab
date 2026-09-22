# VPS

Host público da infraestrutura.

## Papel

A VPS funciona como ponto de entrada da Internet e hospeda o reverse proxy, a rede privada, o gerenciamento e os serviços migrados.

## Componentes

- Caddy
- WireGuard
- Authentik
- Komodo Core
- Aplicações Docker migradas

## Rede

O acesso entre VPS e homelab usa WireGuard. A VPS não deve expor portas de bancos de dados, Redis ou portas internas das aplicações.

## Publicação

O Caddy roda no host e recebe o tráfego público. As aplicações devem preferencialmente publicar suas portas apenas em `127.0.0.1` quando o acesso ocorrer pelo Caddy.

## Persistência

Dados das aplicações ficam em diretórios do host, normalmente abaixo de `/srv/homelab/<serviço>/`. O Compose de cada stack é a referência para os mounts.

## Administração

Komodo é usado para gerenciar as aplicações Docker. SSH deve ser restringido à rede administrativa/WireGuard depois que o acesso pelo túnel estiver validado.
