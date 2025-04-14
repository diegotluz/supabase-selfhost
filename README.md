# Supabase Docker

Este repositório contém uma configuração mínima do Docker Compose para auto-hospedar o Supabase. Siga as instruções abaixo para configurar e executar o ambiente.

## Pré-requisitos

Certifique-se de ter os seguintes itens instalados em sua máquina:

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Como usar este repositório

### 1. Clonar o repositório

```bash
git clone <URL_DO_REPOSITORIO>
cd docker
```

### 2. Configurar variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto e configure as variáveis de ambiente necessárias. Um exemplo de configuração pode ser encontrado na [documentação oficial do Supabase](https://supabase.com/docs/guides/hosting/docker).

### 3. Iniciar os serviços

Para iniciar os serviços, execute o seguinte comando:

```bash
docker compose up -d
```

### 4. Parar os serviços

Para parar os serviços, execute:

```bash
docker compose down
```

### 5. Resetar o ambiente

Para resetar completamente o ambiente (removendo volumes e containers órfãos):

```bash
./reset.sh
```

## Gerar JWT Secret e API Keys

Para configurar o Supabase corretamente, você precisará gerar um `JWT_SECRET`, bem como as chaves `ANON_KEY` e `SERVICE_ROLE_KEY`. Siga as etapas abaixo para gerar essas chaves:

### 1. Gerar o JWT Secret

O `JWT_SECRET` é usado para assinar e validar tokens JWT. Certifique-se de que ele seja uma string aleatória e segura com pelo menos 40 caracteres. Você pode usar um gerador de strings aleatórias ou criar manualmente.

Exemplo de `JWT_SECRET`:
```
2mOgHPHRN5LsMBcmjCeZTEundb3HLrJh62B0TjQ4
```

### 2. Gerar as API Keys

Com o `JWT_SECRET` gerado, você pode criar as chaves `ANON_KEY` e `SERVICE_ROLE_KEY`.

#### Gerar a `ANON_KEY`
A `ANON_KEY` é usada para acessos públicos e anônimos. Use o seguinte payload para gerar o token JWT:
```json
{
  "role": "anon",
  "iss": "supabase",
  "iat": 1744599600,
  "exp": 1902366000
}
```

#### Gerar a `SERVICE_ROLE_KEY`
A `SERVICE_ROLE_KEY` é usada para acessos administrativos. Use o seguinte payload para gerar o token JWT:
```json
{
  "role": "service_role",
  "iss": "supabase",
  "iat": 1744599600,
  "exp": 1902366000
}
```

Você pode usar ferramentas como [jwt.io](https://jwt.io) para gerar os tokens JWT com o `JWT_SECRET`.

### 3. Atualizar o arquivo `.env`

Após gerar as chaves, atualize o arquivo `.env` com os valores gerados:
```
JWT_SECRET=2mOgHPHRN5LsMBcmjCeZTEundb3HLrJh62B0TjQ4
ANON_KEY=<chave_anon_gerada>
SERVICE_ROLE_KEY=<chave_service_role_gerada>
```

### 4. Reiniciar os serviços

Reinicie os serviços para aplicar as alterações:
```bash
docker compose down
docker compose up -d
```

## Estrutura do Repositório

- `docker-compose.yml`: Configuração principal do Docker Compose.
- `volumes/`: Contém arquivos e configurações persistentes para os serviços.
  - `api/kong.yml`: Configuração do Kong Gateway.
  - `db/`: Scripts de inicialização e migração do banco de dados.
  - `functions/`: Funções Edge hospedadas no Supabase.
  - `storage/`: Configurações relacionadas ao serviço de armazenamento.
- `dev/`: Configurações adicionais para desenvolvimento.

## Serviços Disponíveis

Este repositório inclui os seguintes serviços:

- **Studio**: Interface de gerenciamento do Supabase.
- **Kong**: Gateway de API.
- **Auth**: Serviço de autenticação.
- **PostgREST**: API REST para o banco de dados PostgreSQL.
- **Realtime**: Serviço de sincronização em tempo real.
- **Storage**: Serviço de armazenamento de arquivos.
- **Edge Functions**: Execução de funções serverless.
- **Analytics**: Serviço de análise de logs.
- **PostgreSQL**: Banco de dados principal.

## Documentação

Para mais informações, consulte a [documentação oficial do Supabase](https://supabase.com/docs).

## Contribuição

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou enviar pull requests.
