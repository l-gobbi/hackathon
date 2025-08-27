# Simulação de crédito

Este projeto é uma API REST para simulação de crédito, desenvolvida com Quarkus. 
A aplicação permite aos usuários simular ofertas de crédito com base no valor desejado e no prazo, 
calculando os resultados pelos sistemas de amortização SAC e Price.

## 📝 Decisões de Design

- Persistência de Dados: Como não foi definido nos requisitos do desafio, para o armazenamento das simulações, optou-se por salvar os dados calculados com base no sistema de amortização Price.


- Cache: Foi implementado um cache para as consultas de produtos e relatórios diários para melhorar a performance. Um scheduler foi configurado para limpar o cache diariamente à meia-noite (0 0 0 * * ?), garantindo que os dados não fiquem desatualizados. Um endpoint de limpeza manual também foi disponibilizado para invalidação imediata.

## Pré-requisitos para executar o projeto
[WSL (Windows Subsystem for Linux)](https://learn.microsoft.com/pt-br/windows/wsl/install)

[Docker](https://docs.docker.com/get-started/get-docker/) e [Docker Compose](https://docs.docker.com/compose/install/)

Para rodar o projeto, execute o comando:
- "docker compose up --build"

## Tecnologias Utilizadas
- Framework: Quarkus

- Linguagem: Java 17

- Banco de Dados: PostgreSQL (para simulações) e Microsoft SQL Server (para consulta de produtos)

- Mensageria: Azure Event Hubs

- Monitoramento: Prometheus e Grafana

- Contêineres: Docker e Docker Compose

- Cache: Quarkus Cache

- Rate Limiting: Bucket4j

## Funcionalidades principais

- Criação de Simulações: Endpoint para criar novas simulações de crédito.

- Listagem de Simulações: Consulta paginada de todas as simulações realizadas.

- Relatórios Diários: Agregação de dados das simulações por dia.

- Telemetria: Métricas de performance dos endpoints.

- Cache: Mecanismo de cache para otimizar a busca de produtos e relatórios diários. 
    - Limpeza Agendada: Um scheduler limpa o cache diariamente à meia-noite para evitar dados desatualizados.
    - Limpeza Manual: Um endpoint permite a limpeza imediata do cache.

- Health Check: Verificação da saúde das conexões com os bancos de dados.

- Rate Limiting: Proteção contra um número excessivo de requisições.¹

- Métricas: Coleta de métricas com Prometheus e visualização em dashboards do Grafana.

¹ Para ajustar o número de requisições permitidas por segundo por um
  usuário altere a variável "quarkus.rate-limiter.buckets.simulacoes.limits[0].permitted-uses"
  no arquivo application.properties. O valor definido por padrão foi 150, mas o máximo que
  a aplicação suporta pode variar dependendo da capacidade dos dispositivos utilizados.



## Endpoints
- POST http://localhost:8080/api/v1/simulacoes Cria uma nova simulação de crédito.

- GET http://localhost:8080/api/v1/simulacoes Lista as simulações de forma paginada.

- GET http://localhost:8080/api/v1/simulacoes/diarias Retorna um relatório diário de simulações.

- GET http://localhost:8080/api/v1/telemetria Exibe as métricas de telemetria da aplicação.

- POST http://localhost:8080/api/v1/cache/clear Limpa o cache da manualmente.

- Health Check: http://localhost:8080/q/health/

- Swagger UI (Documentação da API): http://localhost:8080/q/swagger-ui/ (também é possível testar no postman através do collection que está na raiz do projeto)

- Grafana Dashboards: http://localhost:3000/dashboards (para facilitar o teste do avaliador foi mantido o login "admin" e a senha "admin" para o grafana)
