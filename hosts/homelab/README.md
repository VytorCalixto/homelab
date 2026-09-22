# Homelab

Host da infraestrutura local.

## Papel

Mantém serviços que não serão migrados para a VPS ou que dependem de recursos locais, armazenamento e acesso à rede doméstica.

## Gerenciamento

As aplicações são executadas em Docker. O estado da máquina e os dados persistentes ficam fora deste repositório; o Compose versionado descreve como os serviços devem ser executados.

## Rede

Quando um serviço local precisar ser publicado externamente, o tráfego deve entrar pela VPS e seguir pelo WireGuard. O serviço não deve receber exposição pública direta.

## Persistência

Os diretórios persistentes e a política de backup de cada aplicação devem ser documentados antes de uma migração ou alteração destrutiva.
