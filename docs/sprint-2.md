# API 6º Semestre ADS — Gestão de Regras

## Documentação — Sprint 2

<p align="center">
  <img src="https://github.com/user-attachments/assets/7757bc1a-e087-46e7-8170-b423b614d4ac" alt="Logo da Bug Busters" width="250">
</p>
<h2 align="center">Bug Busters</h2>

<p align="center">
  <a href="#desafio">Desafio</a> |
  <a href="#goal">Sprint Goal</a> |
  <a href="#us">User Stories</a> |
  <a href="#bdd">Critérios de Aceitação (BDD)</a> |
  <a href="#entregas">Entregas Técnicas por Frente</a> |
  <a href="#dor">DoR</a> |
  <a href="#dod">DoD</a> |
  <a href="#burndown">Burndown</a> |
  <a href="#equipe">Equipe</a>
</p>

> **Status da Sprint:** Concluída  
> **Previsão de entrega:** 25/10/2026  
> **Data de início:** 28/09/2026  
> **Cliente:** Dom Rock  
> **Instituição:** Fatec São José dos Campos — ADS, 6º semestre (2026-2)

[Voltar ao README](../README.md)

---

## 🏅 Desafio e Dor do Negócio <a id="desafio"></a>

Garantir previsibilidade orçamentária, segurança jurídica e transparência operacional na formulação e ativação de campanhas de comissionamento de vendas. Antes da Sprint 2, a concepção de regras comerciais dependia de estimativas empíricas sem capacidade de testar o impacto financeiro real frente ao histórico de vendas, expondo a organização a riscos severos de estouro de teto orçamentário, criação de regras ambíguas ou aprovação de incentivos sem cobertura contratual nas bases salariais. 

A Sprint 2 teve como foco estruturar o ambiente de **Simulação em Sandbox** (cálculo de impacto financeiro em ambiente isolado sem persistência de lançamentos contábeis reais), viabilizar o fluxo de governança **Human-in-the-Loop (HITL)** com aceite formal prévio e ativação vinculada por versão vigente, fornecer **Explicabilidade Algorítmica (Explainable AI - XAI)** com snippets de código Python auditáveis gerados por agente autônomo com ferramentas (*tool calling*), além de implementar o **Fechamento Final de Competência** com versão retroativa e blindagem de reprocessamento e mutações.

---

## 🎯 Meta da Sprint (Sprint Goal) <a id="goal"></a>

Elevar o nível de maturidade da plataforma ComissionAI por meio de simulação financeira preventiva, governança humana e inteligência explicável, priorizando as **User Stories 7, 8 e 9** como núcleo da entrega:
1. **Simulação e Sandbox (US 07):** Permitir ao Head Comercial simular o impacto financeiro de campanhas complexas contra a massa histórica de vendas por competência, avaliando cenários de volume uniforme (80%, 100%, 120% e 150%), memória de cálculo detalhada venda a venda e bloqueio determinístico caso a referência seja insuficiente ou haja ausência de base contratual;
2. **Homologação Human-in-the-Loop e Fechamento (US 08):** Assegurar governança estrita na revisão e homologação explícita de regras, exigindo aceite prévio com validação de correspondência (*fingerprint*), ativação vinculada de versão de campanha e blindagem no fechamento final de competência contra reprocessamentos indevidos e alterações retroativas;
3. **Explainable AI e Agente Autônomo (US 09):** Disponibilizar ao Auditor de Compliance transparência absoluta (*XAI*) com geração de justificativas textuais e snippets de código Python equivalentes executados em sandbox seguro, sustentados por um agente interpretador v2 munido de ferramentas de consulta aos cadastros reais, reinterpretação pontual por bloco de regra e alta tolerância a rate limits.

---

## 📋 User Stories e Critérios de Aceitação <a id="us"></a>

### US 07 — Simulação de Propostas em Sandbox e Projeção Multi-Cenário
> **Como** Head Comercial,  
> **quero** simular o impacto financeiro de uma proposta de campanha contra o histórico de vendas de uma competência de referência,  
> **para** verificar sua aderência ao teto orçamentário e entender o comportamento de faixas de valor sem gerar comissões reais no banco de dados.
* **Story Points:** 8 | **Prioridade:** Alta | **Status:** Concluído
* **Critérios de Aceitação:**
  * Executar a simulação em modo sandbox utilizando o motor de cálculo compartilhado, garantindo 100% de paridade matemática com a apuração oficial sem persistir lançamentos em `tb_resultado_calculo`.
  * Recortar o público-alvo com base na união das regras da campanha (`OU` entre blocos, `E` dentro do bloco), desconsiderando vendas duplicadas por ID.
  * Projetar os cenários de variação de volume uniforme de **80%**, **100%**, **120%** e **150%**, recalculando individualmente a faixa de valor atingida por cada venda e o respectivo delta financeiro.
  * Exibir tabela paginada do público simulado com memória detalhada do cálculo (matrícula, loja, valor de venda, comissão base, comissão proposta, arredondamento `HALF_UP` e fórmula aplicada).
  * Projetar o montante orçamentário considerando a duração da campanha no mês comercial de **30 dias** (`(soma * dias) / 30`) e alertar visualmente caso o teto seja superado.
  * Bloquear a simulação com a mensagem impeditiva **"Referência insuficiente"** caso a competência selecionada resulte em zero vendas para o público da campanha, jamais apresentando `R$ 0,00` como estimativa válida.
* **Cenário de Teste (BDD):**
  * **Dado** que o gestor configure uma campanha com vigência de 15 dias, teto orçamentário de R$ 10.000,00 e taxa de 5% para a marca PRETO no canal ECOMMERCE;
  * **Quando** acionar a simulação contra a competência "07/2025";
  * **Então** o sistema deve processar o cálculo em sandbox sem gravar comissões reais;
  * **E** apresentar os totais projetados para 80%, 100%, 120% e 150%, indicando o status orçamentário e permitindo abrir o modal de memória do cálculo de cada venda.

---

### US 08 — Revisão Homologada (Human-in-the-Loop), Ativação Vinculada e Fechamento Imutável
> **Como** Diretor Financeiro,  
> **quero** revisar e homologar explicitamente os parâmetros da simulação e da versão vigente,  
> **para** ativar regras em produção com segurança jurídica e garantir que a apuração final de competência permaneça imutável e bloqueada contra alterações concorrentes.
* **Story Points:** 5 | **Prioridade:** Alta | **Status:** Concluído
* **Critérios de Aceitação:**
  * Exigir o fluxo formal de aceite: o usuário deve clicar em "Aceitar simulação e continuar" antes de poder submeter a campanha para homologação.
  * Invalidação automática de aceite (*fingerprint*): qualquer alteração posterior em parâmetros, regras, filtros, vigência ou taxas contratuais deve revogar imediatamente o aceite e exigir nova simulação.
  * Ativação vinculada: o sistema deve impedir a publicação e ativação de campanhas que não possuam uma simulação aceita e estritamente válida para seus parâmetros vigentes.
  * Versionamento de campanhas: manter segregação entre `versaoVigente` (ativa e aplicada pelo motor) e `versaoProposta` (em edição/análise), protegendo versões publicadas contra exclusão inadvertida.
  * Fechamento final de competência (BACK-11): registrar a apuração formal em `tb_fechamento_competencia`, vincular cada resultado à versão de apuração aplicada e bloquear permanentemente novos recálculos ou mutações em regras retroativas quando a competência estiver com status `CONCLUIDO`.
* **Cenário de Teste (BDD):**
  * **Dado** que uma simulação foi concluída e aceita pelo Diretor Financeiro com determinado conjunto de regras;
  * **Quando** o operador editar um dos percentuais de taxa ou a vigência da campanha;
  * **Então** o aceite da simulação deve ser revogado automaticamente;
  * **E** o botão de homologação e ativação deve ser bloqueado até que uma nova simulação seja executada e aceita.

---

### US 09 — Explainable AI (XAI), Snippet de Código Python e Reinterpretação Iterativa
> **Como** Auditor de Compliance,  
> **quero** visualizar a justificativa em linguagem clara e o snippet de código Python equivalente gerado pela IA para cada bloco de regra,  
> **para** auditar deterministicamente o raciocínio da inteligência artificial e aprovar ou solicitar ajustes específicos antes da persistência definitiva.
* **Story Points:** 5 | **Prioridade:** Alta | **Status:** Concluído
* **Critérios de Aceitação:**
  * O agente de IA deve decompor prompts em linguagem natural em múltiplos blocos estruturados (`PropostaRegraDTO`), mapeando a origem de cada campo (`TEXTO`, `CONTEXTO`, `BASE_DADOS`, `PADRAO_SISTEMA`).
  * Gerar artefatos explicativos duplos: uma explicação discursiva auditável e uma função Python pura equivalente (`def calcular_comissao(venda: dict) -> float | None`).
  * Disponibilizar reinterpretação pontual por bloco: permitir que o usuário solicite alterações específicas em um bloco isolado (ex: *"mude o valor para R$ 10.000"*) sem descartar ou reprocessar os outros blocos da campanha.
  * Sincronização reativa de artefatos: em caso de edições manuais de campos no formulário (ex: alteração de taxa ou faixa), os artefatos de texto e o snippet Python devem ser atualizados instantaneamente via backend.
  * Execução em sandbox seguro e conformidade com casos de referência (*Golden Cases*), assegurando paridade entre a lógica interpretada pelo agente e o motor Java.
* **Cenário de Teste (BDD):**
  * **Dado** que o gestor forneceu o comando "Pagar 4.5% no canal ecommerce para vendas acima de 5000 e 3% no canal loja fisica";
  * **Quando** o agente processar o comando via API v2;
  * **Então** a interface deve renderizar dois blocos de regra independentes;
  * **E** para cada bloco deve ser exibida a explicação do raciocínio e o snippet Python correspondente, permitindo reinterpretar apenas o segundo bloco sem impactar o primeiro.

---

**Total entregue na Sprint 2: 18 Story Points.**

---

## 🧪 Critérios de Aceitação e Refinamento Técnico <a id="bdd"></a>

**Cenário 1: Execução de simulação em sandbox sem impacto contábil (US 07)**
* **Dado** que o operador acesse o assistente de campanhas e selecione a competência de referência "07/2025" com uma proposta de regra multi-blocos;
* **Quando** o processamento da simulação for disparado via `POST /api/v1/simulacoes`;
* **Então** o motor deve aplicar as regras contra o histórico da competência sem gravar registros na tabela contábil `tb_resultado_calculo`;
* **E** deve retornar a memória de cálculo venda a venda, os totais do público recortado e a projeção nos cenários de 80%, 100%, 120% e 150%.

**Cenário 2: Bloqueio por ausência de base contratual e referência insuficiente (US 07 e FRONT-10)**
* **Dado** que a campanha envolva colaboradores ou combinações de Marca/Cargo sem taxa padrão cadastrada em `tb_basecomiss` ou cujo filtro resulte em 0 vendas na competência;
* **Quando** a simulação ou recorte de público for requisitado;
* **Então** o sistema deve emitir impedimento com mensagem explicativa ("Referência insuficiente" ou "Base contratual ausente");
* **E** bloquear a apresentação de valores fictícios, impedindo a continuidade do fluxo sem a ingestão prévia das taxas faltantes.

**Cenário 3: Rastreabilidade de aceite com invalidação automática por divergência (US 08)**
* **Dado** que uma simulação foi homologada com status `ACEITO` e possui uma assinatura canônica (*fingerprint*) gravada;
* **Quando** qualquer atributo de regra (canal, percentual, faixa de valor), competência ou base de taxas for modificado;
* **Então** a verificação de integridade deve revogar imediatamente o aceite (`evaluateSimulationValidity`);
* **E** a tentativa de ativação da campanha deve retornar `HTTP 409 Conflict` ou `422 Unprocessable Entity` informando a necessidade de nova simulação.

**Cenário 4: Fechamento final de competência e bloqueio de mutações concorrentes (US 08 / BACK-11)**
* **Dado** que a competência "08/2025" foi apurada e o encerramento financeiro foi confirmado via `POST /api/v1/comissoes/fechamento`;
* **Quando** o fechamento for persistido com sucesso em `tb_fechamento_competencia` com status `CONCLUIDO`;
* **Então** chamadas subsequentes para recalcular comissões da competência devem ser rejeitadas com erro `409 Conflict`;
* **E** tentativas de editar, publicar ou excluir regras que afetem o período fechado devem ser bloqueadas para assegurar a inviolabilidade do histórico.

**Cenário 5: Reinterpretação pontual de bloco de regra com preservação de contexto (US 09)**
* **Dado** que uma campanha possui três blocos de regras gerados pela IA a partir de linguagem natural;
* **Quando** o usuário selecionar o bloco 2 e enviar a instrução de ajuste "Reduzir o percentual para 3.8% e adicionar bônus de R$ 150,00";
* **Então** o endpoint `POST /api/v2/reinterpretar-bloco` deve atualizar unicamente os parâmetros, a explicação e o snippet Python do bloco 2;
* **E** os blocos 1 e 3 devem permanecer inalterados, preservando suas configurações e referências de origem.

---

## 🏗️ Entregas Técnicas por Frente <a id="entregas"></a>

### 🔧 1. Backend (Spring Boot 3.x / Java 21 / PostgreSQL / Flyway / Spring Batch)

* **Arquitetura Multi-Regras e Campanhas Estruturadas:**
  * Modelagem relacional e migrations Flyway evolutivas (`V14` a `V23`) estabelecendo as entidades `Campanha` e `Regra` (`tb_campanha`, `tb_regra`), permitindo campanhas compostas por múltiplos blocos de regras.
  * Suporte a faixas de valor por venda (`valor_minimo`, `valor_maximo`, `min_inclusivo`, `max_inclusivo`) e bônus direto (`bonus_percentual` e `bonus_valor_fixo`).
* **Motor Determinístico de Seleção e Resolução de Taxas (`TaxaComissaoResolver`):**
  * Atualização da matriz de especificidade com critérios de desempate determinísticos por faixa de valor e bônus.
  * Bloqueio estrito de ambiguidade: caso regras concorrentes atinjam pontuação idêntica para a mesma venda, o cálculo é impedido com log explícito de conflito.
  * Isolamento de cálculo: o bônus monetário é arredondado separadamente em escala 2 (`RoundingMode.HALF_UP`) e somado à comissão percentual sem contaminação entre taxas.
* **Motor de Simulação Compartilhado e Cenários Orçamentários (BACK-7, BACK-8, BACK-14):**
  * Unificação do motor matemático: a simulação consome o mesmo pipeline do cálculo de comissões oficial (`CalculoComissaoService`), eliminando qualquer risco de discrepância entre o sandbox e a apuração real.
  * Persistência de simulações em `tb_simulacao` e `tb_simulacao_item`, com suporte a memória de cálculo auditável, totais e projeções em 80%, 100%, 120% e 150%.
  * Fator de projeção comercial baseado em mês de 30 dias (`(soma * dias) / 30`) sem arredondamento precoce de frações.
* **Governança de Aceite e Ativação Vinculada (BACK-8):**
  * Endpoints dedicados para formalização de aceite (`POST /api/v1/simulacoes/{id}/aceite` e `DELETE /api/v1/simulacoes/{id}/aceite`).
  * Bloqueio transacional de publicação e ativação de campanhas sem simulação válida vinculada.
* **Versionamento de Campanhas (PR #23):**
  * Controle de versões com segregação entre `versaoVigente` e `versaoProposta` e status `RASCUNHO`, `EM_ANALISE`, `PUBLICADA` e `ARQUIVADA`.
  * Proteção de integridade evitando exclusão de versões ativas em produção.
* **Fechamento Final de Competência com Versão Retroativa e Bloqueio de Mutações (BACK-11 / V23):**
  * Criação da tabela `tb_fechamento_competencia` e coluna `versao_apuracao` em `tb_resultado_calculo`.
  * Endpoint `POST /api/v1/comissoes/fechamento` que consolida a apuração com a versão aprovada mais recente para todo o período da campanha (incluindo vendas anteriores à edição).
  * Bloqueio absoluto contra recálculos subsequentes e alterações de regras em competências encerradas (`HTTP 409 Conflict`).
  * Transação segura com rollback automático em caso de falha, impedindo registros parciais inconsistentes.
* **Serviço de Consultas para o Agente de IA (`ConsultasIaService`):**
  * Endpoints REST somente leitura (`/api/v1/consultas-ia/*`) permitindo ao agente validar marcas, lojas, cargos, colaboradores e taxas de base contratuais diretamente no banco relacional (abordagem Zero-Trust).
* **Ingestão com Spring Batch e Exclusão Segura de Bases:**
  * Otimização de uploads de RH e Vendas com processamento em lote via chunks.
  * Rotas de exclusão para matrículas, vendas e taxas de comissão (`DELETE /api/v1/*`) com validação de dependências e integridade referencial.
* **Qualidade e Cobertura de Testes:**
  * Bateria com mais de 330 testes automatizados de unidade e integração aprovados, incluindo testes de concorrência, casos de ouro (*Golden Cases*) e validação de rotas do fechamento final.

---

### 🎨 2. Frontend (Vue.js 3 / Vite / Pinia / Tailwind CSS)

* **Assistente de Campanhas e Fluxo Human-in-the-Loop:**
  * Estruturação do fluxo em etapas guiadas: Proposta (NLP/Manual) ➔ Simulação Sandbox ➔ Revisão ➔ Ativação Homologada.
  * Componente `CampaignMultiRulesForm` permitindo a criação e manutenção de campanhas compostas por múltiplos blocos de regras.
* **Simulação de Campanha por Competência e Memória do Cálculo (FRONT-4, FRONT-5):**
  * Seleção de competência única com origem em dados efetivados no backend (`GET /api/v1/referencias/competencias`).
  * Recorte de público-alvo consumindo `POST /api/v1/referencias/recortar-publico` com tratamento visual para **"Referência insuficiente"**.
  * Tabela paginada exibindo detalhamento venda a venda e modal interativo de **Memória do Cálculo**, explicitando taxa base, regra aplicada, faixa monetária atingida, arredondamento `HALF_UP` e delta financeiro.
  * Painel de Cenários de Variação (80%, 100%, 120%, 150%) com recálculo individual por venda e contador de migração entre faixas de valor.
* **Governança de Aceite e Invalidação Reativa (FRONT-6):**
  * Botão explícito "Aceitar simulação e continuar" com emissão de token/fingerprint de aceite.
  * Invalidação automática (`evaluateSimulationValidity`): qualquer modificação em campos, taxas ou vigência desativa o aceite e exige nova simulação.
  * Checkbox obrigatória de confirmação humana antes de submeter a ativação da campanha.
* **Gestão de Bases Ausentes e Bloqueio de Cobertura (FRONT-10):**
  * Componente `MissingBasesCard.vue` detectando lacunas contratuais em `tb_basecomiss` para combinações de Marca e Cargo.
  * Bloqueio visual e operacional na simulação e proposta quando o público-alvo não possuir taxa base contratual, exibindo volume de vendas e montante financeiro protegido.
  * Atalho de ação rápida para importação de taxas (`importType=COMISSIONS`) com competência pré-carregada e recálculo reativo pós-upload.
* **Visualização de Artefatos de IA e Reinterpretação por Bloco (FRONT-13):**
  * Renderização elegante de artefatos explicativos (*XAI*) e snippets de código Python com destaque sintático e modo de cópia.
  * Ação de reinterpretação pontual por bloco de regra, enviando prompts de refinamento sem afetar os blocos vizinhos.
* **Consulta de Dados Efetivados e Exclusão Pontual (FRONT-12):**
  * Painel de dados reais (RH, Vendas, Taxas) com filtros por competência, busca textual e paginação, eliminando totalmente mocks ou fallbacks locais.
  * Modais de exclusão pontual com proteção contra violação de integridade relacional.
* **Processamento Assíncrono e UX Desacoplada:**
  * Gerenciamento de jobs assíncronos (`asyncExecutionService.js`) com persistência em `sessionStorage`, widget animado na Topbar e notificações reativas via toasts.

---

### 🧠 3. Inteligência Artificial (FastAPI / Python 3.12 / LangChain / Groq / Sandbox Seguro)

* **Agente Interpretador Autônomo com Ferramentas (*Tool Calling*) (IA-1 a IA-4 / BUG-55):**
  * Arquitetura em laço cognitivo utilizando o modelo Llama-3.3-70b-versatile via Groq, com acesso a ferramentas HTTP conectadas ao Spring Boot:
    * `consultar_marcas()`: validação de marcas registradas no banco;
    * `consultar_lojas()`: verificação de lojas e canais de atendimento;
    * `consultar_cargos()`: validação de cargos funcionais elegíveis;
    * `consultar_matricula(matricula)`: checagem de colaborador e vínculo funcional;
    * `consultar_bases(competencia)`: conferência de cobertura em `tb_basecomiss`;
    * `consultar_regras_concorrentes(filtros)`: identificação prévia de regras sobrepostas.
* **Extração de Múltiplas Propostas de Regras (IA-2 / BUG-53):**
  * Decomposição de comandos complexos em linguagem natural em múltiplos blocos de propostas (`PropostaRegraDTO`).
  * Mapeamento de rastreabilidade de campos (`origensPorCampo`: `TEXTO`, `CONTEXTO`, `BASE_DADOS`, `PADRAO_SISTEMA`) e identificação de trechos textuais de origem.
* **Explainable AI (XAI) e Geração de Snippet Python (IA-5, IA-6):**
  * Redação de justificativas textuais transparentes detalhando o público, vigência, critérios condicionais e taxa final.
  * Síntese de código Python puro equivalente (`def calcular_comissao(venda: dict) -> float | None`) reproduzindo fielmente a lógica condicional inferida.
* **Sandbox Seguro de Execução:**
  * Ambiente contido para execução e validação dos snippets Python contra massa de dados de teste, sem permissões de rede, arquivos ou comandos do sistema operacional.
* **Reinterpretação Localizada e Sincronização de Artefatos (IA-7, IA-8):**
  * Endpoint `POST /api/v2/reinterpretar-bloco` para refinamento iterativo de um único bloco de regra com novo prompt contextual, mantendo intactos os demais blocos.
  * Endpoint `POST /api/v2/atualizar-artefatos` para atualização dinâmica da explicação e do código Python após edição manual de campos na interface pelo usuário.
* **Operações Avançadas sobre Base Contratual (`TipoOperacaoBase`):**
  * Suporte interpretativo a operações dinâmicas sobre a taxa base: `DEFINIR_TAXA`, `ACRESCIMO_PONTOS`, `DESCONTO_PONTOS`, `MULTIPLICADOR_BASE` e `DIVISOR_BASE`.
* **Resiliência, Otimização de Rate Limit e Schemas Estritos (IA-9 / BUG-81):**
  * Otimização do número de chamadas às ferramentas para preservar quotas de RPM/TPM da Groq.
  * Implementação de retry exponencial com jitter e fallback robusto para erros de rede (IPv4).
  * Adequação aos esquemas JSON estritos da Groq com suporte completo a modelos aninhados em `$defs` e teto expandido para `LLM_MAX_OUTPUT_TOKENS=4096`.
* **Suíte de Casos de Ouro (*Golden Cases*):**
  * Conjunto unificado de casos de teste (`tests/golden/golden_cases.json`) espelhado entre o serviço Python e o Spring Boot, garantindo consistência estrita de arredondamento bancário, bônus e desempates.

---

## 🏅 DoR — Definition of Ready <a id="dor"></a>

Critérios de prontidão verificados e atendidos pela equipe antes do início do desenvolvimento das histórias da Sprint 2:

| Critério | Descrição | Atendimento pela Equipe |
| :------- | :-------- | :---------------------- |
| **Contratos de interface v2 definidos** | Payloads de interpretação, reinterpretação e artefatos acordados entre Front, Back e IA. | **Cumprido:** Especificação formal documentada em `CONTRATOS_REGRAS_AVANCADAS.md`. |
| **Regras de simulação e borda detalhadas** | Critérios de recorte de público, cenários de volume e bloqueio por base ausente definidos. | **Cumprido:** Regras de negócio alinhadas e formalizadas com o Product Owner. |
| **Cenários de teste (BDD) estruturados** | Histórias documentadas com critérios em Dado, Quando e Então para simulação e governança. | **Cumprido:** Cenários de validação formulados em BDD para as US 07, 08 e 09. |
| **Governança HITL e aceite acordados** | Fluxo de separação entre aceitar simulação e ativar regra em produção aprovado. | **Cumprido:** Mecanismo de invalidação por divergência (*fingerprint*) acordado. |
| **Estrutura de dados e migrations mapeadas** | Tabelas de campanha, regra, simulação e fechamento estruturadas via Flyway. | **Cumprido:** Migrations V14 a V23 planejadas e integradas no Spring Boot. |
| **Estimativa definida (Planning Poker)** | Esforço relativo ponderado e pontuado por consenso do time de desenvolvimento. | **Cumprido:** 18 Story Points acordados para a Sprint 2. |
| **Casos de Ouro e bases de teste disponíveis** | Arquivos de homologação e casos de borda preparados para testes integrados. | **Cumprido:** Massa histórica e `golden_cases.json` disponibilizados nos repositórios. |
| **Validação de prioridades com o PO** | Escopo, restrições e critérios de entrega aprovados pelo Product Owner. | **Cumprido:** Alinhamento de escopo e homologação de prioridades concluídos. |

---

## 🏅 DoD — Definition of Done <a id="dod"></a>

Critérios cumpridos para considerar as entregas da Sprint 2 formalmente concluídas:

| Critério | Descrição | Evidência de Cumprimento |
| :------- | :-------- | :----------------------- |
| **Critérios de aceitação atendidos** | Todas as regras das User Stories 07, 08 e 09 implementadas e funcionais. | Demonstrado no ciclo completo de simulação, aceite, XAI e fechamento. |
| **Simulação sandbox isolada** | Execução de cálculos hipotéticos sem comitar lançamentos na apuração oficial. | Persistência exclusiva em `tb_simulacao` sem registros em `tb_resultado_calculo`. |
| **Memória do cálculo e cenários validados** | Detalhamento venda a venda com `HALF_UP` e projeção nos cenários 80%, 100%, 120% e 150%. | Modal de cálculo validado contra golden cases e comparativo orçamentário funcional. |
| **Governança HITL e blindagem ativas** | Aceite formal obrigatório, invalidação por alteração e fechamento mensal imutável. | Bloqueio de mutações concorrentes e persistência em `tb_fechamento_competencia`. |
| **Explicabilidade (XAI) e Python auditável** | Geração de texto justificativo e código Python equivalente com sandbox seguro. | Artefatos auditáveis renderizados no front e sincronizados reativamente. |
| **Proteção de cobertura contratual** | Bloqueio de simulação e apuração para públicos sem taxa base cadastrada. | Componente `MissingBasesCard` ativo e validações defensivas no backend. |
| **Bateria de testes automatizados** | Testes de unidade, integração e Golden Cases executados e aprovados. | Mais de 330 testes automatizados aprovados no Spring Boot e suíte de IA no Python. |
| **Código revisado e integrado (Git Flow)** | Pull Requests revisados por pares e mesclados nas branches oficiais. | PRs de backend, frontend e AI aprovados e integrados à branch `dev`/`main`. |
| **Interface Vue.js totalmente integrada** | Frontend SPA consumindo rotas v2 do backend e serviço de IA com dados reais. | Aplicação operando sem dados mockados, com feedback assíncrono multicanal. |
| **Validação funcional pelo PO** | Demonstração final realizada e formalmente aceita pelo Product Owner. | Entrega aprovada e homologada para encerramento da Sprint 2. |

---

## 🏅 Sprint Burndown <a id="burndown"></a>

**Gráfico do burndown da Sprint 2:** 

<!-- Inserir imagem do gráfico de Burndown da Sprint 2 aqui -->
<div align="center">
  <img width="1539" height="564" alt="image" src="https://github.com/user-attachments/assets/bffd62c3-8662-4fa8-84f7-41a1fc5ffd3d" />
</div>

**Período e dados de acompanhamento:** 28/09/2026 a 25/10/2026 — 18 Story Points entregues (100% de conclusão do escopo planejado).

---

## 🎓 Equipe <a id="equipe"></a>

| Membro | Função | GitHub | LinkedIn |
| :--- | :--- | :---: | :---: |
| Davi Miyake | Product Owner | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/DaviMBDev) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://br.linkedin.com/in/davimiyakeb) |
| Renan Tomasi | Scrum Master | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/renan21-tg) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/renan-tomasi/) |
| Humberto Ishii | Team Member | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/HumbertoIshii) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://br.linkedin.com/in/humberto-ishii-silva-754489161) |
| Diego Castilho | Team Member | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/DigoCast) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/diego-castilho-8b87a8301/) |
| Vinicius Elias | Team Member | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/ViniElias) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://br.linkedin.com/in/vinicius-elias-895332235/) |
| Ygor Pereira | Team Member | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/YgorPereira) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ygorrpereira/) |
