# API 6º Semestre ADS — Gestão de Regras

## Documentação — Sprint 1

<p align="center">
  <img src="https://github.com/user-attachments/assets/7757bc1a-e087-46e7-8170-b423b614d4ac" alt="Logo da Bug Busters" width="250">
</p>
<h2 align="center">Bug Busters</h2>

<p align="center">
  <a href="#desafio">Desafio</a> |
  <a href="#us">User Stories</a> |
  <a href="#dor">DoR</a> |
  <a href="#dod">DoD</a> |
  <a href="#burndown">Burndown</a> |
  <a href="#equipe">Equipe</a>
</p>

> **Status da Sprint:** Em andamento  
> **Previsão de entrega:** 27/09/2026  
> **Data de início:** 07/09/2026  
> **Cliente:** Dom Rock  
> **Instituição:** Fatec São José dos Campos — ADS, 6º semestre (2026-2)

[Voltar ao README](../README.md)

## 🏅 Desafio <a id="desafio"></a>

Implementar as funcionalidades iniciais de gestão de regras de negócio e cálculo de comissionamento: cadastro, edição e remoção manual de regras; aplicação de regras ativas às vendas; bloqueio do cadastro de regras sem data final definida; geração de logs imutáveis dos cálculos; tradução de regras em linguagem natural para o padrão executável da aplicação; e importação das bases de RH, vendas e taxas de comissão para viabilizar o cálculo por competência.

## 📋 User Stories <a id="us"></a>

Histórias 1 a 6 do [backlog do produto](../README.md#backlog-do-produto), previstas para a Sprint 1.

| Rank | Prioridade | User Story | Story Points | Sprint | Status |
| :--: | :--------: | :--------- | :----------: | :----: | :----: |
| 1 | Alta | Como gestor de negócios, quero cadastrar, editar e remover regras de negócio manualmente para ter controle sobre as comissões. | 3 | 1 | Backlog |
| 2 | Alta | Como administrador, quero que o motor de regras cruze uma venda com uma regra de comissão ativa para calcular o valor a ser pago. | 5 | 1 | Backlog |
| 3 | Média | Como administrador, quero que o sistema bloqueie o cadastro de regras de negócio que não possuam data final definida, para evitar comissionamentos por tempo indeterminado. | 2 | 1 | Backlog |
| 4 | Alta | Como administrador, quero que cada cálculo de comissão executado gere um log imutável para garantir a rastreabilidade em caso de contestações financeiras. | 3 | 1 | Backlog |
| 5 | Alta | Como gestor de negócios, quero digitar uma regra em texto livre e ter o sistema traduzindo-a para o padrão executável da aplicação. | 8 | 1 | Backlog |
| 6 | Alta | Como administrador, quero importar as bases de RH, vendas e taxas de comissão para compor os dados do sistema e viabilizar o cálculo de comissionamento por competência. | 8 | 1 | Backlog |

**Total estimado: 29 Story Points.**

Status não informado na documentação: **PRENCHER**.

## 🏅 DoR — Definition of Ready <a id="dor"></a>

Critérios sugeridos a partir do modelo, para verificar se uma história está pronta para desenvolvimento. **Validação dos critérios pela equipe: PRENCHER.**

| Critério | Descrição |
| :------- | :-------- |
| Clareza na descrição | A User Story descreve a pessoa usuária, a ação desejada e o objetivo. |
| Critérios de aceitação | A história possui critérios objetivos que indicam o que é necessário para considerá-la concluída. |
| Cenários de teste | A história possui pelo menos um cenário de teste estruturado em Dado, Quando e Então. |
| Dependências identificadas | As dependências de outras histórias, dados e serviços estão identificadas e disponíveis para iniciar o desenvolvimento. |
| Referência visual | O protótipo correspondente está disponível e vinculado, quando houver interface a implementar. |
| Escopo técnico validado | Está definido se a história envolve frontend, backend, IA ou integração entre esses componentes. |
| Regras de negócio definidas | As regras de comissionamento, entradas, resultados esperados e exceções aplicáveis à história estão descritos. |
| Estimativa definida | A história possui uma estimativa discutida pela equipe. |
| Documentos de apoio | Os arquivos, exemplos de dados e instruções necessários estão disponíveis. |
| Validação com PO | A história foi discutida com o Product Owner e compreendida pela equipe técnica. |

## 🏅 DoD — Definition of Done <a id="dod"></a>

Critérios sugeridos a partir do modelo, para verificar se uma história pode ser considerada concluída. **Validação dos critérios pela equipe: PRENCHER.**

| Critério | Descrição |
| :------- | :-------- |
| Critérios de aceitação atendidos | Todos os critérios definidos para a história foram implementados e validados. |
| Regras de negócio validadas | O comportamento foi conferido com exemplos de entrada e saída e com as exceções previstas na história. |
| Dados persistidos corretamente | As operações de cadastro, alteração e exclusão persistem corretamente, quando aplicável. |
| Testes executados | Os cenários definidos para a história foram executados, incluindo casos de erro pertinentes. |
| Código revisado | A implementação passou por revisão de código entre integrantes da equipe. |
| Funcionalidade integrada | A funcionalidade foi integrada e testada no fluxo correspondente da aplicação. |
| Documentação atualizada | As regras, os endpoints e as instruções afetadas pela implementação foram documentados. |
| Validação funcional pelo PO | O Product Owner verificou e aprovou a entrega da história. |

 ## 🏅 Sprint Burndown <a id="burndown"></a>

**Gráfico do burndown da Sprint 1:** 

<img width="1560" height="735" alt="image" src="https://github.com/user-attachments/assets/e91b3bd8-4e24-41f2-8759-5564cee61cbc" />

**Período e dados de acompanhamento:** 07/09 - 27/09

## 🎓 Equipe <a id="equipe"></a>

| Membro | Função | GitHub | LinkedIn |
| :--- | :--- | :---: | :---: |
| Davi Miyake | Product Owner | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/DaviMBDev) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://br.linkedin.com/in/davimiyakeb) |
| Renan Tomasi | Scrum Master | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/renan21-tg) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/renan-tomasi/) |
| Humberto Ishii | Team Member | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/HumbertoIshii) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://br.linkedin.com/in/humberto-ishii-silva-754489161) |
| Diego Castilho | Team Member | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/DigoCast) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/diego-castilho-8b87a8301/) |
| Vinicius Elias | Team Member | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/ViniElias) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/vinicius-elias-895332235/) |
| Ygor Pereira | Team Member | [![GitHub Badge](https://img.shields.io/badge/GitHub-111217?style=flat-square&logo=github&logoColor=white)](https://github.com/YgorPereira) | [![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ygorrpereira/) |
