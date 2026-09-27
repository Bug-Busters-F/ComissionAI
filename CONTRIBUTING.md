# Instalação e execução — ComissionAI

Este guia reúne a preparação do ambiente, a instalação e a execução dos três repositórios do ComissionAI, projeto da equipe Bug Busters desenvolvido em parceria com a Dom Rock.

## 1. Repositórios e arquitetura

| Repositório | Responsabilidade | Tecnologias |
| --- | --- | --- |
| [ComissionAI-frontend](https://github.com/Bug-Busters-F/ComissionAI-frontend) | Interface web | Vue 3, Vite, Tailwind CSS, Pinia e Vue Router |
| [ComissionAI-backend](https://github.com/Bug-Busters-F/ComissionAI-backend) | API, regras de negócio e persistência | Java 21, Spring Boot, Maven, PostgreSQL e Flyway |
| [ComissionAI-ai](https://github.com/Bug-Busters-F/ComissionAI-ai) | Interpretação de regras em linguagem natural | Python, FastAPI, Pydantic e SDK do provedor de LLM |

O fluxo previsto é **front-end → back-end → serviço de IA**. O back-end acessa o PostgreSQL; o serviço de IA recebe o contexto pela API e não acessa diretamente o banco de negócio.

## 2. Pré-requisitos

| Ferramenta | Quando é necessária |
| --- | --- |
| Git e acesso aos repositórios no GitHub | Clonagem dos projetos |
| Node.js 22.x e npm | Front-end; versão compatível com o Vite registrado no projeto |
| Docker em execução e Docker Compose v2 | Back-end, banco e IA pelo caminho Docker |
| JDK 21 | Apenas para executar o back-end fora do Docker/Dev Container |
| Python 3.12 | Apenas para executar a IA fora do Docker |
| VS Code e extensão Dev Containers | Apenas para a alternativa de desenvolvimento em container |
| Chave de API de um provedor de LLM suportado | Interpretação real de textos pela IA |

Instale as ferramentas correspondentes ao caminho escolhido. No Windows, o Docker Desktop fornece Docker e Compose. O Maven Wrapper do back-end permite executar o projeto sem instalar Maven separadamente.

Confira as ferramentas no terminal:

```bash
git --version
node --version
npm --version
docker --version
docker compose version
```

Para execução local, confira também `java -version` e `python --version`. Em sistemas que usam o comando `python3`, substitua `python` por `python3` na criação do ambiente virtual.

## 3. Clonar os projetos

Execute em uma pasta destinada ao projeto:

```bash
mkdir ComissionAI
cd ComissionAI
git clone https://github.com/Bug-Busters-F/ComissionAI-frontend.git
git clone https://github.com/Bug-Busters-F/ComissionAI-backend.git
git clone https://github.com/Bug-Busters-F/ComissionAI-ai.git
```

Se você já possui os repositórios na máquina, utilize as pastas existentes.

Os comandos das próximas seções informam a pasta de execução. Use terminais separados para os processos que permanecerem em primeiro plano.

## 4. Caminho principal: IA e back-end com Docker

Este caminho utiliza os arquivos Compose existentes em cada repositório. O front-end será executado com npm. Execute os comandos Compose na pasta de cada serviço, conforme indicado abaixo.

### 4.1. Configurar e iniciar a IA

Na pasta `ComissionAI-ai`, copie o arquivo de exemplo:

**Windows — PowerShell:**

```powershell
Copy-Item .env.example .env
```

**Linux/macOS:**

```bash
cp .env.example .env
```

Edite o `.env`:

```dotenv
LLM_PROVIDER=gemini
LLM_SDK=google-genai
LLM_API_KEY=SUBSTITUA_PELA_SUA_CHAVE
LLM_MODEL=SUBSTITUA_PELO_MODELO_DISPONIVEL_NA_SUA_CONTA
LLM_TEMPERATURE=0.0
LLM_TIMEOUT_SECONDS=30
LLM_MAX_OUTPUT_TOKENS=1024
APP_HOST=0.0.0.0
APP_PORT=8000
APP_RELOAD=false
```

Substitua os dois valores indicados antes de executar. `LLM_MODEL` pode ficar vazio para usar o padrão implementado pelo provedor no código, mas esse modelo precisa estar disponível na sua conta. Ao trocar de provedor, ajuste também o SDK e o modelo.

| Provedor | `LLM_PROVIDER` | `LLM_SDK` |
| --- | --- | --- |
| Gemini | `gemini` | `google-genai` |
| Groq | `groq` | `groq` |
| OpenAI | `openai` | `openai` |
| Anthropic | `anthropic` | `anthropic` |

Inicie o serviço:

```bash
docker compose up --build -d
docker compose ps
docker compose logs -f ai
```

Use `Ctrl+C` para sair do acompanhamento de logs; o container continuará ativo. O build instala as dependências e o SDK definido em `LLM_SDK`. Reconstrua a imagem se trocar o SDK.

- API: `http://localhost:8000`
- Swagger: `http://localhost:8000/docs`
- Verificação de disponibilidade: `http://localhost:8000/health`

O endpoint `/health` retorna `{"status":"ok"}` quando a API está disponível. Ele não valida a chave nem a conectividade com o provedor de LLM.

### 4.2. Iniciar back-end e banco

Em outro terminal, na pasta `ComissionAI-backend/backend`:

```bash
docker compose up --build -d
docker compose ps
docker compose logs -f backend
```

Esse Compose inicia o PostgreSQL 16 e o back-end, usando:

| Configuração | Valor do Compose |
| --- | --- |
| Banco | `bugbusters_db` |
| Usuário e senha locais | `postgres` / `postgres` |
| Banco acessível pela máquina | `localhost:5432` |
| Banco acessível pelo back-end em container | `postgres:5432` |
| Serviço de IA acessível pelo back-end | `http://host.docker.internal:8000` |
| Back-end | `http://localhost:8080` |
| Swagger do back-end | `http://localhost:8080/swagger-ui.html` |

O Compose já inclui o mapeamento `host.docker.internal:host-gateway`, permitindo acessar a porta da IA publicada na máquina. Os dois Compose podem funcionar em redes separadas com essa configuração.

**Atenção à configuração de credenciais:** o Compose do back-end declara `DB_USER` e `DB_PASSWORD`, mas o `application.yml` lê `POSTGRES_USER` e `POSTGRES_PASSWORD`. Os valores padrão coincidem e permitem a configuração local apresentada. Se personalizar as credenciais, use os nomes lidos pelo `application.yml` no serviço `backend` e mantenha-os coerentes com o serviço `postgres`.

As migrações Flyway estão habilitadas no perfil `dev` e são executadas na inicialização. Se o banco ainda estiver iniciando, acompanhe os logs; o back-end possui política de reinício no Compose.

### 4.3. Instalar e iniciar o front-end

Em outro terminal, na pasta `ComissionAI-frontend`:

```bash
npm ci
npm run dev
```

`npm ci` instala as versões do `package-lock.json`. Abra o endereço informado pelo Vite no terminal.

Para gerar e visualizar o build:

```bash
npm run build
npm run preview
```

**Estado da integração:** o front-end contém a estrutura inicial, mas ainda não possui configuração de URL da API, proxy ou chamadas HTTP implementadas. Iniciar os três serviços não significa que a interface já esteja integrada. A comunicação com o back-end e a configuração de CORS ou proxy ainda precisam ser implementadas; não há uma variável `VITE_*` existente para preencher.

## 5. Alternativa: executar IA e back-end localmente

Use esta alternativa para desenvolver com recarga local ou depurar os serviços. Ela substitui a execução dos respectivos containers da seção 4; não inicie duas instâncias na mesma porta.

### 5.1. IA com ambiente virtual Python

Na pasta `ComissionAI-ai`, crie e ative o ambiente:

**Windows — PowerShell:**

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**Linux/macOS:**

```bash
python3 -m venv venv
source venv/bin/activate
```

Com o ambiente ativo:

```bash
python -m pip install -r requirements.txt
python -m pip install google-genai
```

Instale apenas o SDK do provedor escolhido: substitua `google-genai` por `groq`, `openai` ou `anthropic` quando necessário. O `requirements.txt` não instala esses SDKs automaticamente.

Prepare o `.env` conforme a seção 4.1 e inicie:

```bash
python -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --app-dir src --reload
```

Os parâmetros desse comando definem host, porta e recarga. Alterar apenas `APP_HOST`, `APP_PORT` ou `APP_RELOAD` no `.env` não substitui os argumentos do Uvicorn. No Compose, `APP_PORT` altera a porta publicada na máquina; a porta interna continua sendo `8000`.

Se o PowerShell bloquear a ativação do ambiente, use diretamente `.\venv\Scripts\python.exe` no lugar de `python` nos comandos de instalação, execução e testes.

### 5.2. Banco para desenvolvimento local

Na raiz de `ComissionAI-backend`, inicie somente o banco da configuração de desenvolvimento:

```bash
docker compose -f .devcontainer/docker-compose.yml up -d db
```

Esse caminho usa **`comissai_db` em `localhost:5433`**, diferente do banco da seção 4.2. São configurações e volumes separados; os dados não são compartilhados automaticamente.

### 5.3. Back-end com JDK 21 e Maven Wrapper

Na pasta `ComissionAI-backend/backend`:

**Windows — PowerShell:**

```powershell
$env:SPRING_PROFILES_ACTIVE="dev"
$env:DB_HOST="localhost"
$env:DB_PORT="5433"
$env:DB_NAME="comissai_db"
$env:POSTGRES_USER="postgres"
$env:POSTGRES_PASSWORD="postgres"
$env:AI_SERVICE_URL="http://localhost:8000"
.\mvnw.cmd spring-boot:run
```

**Linux/macOS:**

```bash
export SPRING_PROFILES_ACTIVE=dev
export DB_HOST=localhost
export DB_PORT=5433
export DB_NAME=comissai_db
export POSTGRES_USER=postgres
export POSTGRES_PASSWORD=postgres
export AI_SERVICE_URL=http://localhost:8000
sh mvnw spring-boot:run
```

O Wrapper baixa a distribuição Maven e as dependências necessárias. Se Maven já estiver instalado, `mvn spring-boot:run` é equivalente.

As variáveis acima valem para o terminal atual. A configuração atual do back-end não carrega automaticamente um arquivo `.env`.

### 5.4. Alternativa do back-end com Dev Container

1. Instale VS Code, extensão Dev Containers e mantenha o Docker em execução.
2. Abra a raiz de `ComissionAI-backend` e execute **Dev Containers: Reopen in Container** pela paleta de comandos.
3. Confira o caminho de trabalho: o `devcontainer.json` fixa `/workspaces/comissionai-backend`; se a montagem usar outro nome, ajuste `workspaceFolder` para o caminho efetivamente montado.
4. No terminal do container, entre em `backend` e execute:

   ```bash
   export AI_SERVICE_URL=http://host.docker.internal:8000
   mvn spring-boot:run
   ```

O ambiente fornece Java 21 e Maven. O banco é acessado internamente por `db:5432`, com o nome `comissai_db`. No Docker Desktop, `host.docker.internal` permite acessar a IA publicada na máquina. No Docker Engine Linux, adicione `extra_hosts: ["host.docker.internal:host-gateway"]` ao serviço `app` do Compose de desenvolvimento se esse nome não resolver.

## 6. Verificação e testes

Após iniciar os serviços:

1. Abra o endereço do front-end informado pelo Vite.
2. Abra o Swagger do back-end em `http://localhost:8080/swagger-ui.html`.
3. Consulte `http://localhost:8000/health` e abra `http://localhost:8000/docs`.
4. Para validar a comunicação back-end → IA, use no Swagger do back-end a rota `POST /api/v1/interpretador/extrair-regra`, seguindo o esquema de entrada exibido. Essa verificação requer credenciais válidas e pode consumir a cota do provedor.

Para verificar cada componente, execute:

| Componente | Pasta | Comando |
| --- | --- | --- |
| Front-end | `ComissionAI-frontend` | `npm run build` |
| Back-end — Windows | `ComissionAI-backend/backend` | `.\mvnw.cmd test` |
| Back-end — Linux/macOS | `ComissionAI-backend/backend` | `sh mvnw test` |
| Back-end — Dev Container | `backend`, dentro do container | `mvn test` |
| IA — ambiente virtual ativo | `ComissionAI-ai` | `python -m unittest discover -s tests` |

O front-end ainda não define scripts de testes ou lint no `package.json`; complemente o build com a verificação manual das telas alteradas. O build Docker do back-end pula testes, portanto não substitui a execução da suíte. A imagem Docker da IA não copia `tests/` nem `scripts/`; execute os testes pelo ambiente local.

Para testar especificamente o acesso real ao provedor de LLM, na raiz da IA e com o ambiente virtual ativo:

```bash
python scripts/test_llm_live.py --smoke
```

## 7. Logs, banco e encerramento

Execute cada comando na pasta indicada:

| Objetivo | Pasta | Comando |
| --- | --- | --- |
| Logs da IA | `ComissionAI-ai` | `docker compose logs -f ai` |
| Logs do back-end | `ComissionAI-backend/backend` | `docker compose logs -f backend` |
| Acessar o banco do caminho Docker | `ComissionAI-backend/backend` | `docker compose exec postgres psql -U postgres -d bugbusters_db` |
| Acessar o banco do desenvolvimento local | `ComissionAI-backend` | `docker compose -f .devcontainer/docker-compose.yml exec db psql -U postgres -d comissai_db` |
| Encerrar a IA em Docker | `ComissionAI-ai` | `docker compose down` |
| Encerrar back-end e banco do caminho Docker | `ComissionAI-backend/backend` | `docker compose down` |
| Encerrar os containers de desenvolvimento | `ComissionAI-backend` | `docker compose -f .devcontainer/docker-compose.yml down` |

Encerre processos locais com `Ctrl+C`. Os comandos `down` acima preservam os volumes de banco; adicionar `-v` remove esses dados.

Se houver falha de conexão, confira primeiro a configuração correspondente ao caminho escolhido: banco em `5432` ou `5433`, nome do banco e `AI_SERVICE_URL`. Dentro de um container, `localhost` aponta para o próprio container.

---

Equipe Bug Busters
