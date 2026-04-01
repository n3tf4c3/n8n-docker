# n8n-docker

Stack local do n8n com PostgreSQL (pgvector) e Redis via Docker Compose.

## Servicos

- `postgres`: banco de dados principal do n8n.
- `redis`: fila para execucao em modo worker.
- `n8n-editor`: interface web do n8n.
- `n8n-worker`: processamento de jobs em background.
- `ngrok` (opcional): exemplo no compose, atualmente comentado.

## Pre-requisitos

- Docker e Docker Compose instalados.
- Git (opcional, para versionamento).

## Configuracao

1. Copie o arquivo de exemplo de ambiente:

```bash
cp .env.example .env
```

No PowerShell:

```powershell
Copy-Item .env.example .env
```

2. Edite o `.env` e ajuste, no minimo:
- `POSTGRES_PASSWORD`
- `WEBHOOK_URL`
- `TZ` (se necessario)

3. Crie a rede externa esperada pelo compose:

```bash
docker network create observability_net
```

## Subir o ambiente

```bash
docker compose up -d
```

Abrir n8n em:
- `http://localhost:5678` (ou a porta definida em `N8N_EDITOR_PORT`)

## Comandos uteis

Subir/recriar servicos:

```bash
docker compose up -d --force-recreate
```

Ver logs:

```bash
docker compose logs -f n8n-editor n8n-worker postgres redis
```

Parar ambiente:

```bash
docker compose down
```

Parar ambiente e remover volumes nomeados/rede criada pelo compose:

```bash
docker compose down -v
```

## Persistencia de dados

Os dados locais ficam nestas pastas (ignoradas no Git):

- `./n8n-data`
- `./postgres-data`
- `./redis-data`

## Observacoes

- Este projeto usa imagens fixadas por digest no `.env.example`, o que melhora reprodutibilidade.
- `NODE_TLS_REJECT_UNAUTHORIZED=0` esta ativo no compose para ambiente local/dev. Nao use esse ajuste em producao.
- Para usar `ngrok`, descomente o servico correspondente no `docker-compose.yml` e configure `NGROK_*` no `.env`.
