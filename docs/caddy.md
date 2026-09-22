# Caddy

O Caddy é o reverse proxy público da VPS. Ele roda diretamente no host e é responsável por TLS e encaminhamento para as aplicações.

## Organização

O arquivo principal é `config/caddy/Caddyfile`, que importa `config/caddy/*.caddy`. Cada serviço publicado deve preferencialmente ter seu próprio arquivo.

Exemplo: `config/caddy/securo.caddy`.

```caddy
securo.rotiav.com.br {
    reverse_proxy :3132
}
```

## Publicação

Aplicação na VPS:

```caddy
app.example.com {
    reverse_proxy 127.0.0.1:PORTA
}
```

Aplicação no homelab:

```caddy
app.example.com {
    reverse_proxy <IP-WIREGUARD-HOMELAB>:PORTA
}
```

## Alteração

Validar antes de recarregar:

```bash
sudo caddy validate --config /etc/caddy/Caddyfile
sudo systemctl reload caddy
sudo systemctl status caddy
```

Logs:

```bash
journalctl -u caddy --since "10 minutes ago"
```

## Checklist

- [ ] DNS aponta para a VPS.
- [ ] Aplicação está saudável.
- [ ] Aplicação não está exposta diretamente na Internet.
- [ ] Porta local está correta.
- [ ] Caddyfile é válido.
- [ ] HTTPS funciona.
- [ ] Logs mostram as requisições esperadas.
- [ ] Autenticação está configurada quando necessária.
- [ ] Endpoint administrativo não ficou público por engano.

## Autenticação

Quando a aplicação suporta OIDC, preferir a integração direta com Authentik em vez de adicionar uma camada genérica de autenticação no Caddy. Isso preserva os claims e o contexto de identidade esperados pela aplicação.