# <span style="color:orange">Gestão</span> <span style="color:lightblue">de Regras</span>

Projeto de API - **6º Semestre (2026-2)** da Fatec São José dos Campos - **Bug Busters**

O objetivo deste projeto é desenvolver uma plataforma para o gerenciamento de regras de negócio e o cálculo de comissionamento, utilizando técnicas de engenharia de software assistida por IA. A aplicação busca centralizar regras que podem sofrer alterações ao longo do tempo, permitindo sua criação, validação, simulação, ativação e rastreabilidade.

O sistema integra bases de RH, vendas e comissionamento para realizar apurações mensais, considerando cargos, marcas, lojas, períodos de competência e excepcionalidades como admissões, demissões, afastamentos, férias e bônus. Também utiliza IA generativa para interpretar regras descritas em linguagem natural, apoiar simulações e explicar os resultados apresentados aos usuários.

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
  <img src="https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D" />
  <img src="https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
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
| 1 | Alta | Como gestor de negócios, quero cadastrar, editar e remover regras de negócio manualmente para ter controle sobre as comissões. | 3 | 1 |
| 2 | Alta | Como administrador, quero que o motor de regras cruze uma venda com uma regra de comissão ativa para calcular o valor a ser pago. | 5 | 1 |
| 3 | Média | Como administrador, quero que o sistema bloqueie o cadastro de regras de negócio que não possuam data final definida, para evitar comissionamentos por tempo indeterminado. | 2 | 1 |
| 4 | Alta | Como administrador, quero que cada cálculo de comissão executado gere um log imutável para garantir a rastreabilidade em caso de contestações financeiras. | 3 | 1 |
| 5 | Alta | Como gestor de negócios, quero digitar uma regra em texto livre e ter o sistema traduzindo-a para o padrão executável da aplicação. | 8 | 1 |
| 6 | Alta | Como administrador, quero importar as bases de RH, vendas e taxas de comissão para compor os dados do sistema e viabilizar o cálculo de comissionamento por competência. | 8 | 1 |
| 7 | Alta | Como gestor de negócios, quero simular o impacto financeiro de uma nova regra processando um histórico de vendas (sandbox). | 8 | 2 |
| 8 | Alta | Como gestor de negócios, quero revisar o resultado da simulação antes de confirmar a ativação da regra em produção. | 5 | 2 |
| 9 | Média | Como gestor de negócios, quero ser alertado quando houver anomalias nas vendas de um colaborador (outliers). | 8 | 3 |
| 10 | Baixa | Como gestor de negócios, quero receber sugestões de ajuste nos parâmetros caso a simulação estoure meu orçamento de comissões. | 5 | 3 |
| 11 | Média | Como gestor de negócios, quero emitir o relatório de fechamento do período detalhando comissões por canal e equipe. | 5 | 3 |
| 12 | Média | Como administrador, quero visualizar o histórico de alterações das regras de negócio (logs de auditoria) para saber quem modificou taxas ou validades. | 3 | 3 |

Estimativas em Story Points.

### Roadmap

<img width="1246" height="696" alt="image" src="https://github.com/user-attachments/assets/36b05001-fd0c-46f8-9b3f-35a6b42c23c0" />

---

### Sprint 1

VIDEO AQUI

### Sprint 2

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
