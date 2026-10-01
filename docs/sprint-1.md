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

## 🎯 Meta da Sprint (Sprint Goal) <a id="goal"></a>

Estabelecer a espinha dorsal de dados e regras da plataforma ComissionAI, priorizando as **User Stories 2, 4 e 6** como núcleo da entrega: viabilizar a ingestão validada das bases mensais (RH, Vendas e Taxas), processar o cálculo determinístico de comissões por competência com segregação de impedimentos contratuais e registrar logs imutáveis de auditoria. Complementarmente, disponibilizar a interface para gestão manual de regras (US 1, US 3) e o canal de entrada assistido por IA para estruturação preliminar de campanhas (US 5).

---

## 📋 User Stories e Critérios de Aceitação <a id="us"></a>

### US 01 — Gestão Manual de Regras
> **Como** Analista de Operações,  
> **quero** cadastrar, editar e remover regras manuais de comissionamento,  
> **para** manter as taxas e vigências de mercado devidamente atualizadas.
* **Story Points:** 3 | **Prioridade:** Alta | **Status:** Concluído
* **Critérios de Aceitação:**
  * Permitir preenchimento de canal, percentual, marca, cargo e período de vigência.
  * Atualizar imediatamente a listagem após operações de edição ou exclusão.
* **Cenário de Teste (BDD):**
  * **Dado** que o operador acesse o formulário de regras;
  * **Quando** preencher os parâmetros obrigatórios e salvar;
  * **Então** a regra deve ser persistida com sucesso e refletir na tabela de regras ativas.

---

### US 02 — Processamento Determinístico e Segregação de Impedimentos (Item Crítico da Meta)
> **Como** Analista de Operações,  
> **quero** executar o processamento de vendas contra o quadro funcional e regras ativas,  
> **para** apurar comissões devidas e identificar inconsistências cadastrais de colaboradores.
* **Story Points:** 5 | **Prioridade:** Alta | **Status:** Concluído
* **Critérios de Aceitação:**
  * Cruzar a matrícula da venda com a admissão/demissão do RH no mês de apuração.
  * Realizar o cálculo com tipo monetário (`BigDecimal`) com arredondamento `HALF_UP`.
  * Segregar vendas fora da vigência do contrato em lista separada com justificativa técnica.
* **Cenário de Teste (BDD):**
  * **Dado** que uma competência possui vendas válidas e vendas com data anterior à admissão do colaborador;
  * **Quando** o processamento da competência for acionado;
  * **Então** as vendas elegíveis devem ter suas comissões calculadas;
  * **E** as inconsistentes devem ser movidas para a aba "Vendas com impedimento" detalhando a causa (ex: *"Data da venda anterior à admissão"*).

---

### US 03 — Bloqueio de Regras sem Vigência Limite
> **Como** Auditor de Compliance,  
> **quero** que o sistema bloqueie o cadastro de regras sem data final especificada,  
> **para** prevenir comissionamentos indeterminados e passivos financeiros.
* **Story Points:** 2 | **Prioridade:** Média | **Status:** Concluído
* **Critérios de Aceitação:**
  * Rejeitar payloads ou submissões de formulário sem campo de término de vigência.
  * Exibir mensagem impeditiva clara impedindo a transação no banco.
* **Cenário de Teste (BDD):**
  * **Dado** que o formulário de regra é submetido sem a data de vigência final;
  * **Quando** o backend validar os dados recebidos;
  * **Então** a requisição deve ser recusada e a regra não deve ser inserida.

---

### US 04 — Rastreabilidade e Log Imutável de Cálculos (Item Crítico da Meta)
> **Como** Auditor de Compliance,  
> **quero** que cada cálculo registre um log imutável discriminando a origem da taxa aplicada,  
> **para** assegurar auditoria irrefutável em casos de contestação de pagamentos.
* **Story Points:** 3 | **Prioridade:** Alta | **Status:** Concluído
* **Critérios de Aceitação:**
  * Gravar cada resultado em modo imutável (*append-only*), vinculando data, hora, taxa e fórmula.
  * Discriminar explicitamente se a taxa decorreu de "Regra de negócio" ou "Taxa base".
* **Cenário de Teste (BDD):**
  * **Dado** que o lote de vendas foi apurado;
  * **Quando** a visualização detalhada for consultada;
  * **Então** cada item deve expor a origem da taxa, valor final e carimbo de execução inalterável.

---

### US 05 — Estruturação de Campanhas via Linguagem Natural e Simulação
> **Como** Head Comercial,  
> **quero** formular propostas de campanhas em texto livre,  
> **para** que o sistema extraia parâmetros estruturados e projete o impacto contra o teto orçamentário.
* **Story Points:** 8 | **Prioridade:** Alta | **Status:** Concluído
* **Critérios de Aceitação:**
  * Extrair marca, percentual e vigência a partir do texto enviado.
  * Projetar cenários de volume (80%, 100% e 120%) contra o histórico de vendas.
  * Emitir alerta visual de estouro orçamentário caso a projeção supere o limite estipulado.
* **Cenário de Teste (BDD):**
  * **Dado** que uma proposta de 10% para marca PRETO com teto de R$ 7.500,00 foi inserida;
  * **Quando** o modelo estruturar os dados e rodar a simulação;
  * **Então** o sistema deve exibir os parâmetros em tela e indicar "Orçamento excedido" no cenário que ultrapassar o teto.

---

### US 06 — Ingestão e Validação de Cargas Mensais (Item Crítico da Meta)
> **Como** Administrador de Dados,  
> **quero** importar e sincronizar as planilhas de ciclo mensal (RH e Vendas) e taxas,  
> **para** compor a base oficial de processamento de cada competência.
* **Story Points:** 8 | **Prioridade:** Alta | **Status:** Concluído
* **Critérios de Aceitação:**
  * Validar a integridade estrutural das planilhas de RH e Vendas antes de efetivá-las.
  * Bloquear o cálculo de competências cujas bases apresentem falhas de integridade ou estejam pendentes.
* **Cenário de Teste (BDD):**
  * **Dado** o upload de planilhas correspondentes à competência "07/2025";
  * **Quando** o módulo de ingestão concluir a checagem de vínculos;
  * **Então** os dados devem figurar como efetivados e habilitar o botão de cálculo de comissões.

**Total entregue na Sprint 1: 29 Story Points.**

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

Critérios de prontidão verificados e atendidos pela equipe antes do início do desenvolvimento das histórias da Sprint 1:

| Critério | Descrição | Atendimento pela Equipe |
| :------- | :-------- | :---------------------- |
| **Clareza na descrição** | A User Story descreve explicitamente o papel, a ação e o valor de negócio gerado. | **Cumprido:** USs revisadas com papéis operacionais e corporativos reais da Dom Rock. |
| **Critérios de aceitação** | A história possui parâmetros verificáveis que determinam a conclusão da demanda. | **Cumprido:** Critérios objetivos descritos individualmente para cada uma das 6 histórias. |
| **Cenários de teste (BDD)** | A história possui ao menos um cenário estruturado em Dado, Quando e Então. | **Cumprido:** Cenários de validação formulados em BDD para cada US. |
| **Dependências identificadas** | Relações entre esquemas de banco, planilhas e serviços mapeadas. | **Cumprido:** Definição das chaves de vínculo (matrícula, data de admissão e regras ativas). |
| **Regras de negócio definidas** | Políticas de arredondamento (`BigDecimal`), cálculo e impedimentos esclarecidas. | **Cumprido:** Política de cálculo determinístico e regras de segregação especificadas. |
| **Estimativa definida** | O esforço relativo foi ponderado e pontuado por consenso do time. | **Cumprido:** 29 Story Points distribuídos e acordados no Planning Poker. |
| **Documentos de apoio** | Bases de dados e arquivos de exemplo disponibilizados para testes. | **Cumprido:** Planilhas de homologação de Vendas, RH e Taxas carregadas no repositório. |
| **Validação com PO** | O escopo e prioridade das histórias foram formalmente validados com o PO. | **Cumprido:** Planejamento e priorização alinhados com o Product Owner antes do ciclo. |

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
