# Log API .Net Core 3.1 com Elastic Search e Serilog

Projeto de exemplo de utilização do Serilog em conjunto com ElasticSearch e o Kibana, utilizando Docker.


## Preparando o Projeto

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


