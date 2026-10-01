# <span style="color:orange">Gestão</span> <span style="color:lightblue">de Regras</span>

Projeto de API - **6º Semestre (2026-2)** da Fatec São José dos Campos - **Bug Busters**

O objetivo deste projeto é solucionar um gargalo crítico enfrentado pela **Dom Rock**: a vulnerabilidade, ineficiência e falta de conformidade na apuração manual de comissões de vendas.

### O Desafio e a Dor do Cliente
Atualmente, a gestão de incentivos depende de planilhas desarticuladas, gerando impactos operacionais e financeiros imediatos:
* **Inconsistências Temporais e Passivo Trabalhista:** Lançamentos de vendas atribuídos a colaboradores fora do período de vigência de seu contrato (ex: vendas registradas antes da data de admissão do colaborador) geram comissionamentos indevidos e passivos contábeis.
* **Falta de Rastreabilidade e Auditoria:** Ausência de registros imutáveis que comprovem a origem exata da taxa aplicada (se proveniente de uma regra de campanha específica ou da taxa-base por cargo/marca).
* **Insegurança Orçamentária em Campanhas:** Gestores comerciais lançam propostas de comissionamento sem capacidade de prever o impacto financeiro real frente ao histórico de vendas, resultando em estouros frequentes do teto orçamentário.

A plataforma **ComissionAI** resolve esse cenário unificando a ingestão de dados de RH, Vendas e Comissões sob um motor de cálculo determinístico e imutável, dotado de barreira de validação temporal de vínculos, proteção contra duplicações e interpretação assistida por IA com simulação orçamentária prévia (sandbox).

![Status](https://img.shields.io/badge/status-Em%20desenvolvimento-yellow)
![API](https://img.shields.io/badge/API-FATEC-blue)

| Cliente | Periodo/Curso | Professor M2 | Professor P2 | Contato Cliente |
| -------- | -------------- | ------------- | ------------ | --------------- |
| Dom Rock | 6º ADS (Análise e Desenvolvimento de Sistemas) | Claudio Lima<br>claudio.lima@cps.sp.gov.br | Walmir Duque<br>jose.duque@cps.sp.gov.br | Andre F. de Almeida<br>andre.almeida@domrock.com.br |

## Índice

- [Gestão de Regras](#gestão-de-regras)
  - [Índice](#índice)
  - [Documentos](#documentos)
    - [Tecnologias Utilizadas](#tecnologias-utilizadas)
    - [Cronograma e Sprints](#cronograma-e-sprints)
    - [Backlog do Produto](#backlog-do-produto)
    - [Roadmap](#roadmap)
  - [Manual de Instalação](#manual-de-instalação)
  - [Autores](#autores)

## Documentos

### Tecnologias Utilizadas

<p align="left">
  <img src="https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D" alt="Vue.js" />
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript" />
  <img src="https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java" />
  <img src="https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white" alt="Spring Boot" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL" />
   <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
</p>
### Cronograma e Sprints

| Sprint | Previsão | Status |
| ------ | -------- | ------ |
| Kick Off | 26/08 | Concluido |  
| [01](docs/sprint-1.md) | 27/09 | Concluida |
| [02](docs/sprint-2.md) | 25/10 | Em andamento |
| [03](docs/sprint-3.md) | 22/11 | A fazer |
| Feira de Soluções | 03/12 | A fazer |

### Backlog do Produto

| Rank | Prioridade | User Story | Estimativa | Sprint |
| :--: | :--------: | :--------- | :--------: | :----: |
| 1 | Alta | Como **Analista de Operações**, quero cadastrar, editar e remover regras manuais para manter as taxas vigentes atualizadas. | 3 | 1 |
| 2 | Alta | Como **Analista de Operações**, quero executar o processamento de vendas contra regras ativas para apurar valores devidos de forma determinística e ágil. | 5 | 1 |
| 3 | Média | Como **Auditor de Compliance**, quero que o sistema bloqueie regras sem data final para evitar comissionamento por tempo indeterminado e custos descontrolados. | 2 | 1 |
| 4 | Alta | Como **Auditor de Compliance**, quero que cada cálculo executado gere um log imutável discriminando taxa-base ou regra de campanha para auditoria irrefutável. | 3 | 1 |
| 5 | Alta | Como **Head Comercial**, quero formular propostas em linguagem natural para que o sistema interprete e estruture parâmetros executáveis automaticamente. | 8 | 1 |
| 6 | Alta | Como **Administrador de Dados**, quero importar e validar as bases de RH, vendas e taxas para garantir que apenas dados íntegros entrem no cálculo por competência. | 8 | 1 |
| 7 | Alta | Como **Head Comercial**, quero simular o impacto financeiro de uma proposta contra o histórico de vendas para verificar aderência ao orçamento sem gerar comissão real. | 8 | 2 |
| 8 | Alta | Como **Diretor Financeiro**, quero revisar e aprovar explicitamente os parâmetros da simulação para homologar a ativação da regra em produção. | 5 | 2 |
| 9 | Alta | Como **Auditor de Compliance**, quero visualizar o snippet explicativo de código gerado pelo agente para auditar a interpretação lógica da regra antes de sua persistência (*Explainable AI*). | 5 | 2 |
| 10 | Média | Como **Auditor de Compliance**, quero ser alertado quando houver anomalias nas vendas diárias de um colaborador (outliers estatísticos) para verificação preventiva. | 8 | 3 |
| 11 | Baixa | Como **Head Comercial**, quero receber sugestões de ajuste de parâmetros caso a simulação estoure o teto orçamentário previsto. | 5 | 3 |
| 12 | Média | Como **Gerente de Vendas**, quero emitir o relatório consolidado de fechamento detalhando resultados por canal e equipe. | 5 | 3 |
| 13 | Média | Como **Administrador de TI**, quero consultar os históricos de envios e validações para auditar o ciclo de carga e integridade das bases. | 3 | 3 |

Estimativas em story points

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
* **Explainable AI (XAI):** Conjunto de práticas e técnicas que tornam as decisões e o raciocínio de modelos de inteligência artificial transparentes e auditáveis por humanos.
* **Snippet:** Trecho curto e autoexplicativo de código (em Python) gerado pelo agente para detalhar a lógica condicional inferida antes de sua persistência definitiva no banco relacional.
* **Competência:** Mês e ano de referência (formato MM/AAAA) no qual as vendas ocorreram e sobre o qual os vínculos contratuais e taxas ativas são processados.
* **Impedimento:** Inconsistência cadastral ou temporal (ex: venda registrada antes da data de admissão do colaborador) que segrega o cálculo individual para auditoria manual sem travar o restante do lote processado.
* **Log Imutável:** Registro de auditoria em banco de dados gravado exclusivamente em modo *append-only* (sem permissão de edição ou deleção), assegurando prova histórica e rastreabilidade de execuções.
* **Story Points (Estimativa):** Métrica ágil que quantifica o esforço relativo, complexidade técnica e riscos envolvidos na entrega de cada User Story.



### Critérios de Prontidão (Definition of Ready - DoR)
Para que uma User Story seja considerada pronta para execução na Sprint, ela deve atender aos seguintes critérios:
* [x] **Contrato de Interface Definido:** Payloads de entrada e saída (DTOs / Schemas de interpretação) acordados entre Front, Back e serviço de IA.
* [x] **Regras de Negócio e Casos de Borda Detalhados:** Especificação explícita de critérios de bloqueio (ex: divergência temporal de admissão, ausência de taxa base, duplicidade de lote).
* [x] **Critérios de Aceitação Formalizados:** Cenários de validação documentados no formato BDD/Gherkin.
* [x] **Estrutura de Dados Mapeada:** Tabelas e constraints de banco de dados provisionadas via migrations do Flyway.

### Roadmap

<img width="1246" height="696" alt="image" src="https://github.com/user-attachments/assets/36b05001-fd0c-46f8-9b3f-35a6b42c23c0" />

---

### Sprint 1

https://github.com/user-attachments/assets/3d2bf9c3-1b60-414b-8e01-1c0d258c6be4

#### Sprint Goal
Entregar a base funcional e a infraestrutura de dados da plataforma ComissionAI, viabilizando a ingestão validada das bases mensais (RH, Vendas e Taxas), o cálculo determinístico de comissões com segregação de impedimentos por vínculo trabalhista, a proteção contra reprocessamento e o fluxo assistido por IA para estruturação e simulação preliminar de campanhas.

#### Critérios de Aceitação e Refinamento Técnico (BDD / Gherkin)

**Cenário 1: Apuração determinística com rastreabilidade de origem**
* **Dado** que a competência "07/2025" possui arquivos de RH, Vendas e Taxas efetivados no banco de dados
* **Quando** o usuário aciona o processamento da competência através de "Calcular comissões"
* **Então** o sistema deve calcular as vendas elegíveis com precisão monetária (duas casas decimais)
* **E** registrar a origem da taxa aplicada como "Regra de negócio" ou "Taxa base" junto à data de execução.

**Cenário 2: Segregação automática de vendas com impedimento temporal**
* **Dado** que um registro de venda possui data de realização anterior à admissão do colaborador indicada no RH
* **Quando** o motor de comissionamento processar a competência
* **Então** o registro deve ser segregado da apuração de sucesso e listado na aba "Vendas com impedimento"
* **E** o motivo impeditivo deve ser exposto detalhadamente para auditoria operacional.

**Cenário 3: Proteção contra reprocessamento duplicado e preservação de histórico**
* **Dado** que uma competência já foi previamente apurada e possui cálculos registrados
* **Quando** o usuário solicitar um novo recálculo
* **Então** as vendas não alteradas devem manter seus resultados originais preservados
* **E** o sistema deve evitar a geração de duplicidades financeiras ou redundância em registros de log.

**Cenário 4: Interpretação de proposta em linguagem natural e alerta de estouro de teto**
* **Dado** que o gestor insere uma proposta em texto livre ("Aumentar comissao PRETO 10% em agosto de 2025") com teto de orçamento
* **Quando** a IA interpretar a proposta e a simulação for executada nos cenários de 80%, 100% e 120%
* **Então** os parâmetros de vigência, percentual e marca devem ser estruturados em tela
* **E** caso a projeção de custo supere o orçamento estipulado, o sistema deve emitir alerta visual explícito de "Orçamento excedido".

<!-- ### Sprint 2

VIDEO AQUI

### Sprint 3

VIDEO AQUI -->

### Manual de Instalação

Acesse o manual de instalação seguindo os passos pelo arquivo [CONTRIBUTING.md](./CONTRIBUTING.md)

---

## Autores

| Função | Nome | GitHub | Linkedin |
| :----: | :--- | :----: | :------: |
| Product Owner | Davi Miyake | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/DaviMBDev) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://br.linkedin.com/in/davimiyakeb) |
| Scrum Master | Renan Tomasi | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/renan21-tg) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/renan-tomasi/) |
| Team Member | Humberto Ishii | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/HumbertoIshii) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://br.linkedin.com/in/humberto-ishii-silva-754489161) |
| Team Member | Diego Castilho | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/DigoCast) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/diego-castilho-8b87a8301/) |
| Team Member | Vinicius Elias | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/ViniElias) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/vinicius-elias-895332235/) |
| Team Member | Ygor Pereira | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/YgorPereira) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ygorrpereira/) |

<img width="438" height="149" alt="bug-busters-logo-black" src="https://github.com/user-attachments/assets/7757bc1a-e087-46e7-8170-b423b614d4ac" />
