# Log API .Net Core 3.1 com Elastic Search e Serilog

Projeto de exemplo de utilização do Serilog em conjunto com ElasticSearch e o Kibana, utilizando Docker.

### Executar o Projeto:

Pré requisitos: :.Net Core 3.1, Docker e VSCode

1. Clone o repositório

2. Na pasta do projeto execute:

```
dotnet restore
```

3. Ainda na pasta do projeto suba os containers:

```
docker-compose up -d
```

4. Acesse a aplicação para registrar um log de exemplo: http://localhost:8080

5. Vá a url do kibana: http://localhost:5601 - será necessário definir um novo padrão de índice (Index Pattern) para exibir os dados: https://www.elastic.co/guide/en/kibana/current/tutorial-define-index.html 

6 - Após isso basta acessar o discovery do Kibana e é possível ver os logs.


## Preparando o Projeto

- (Em breve passo a passo da criação e configuração do projeto)

### Serilog - https://serilog.net/

### ElasticSearch - https://www.elastic.co/

### Kibana - https://www.elastic.co/pt/kibana


### Docker e Docker Compose

O Docker Compose é uma ferramenta para definir e executar aplicações em multiplos containers de forma orquestrada, para ser possível executar a aplicação de forma simplificada com os 3 containers necessários (APP, Kibana, ElasticSearch) usamos o docker compose, para isso é necessário um arquivo de configuração onde se encontra as definições de imagem e infraestrutura necessárias para a aplicação, segue o exemplo para esse caso:

``` yaml

version: '3.4'

services:
  
  elasticsearch:
   container_name: elasticsearch
   image: docker.elastic.co/elasticsearch/elasticsearch:7.9.2
   ports:
    - 9200:9200
   volumes:
    - elasticsearch-data:/usr/share/elasticsearch/data
   environment:
    - node.name=elasticsearch
    - xpack.monitoring.enabled=true
    - xpack.watcher.enabled=false
    - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    - discovery.type=single-node
   ulimits:
      memlock:
        soft: -1
        hard: -1

  kibana:
   container_name: kibana
   image: docker.elastic.co/kibana/kibana:7.9.2
   ports:
    - 5601:5601
   environment:
    - ELASTICSEARCH_URL=http://elasticsearch:9200

  logging:
    image: logging
    depends_on: [elasticsearch]
    build:
      context: .
      dockerfile: api/Dockerfile
    environment:
    - ASPNETCORE_ENVIRONMENT=Development
    ports:
      - 8080:80

volumes:
  elasticsearch-data:
```

Caso queira saber mais detalhes sobre o docker compose: https://docs.docker.com/compose/


