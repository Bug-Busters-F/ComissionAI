# Instalação e execução — ComissionAI

Guia para desenvolvimento local: PostgreSQL, backend e IA com Docker; frontend com npm.

## 1. Pré-requisitos

- Git.
- Docker em execução e Docker Compose v2.
- Node.js 24 e npm. Alternativa compatível com os testes: Node.js 22.12 ou superior na linha 22.
- Chave de API de um provedor de IA. O exemplo abaixo usa Groq.

Não é necessário instalar Java, Maven ou Python para esse caminho.

## 2. Clonar os repositórios

Os comandos usam `dev`, onde estão as funcionalidades da Sprint 3. Depois da publicação em `main`, substitua `--branch dev` por `--branch main` nos três comandos. Use versões compatíveis dos três projetos.

```bash
mkdir ComissionAI
cd ComissionAI
git clone --branch dev https://github.com/Bug-Busters-F/ComissionAI-frontend.git
git clone --branch dev https://github.com/Bug-Busters-F/ComissionAI-backend.git
git clone --branch dev https://github.com/Bug-Busters-F/ComissionAI-ai.git
```

Execute as próximas etapas em terminais separados, nas pastas indicadas.

## 3. Configurar e iniciar a IA

Na pasta `ComissionAI-ai`, copie `.env.example` para `.env`:

```bash
cp .env.example .env
```

No PowerShell, use `Copy-Item .env.example .env`.

Edite estas opções no `.env`, mantendo as demais configurações do exemplo:

```dotenv
LLM_PROVIDER=groq
LLM_SDK=groq
LLM_API_KEY=SUA_CHAVE_DA_GROQ
LLM_MODEL=openai/gpt-oss-20b
LLM_MAX_OUTPUT_TOKENS=4096
BACKEND_BASE_URL=http://host.docker.internal:8080
AGENT_USE_LLM_DECISIONS=false
APP_PORT=8000
AI_SERVICE_API_KEY=SUA_CHAVE_INTERNA_COMPARTILHADA
AI_SERVICE_AUTH_HEADER=X-Internal-Api-Key
AI_INBOUND_AUTH_OBRIGATORIA=true
```

Substitua os valores `SUA_CHAVE_*`. A chave da Groq autentica o provedor. A chave interna é um segredo definido por você, igual no backend e na IA; não é a chave da Groq e não vai no frontend. O modelo precisa estar disponível na sua conta.

```bash
docker compose up --build -d
docker compose logs -f ai
```

`Ctrl+C` encerra somente o acompanhamento dos logs.

- Disponibilidade: http://localhost:8000/health
- Documentação: http://localhost:8000/docs

O `/health` confirma que a API iniciou; não testa a chave do provedor nem a interpretação.

## 4. Configurar e iniciar backend e banco

Na pasta `ComissionAI-backend/backend`, crie um arquivo `.env` com a mesma chave interna da IA:

```dotenv
AI_SERVICE_API_KEY=SUA_CHAVE_INTERNA_COMPARTILHADA
AI_SERVICE_AUTH_HEADER=X-Internal-Api-Key
```

Inicie os serviços:

```bash
docker compose up --build -d
docker compose ps
docker compose logs -f backend
```

O Compose cria o PostgreSQL e executa as migrações automaticamente.

| Serviço | Endereço/configuração local |
| --- | --- |
| Backend | http://localhost:8080 |
| Swagger | http://localhost:8080/swagger-ui.html |
| PostgreSQL | localhost:5432 |
| Banco | bugbusters_db |
| Usuário / senha do banco | postgres / postgres |

Os Compose incluem o mapeamento de `host.docker.internal`, usado na comunicação entre backend e IA.

Se personalizar as credenciais do banco, ajuste também `POSTGRES_USER` e `POSTGRES_PASSWORD` no ambiente do serviço `backend`: são os nomes lidos pela aplicação; `DB_USER` e `DB_PASSWORD` declarados no Compose não substituem esses valores.

## 5. Configurar e iniciar o frontend

Na pasta `ComissionAI-frontend`, copie `.env.example` para `.env` e configure:

```dotenv
VITE_API_URL=/api/v1
VITE_BACKEND_URL=http://127.0.0.1:8080
VITE_CAMPAIGN_PROPOSALS_MODE=api
VITE_ENABLE_WORKFLOW_PREVIEW=false
VITE_ANOMALIES_DEMO=false
```

```bash
npm ci
npm run dev
```

Abra o endereço informado pelo Vite, normalmente http://localhost:5173. O proxy encaminha `/api` ao backend. Reinicie o Vite após alterar o `.env`.

### Primeiro acesso

Em um banco novo, o backend cria:

- E-mail: `admin@comissai.local`
- Senha: `Admin@123`

Entre no sistema, altere a senha em Perfil e cadastre os demais usuários em Gerenciamento. Não existe cadastro público.

## 6. Conferir a integração

1. Confira `docker compose ps` nas pastas da IA e do backend.
2. Acesse o `/health` da IA e a documentação do backend.
3. Entre no frontend e teste a interpretação de uma proposta. Essa ação usa a API real e pode consumir a cota do provedor.
4. Importe RH, Vendas e Taxas para usar simulação e apuração. O banco novo não contém bases nem campanhas de exemplo.
5. Para apurar uma campanha publicada, vincule os três tipos de base da competência à campanha. Importar em Dados não cria esses vínculos automaticamente.

### Testes

| Componente | Pasta | Comando |
| --- | --- | --- |
| Frontend | ComissionAI-frontend | `npm test` e `npm run build` |
| Backend — Linux/macOS | ComissionAI-backend/backend | `sh mvnw test` |
| Backend — PowerShell | ComissionAI-backend/backend | `.\mvnw.cmd test` |
| IA — ambiente virtual ativo | ComissionAI-ai | `python -m unittest discover -s tests` |

Os testes do backend fora do Docker exigem JDK 21. Para testar a IA, use Python 3.12, ambiente virtual e dependências de `requirements.txt`, mais o SDK escolhido. Execute com configuração isolada de testes: uma chave interna real no `.env` pode bloquear testes de rota que não enviam esse cabeçalho. Os testes específicos de autenticação configuram suas próprias credenciais.

O build Docker do backend não executa a suíte. O proxy descrito é de desenvolvimento; `npm run preview` não substitui a configuração de encaminhamento da API na hospedagem do build.

## 7. Alternativa: IA local, sem container

Não execute a IA em Docker e localmente na mesma porta.

Na pasta `ComissionAI-ai`, com Python 3.12:

```bash
python -m venv venv
```

Ative com `source venv/bin/activate` no Linux/macOS ou `.\venv\Scripts\Activate.ps1` no PowerShell. No Linux, use `python3` para criar o ambiente se necessário.

```bash
python -m pip install -r requirements.txt
python -m pip install groq
python -m uvicorn app.main:app --app-dir src --host 0.0.0.0 --port 8000 --reload
```

Nesse caso, use `BACKEND_BASE_URL=http://localhost:8080` no `.env` da IA. Mantenha a mesma chave interna do backend.

## 8. Encerrar e resolver problemas comuns

- Frontend ou IA local: `Ctrl+C` no respectivo terminal.
- IA em Docker: `docker compose down` na pasta `ComissionAI-ai`.
- Backend e banco: `docker compose down` na pasta `ComissionAI-backend/backend`.
- Esses comandos preservam os dados. Para resetar o banco, use `docker compose down -v` na pasta do backend e depois `docker compose up -d`; isso apaga todas as bases, usuários e campanhas daquele volume e recria o administrador inicial.

| Sintoma | Conferir |
| --- | --- |
| Frontend sem conexão | Backend ativo, URL do proxy e porta informada pelo Vite |
| IA rejeita a chamada interna | Mesma chave e mesmo cabeçalho no backend e na IA; recrie os containers após alterar os arquivos `.env` |
| IA não consulta o backend | `BACKEND_BASE_URL`: `host.docker.internal` no Docker; `localhost` na execução local |
| Provedor retorna 429 | Cota e limite de requisições do provedor; aguarde ou ajuste o plano/modelo |
| Simulação/apuração bloqueada | Diagnóstico das bases, datas de admissão/venda, cobertura das taxas e vínculos da campanha |
| Porta ocupada | Outra instância local ou container usando 5173, 8080, 8000 ou 5432 |

Não adicione os arquivos `.env` com credenciais ao Git.
