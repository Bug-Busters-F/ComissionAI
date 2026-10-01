# API 6º Semestre ADS — Gestão de Regras

## Documentação — Sprint 1

<p align="center">
  <img src="https://github.com/user-attachments/assets/7757bc1a-e087-46e7-8170-b423b614d4ac" alt="Logo da Bug Busters" width="250">
</p>
<h2 align="center">Bug Busters</h2>

<p align="center">
  <a href="#desafio">Desafio</a> |
  <a href="#goal">Sprint Goal</a> |
  <a href="#us">User Stories</a> |
  <a href="#bdd">Critérios de Aceitação (BDD)</a> |
  <a href="#dor">DoR</a> |
  <a href="#dod">DoD</a> |
  <a href="#burndown">Burndown</a> |
  <a href="#equipe">Equipe</a>
</p>

> **Status da Sprint:** Concluída  
> **Previsão de entrega:** 27/09/2026  
> **Data de início:** 07/09/2026  
> **Cliente:** Dom Rock  
> **Instituição:** Fatec São José dos Campos — ADS, 6º semestre (2026-2)

[Voltar ao README](../README.md)

---

## 🏅 Desafio e Dor do Negócio <a id="desafio"></a>

Solucionar a vulnerabilidade e os passivos trabalhistas decorrentes da apuração manual de comissões em planilhas descentralizadas. A Sprint 1 focou em estruturar a ingestão de dados íntegros, estabelecer o motor determinístico de cálculo de regras com persistência imutável e proteção contra duplicidades, além de viabilizar o fluxo assistido por IA para estruturação e simulação preliminar de campanhas comerciais.

---

## 🎯 Sprint Goal <a id="goal"></a>

Entregar a infraestrutura transacional e de dados da plataforma ComissionAI, viabilizando a ingestão validada das bases mensais (RH, Vendas e Taxas), o cálculo determinístico de comissões individuais e por competência com segregação de impedimentos temporais, a proteção contra reprocessamento duplicado e a esteira de IA para interpretação de regras em linguagem natural com simulação orçamentária prévia.

---

## 📋 User Stories da Sprint <a id="us"></a>

Histórias 1 a 6 do [backlog do produto](../README.md#backlog-do-produto), executadas e entregues na Sprint 1:

| Rank | Prioridade | User Story | Story Points | Sprint | Status |
| :--: | :--------: | :--------- | :----------: | :----: | :----: |
| 1 | Alta | Como **Analista de Operações**, quero cadastrar, editar e remover regras manuais para manter as taxas vigentes atualizadas. | 3 | 1 | Concluído |
| 2 | Alta | Como **Analista de Operações**, quero executar o processamento de vendas contra regras ativas para apurar valores devidos de forma determinística e ágil. | 5 | 1 | Concluído |
| 3 | Média | Como **Auditor de Compliance**, quero que o sistema bloqueie regras sem data final para evitar comissionamento por tempo indeterminado e custos descontrolados. | 2 | 1 | Concluído |
| 4 | Alta | Como **Auditor de Compliance**, quero que cada cálculo executado gere um log imutável discriminando taxa-base ou regra de campanha para auditoria irrefutável. | 3 | 1 | Concluído |
| 5 | Alta | Como **Head Comercial**, quero formular propostas em linguagem natural para que o sistema interprete e estruture parâmetros executáveis automaticamente. | 8 | 1 | Concluído |
| 6 | Alta | Como **Administrador de Dados**, quero importar e validar as bases de RH, vendas e taxas para garantir que apenas dados íntegros entrem no cálculo por competência. | 8 | 1 | Concluído |

**Total entregue: 29 Story Points.**

### Legenda, Papéis e Convenções do Backlog

#### Personas e Papéis de Usuário
* **Analista de Operações:** Responsável pela manutenção operacional do catálogo de regras, atualização manual de taxas, vigências e exceções de mercado.
* **Administrador de Dados:** Responsável pelo controle de integridade, ingestão e validação das cargas mensais de arquivos (RH, Vendas e Comissões) no sistema.
* **Head Comercial:** Perfil estratégico focado na formulação de campanhas de incentivo em linguagem natural, validação de viabilidade comercial e simulação orçamentária prévia (modo *sandbox*).
* **Diretor Financeiro:** Autoridade orçamentária com poder de homologação explícita (aprovação humana) para converter propostas e simulações em regras ativas no ambiente produtivo.
* **Auditor de Compliance:** Responsável pela governança, conformidade legal e fiscal, exigindo logs imutáveis, detecção de anomalias operacionais e rastreabilidade total do raciocínio da IA.
* **Gerente de Vendas:** Liderança operacional que consome relatórios consolidados de fechamento por equipe e canal para acompanhamento de metas.
* **Administrador de TI:** Responsável pela sustentação técnica, monitoramento de concorrência e auditoria de logs de acesso e de modificação no banco de dados.

#### Termos Técnicos e de Negócio
* **Explainable AI (XAI):** Práticas e técnicas voltadas a tornar as decisões e o raciocínio de modelos de inteligência artificial transparentes, compreensíveis e auditáveis por seres humanos.
* **Snippet:** Trecho conciso e autoexplicativo de código (em Python) gerado pelo agente para detalhar a lógica condicional inferida antes de sua persistência definitiva no banco relacional.
* **Competência:** Mês e ano de referência (formato MM/AAAA) no qual as vendas ocorreram e sobre o qual os vínculos contratuais e taxas ativas são processados.
* **Impedimento:** Inconsistência cadastral ou temporal (ex: venda registrada antes da data de admissão do colaborador) que segrega o cálculo individual para auditoria manual sem travar o processamento do restante do lote.
* **Log Imutável:** Registro de auditoria gravado no banco de dados exclusivamente em modo *append-only* (sem permissão de edição ou exclusão), garantindo histórico fiel e rastreabilidade jurídica de execuções.
* **Story Points (Estimativa):** Unidade de medida ágil que pondera esforço relativo, complexidade técnica e riscos envolvidos na entrega de cada User Story.

---

## 🧪 Critérios de Aceitação e Refinamento Técnico (BDD / Gherkin) <a id="bdd"></a>

**Cenário 1: Apuração determinística com rastreabilidade de origem (US 2 e US 4)**
* **Dado** que a competência "07/2025" possui arquivos de RH, Vendas e Taxas efetivados no banco de dados;
* **Quando** o usuário aciona o processamento da competência através de "Calcular comissões";
* **Então** o sistema deve processar as vendas elegíveis com precisão monetária (duas casas decimais via `BigDecimal`);
* **E** registrar a origem da taxa aplicada como "Regra de negócio" ou "Taxa base" junto à data/hora de execução no log imutável.

**Cenário 2: Segregação automática de vendas com impedimento temporal (US 2)**
* **Dado** que um registro de venda possui data de realização anterior à admissão do colaborador indicada no RH;
* **Quando** o motor de comissionamento processar a competência;
* **Então** o registro deve ser segregado da apuração de sucesso e listado na aba "Vendas com impedimento";
* **E** o motivo impeditivo deve ser exposto detalhadamente para auditoria operacional (ex: *"Data da venda anterior à admissão"*).

**Cenário 3: Proteção contra reprocessamento duplicado e idempotência (US 2 e US 4)**
* **Dado** que uma competência ou venda individual já foi previamente apurada e possui registros salvos;
* **Quando** o usuário solicitar um novo recálculo ou reenviar a mesma requisição;
* **Então** as vendas inalteradas devem manter seus resultados originais preservados;
* **E** o sistema não deve gerar duplicidades financeiras nem redundância de registros de log.

**Cenário 4: Bloqueio de regras sem vigência final (US 3)**
* **Dado** que o operador tenta cadastrar uma nova regra manual sem preencher o campo de data de término de vigência;
* **Quando** a solicitação de persistência for enviada;
* **Então** o sistema deve rejeitar o cadastro informando a obrigatoriedade da vigência limite para fins de compliance.

**Cenário 5: Interpretação de proposta via IA e simulação orçamentária (US 5)**
* **Dado** que o gestor insere uma proposta em texto livre ("Aumentar comissao PRETO 10% em agosto de 2025") com teto orçamentário definido;
* **Quando** o serviço de IA interpretar a proposta e gerar a simulação nos cenários de 80%, 100% e 120%;
* **Então** os parâmetros de vigência, percentual e marca devem ser estruturados e revisáveis em tela;
* **E** caso a projeção supere o teto estipulado, o sistema deve exibir alerta visual explícito de "Orçamento excedido".

---

## 🏅 DoR — Definition of Ready <a id="dor"></a>

Critérios verificados e atendidos para que as histórias da Sprint 1 entrassem em desenvolvimento:

| Critério | Descrição | Atendimento na Sprint 1 |
| :------- | :-------- | :---------------------- |
| Clareza na descrição | User Story descreve persona, ação desejada e valor de negócio. | Atendido (personas refinadas no Backlog do Produto). |
| Critérios de aceitação | Critérios objetivos definidos em BDD cobrindo caminhos feliz e de exceção. | Atendido (cenários 1 a 5 documentados). |
| Cenários de teste | Casos de borda estruturados em Dado, Quando e Então. | Atendido (regras de bloqueio e impedimentos mapeados). |
| Dependências identificadas | Relações entre RH, Vendas, Taxas e Regras mapeadas antes do início. | Atendido (estrutura relacional mapeada). |
| Referência visual | Telas e componentes validados com base nos mockups funcionais. | Atendido (fluxo de dados e campanhas validado). |
| Escopo técnico validado | Fatiamento técnico entre Spring Boot, FastAPI e Vue.js definido. | Atendido (responsabilidades distribuídas por serviço). |
| Regras de negócio definidas | Fórmulas, precisão decimal (`HALF_UP`) e políticas de cálculo especificadas. | Atendido (cálculo determinístico com rastreabilidade). |
| Estimativa definida | Story Points estimados e acordados pela equipe técnica. | Atendido (total de 29 Story Points). |
| Documentos de apoio | Arquivos de homologação (planilhas de teste) disponíveis. | Atendido (bases de competência 07/2025 e 08/2025 integradas). |
| Validação com PO | Histórias refinadas e priorizadas com o Product Owner. | Atendido e validado para início do ciclo. |

---

## 🏅 DoD — Definition of Done <a id="dod"></a>

Critérios cumpridos para considerar as histórias da Sprint 1 formalmente concluídas:

| Critério | Descrição | Evidência de Cumprimento |
| :------- | :-------- | :----------------------- |
| Critérios de aceitação atendidos | Todas as regras da US implementadas e em execução. | Demonstrado no vídeo funcional e interface web. |
| Regras de negócio validadas | Cálculo determinístico e segregação de impedimentos funcionais. | 4.975 vendas calculadas e 43 impedimentos identificados. |
| Dados persistidos corretamente | Flyway migrations executadas e integridade referencial mantida. | Tabelas de vendas, vínculos e regras persistidas no PostgreSQL. |
| Testes executados | Testes de unidade e fluxos de API testados localmente e integrados. | Validação de carga e execução de cálculo sem falhas de concorrência. |
| Código revisado | Pull Requests revisados e mesclados seguindo Git Flow. | Branches padronizadas integradas à branch de release da sprint. |
| Funcionalidade integrada | Frontend Vue.js consumindo endpoints Spring Boot e serviço Python. | Ciclo completo demonstrado em gravação contínua. |
| Documentação atualizada | README e relatório de sprint atualizados com contratos e logs. | Documentação sincronizada com o escopo entregue. |
| Validação funcional pelo PO | Demonstração final realizada e aceita pelo Product Owner. | Entrega aprovada e validada para fechamento da Sprint 1. |

---

## 🏅 Sprint Burndown <a id="burndown"></a>

**Gráfico do burndown da Sprint 1:** 

<img width="1560" height="735" alt="image" src="https://github.com/user-attachments/assets/e91b3bd8-4e24-41f2-8759-5564cee61cbc" />

**Período e dados de acompanhamento:** 07/09/2026 a 27/09/2026 — 29 Story Points entregues.

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
