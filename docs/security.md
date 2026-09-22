# Segurança

## Camadas

```text
Internet
  │
  ▼
Firewall
  │
  ▼
Caddy
  │
  ▼
Aplicação
  │
  ▼
Authentik / autenticação
```

Nenhuma camada substitui as demais.

## Firewall

Portas públicas esperadas na VPS:

| Porta | Protocolo | Uso |
|---|---|---|
| 80 | TCP | HTTP / redirect |
| 443 | TCP | HTTPS |
| 443 | UDP | HTTP/3, se habilitado |
| 51820 | UDP | WireGuard |
| 22 | TCP | SSH, inicialmente |

Não publicar diretamente PostgreSQL, Redis, Komodo/Periphery ou portas internas das aplicações.

## SSH

O objetivo é restringir SSH à rede administrativa/WireGuard depois de validar o túnel.

Antes de restringir:

1. confirmar handshake;
2. abrir uma segunda sessão pelo endereço privado;
3. testar comandos administrativos;
4. só então alterar o firewall.

## Fail2Ban

Fail2Ban pode proteger serviços com logs adequados, especialmente SSH.

Verificar:

```bash
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

Confirmar backend de logs, filtro, identificação do IP real e exclusões da rede administrativa.

Fail2Ban complementa o firewall e não o substitui.

## Caddy

Os access logs devem permitir identificar IP, host, método, URI, status, User-Agent e duração.

Não tratar Caddy sozinho como sistema completo de detecção de bots ou abuso HTTP.

## Proteção HTTP

Quando houver necessidade:

1. começar com logs;
2. identificar padrões de abuso;
3. avaliar rate limiting;
4. avaliar CrowdSec ou solução equivalente;
5. começar em observação;
6. validar falsos positivos;
7. só então aplicar bloqueios.

## Authentik

Authentik centraliza autenticação para aplicações compatíveis com OIDC.

SSO não elimina a necessidade de firewall, HTTPS, atualização das aplicações e proteção dos secrets.

## Docker

Quando uma porta só precisa ser acessada localmente pelo Caddy:

```yaml
ports:
  - "127.0.0.1:PORTA:PORTA"
```

Quando aplicações precisam conversar entre si, preferir uma rede Docker interna em vez de publicar a porta.

Sempre que suportado, usar versões conhecidas, usuário não-root, capabilities mínimas e <code>no-new-privileges</code>.

## Secrets

Nunca versionar senhas, tokens, chaves privadas, secrets de OIDC ou chaves de criptografia.

Se um secret for exposto, considerá-lo comprometido e fazer rotação.
