Relatório de Testes – Time de QA
Projeto: Jornada_Fast_ADOS – Análise de Dados das Ordens de Serviço
Responsável: Danilo
Foco: Testes de infraestrutura com Docker

Objetivo do Teste
O objetivo do teste foi verificar se a aplicação está configurada corretamente para rodar em ambiente Docker, garantindo que os serviços essenciais, como o banco de dados e a API FastAPI, são inicializados com sucesso.

Ambiente de Testes
Ferramenta utilizada: Docker + Docker Compose

Sistema operacional: (adicione se quiser, ex: Ubuntu 22.04 / Windows 11)

Comando executado:
docker compose up -d

Containers Envolvidos
fastapi_app: container da API desenvolvida em FastAPI

postgres_db: container do banco de dados PostgreSQL

jornada_fast_ados_default: rede criada automaticamente

jornada_fast_ados_postgres_data: volume para persistência de dados

Resultado dos Testes
Após a execução do comando docker compose up -d, todos os containers foram inicializados corretamente, conforme indicado na saída do terminal:

[+] Running 5/5
✔ api Built
✔ Network jornada_fast_ados_default Created
✔ Volume "jornada_fast_ados_postgres_data" Created
✔ Container postgres_db Started
✔ Container fastapi_app Started

A verificação dos containers com o comando docker ps também confirmou que os serviços estavam em execução contínua, sem interrupções.

Observações
A infraestrutura Docker do projeto está funcionando como esperado.

Tanto o banco de dados quanto a API estão sendo inicializados corretamente.

A criação de volume e rede Docker ocorreu automaticamente, sem falhas.

A documentação Swagger da API foi acessada com sucesso em http://localhost:8000/docs, confirmando que o container da API está funcional.

Conclusão
O ambiente Docker do projeto Jornada_Fast_ADOS está operacional e preparado para receber evoluções no desenvolvimento da aplicação. Os testes de infraestrutura com Docker foram bem-sucedidos e não foram encontrados erros na inicialização dos containers.
